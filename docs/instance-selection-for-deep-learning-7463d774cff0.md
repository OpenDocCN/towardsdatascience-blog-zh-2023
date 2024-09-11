# 深度学习的实例选择

> 原文：[https://towardsdatascience.com/instance-selection-for-deep-learning-7463d774cff0?source=collection_archive---------11-----------------------#2023-06-07](https://towardsdatascience.com/instance-selection-for-deep-learning-7463d774cff0?source=collection_archive---------11-----------------------#2023-06-07)

## 如何为您的机器学习工作负载选择最佳机器

[](https://chaimrand.medium.com/?source=post_page-----7463d774cff0--------------------------------)[![Chaim Rand](../Images/c52659c389f167ad5d6dc139940e7955.png)](https://chaimrand.medium.com/?source=post_page-----7463d774cff0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7463d774cff0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7463d774cff0--------------------------------) [Chaim Rand](https://chaimrand.medium.com/?source=post_page-----7463d774cff0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9440b37e27fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finstance-selection-for-deep-learning-7463d774cff0&user=Chaim+Rand&userId=9440b37e27fe&source=post_page-9440b37e27fe----7463d774cff0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7463d774cff0--------------------------------) · 12分钟阅读 · 2023年6月7日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7463d774cff0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finstance-selection-for-deep-learning-7463d774cff0&user=Chaim+Rand&userId=9440b37e27fe&source=-----7463d774cff0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7463d774cff0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finstance-selection-for-deep-learning-7463d774cff0&source=-----7463d774cff0---------------------bookmark_footer-----------)![](../Images/177887cd1f82961bcb53c3f57d959134.png)

图片由 [Cezary Morga](https://unsplash.com/@cezarymorga?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我们日常的人工智能开发过程中，我们不断做出关于在何处运行每个机器学习（ML）工作负载的最合适机器的决定。这些决策不是随意做出的，因为它们对开发的速度和成本有着重要影响。将一台配有一个或多个GPU的机器分配给运行一个顺序算法（例如，*连通组件*算法的标准实现）可能被视为浪费，而在CPU上训练一个大型语言模型则可能需要极其长的时间。

在大多数情况下，我们将有一系列的机器选项可供选择。在使用云服务基础设施进行 ML 开发时，我们通常可以选择各种硬件规格差异极大的机器类型。这些通常被分组为不同类型的机器家族（在 AWS 上称为[实例类型](https://aws.amazon.com/ec2/instance-types/?trk=3fc1271f-8d0f-43b5-b177-4fba4b680f8b&sc_channel=ps&ef_id=CjwKCAjwvdajBhBEEiwAeMh1U_5B_UvAVhlBq4He6fU_TBPuLafVduG8_SjT0GOhgh32WklDkroGDxoCzuAQAvD_BwE%3AG%3As&s_kwcid=AL%214422%213%21645125292191%21e%21%21g%21%21aws+ec2+instance+types%2119574556935%21145779862552)），GCP 上的[机器家族](https://cloud.google.com/compute/docs/machine-resource)，以及 Microsoft Azure 上的[虚拟机系列](https://azure.microsoft.com/en-us/pricing/details/virtual-machines/series/)，每个家族针对不同的用例。面对众多选择，很容易感到不知所措或遭受[选择过载](https://en.wikipedia.org/wiki/Overchoice)，因此存在许多在线资源来帮助导航实例选择的过程。

在这篇文章中，我们希望将注意力集中在为深度学习（DL）工作负载选择合适的实例类型上。DL 工作负载通常非常计算密集，并且经常需要专用的硬件加速器，例如 GPU。我们在这篇文章中的意图是提出一些选择 DL 机器类型的指导原则，并强调在做出决定时应考虑的机器类型之间的一些主要区别。

## 这个实例选择指南的不同之处

在我们看来，许多现有的实例指南往往错失了大量机会。它们通常涉及根据一些预定义的属性（例如，计算要求、内存要求、网络要求等）对应用程序进行分类，并根据这些属性提出选择实例类型的流程图。它们往往低估了许多机器学习应用的复杂性以及以这种方式分类并不总是能充分预测其性能挑战的简单事实。我们发现，简单地遵循这些指南，有时会导致选择次优的实例类型。正如我们将看到的，我们提出的方法更加实践和数据驱动。它涉及定义清晰的指标来衡量应用程序的性能，并提供工具来比较其在不同实例类型选项上的表现。我们相信，这种方法是确保你真正最大化机会所必需的。

## 免责声明

请不要将我们提到的任何特定实例类型、深度学习库、云服务提供商等视为对其使用的推荐。对您来说最佳的选择将取决于您项目的独特细节。此外，我们提出的任何建议都不应被视为比建议更多的内容，应在应用之前经过仔细评估并根据您的用例进行调整。

# 第一部分：实例类型选择的建议原则

与任何其他重要的开发设计决策一样，强烈建议您制定一套明确的指南，以便找到最佳解决方案。使用您之前项目中使用过的或最熟悉的机器类型是最简单的选择。然而，这样做可能会导致您错失重大节省成本和/或加快整体开发时间的机会。在本节中，我们提出了一些实例类型搜索的指导原则。

## 定义明确的指标和比较工具

我们将讨论的一个重要指导原则是需要明确界定**比较不同实例类型性能的指标**和**测量这些指标的工具**。如果没有明确的效用函数定义，您将无法知道所选择的机器是否最优。您的效用函数可能在不同项目之间不同，甚至可能在单个项目进行过程中发生变化。当预算紧张时，您可能会优先考虑降低成本而不是提高速度。当面临重要的客户截止日期时，您可能会不惜一切代价优先考虑速度。

**示例：每美元样本数指标** 在之前的帖子中（例如，[这里](/cloud-ml-performance-checklist-caa51e798002)），我们曾提出 ***每美元样本数*** — 即每花费一美元所处理的样本数 — 作为运行深度学习模型（用于训练或推理）的性能衡量标准。***每美元样本数***的公式为：

![](../Images/ac69309639f2d604afaff5296b1bdfcd.png)

每美元样本数公式（由作者提供）

…其中 *每秒样本数 = 批量大小 * 每秒批次数*。训练实例的成本通常可以在线找到。当然，仅优化这个指标可能是不够的：它可能会最小化训练的总体成本，但如果不包括考虑整体开发时间的指标，您可能会错过所有客户截止日期。另一方面，开发速度有时可以通过并行训练多个实例来控制，从而使我们能够无论选择什么实例类型都能达到我们的速度目标。无论如何，我们的简单示例展示了需要考虑多个性能指标，并根据机器学习项目的预算和调度限制等细节来权衡它们。

如果没有测量它们的方法，制定指标是无用的。关键在于你需要定义并构建工具，将你选择的指标集成到你的应用程序中。在下面的代码块中，我们展示了一个基于PyTorch的简单训练循环，其中包含一行简单的代码，用于定期打印每秒处理的平均样本数。将其除以实例类型的公布成本（每秒）就得到了我们之前提到的每美元成本指标。

```py
 import time

    batch_size = 128
    data_loader = get_data_loader(batch_size)
    global_batch_size = batch_size * world_size
    interval = 100
    t0 = time.perf_counter()

    for idx, (inputs, target) in enumerate(data_loader, 1):
        train_step(inputs, target)
        if idx % interval == 0:
            time_passed = time.perf_counter() - t0
            samples_processed = global_batch_size * interval
            print(f'{samples_processed / time_passed} samples/second')
            t0 = time.perf_counter()
```

## 拥有广泛的选项

一旦我们明确了我们的效用函数，选择最佳实例类型就简化为寻找最大化效用函数的实例类型。显然，我们可以选择的实例类型的搜索空间越大，我们能获得的整体效用结果就越高。因此，需要有**大量**的选项。但我们也应该追求实例类型的**多样性**。深度学习项目通常涉及运行多个应用工作负载，这些负载在系统需求和系统利用模式上差异很大。对于某个工作负载，最佳机器类型的规格可能与另一个工作负载的最佳规格大相径庭。拥有**大量**和**多样化**的实例类型将提高你最大化所有项目工作负载性能的能力。

## 考虑多种选项

一些实例选择指南会建议将你的深度学习应用（例如，通过模型的大小和/或是否执行训练或推理）进行分类，并相应地选择一个（单一的）计算实例。例如，AWS推荐使用某些类型的实例（例如，[Amazon EC2 g5](https://aws.amazon.com/ec2/instance-types/g5/)系列）用于机器学习推理，而其他（更强大的）实例类型（例如，[Amazon EC2 p4](https://aws.amazon.com/ec2/instance-types/p4/)系列）用于机器学习训练。然而，正如我们在介绍中提到的，我们认为盲目遵循这种指导可能会错失性能优化的机会。实际上，我们发现对于许多训练工作负载，包括那些具有大型机器学习模型的工作负载，我们的效用函数在被认为是针对推理的实例上得到最大化。

当然，我们并不期望你测试*每一个*可用的实例类型。有许多实例类型可以（而且应该）仅凭其硬件规格就被排除。我们不建议花时间评估大语言模型在CPU上的性能。如果我们知道我们的模型需要高精度的算术运算以实现成功收敛，我们也不会花时间在Google Cloud TPU上运行它（参见[这里](https://cloud.google.com/tpu/docs/intro-to-tpu#when_to_use_tpus)）。但在没有明显的硬件限制的情况下，我们认为实例类型应该仅根据**性能数据结果**来排除。

多GPU [Amazon EC2 g5](https://aws.amazon.com/ec2/instance-types/g5/) 实例通常不被考虑用于模型训练的原因之一是，与 [Amazon EC2 p4](https://aws.amazon.com/ec2/instance-types/p4/) 相比，GPU之间的通信介质是PCIe，而不是 [NVLink](https://en.wikipedia.org/wiki/NVLink)，因此支持的数据显示量更低。然而，尽管高GPU间通信速率对多GPU训练确实重要，但PCIe支持的带宽可能对你的网络来说足够，或者你可能会发现其他性能瓶颈阻止了你充分利用NVLink连接的速度。唯一确定的方式是通过实验和性能评估。

任何实例类型在实现我们的效用函数目标时都是公平的，在寻找实例类型的过程中，我们经常发现自己为那些低功耗、更环保、被低估且价格较低的黑马打气。

## 以最大化选项的方式开发你的工作负载

不同的实例类型可能对我们的实现施加不同的限制。它们可能需要不同的初始化顺序，支持不同的浮点数据类型，或依赖不同的软件安装。考虑到这些差异开发代码将减少你对特定实例类型的依赖，并提高你利用性能优化机会的能力。

一些高级API支持多种实例类型。例如，[PyTorch Lightening](https://www.pytorchlightning.ai/index.html)内置了在多种不同处理器上运行深度学习模型的支持，隐藏了每种处理器所需的实现细节。支持的处理器包括CPU、[GPU](https://lightning.ai/docs/pytorch/latest/accelerators/gpu_basic.html)、[Google Cloud TPU](https://lightning.ai/docs/pytorch/stable/accelerators/tpu.html)、[HPU (Habana Gaudi)](https://lightning.ai/docs/pytorch/latest/accelerators/hpu_basic.html)等。然而，需注意的是，运行在特定处理器类型上的某些适配可能需要对模型定义进行代码更改（不改变模型架构）。你还可能需要包含一些针对加速器类型的条件代码块。某些API优化可能只针对特定加速器实现，但不针对其他加速器（例如，针对GPU的 [scaled dot product attention (SDPA)](https://pytorch.org/tutorials/intermediate/scaled_dot_product_attention_tutorial.html) API）。一些超参数，如批处理大小，可能需要调整以达到最佳性能。我们在关于 [专用AI训练加速器](/tpu-training-6eb84100d138) 的博客系列中演示了可能需要的额外更改示例。

## （重新）持续评估

重要的是，在我们当前不断创新的深度学习运行时优化环境中，性能比较结果会迅速过时。新的实例类型会定期发布，这会扩大我们的搜索空间，并提供提高效用的潜力。另一方面，流行的实例类型可能会达到生命周期终点，或因全球需求高而难以获得。软件栈的不同层级的优化（例如，见[这里](https://openai.com/research/triton)）也可以显著推动性能提升。例如，PyTorch最近发布了一种新的图编译模式，据报道，[在现代GPU上训练速度提高了多达51%](https://pytorch.org/get-started/pytorch-2.0/#pytorch-2x-faster-more-pythonic-and-as-dynamic-as-ever)。截至本文撰写时，这些加速尚未在其他加速器上演示过。这是一个显著的加速，可能迫使我们重新评估一些以前的实例选择决策。（有关PyTorch编译模式的更多信息，请参见[我们关于这一主题的最新文章](https://medium.com/towards-data-science/tips-and-tricks-for-upgrading-to-pytorch-2-3127db1d1f3d)。）因此，性能比较**不**应是一次性的活动；为了充分利用所有这些令人惊叹的创新，它应该定期进行和更新。

# 第二部分：实例类型之间的差异

了解可用实例类型的详细信息，特别是它们之间的差异，对于决定哪些实例类型适合性能评估至关重要。在本节中，我们将这些信息分为三类：硬件规格、软件栈支持和实例可用性。

## 硬件规格

潜在实例类型之间最重要的差异在于其硬件规格的详细信息。有许多硬件细节可能对深度学习工作负载的性能产生重大影响。这些包括：

+   硬件加速器的具体细节：我们使用哪些AI加速器（例如，GPU/HPU/TPU），每个加速器支持多少内存，能够运行多少FLOP，它支持什么基础类型（例如，bfloat16/float32）等。

+   硬件加速器之间的通信介质及其支持的带宽

+   多个实例之间的通信介质及其支持的带宽（例如，实例类型是否包括高带宽网络，如[Amazon EFA](https://aws.amazon.com/hpc/efa/)或[Google FastSocket](https://cloud.google.com/blog/products/ai-machine-learning/how-to-optimize-google-cloud-for-deep-learning-training)）。

+   样本数据摄取的网络带宽

+   整体CPU计算能力（通常负责样本数据输入管道）与加速器计算能力之间的比率。

对于AWS上ML实例类型硬件规格差异的全面详细评审，请查看以下TDS文章：

[](/choosing-the-right-gpu-for-deep-learning-on-aws-d69c157d8c86?source=post_page-----7463d774cff0--------------------------------) [## 在AWS上选择适合深度学习的GPU

### 如何选择适合深度学习训练和推理的Amazon EC2 GPU实例——从最佳性能到…

towardsdatascience.com](/choosing-the-right-gpu-for-deep-learning-on-aws-d69c157d8c86?source=post_page-----7463d774cff0--------------------------------)

深入了解你使用的实例类型的细节不仅对了解哪些实例类型与你相关很重要，还对理解和克服开发过程中发现的运行时性能问题至关重要。这在我们之前的许多[博客文章](https://chaimrand.medium.com/)中得到了证明（例如，[这里](/cloud-ml-performance-checklist-caa51e798002)）。

## 软件栈支持

你的实例类型搜索的另一个输入应是你正在考虑的实例类型的软件支持矩阵。某些软件组件、库和/或API仅支持特定的实例类型。如果你的工作负载需要这些，那么你的搜索范围会更有限。例如，一些模型依赖于为GPU构建的计算内核，但不支持其他类型的加速器。另一个例子是Amazon SageMaker提供的专用库，该库可以提升多实例训练的性能，但截至本文写作时，只支持[有限数量的实例类型](https://docs.aws.amazon.com/sagemaker/latest/dg/distributed-model-parallel-support.html)（更多细节，请参见[这里](/smart-distributed-training-on-amazon-sagemaker-with-smd-part-1-cd296f87a0ee)）。还要注意，一些较新的实例类型，例如基于[AWS Trainium](https://aws.amazon.com/machine-learning/trainium/)的[Amazon EC2 trn1](https://aws.amazon.com/ec2/instance-types/trn1/)实例，对所[支持](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/index.html)的框架有一定限制。

## 实例可用性

过去几年经历了长时间的芯片短缺，这导致了HW组件的供应下降，特别是GPU等加速器。不幸的是，这与近期在大型生成AI模型开发中的里程碑带来的需求显著增加相吻合。供需不平衡造成了获取我们选择的机器类型的不确定性。如果我们曾经认为能够随意启动任何类型的机器，现在我们需要适应我们的首选可能完全不可用的情况。

实例类型的可用性是评估和选择它们的重要因素。不幸的是，衡量可用性可能非常困难，预测和规划更是难上加难。实例的可用性可能会突然变化，今天在这里，明天可能就不见了。

请注意，在我们使用多个实例的情况下，除了实例类型的可用性，我们可能还需要它们在同一数据中心的**共置**（例如，参见[这里](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html#placement-groups-cluster)）。机器学习工作负载通常依赖于实例之间的低网络延迟，它们之间的距离可能会影响性能。

另一个重要的考虑因素是低成本[spot](https://aws.amazon.com/ec2/spot/?cards.sort-by=item.additionalFields.startDateTime&cards.sort-order=asc)实例的可用性。许多云服务提供商从剩余的云服务容量中提供折扣计算引擎（例如，AWS中的[Amazon EC2 Spot Instances](https://aws.amazon.com/ec2/spot/?cards.sort-by=item.additionalFields.startDateTime&cards.sort-order=asc)，Google Cloud Platform中的[Preemptible VM Instances](https://cloud.google.com/compute/docs/instances/preemptible)，以及Microsoft Azure中的[Low-Priority VMs](https://docs.microsoft.com/en-us/azure/batch/batch-low-pri-vms)）。Spot实例的缺点在于它们可能会被突然中断且几乎没有预警。如果可用，并且你在应用程序中编程了容错，Spot实例可以显著节省成本。

# 总结

在这篇文章中，我们回顾了一些关于深度学习工作负载实例类型选择的考虑因素和建议。实例类型的选择对项目成功有关键影响，发现最优实例的过程应相应进行。这篇文章绝非详尽无遗。可能还有*甚至是关键的*考虑因素未被讨论，这些因素可能适用于你的深度学习项目，并应予以考虑。

过去几年AI发展的爆炸性增长伴随着许多新的专用AI加速器的出现。这导致了可用实例类型选项的增加，并提供了优化的机会。同时，这也使得寻找最优实例类型变得更加具有挑战性和激动人心。祝你好运 :)!!
