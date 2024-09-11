# PyTorch 模型性能分析与优化 — 第6部分

> 原文：[https://towardsdatascience.com/pytorch-model-performance-analysis-and-optimization-part-6-b87412a0371b?source=collection_archive---------3-----------------------#2023-09-20](https://towardsdatascience.com/pytorch-model-performance-analysis-and-optimization-part-6-b87412a0371b?source=collection_archive---------3-----------------------#2023-09-20)

## 如何使用 PyTorch Profiler、PyTorch Hooks 和 TensorBoard 识别和分析反向传播中的性能问题

[](https://chaimrand.medium.com/?source=post_page-----b87412a0371b--------------------------------)[![Chaim Rand](../Images/c52659c389f167ad5d6dc139940e7955.png)](https://chaimrand.medium.com/?source=post_page-----b87412a0371b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b87412a0371b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b87412a0371b--------------------------------) [Chaim Rand](https://chaimrand.medium.com/?source=post_page-----b87412a0371b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9440b37e27fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpytorch-model-performance-analysis-and-optimization-part-6-b87412a0371b&user=Chaim+Rand&userId=9440b37e27fe&source=post_page-9440b37e27fe----b87412a0371b---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b87412a0371b--------------------------------) · 8分钟阅读 · 2023年9月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb87412a0371b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpytorch-model-performance-analysis-and-optimization-part-6-b87412a0371b&user=Chaim+Rand&userId=9440b37e27fe&source=-----b87412a0371b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb87412a0371b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpytorch-model-performance-analysis-and-optimization-part-6-b87412a0371b&source=-----b87412a0371b---------------------bookmark_footer-----------)![](../Images/acfc647db438fa36a7a5dabe53b3b43a.png)

图片来源：[David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是我们关于使用[PyTorch Profiler](https://pytorch.org/tutorials/recipes/recipes/profiler_recipe.html)和[TensorBoard](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html)分析和优化PyTorch模型的[系列文章](/pytorch-model-performance-analysis-and-optimization-10c3c5822869)的第六部分。在这篇文章中，我们将探讨分析中一种更复杂的性能问题——**训练步骤的反向传播过程中的瓶颈**。我们将解释这种类型的瓶颈为什么特别具有挑战性，并提出一种使用PyTorch内置的[为训练步骤的不同部分附加钩子](https://pytorch.org/docs/stable/generated/torch.nn.Module.html#torch.nn.Module.register_full_backward_hook)来分析它的方法。非常感谢[Yitzhak Levi](https://www.linkedin.com/in/yitzhak-levi-49a217201/)对本文的贡献。

# 玩具模型

为了方便讨论，我们使用流行的[timm](https://pypi.org/project/timm/) Python模块（版本0.9.7）定义了一个简单的基于Vision Transformer（ViT）的分类模型。我们将模型定义为*patch_drop_rate*标志设置为*0.5*，这会使模型在每个训练步骤中随机丢弃一半的补丁。训练脚本被编程为[最小化非确定性](https://pytorch.org/docs/stable/notes/randomness.html)，使用[torch.use_deterministic_algorithms](https://pytorch.org/docs/stable/generated/torch.use_deterministic_algorithms.html#torch.use_deterministic_algorithms)函数和[cuBLAS](https://docs.nvidia.com/cuda/cublas/index.html#cublasApi_reproducibility)环境变量*CUBLAS_WORKSPACE_CONFIG*。请参见下面的代码块以获取完整的模型定义：

```py
import torch, time, os
import torch.optim
import torch.profiler
import torch.utils.data
from timm.models.vision_transformer import VisionTransformer
from torch.utils.data import Dataset

# use the GPU
device = torch.device("cuda:0")

# configure PyTorch to use reproducible algorithms
torch.manual_seed(0)
os.environ[
        "CUBLAS_WORKSPACE_CONFIG"
    ] = ":4096:8"
torch.use_deterministic_algorithms(True)

# define the ViT-backed classification model
model = VisionTransformer(patch_drop_rate=0.5).cuda(device)
# define the loss function
loss_fn = torch.nn.CrossEntropyLoss()
# define the training optimizer
optimizer = torch.optim.SGD(model.parameters(), lr=0.001, momentum=0.9)

# use random data
class FakeDataset(Dataset):
    def __len__(self):
        return 1000000

    def __getitem__(self, index):
        rand_image = torch.randn([3, 224, 224], dtype=torch.float32)
        label = torch.tensor(data=[index % 1000], dtype=torch.int64)
        return rand_image, label

train_set = FakeDataset()
train_loader = torch.utils.data.DataLoader(train_set, batch_size=128, 
                                           num_workers=8, pin_memory=True)

t0 = time.perf_counter()
summ = 0
count = 0
model.train()

# training loop wrapped with profiler object
with torch.profiler.profile(
    schedule=torch.profiler.schedule(wait=1, warmup=4, active=3, repeat=1),
    on_trace_ready=torch.profiler.tensorboard_trace_handler('/tmp/perf')
) as prof:
    for step, data in enumerate(train_loader):
        inputs = data[0].to(device=device, non_blocking=True)
        label = data[1].squeeze(-1).to(device=device, non_blocking=True)
        with torch.profiler.record_function('forward'):
            outputs = model(inputs)
            loss = loss_fn(outputs, label)
        optimizer.zero_grad(set_to_none=True)
        with torch.profiler.record_function('backward'):
            loss.backward()
        with torch.profiler.record_function('optimizer_step'):
            optimizer.step()
        prof.step()
        batch_time = time.perf_counter() - t0
        if step > 1:  # skip first step
            summ += batch_time
            count += 1
        t0 = time.perf_counter()
        if step > 500:
            break

    print(f'average step time: {summ/count}')
```

我们将在一个[Amazon EC2 g5.2xlarge](https://aws.amazon.com/ec2/instance-types/g5/)实例（包含NVIDIA A10G GPU和8个vCPUs）上进行实验，并使用官方[AWS PyTorch 2.0 Docker镜像](https://github.com/aws/deep-learning-containers)。

# 初始性能结果

在下面的图像中，我们捕捉了在[TensorBoard插件](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html)*Trace View*中显示的性能结果：

![](../Images/8a66a097dd3a3a3ed1df10d9c683b002.png)

反向传播中的瓶颈（作者）

在训练步骤的前向传递中，操作被集中在顶部线程中，但在底部线程的反向传递中似乎出现了性能问题。在这里我们看到一个操作，*GatherBackward*，占用了大量的追踪部分。仔细观察，我们可以看到底层操作包括“to”、“copy_”和“cudaStreamSynchronize”。正如我们在[第2部分](/pytorch-model-performance-analysis-and-optimization-part-2-3bc241be91)中看到的，这些操作通常表示数据从主机复制到设备——这是我们在训练步骤中希望避免的。

到这个时候，你自然会问：为什么会发生这种情况？我们模型定义的哪个部分导致了这个问题？*GatherBackward*追踪提示可能涉及一个[torch.gather](https://pytorch.org/docs/stable/generated/torch.gather.html)操作，但它来自哪里以及为什么会引发同步事件呢？

在我们之前的帖子中（例如，[这里](/pytorch-model-performance-analysis-and-optimization-part-2-3bc241be91)），我们提倡使用标记的[torch.profiler.record_function](https://pytorch.org/tutorials/beginner/profiler.html#performance-debugging-using-profiler)上下文管理器来确定性能问题的来源。这里的问题是性能问题发生在反向传递中，而我们无法控制！特别是，**我们无法将反向传递中的单个操作用上下文管理器包裹。** 理论上，可以通过深入分析追踪视图并将反向传递中的每个片段与前向传递中的对应操作匹配来识别问题模型操作。然而，这不仅可能非常繁琐，而且还需要对模型训练步骤的所有底层操作有深入的了解。使用[torch.profiler.record_function](https://pytorch.org/tutorials/beginner/profiler.html#performance-debugging-using-profiler)标签的优点在于它允许我们轻松定位模型中的问题部分。理想情况下，我们希望即使在反向传递中出现性能问题时，也能保留相同的能力。在下一部分，我们将描述如何使用PyTorch hooks实现这一点。

# 使用PyTorch反向钩子的性能分析

尽管 PyTorch 不允许你封装单独的反向传播操作，但它允许你使用其钩子支持来前置和/或后置自定义功能。PyTorch 支持为[torch.Tensors](https://pytorch.org/docs/stable/generated/torch.Tensor.register_hook.html)和[torch.nn.Modules](https://pytorch.org/docs/stable/generated/torch.nn.Module.html#torch.nn.Module.register_full_backward_hook)注册钩子。尽管我们在本文中提出的技术将依赖于**向模块注册反向钩子**，但张量钩子的注册也可以类似地用于替换或增强基于模块的方法。

在下面的代码块中，我们定义了一个包装函数，该函数接受一个模块并注册一个[full_backward_hook](https://pytorch.org/docs/stable/generated/torch.nn.Module.html#torch.nn.Module.register_full_backward_hook)和一个[full_backward_pre_hook](https://pytorch.org/docs/stable/generated/torch.nn.Module.html#torch.nn.Module.register_full_backward_pre_hook)（尽管在实际应用中一个就足够了）。每个钩子被编程为使用[torch.profiler.record_function](https://pytorch.org/tutorials/beginner/profiler.html#performance-debugging-using-profiler)函数向捕获的性能分析跟踪中添加一条消息。[backward_pre_hook](https://pytorch.org/docs/stable/generated/torch.nn.Module.html#torch.nn.Module.register_full_backward_pre_hook)被编程为打印一个“before”消息，而[backward_hook](https://pytorch.org/docs/stable/generated/torch.nn.Module.html#torch.nn.Module.register_full_backward_hook)则打印一个“after”消息。一个可选的*details*字符串被附加以区分相同模块类型的多个实例。

```py
def backward_hook_wrapper(module, details=None):

    # define register_full_backward_pre_hook function
    def bwd_pre_hook_print(self, output):
        message = f'before backward of {module.__class__.__qualname__}'
        if details:
            message = f'{message}: {details}'
        with torch.profiler.record_function(message):
            return output

    # define register_full_backward_hook function
    def bwd_hook_print(self, input, output):
        message = f'after backward of {module.__class__.__qualname__}'
        if details:
            message = f'{message}: {details}'
        with torch.profiler.record_function(message):
            return input

    # register hooks
    module.register_full_backward_pre_hook(bwd_pre_hook_print)
    module.register_full_backward_hook(bwd_hook_print)
    return module
```

使用*backward_hook_wrapper*函数，我们可以开始定位性能问题的来源。我们从封装模型和损失函数开始，如下代码块所示：

```py
model = backward_hook_wrapper(model)
loss_fn = backward_hook_wrapper(loss_fn)
```

使用[TensorBoard 插件](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html) *Trace View* 的搜索框，我们可以识别“before”和“after”消息的位置，并推断出模型和损失的反向传播开始和结束的位置。这使我们能够得出性能问题发生在模型的反向传播过程中。下一步是用我们的*backward_hook_wrapper*函数封装 Vision Transformer 的内部模块：

```py
model.patch_embed = backward_hook_wrapper(model.patch_embed)
model.pos_drop = backward_hook_wrapper(model.pos_drop)
model.patch_drop = backward_hook_wrapper(model.patch_drop)
model.norm_pre = backward_hook_wrapper(model.norm_pre)
model.blocks = backward_hook_wrapper(model.blocks)
model.norm = backward_hook_wrapper(model.norm)
model.fc_norm = backward_hook_wrapper(model.fc_norm)
model.head_drop = backward_hook_wrapper(model.head_drop)
```

在上面的代码块中，我们指定了每个内部模块。另一种将模型的一级模块全部封装起来的方法是迭代其[命名子模块](https://pytorch.org/docs/stable/generated/torch.nn.Module.html#torch.nn.Module.named_children)：

```py
for submodule in model.named_children():
    submodule = backward_hook_wrapper(submodule)
```

下图显示了“before backward of PatchDropout”消息出现在有问题的*GatherBackward*操作之前：

![](../Images/781626897e23f9935309b0fe43cd9da6.png)

在 Trace View 中识别有问题的反向操作的来源（作者）

我们的性能分析表明，性能问题的来源是[*PathDropout*](https://github.com/huggingface/pytorch-image-models/blob/v0.9.7/timm/layers/patch_dropout.py#L7)模块。检查该模块的[*forward*](https://github.com/huggingface/pytorch-image-models/blob/v0.9.7/timm/layers/patch_dropout.py#L27)函数，我们确实看到调用了[torch.gather](https://pytorch.org/docs/stable/generated/torch.gather.html)。

对于我们的示例模型，我们只需进行两次分析迭代即可找出性能问题的来源。在实际应用中，可能需要额外的迭代。

请注意，PyTorch包括[torch.nn.modules.module.register_module_full_backward_hook](https://pytorch.org/docs/stable/generated/torch.nn.modules.module.register_module_full_backward_hook.html)函数，它会在一次调用中将一个hook附加到训练步骤中的*所有*模块上。虽然这在简单的情况下（如我们的示例）可能足够，但它不能区分相同模块类型的不同实例。

现在我们已经知道了性能问题的来源，我们可以开始着手修复它。

# 优化建议：尽可能使用索引代替gather

既然我们知道问题的来源在于[torch.gather](https://pytorch.org/docs/stable/generated/torch.gather.html)操作的DropPatches模块，我们可以研究一下导致长时间主机-设备同步事件的触发因素。我们的调查使我们回到了[torch.use_deterministic_algorithms](https://pytorch.org/docs/stable/generated/torch.use_deterministic_algorithms.html)函数的文档，该函数告知我们，*当作用于需要梯度的CUDA张量时*，[torch.gather](https://pytorch.org/docs/stable/generated/torch.gather.html)表现出非确定性行为，除非[torch.use_deterministic_algorithms](https://pytorch.org/docs/stable/generated/torch.use_deterministic_algorithms.html)函数的*mode*设置为*True*。换句话说，通过将脚本配置为使用确定性算法，我们修改了[torch.gather](https://pytorch.org/docs/stable/generated/torch.gather.html)反向传递的默认行为。事实证明，正是这种变化导致了同步事件的需求。确实，如果我们去除这个配置，性能问题就会消失！问题是，我们能否在不付出性能代价的情况下保持算法的确定性。

在下面的代码块中，我们提出了一种[ *PathDropout* ](https://github.com/huggingface/pytorch-image-models/blob/v0.9.7/timm/layers/patch_dropout.py#L7)模块的*forward*函数的替代实现，它使用[torch.Tensor索引](https://pytorch.org/cppdocs/notes/tensor_indexing.html)而不是[torch.gather](https://pytorch.org/docs/stable/generated/torch.gather.html)来生成相同的输出。修改的代码行已被突出显示。

```py
from timm.layers import PatchDropout

class MyPatchDropout(PatchDropout):
    def forward(self, x):
        prefix_tokens = x[:, :self.num_prefix_tokens]
        x = x[:, self.num_prefix_tokens:]
        B = x.shape[0]
        L = x.shape[1]
        num_keep = max(1, int(L * (1\. - self.prob)))
        keep_indices = torch.argsort(torch.randn(B, L, device=x.device),
                                     dim=-1)[:, :num_keep]

        # The following three lines were modified from the original
        # to use PyTorch indexing rather than torch.gather
        stride = L * torch.unsqueeze(torch.arange(B, device=x.device), 1)
        keep_indices = (stride + keep_indices).flatten()
        x = x.reshape(B * L, -1)[keep_indices].view(B, num_keep, -1)

        x = torch.cat((prefix_tokens, x), dim=1)
        return x

model.patch_drop = MyPatchDropout(
    prob = model.patch_drop.prob,
    num_prefix_tokens = model.patch_drop.num_prefix_tokens
)
```

在下图中，我们捕捉了上述更改后的*跟踪视图*：

![](../Images/3f3ef66d8d70c3d1c0f51b9836b25c16.png)

优化后的跟踪视图（作者提供）

我们可以清楚地看到，冗长的同步事件不再出现。

在我们的玩具模型中，我们很幸运地使用了[torch.gather](https://pytorch.org/docs/stable/generated/torch.gather.html)操作的方式使其可以被PyTorch索引替代。自然地，这并非总是如此；其他[torch.gather](https://pytorch.org/docs/stable/generated/torch.gather.html)的用法可能没有基于索引的等效实现。

# 结果

在下表中，我们比较了在不同场景下训练我们的玩具模型的性能结果：

![](../Images/4747889a916d822f41a4c4b7a40676c3.png)

优化结果（作者提供）

在我们的玩具示例中，优化带来了适度但可测量的影响——约2%的性能提升。有趣的是，在可重复模式下的torch索引表现优于默认的（非确定性）[torch.gather](https://pytorch.org/docs/stable/generated/torch.gather.html)。基于这些发现，评估使用索引而非[torch.gather](https://pytorch.org/docs/stable/generated/torch.gather.html)的选项可能是一个好主意。

# 总结

尽管PyTorch（正当的）以易于调试和追踪而闻名，[torch.autograd](https://pytorch.org/tutorials/beginner/blitz/autograd_tutorial.html)仍然有些神秘，分析训练步骤的反向传递可能相当困难。为了应对这一挑战，PyTorch支持在反向传播的不同阶段插入钩子。在这篇文章中，我们展示了如何使用PyTorch反向钩子，以及[torch.profiler.record_function](https://pytorch.org/tutorials/beginner/profiler.html#performance-debugging-using-profiler)，通过迭代过程识别反向传递中的性能问题来源。我们将这种技术应用于一个简单的ViT模型，并了解了[torch.gather](https://pytorch.org/docs/stable/generated/torch.gather.html)操作的一些细微差别。

在这篇文章中，我们探讨了一种非常特定的性能瓶颈。请务必查看我们的[其他中等博客文章](https://chaimrand.medium.com/)，它们涵盖了关于性能分析和机器学习工作负载优化的各种话题。
