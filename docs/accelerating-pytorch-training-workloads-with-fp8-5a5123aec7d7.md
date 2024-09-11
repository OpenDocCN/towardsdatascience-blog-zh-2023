# 使用 FP8 加速 PyTorch 训练工作负载 — 第 1 部分

> 原文：[https://towardsdatascience.com/accelerating-pytorch-training-workloads-with-fp8-5a5123aec7d7?source=collection_archive---------4-----------------------#2023-11-15](https://towardsdatascience.com/accelerating-pytorch-training-workloads-with-fp8-5a5123aec7d7?source=collection_archive---------4-----------------------#2023-11-15)

## 如何最大化利用现代 GPU

[](https://chaimrand.medium.com/?source=post_page-----5a5123aec7d7--------------------------------)[![Chaim Rand](../Images/c52659c389f167ad5d6dc139940e7955.png)](https://chaimrand.medium.com/?source=post_page-----5a5123aec7d7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5a5123aec7d7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5a5123aec7d7--------------------------------) [Chaim Rand](https://chaimrand.medium.com/?source=post_page-----5a5123aec7d7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9440b37e27fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faccelerating-pytorch-training-workloads-with-fp8-5a5123aec7d7&user=Chaim+Rand&userId=9440b37e27fe&source=post_page-9440b37e27fe----5a5123aec7d7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5a5123aec7d7--------------------------------) ·9 分钟阅读·2023 年 11 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5a5123aec7d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faccelerating-pytorch-training-workloads-with-fp8-5a5123aec7d7&user=Chaim+Rand&userId=9440b37e27fe&source=-----5a5123aec7d7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5a5123aec7d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faccelerating-pytorch-training-workloads-with-fp8-5a5123aec7d7&source=-----5a5123aec7d7---------------------bookmark_footer-----------)![](../Images/39337db002266767825d449c2f59b599.png)

图片来源于 [Deva Darshan](https://unsplash.com/@darshan394?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

过去几年在 AI 领域取得了革命性进展，最近最好的例证是基于 LLM 的应用程序（如[ChatGPT](https://en.wikipedia.org/wiki/ChatGPT)）的普及和流行。这些突破是由用于训练 AI 模型的设备同样令人振奋的发展推动的。新颖而创新的架构、复杂的张量处理核心和专用的 HW 加速器使得 AI 模型的收敛速度越来越快，规模也越来越大。在这篇文章中，我们将专注于 AI 专用 HW 中的一项特定进展 —— **专用的 8 位浮点（FP8）张量处理核心的加入**。出现在最现代的 AI HW 架构中（例如[Nvidia Hopper](https://www.nvidia.com/en-eu/data-center/technologies/hopper-architecture/)、[Nvidia Ada Lovelace](https://www.nvidia.com/en-eu/geforce/ada-lovelace-architecture/)和[Habana Gaudi2](https://www.intel.com/content/www/us/en/developer/articles/technical/habana-gaudi2-processor-for-deep-learning.html)），FP8 张量核心使得每秒浮点运算次数（FLOPS）显著增加，同时为 AI 训练和推断工作负载的内存优化和能耗节约提供了机会。

利用 HW 级别的 FP8 能力需要我们在构建 AI 训练和推断应用程序时，使用适当的 SW 栈和开发框架提供支持。在这篇文章中，我们将描述如何修改一个 PyTorch 训练脚本，以利用[**Nvidia H100 GPU**](https://www.nvidia.com/en-eu/data-center/h100/)的内置 FP8 数据类型支持。我们将从提供使用 FP8 数据类型的动机开始。接着，我们将审查由[Transformer Engine](https://github.com/NVIDIA/TransformerEngine)库提供的 FP8 特定的 PyTorch API 支持，并展示如何将它们集成到一个简单的训练脚本中。虽然我们不会深入探讨 FP8 在 AI 训练中的理论背景，但我们将指出使用它可能涉及的潜在挑战。最后，我们将展示 FP8 数据类型的显著优化机会。

## 免责声明

请不要将我们提到的任何 SW 组件、方法或服务解读为对其使用的认可。ML 开发的最佳设计将根据您自己 AI 工作负载的具体细节而大相径庭。请还记住，我们将提及的一些 SW 包和组件的 API 和行为可能在您阅读本文时已经发生变化。强烈建议您根据最新的 HW 和 SW 来评估任何潜在的设计决策。

# 动机

随着 AI 模型越来越复杂，训练它们所需的硬件也变得越来越复杂。被称为支持“前所未有的性能和可扩展性”的 [**Nvidia H100 GPU**](https://www.nvidia.com/en-eu/data-center/h100/) 是（在撰写本文时）Nvidia 最新、最强大的 AI 加速器，专门设计用于支持下一代 AI 开发。随着当前 AI 热潮的全面展开，对这些 GPU 的需求非常巨大（例如，请见 [这里](https://www.hpcwire.com/2023/08/17/nvidia-h100-are-550000-gpus-enough-for-this-year/)）。因此，不出所料，这些 GPU 的成本非常高 — 对许多读者而言，甚至可能是**令人望而却步的**。幸运的是，AWS、GCP 和 Microsoft Azure 等云服务提供商提供了“按需付费”（按小时/按秒）使用 H100 驱动的机器的机会，从而使更广泛的 AI 开发者社区能够使用它们。

在 AWS 中，H100 GPU 作为 [最近宣布的](https://aws.amazon.com/blogs/aws/new-amazon-ec2-p5-instances-powered-by-nvidia-h100-tensor-core-gpus-for-accelerating-generative-ai-and-hpc-applications/) [AWS EC2 p5 实例系列](https://aws.amazon.com/ec2/instance-types/p5/)的一部分提供。这些实例被宣称能够**“相比于前一代基于 GPU 的 EC2 实例，将解决问题的时间缩短最多 4 倍，并将训练 ML 模型的成本降低最多 40%”**。

在 [最近的一篇文章](/instance-selection-for-deep-learning-7463d774cff0) 中，我们讨论了选择 ML 训练实例时应考虑的一些因素。我们强调了最优化的实例类型将非常依赖于具体项目。特别是在 ML 训练实例方面 — **更大并*不*总是更好**。这一点在 p5 实例系列中尤为明显。确实，p5 实例可能会超越其他任何实例类型 — 毕竟，H100 是无可争议的性能怪兽。但一旦你考虑到 p5 的成本（在撰写本文时，8-GPU p5.48xlarge 实例的价格为每小时 $98.32），你可能会发现其他实例类型更为合适。

在下一节中，我们将会在 p5.48xlarge 上训练一个相对较大的计算机视觉模型，并将其性能与包含 8 个 [Nvidia A100 GPU](https://www.nvidia.com/en-eu/data-center/a100/) 的 [p4d.24xlarge](https://aws.amazon.com/ec2/instance-types/p4/) 进行比较。

## 玩具模型

在下面的代码块中，我们定义了一个以 [Vision Transformer](https://en.wikipedia.org/wiki/Vision_transformer) (ViT) 为基础的分类模型（使用流行的 [timm](https://pypi.org/project/timm/) Python 包版本 0.9.10）以及一个随机生成的数据集。ViT 骨干网有多种形态和尺寸。这里我们选择了通常称为 ViT-Huge 配置的模型 — 具有 **632** 百万参数 — 以更好地利用 H100 对大模型的容量。

```py
import torch, time
import torch.optim
import torch.utils.data
import torch.distributed as dist
from torch.nn.parallel.distributed import DistributedDataParallel as DDP
import torch.multiprocessing as mp

# modify batch size according to GPU memory
batch_size = 64

from timm.models.vision_transformer import VisionTransformer

from torch.utils.data import Dataset

# use random data
class FakeDataset(Dataset):
    def __len__(self):
        return 1000000

    def __getitem__(self, index):
        rand_image = torch.randn([3, 224, 224], dtype=torch.float32)
        label = torch.tensor(data=[index % 1000], dtype=torch.int64)
        return rand_image, label

def mp_fn(local_rank, *args):
    # configure process
    dist.init_process_group("nccl",
                            rank=local_rank,
                            world_size=torch.cuda.device_count())
    torch.cuda.set_device(local_rank)
    device = torch.cuda.current_device()

    # create dataset and dataloader
    train_set = FakeDataset()
    train_loader = torch.utils.data.DataLoader(
        train_set, batch_size=batch_size,
        num_workers=12, pin_memory=True)

    # define ViT-Huge model
    model = VisionTransformer(
            embed_dim=1280,
            depth=32,
            num_heads=16,
        ).cuda(device)
    model = DDP(model, device_ids=[local_rank])

    # define loss and optimizer
    criterion = torch.nn.CrossEntropyLoss()
    optimizer = torch.optim.SGD(model.parameters(), lr=0.001, momentum=0.9)

    model.train()

    t0 = time.perf_counter()
    summ = 0
    count = 0

    for step, data in enumerate(train_loader):
        # copy data to GPU
        inputs = data[0].to(device=device, non_blocking=True)
        label = data[1].squeeze(-1).to(device=device, non_blocking=True)

        # use mixed precision to take advantage of bfloat16 support
        with torch.autocast(device_type='cuda', dtype=torch.bfloat16):
            outputs = model(inputs)
            loss = criterion(outputs, label)
        optimizer.zero_grad(set_to_none=True)
        loss.backward()
        optimizer.step()

        # capture step time
        batch_time = time.perf_counter() - t0
        if step > 10:  # skip first steps
            summ += batch_time
            count += 1
        t0 = time.perf_counter()
        if step > 50:
            break
    print(f'average step time: {summ/count}')

if __name__ == '__main__':
    mp.spawn(mp_fn,
             args=(),
             nprocs=torch.cuda.device_count(),
             join=True)
```

我们在[p5.48xlarge](https://aws.amazon.com/ec2/instance-types/p5/)和[p4d.24xlarge](https://aws.amazon.com/ec2/instance-types/p4/)实例类型上训练了这个模型，使用的是专用的 PyTorch 2.1 [AWS 深度学习容器](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)（763104351884.dkr.ecr.us-east-1.amazonaws.com/pytorch-training:2.1.0-gpu-py310-cu121-ubuntu20.04-ec2）。

不出所料，p5 的每步时间性能远远超过 p4d 的性能 —— 每步 0.199 秒对比 0.41 秒 —— 快了两倍多！！这意味着训练大型 ML 模型的时间将减少一半。然而，当你考虑到成本差异（p4d 每小时 32.77 美元 vs p5 每小时 98.32 美元 —— 截至本篇文章撰写时），一个完全不同的故事展现在眼前。**p5 的性价比比 p4d 差 ~30%**！！这远未达到在[p5 宣布](https://aws.amazon.com/blogs/aws/new-amazon-ec2-p5-instances-powered-by-nvidia-h100-tensor-core-gpus-for-accelerating-generative-ai-and-hpc-applications/)中提到的 40% 改进。

此时你可能得出两种可能的结论。第一种可能是，尽管有很多宣传，p5 可能根本不是适合你的机器。第二种可能是 p5 可能仍然具有潜力，但需要对模型进行调整，以充分利用其潜力。在接下来的章节中，我们将采用第二种方法，演示如何使用 FP8 数据类型 —— p5 实例类型特有 —— 完全改变比较的性价比结果。

# 将 FP8 与 Transformer Engine 集成

我们应该强调的是，截至撰写本文时，PyTorch（版本 2.1）不包括本地的 8 位浮点数据类型。为了让我们的脚本使用 FP8，我们将使用[Transformer Engine](https://github.com/NVIDIA/TransformerEngine)（TE），这是一个加速 NVIDIA GPU 上 Transformer 模型的专用库。TE（版本 0.12）已预装在 AWS PyTorch 2.1 DL 容器中。

尽管使用 FP8 进行训练的理论超出了本文的范围（例如，见[这里](https://arxiv.org/pdf/2209.05433.pdf)），但重要的是要知道**使用 FP8 的机制比**[**16 位替代方案**](https://pytorch.org/docs/stable/amp.html)（float16 和 bfloat16）**复杂得多**。幸运的是，TE 实现隐藏了所有繁琐的细节。有关如何使用 TE API，请参阅官方[文档](https://docs.nvidia.com/deeplearning/transformer-engine/user-guide/examples/fp8_primer.html)以及这个简单的[示例](https://github.com/NVIDIA/TransformerEngine/blob/main/examples/pytorch/mnist/main.py)。要了解更多幕后情况，请务必查看以下两个视频教程。

[](https://www.nvidia.com/en-us/on-demand/session/gtcspring23-s51393/?source=post_page-----5a5123aec7d7--------------------------------) [## 使用Transformer Engine进行FP8训练 | NVIDIA On-Demand

### 本次会话将包括FP8和混合精度介绍，Transformer Engine特性概述以及...

www.nvidia.com](https://www.nvidia.com/en-us/on-demand/session/gtcspring23-s51393/?source=post_page-----5a5123aec7d7--------------------------------) [](https://www.nvidia.com/en-us/on-demand/session/gtcspring23-s52166/?source=post_page-----5a5123aec7d7--------------------------------) [## 深度学习的FP8 | NVIDIA On-Demand

### FP8是加速深度学习（DL）训练的自然进展，超越了现代常见的16位格式…

www.nvidia.com](https://www.nvidia.com/en-us/on-demand/session/gtcspring23-s52166/?source=post_page-----5a5123aec7d7--------------------------------)

要修改我们的模型以使用TE，我们将TE的专用Transformer层包装在一个符合timm的[block layer signature](https://github.com/huggingface/pytorch-image-models/blob/v0.9.10/timm/models/vision_transformer.py#L114)的自定义Transformer块类中。

```py
import transformer_engine.pytorch as te
from transformer_engine.common import recipe

class TE_Block(te.transformer.TransformerLayer):
    def __init__(
            self,
            dim,
            num_heads,
            mlp_ratio=4.,
            qkv_bias=False,
            qk_norm=False,
            proj_drop=0.,
            attn_drop=0.,
            init_values=None,
            drop_path=0.,
            act_layer=None,
            norm_layer=None,
            mlp_layer=None
    ):
        super().__init__(
            hidden_size=dim,
            ffn_hidden_size=int(dim * mlp_ratio),
            num_attention_heads=num_heads,
            hidden_dropout=proj_drop,
            attention_dropout=attn_drop
            )
```

接下来，我们修改VisionTransformer初始化以使用我们的自定义块层：

```py
 model = VisionTransformer(
      embed_dim=1280,
      depth=32,
      num_heads=16,
      block_fn=TE_Block
      ).cuda(device)
```

到目前为止，我们没有对H100做任何特定的更改 —— 相同的代码可以在我们的A100-powered p4d实例上运行。最后一个修改是将模型前向传递包装在[te.fp8_autocast](https://github.com/NVIDIA/TransformerEngine/blob/release_v0.12/transformer_engine/pytorch/fp8.py#L453)上下文管理器中。此更改需要支持FP8的GPU：

```py
with torch.autocast(device_type='cuda', dtype=torch.bfloat16):
    with te.fp8_autocast(enabled=True):
        outputs = model(inputs)
    loss = criterion(outputs, label)
```

## 关于使用FP8的一些注意事项

使用8位浮点表示（而不是16位或32位表示）意味着较低的精度和较小的动态范围。这些因素可能会对模型的收敛速度和/或速度产生显著影响。尽管底层TE FP8实现旨在解决这一挑战，但不能保证适用于您的模型。您可能需要调整底层FP8机制（例如使用TE [recipe](https://github.com/NVIDIA/TransformerEngine/blob/release_v0.12/transformer_engine/common/recipe.py) API）、调整一些超参数，和/或限制FP8在模型子部分的应用。您可能会发现，尽管尽了最大努力，您的模型仍然无法与FP8兼容。

# 结果

在下表中，我们总结了在p4d.24xlarge和p5.48xlarge EC2实例类型上的实验结果，包括使用和不使用TE库的情况。对于p5.48xlarge实验，我们增加了批量大小以增加80 GB GPU内存的利用率。使用FP8可以减少GPU内存消耗，从而进一步增加批量大小。

![](../Images/51d33ff0bf6fc87a8a5cd2712726b5e2.png)

实验结果（作者提供）

我们可以看到，使用 TE transformer 块提高了 p4d（约 19%）和 p5（约 32%）实例类型的价格性能。使用 FP8 使 p5 的性能额外提升了约 20%。在 TE 和 FP8 优化后，**基于 H100 的 p5.48large 的价格性能优于基于 A100 的 p4d.24large**——尽管差距不算很大（约 2%）。考虑到训练速度提高了 3 倍，我们可以安全地得出结论，p5 将是训练我们优化模型的更佳实例类型。

请注意，价格性能的相对小幅提升（远低于 p5 公告中提到的 40%）使我们期待更多针对 H100 的特定优化……不过这些还需要等到另一篇文章中讨论 :).

# 总结

在这篇文章中，我们展示了如何编写 PyTorch 训练脚本以使用 8 位浮点类型。我们进一步展示了使用 FP8 如何成为发挥现代 GPU（如 Nvidia H100）最佳性能的关键因素。重要的是，FP8 的可行性以及对训练性能的影响可能会因模型细节的不同而大相径庭。

本文继续了关于优化机器学习工作负载的长期系列出版物。务必查看我们关于这一重要主题的[其他文章](https://chaimrand.medium.com/)。

**更新**（2024年5月21日）：请查看我们的[续篇](https://chaimrand.medium.com/pytorch-native-fp8-fedc06f1c9f7)，其中涵盖了新发布的 PyTorch 原生 FP8 数据类型。
