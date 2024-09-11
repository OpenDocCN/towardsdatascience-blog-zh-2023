# PyTorch 模型性能分析与优化

> 原文：[https://towardsdatascience.com/pytorch-model-performance-analysis-and-optimization-10c3c5822869?source=collection_archive---------1-----------------------#2023-06-12](https://towardsdatascience.com/pytorch-model-performance-analysis-and-optimization-10c3c5822869?source=collection_archive---------1-----------------------#2023-06-12)

## 如何使用 PyTorch Profiler 和 TensorBoard 加速训练并降低成本

[](https://chaimrand.medium.com/?source=post_page-----10c3c5822869--------------------------------)[![Chaim Rand](../Images/c52659c389f167ad5d6dc139940e7955.png)](https://chaimrand.medium.com/?source=post_page-----10c3c5822869--------------------------------)[](https://towardsdatascience.com/?source=post_page-----10c3c5822869--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----10c3c5822869--------------------------------) [Chaim Rand](https://chaimrand.medium.com/?source=post_page-----10c3c5822869--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9440b37e27fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpytorch-model-performance-analysis-and-optimization-10c3c5822869&user=Chaim+Rand&userId=9440b37e27fe&source=post_page-9440b37e27fe----10c3c5822869---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----10c3c5822869--------------------------------) ·14 分钟阅读·2023年6月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F10c3c5822869&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpytorch-model-performance-analysis-and-optimization-10c3c5822869&user=Chaim+Rand&userId=9440b37e27fe&source=-----10c3c5822869---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F10c3c5822869&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpytorch-model-performance-analysis-and-optimization-10c3c5822869&source=-----10c3c5822869---------------------bookmark_footer-----------)![](../Images/7f84d26fc7820a898904c7592bfaed50.png)

图片由 [Torsten Dederichs](https://unsplash.com/es/@tdederichs?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

训练深度学习模型，尤其是大型模型，可能是一项昂贵的开支。我们可以用来管理这些成本的主要方法之一是性能优化。性能优化是一个迭代的过程，在这个过程中我们不断寻找提高应用程序性能的机会，并利用这些机会。在之前的文章中（例如，[这里](/overcoming-data-preprocessing-bottlenecks-with-tensorflow-data-service-nvidia-dali-and-other-d6321917f851)），我们强调了进行这种分析时拥有适当工具的重要性。所选择的工具可能会依赖于多种因素，包括训练加速器的类型（例如，GPU、HPU或其他）以及训练框架。

![](../Images/e7992ab67fcc7dccf2f93ae2d9e2c713.png)

性能优化流程（作者提供）

本文将重点讨论在GPU上使用PyTorch进行训练。更具体地说，我们将重点关注PyTorch内置的性能分析器，[PyTorch Profiler](https://pytorch.org/tutorials/recipes/recipes/profiler_recipe.html)，以及查看其结果的方式之一，[PyTorch Profiler TensorBoard插件](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html)。

本文并不打算取代官方的PyTorch文档，无论是关于[PyTorch Profiler](https://pytorch.org/tutorials/recipes/recipes/profiler_recipe.html)还是使用TensorBoard插件[分析性能分析结果](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html)。我们的意图是展示这些工具如何在日常开发过程中使用。实际上，如果你还没有阅读官方文档，我们建议你在阅读本文之前先查看一下官方文档。

一段时间以来，我特别对[TensorBoard插件教程](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html)中的一个部分感到好奇。该教程介绍了一个基于Resnet架构的分类模型，并在流行的Cifar10数据集上进行训练。它接着展示了如何使用PyTorch Profiler和TensorBoard插件来[识别和修复数据加载器中的瓶颈](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html#improve-performance-with-the-help-of-profiler)。输入数据管道中的性能瓶颈并不少见，我们在之前的一些文章中对此进行了详细讨论（例如，[这里](/overcoming-data-preprocessing-bottlenecks-with-tensorflow-data-service-nvidia-dali-and-other-d6321917f851)）。令我惊讶的是教程中呈现的最终（优化后）结果（截至本文撰写时），我们已将其粘贴如下：

![](../Images/3037fbe369a1e810193072797343f468.png)

优化后的性能（来自 [PyTorch 网站](https://pytorch.org/tutorials/_static/img/profiler_overview2.png)）

如果你仔细观察，你会发现优化后的 GPU 使用率为 40.46%。现在无法掩饰这一点：这些结果绝对糟糕，应该让你夜不能寐。正如我们以前扩展过的（例如，[这里](/cloud-ml-performance-checklist-caa51e798002)），GPU 是我们训练机器中最昂贵的资源，我们的目标应是最大化其使用率。40.46% 的利用率结果通常代表了显著的训练加速和成本节省机会。我们肯定能做得更好！在这篇博客文章中，我们将*尝试*做得更好。我们将从尝试重现官方教程中展示的结果开始，并看看是否可以使用相同的工具进一步提高训练性能。

# 玩具示例

以下代码块包含了由 [TensorBoard-plugin tutorial](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html) 定义的训练循环，做了两个小的修改：

1.  我们使用了一个与教程中使用的 [CIFAR10](https://pytorch.org/vision/stable/generated/torchvision.datasets.CIFAR10.html) 数据集具有相同属性和行为的虚拟数据集。进行此更改的动机可以在 [这里](http://groups.csail.mit.edu/vision/TinyImages/) 找到。

1.  我们用 *warmup* 标志设置为 *3* 和 *repeat* 标志设置为 *1* 初始化了 [torch.profiler.schedule](https://pytorch.org/docs/stable/profiler.html#torch.profiler.schedule)。我们发现，这稍微增加的预热步骤数量改善了分析结果的稳定性。

```py
import numpy as np
import torch
import torch.nn
import torch.optim
import torch.profiler
import torch.utils.data
import torchvision.datasets
import torchvision.models
import torchvision.transforms as T
from torchvision.datasets.vision import VisionDataset
from PIL import Image

class FakeCIFAR(VisionDataset):
    def __init__(self, transform):
        super().__init__(root=None, transform=transform)
        self.data = np.random.randint(low=0,high=256,size=(10000,32,32,3),dtype=np.uint8)
        self.targets = np.random.randint(low=0,high=10,size=(10000),dtype=np.uint8).tolist()

    def __getitem__(self, index):
        img, target = self.data[index], self.targets[index]
        img = Image.fromarray(img)
        if self.transform is not None:
            img = self.transform(img)
        return img, target

    def __len__(self) -> int:
        return len(self.data)

transform = T.Compose(
    [T.Resize(224),
     T.ToTensor(),
     T.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))])

train_set = FakeCIFAR(transform=transform)
train_loader = torch.utils.data.DataLoader(train_set, batch_size=32, 
                                           shuffle=True)

device = torch.device("cuda:0")
model = torchvision.models.resnet18(weights='IMAGENET1K_V1').cuda(device)
criterion = torch.nn.CrossEntropyLoss().cuda(device)
optimizer = torch.optim.SGD(model.parameters(), lr=0.001, momentum=0.9)
model.train()

# train step
def train(data):
    inputs, labels = data[0].to(device=device), data[1].to(device=device)
    outputs = model(inputs)
    loss = criterion(outputs, labels)
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()

# training loop wrapped with profiler object
with torch.profiler.profile(
        schedule=torch.profiler.schedule(wait=1, warmup=4, active=3, repeat=1),
        on_trace_ready=torch.profiler.tensorboard_trace_handler('./log/resnet18'),
        record_shapes=True,
        profile_memory=True,
        with_stack=True
) as prof:
    for step, batch_data in enumerate(train_loader):
        if step >= (1 + 4 + 3) * 1:
            break
        train(batch_data)
        prof.step()  # Need to call this at the end of each step 
```

在教程中使用的 GPU 是 Tesla V100-DGXS-32GB。在这篇文章中，我们尝试重现并改进教程中的性能结果，使用的是包含 Tesla V100-SXM2–16GB GPU 的 [Amazon EC2 p3.2xlarge](https://aws.amazon.com/ec2/instance-types/p3/) 实例。尽管它们共享相同的架构，但这两种 GPU 之间存在一些差异，你可以在 [这里](https://www.nvidia.com/en-us/data-center/v100/) 了解更多信息。我们使用 [AWS PyTorch 2.0 Docker 镜像](https://github.com/aws/deep-learning-containers) 运行训练脚本。TensorBoard 观众概览页面中显示的训练脚本性能结果如下面的图像所示：

![](../Images/1be993ae78153718412f84ece3a05abf.png)

TensorBoard Profiler 概览标签中的基线性能结果（作者捕获）

我们首先注意到，与教程相反，我们实验中的概览页面（[torch-tb-profiler](https://pypi.org/project/torch-tb-profiler/) 版本0.4.1）将三个分析步骤合并为一个。因此，平均总体步骤时间是80毫秒，而不是报告中的240毫秒。这在Trace标签中可以清楚地看到（根据我们的经验，Trace标签几乎总是提供更准确的报告），每个步骤大约需要80毫秒。

![](../Images/fa7e6b427fb8929b878a816affdb8996.png)

TensorBoard Profiler跟踪视图标签中的基准性能结果（作者捕获）

请注意，我们的起始点31.65% GPU利用率和80毫秒步骤时间与教程中的起始点23.54%和132毫秒有所不同。这可能是由于训练环境的差异，包括GPU类型和PyTorch版本。我们还注意到，虽然教程基准结果明确诊断出DataLoader是瓶颈，我们的结果则没有。我们发现数据加载瓶颈往往会伪装成“CPU Exec”或“Other”在概览标签中显示为高百分比。

## 优化 #1：多进程数据加载

让我们开始应用[多进程数据加载](https://pytorch.org/docs/stable/data.html#single-and-multi-process-data-loading)如[教程中所述](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html#improve-performance-with-the-help-of-profiler)。鉴于[Amazon EC2 p3.2xlarge](https://aws.amazon.com/ec2/instance-types/p3/)实例具有8个vCPUs，我们将[DataLoader](https://pytorch.org/docs/stable/data.html#torch.utils.data.DataLoader)工作进程数设置为8，以获得最佳性能：

```py
train_loader = torch.utils.data.DataLoader(train_set, batch_size=32, 
                               shuffle=True, num_workers=8)
```

该优化的结果如下所示：

![](../Images/f7c0643684275394383b2a2154d54f5b.png)

TensorBoard Profiler概览标签中的多进程数据加载结果（作者捕获）

单行代码的更改使GPU利用率提高了200%以上（从31.65%提升至72.81%），并且将我们的训练步骤时间减少了一半以上（从80毫秒降至37毫秒）。

这就是教程中的优化过程结束的地方。尽管我们的GPU利用率（72.81%）明显高于教程中的结果（40.46%），但我毫不怀疑，你会发现这些结果仍然相当不令人满意。

**个人评论，你可以选择跳过**：想象一下如果PyTorch在GPU训练时**默认**应用多进程数据加载将节省多少全球资金！的确，使用多进程可能会有一些不良副作用。然而，必须有某种形式的自动检测算法可以运行，以排除潜在问题的存在并相应地应用此优化。

## 优化 #2：内存锁定

如果我们分析最后一次实验的Trace视图，我们可以看到仍然有相当多的时间（37毫秒中的10毫秒）花费在将训练数据加载到GPU上。

![](../Images/6d1d7033866a30dc56d38e38515eba0d.png)

Trace视图标签中的多进程数据加载结果（由作者捕获）

为了解决这个问题，我们将应用另一种PyTorch推荐的优化来简化数据输入流程，[内存固定](https://pytorch.org/docs/stable/notes/cuda.html#use-pinned-memory-buffers)。使用固定内存可以提高主机到GPU的数据复制速度，更重要的是，可以使它们异步进行。这意味着我们可以在对当前批次进行训练步骤的同时**在GPU中**准备下一个训练批次。需要注意的是，尽管[异步执行](https://pytorch.org/docs/stable/notes/cuda.html#asynchronous-execution)通常会提高性能，但也可能**降低时间测量的准确性**。为了我们博客文章的目的，我们将继续使用PyTorch Profiler报告的测量结果。有关如何获得准确测量的说明，请参见[这里](https://pytorch.org/docs/stable/notes/cuda.html#asynchronous-execution)。有关内存固定及其副作用的更多详细信息，请参阅[PyTorch文档](https://pytorch.org/docs/stable/notes/cuda.html#use-pinned-memory-buffers)。

这种内存固定优化需要对两行代码进行更改。首先，我们将[DataLoader](https://pytorch.org/docs/stable/data.html#torch.utils.data.DataLoader)的*pin_memory*标志设置为True。

```py
train_loader = torch.utils.data.DataLoader(train_set, batch_size=32, 
                          shuffle=True, num_workers=8, pin_memory=True)
```

然后我们将主机到设备的内存传输（在*train*函数中）修改为非阻塞模式：

```py
inputs, labels = data[0].to(device=device, non_blocking=True), \
                 data[1].to(device=device, non_blocking=True)
```

内存固定优化的结果如下所示：

![](../Images/99308bf791b6cab1174661fa1a488451.png)

TensorBoard Profiler概览标签中的内存固定结果（由作者捕获）

我们的GPU利用率现在达到了92.37%，并且步骤时间进一步减少。但我们仍然可以做得更好。请注意，尽管进行了优化，性能报告仍继续表明我们花费了大量时间将数据复制到GPU上。我们将在下面的第4步中回到这个问题。

## 优化 #3：增加批量大小

对于我们的下一个优化，我们将注意力转向最后一次实验的[内存视图](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html#analyze-performance-with-other-advanced-features)：

![](../Images/2b16dc265436222bd68a440d24fd5ac9.png)

TensorBoard Profiler中的内存视图（由作者捕获）

图表显示，在 16 GB 的 GPU 内存中，我们的利用率峰值低于 1 GB。这是资源利用不足的极端例子，通常（但并非总是）表明存在提升性能的机会。控制内存利用率的一种方法是增加批量大小。在下面的图像中，我们展示了将批量大小增加到 512（内存利用率增加到 11.3 GB）时的性能结果。

![](../Images/0c4604a9367bc204d0d41ea6113f89f3.png)

在 TensorBoard Profiler 概述选项卡中增加批量大小的结果（由作者捕捉）

尽管 GPU 利用率的度量变化不大，但我们的训练速度已经显著提高，从每秒 1200 个样本（批量大小 32 时 46 毫秒）增加到每秒 1584 个样本（批量大小 512 时 324 毫秒）。

**警告**：与我们之前的优化相反，增加批量大小可能会对您的训练应用程序的行为产生影响。不同的模型对批量大小的变化表现出不同程度的敏感性。有些模型可能只需要对优化器设置进行一些调整。对于其他模型，调整到较大的批量大小可能会更困难甚至不可能。有关训练大批量数据的一些挑战，请参见[这篇之前的文章](/a-guide-to-highly-distributed-dnn-training-9e4814fb8bd3)。

## 优化 #4：减少主机到设备的拷贝

您可能注意到了之前结果中饼图上代表主机到设备数据拷贝的大红色眼睛。解决这种瓶颈的最直接方法是查看是否可以减少每批次中的数据量。注意，在我们的图像输入的情况下，我们将数据类型从 8 位无符号整数转换为 32 位浮点数，并在执行数据拷贝之前应用归一化。在下面的代码块中，我们建议对输入数据流进行更改，将数据类型转换和归一化的操作延迟到数据在 GPU 上之后：

```py
# maintain the image input as an 8-bit uint8 tensor
transform = T.Compose(
    [T.Resize(224),
     T.PILToTensor()
     ])
train_set = FakeCIFAR(transform=transform)
train_loader = torch.utils.data.DataLoader(train_set, batch_size=1024, 
                                           shuffle=True, num_workers=8,
                                           pin_memory=True)

device = torch.device("cuda:0")
model = torchvision.models.resnet18(weights='IMAGENET1K_V1').cuda(device)
criterion = torch.nn.CrossEntropyLoss().cuda(device)
optimizer = torch.optim.SGD(model.parameters(), lr=0.001, momentum=0.9)
model.train()

# train step
def train(data):
    inputs, labels = data[0].to(device=device, non_blocking=True), \
                     data[1].to(device=device, non_blocking=True)
    # convert to float32 and normalize
    inputs = (inputs.to(torch.float32) / 255\. - 0.5) / 0.5
    outputs = model(inputs)
    loss = criterion(outputs, labels)
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
```

由于此更改，从 CPU 到 GPU 复制的数据量减少了 4 倍，红色的眼睛几乎消失：

![](../Images/f892c522888084fa498da4e0e0103bbe.png)

在 TensorBoard Profiler 概述选项卡中减少 CPU 到 GPU 拷贝的结果（由作者捕捉）

我们现在达到了 97.51%(!!) 的 GPU 利用率和每秒 1670 个样本的训练速度！让我们看看还能做些什么。

## 优化 #5：将梯度设置为 None

在这一阶段，我们似乎已经完全利用了 GPU，但这并不意味着我们不能更有效地利用它。一种流行的优化方法是将模型参数梯度设置为 *None* 而不是 *zero*，以减少 GPU 中的内存操作。有关此优化的更多细节，请参阅 [PyTorch 文档](https://pytorch.org/tutorials/recipes/recipes/tuning_guide.html#use-parameter-grad-none-instead-of-model-zero-grad-or-optimizer-zero-grad)。实施此优化所需的只是将 *optimizer.zero_grad* 调用的 *set_to_none* 设置为 *True*：

```py
optimizer.zero_grad(set_to_none=True)
```

在我们的案例中，这种优化并未显著提升我们的性能。

## 优化 #6: 自动混合精度

GPU 内核视图显示了 GPU 内核活动的时间量，可以作为提高 GPU 利用率的有用资源：

![](../Images/0a4d656c21df4c0b7f6dc79187fd8989.png)

TensorBoard Profiler 中的内核视图（作者捕获）

本报告中最明显的细节之一是 GPU [Tensor Cores](https://www.nvidia.com/en-us/data-center/tensor-cores/) 的使用不足。Tensor Cores 是相对较新的 GPU 架构上提供的矩阵乘法专用处理单元，可以显著提升 AI 应用的性能。它们的未被使用可能代表了一个重要的优化机会。

由于 Tensor Cores 是专门为混合精度计算设计的，一种直接增加其利用率的方法是修改我们的模型以使用 [自动混合精度（AMP）](https://pytorch.org/tutorials/recipes/recipes/amp_recipe.html)。在 AMP 模式下，模型的部分会自动转换为较低精度的 16 位浮点数，并在 GPU TensorCores 上运行。

重要的是，完整实现 AMP 可能需要 [梯度缩放](https://pytorch.org/tutorials/recipes/recipes/amp_recipe.html#adding-gradscaler)，我们在演示中没有包括。请在适应之前务必查看有关混合精度训练的 [文档](https://pytorch.org/tutorials/recipes/recipes/amp_recipe.html)。

启用 AMP 所需对训练步骤的修改在下面的代码块中演示。

```py
def train(data):
    inputs, labels = data[0].to(device=device, non_blocking=True), \
                     data[1].to(device=device, non_blocking=True)
    inputs = (inputs.to(torch.float32) / 255\. - 0.5) / 0.5
    with torch.autocast(device_type='cuda', dtype=torch.float16):
        outputs = model(inputs)
        loss = criterion(outputs, labels)
    # Note - torch.cuda.amp.GradScaler() may be required  
    optimizer.zero_grad(set_to_none=True)
    loss.backward()
    optimizer.step()
```

Tensor Core 利用率的影响显示在下图中。尽管它继续显示出进一步改进的机会，但仅通过一行代码，利用率从 0% 跃升至 26.3%。

![](../Images/9c1640ca3893f2528ea939153468c655.png)

Tensor Core 利用率与 AMP 优化在 TensorBoard Profiler 中的内核视图（作者捕获）

除了增加 Tensor Core 利用率外，使用 AMP 还降低了 GPU 内存利用率，从而释放出更多空间以增加批量大小。下图捕获了在 AMP 优化后和批量大小设置为 **1024** 时的训练性能结果：

![](../Images/fbeaa6ad297094aa3f249dc2a64ade35.png)

TensorBoard Profiler 概览标签中的AMP优化结果（作者捕获）

尽管GPU利用率略有下降，但我们的主要吞吐量指标仍然增加了近50%，从每秒1670个样本提高到2477个样本。我们正处于一个好状态！

**警告**：降低模型部分的精度可能会对其收敛性产生重要影响。与增加批量大小的情况一样（见上文），使用混合精度的影响会因模型而异。在某些情况下，AMP将几乎无需努力即可工作。其他情况下，可能需要更多的工作来调整自动缩放器。还有些情况下，可能需要明确设置模型不同部分的精度类型（即，*手动*混合精度）。

欲了解有关使用混合精度作为内存优化方法的更多细节，请参阅我们的[上一篇博客文章](/how-to-increase-training-performance-through-memory-optimization-1000d30351c8)。

## 优化 #7：以图模式训练

我们将应用的最终优化是模型编译。与PyTorch默认的“急切执行”模式相反，[compile](https://pytorch.org/docs/stable/generated/torch.compile.html) API将模型转换为中间计算图，然后将其编译为低级计算内核，以优化底层训练加速器的方式进行处理。有关PyTorch 2中模型编译的更多信息，请查看我们的[上一篇文章](https://medium.com/towards-data-science/tips-and-tricks-for-upgrading-to-pytorch-2-3127db1d1f3d)。

以下代码块演示了应用模型编译所需的更改：

```py
model = torchvision.models.resnet18(weights='IMAGENET1K_V1').cuda(device)
model = torch.compile(model)
```

模型编译优化的结果如下所示：

![](../Images/bb76afa59cdbd5da27b174ba4da08e85.png)

TensorBoard Profiler 概览标签中的图编译结果（作者捕获）

与之前实验中的2477个样本相比，模型编译进一步将我们的吞吐量提高到每秒3268个样本，性能提升了额外的32%（!!）。

图编译改变训练步骤的方式在TensorBoard插件的不同视图中非常明显。例如，Kernel View显示了新（融合）GPU内核的使用，而Trace View（如下所示）显示出与之前完全不同的模式。

![](../Images/1c8f0a07215f762ff0d0d411563111c8.png)

TensorBoard Profiler 追踪视图标签中的图编译结果（作者捕获）

# 临时结果

在下表中，我们总结了我们应用的连续优化的结果。

![](../Images/d3db3ec31e38435735bf806d5c9f6f2d.png)

性能结果总结（作者）

通过使用PyTorch Profiler和TensorBoard插件进行**分析和优化**的迭代方法，**我们将性能提高了817%**！！

**我们的工作完成了吗？绝对没有！** 我们实施的每项优化都会揭示新的性能改进机会。这些机会以资源被释放的形式呈现（例如，转向混合精度使我们能够增加批量大小），或者以新发现的性能瓶颈的形式呈现（例如，我们最终的优化揭示了主机到设备数据传输中的瓶颈）。此外，还有许多我们在这篇文章中没有尝试的知名优化形式（例如，参见 [这里](https://pytorch.org/tutorials/recipes/recipes/tuning_guide.html) 和 [这里](https://pytorch.org/docs/stable/notes/cuda.html)）。最后，新库优化（例如，我们在第7步演示的模型编译功能）不断发布，进一步推动我们的性能提升目标。正如我们在介绍中强调的，要充分利用这些机会，性能优化必须成为你开发工作流程中的一个迭代且持续的部分。

# 总结

在这篇文章中，我们展示了对一个玩具分类模型进行性能优化的重大潜力。虽然你可以使用其他性能分析工具，每种工具都有其优缺点，但我们选择了 PyTorch Profiler 和 TensorBoard 插件，因为它们的集成方便。

我们应该强调的是，成功优化的路径会因训练项目的细节而大相径庭，包括模型架构和训练环境。在实践中，实现你的目标可能比我们在这里展示的例子要困难得多。我们描述的一些技术可能对你的性能影响不大，甚至可能使其更糟。我们还指出，我们选择的具体优化以及选择应用的顺序在某种程度上是随意的。强烈建议你根据项目的具体细节开发自己的工具和技术，以实现优化目标。

机器学习工作负载的性能优化有时被视为次要的、非关键的和令人厌烦的。我希望我们已经成功地说服你，开发时间和成本的节省潜力值得对性能分析和优化进行有意义的投资。而且，你可能会发现这其实也很有趣 :）。

## 下一步？

这仅仅是冰山一角。性能优化远比我们在这里所涵盖的内容要复杂得多。在[这篇文章的续集](https://medium.com/towards-data-science/pytorch-model-performance-analysis-and-optimization-part-2-3bc241be91)中，我们将深入探讨一个在 PyTorch 模型中相当常见的性能问题，即计算的部分在 CPU 上运行而不是 GPU 上，这种情况通常对开发者来说是未知的。我们还鼓励你查看我们在[Medium上的其他文章](https://chaimrand.medium.com/)，其中许多文章涉及机器学习工作负载性能优化的不同方面。
