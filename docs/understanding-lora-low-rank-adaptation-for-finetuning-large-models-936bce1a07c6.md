# 了解 LoRA — 低秩适配用于微调大型模型

> 原文：[https://towardsdatascience.com/understanding-lora-low-rank-adaptation-for-finetuning-large-models-936bce1a07c6?source=collection_archive---------1-----------------------#2023-12-22](https://towardsdatascience.com/understanding-lora-low-rank-adaptation-for-finetuning-large-models-936bce1a07c6?source=collection_archive---------1-----------------------#2023-12-22)

## 这一参数高效微调方法背后的数学原理

[](https://bhavinjawade.medium.com/?source=post_page-----936bce1a07c6--------------------------------)[![Bhavin Jawade](../Images/d10f204b2d0a8a3fd6887048ecaa307d.png)](https://bhavinjawade.medium.com/?source=post_page-----936bce1a07c6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----936bce1a07c6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----936bce1a07c6--------------------------------) [Bhavin Jawade](https://bhavinjawade.medium.com/?source=post_page-----936bce1a07c6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F11a205eeb0d3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-lora-low-rank-adaptation-for-finetuning-large-models-936bce1a07c6&user=Bhavin+Jawade&userId=11a205eeb0d3&source=post_page-11a205eeb0d3----936bce1a07c6---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----936bce1a07c6--------------------------------) · 4 分钟阅读 · 2023年12月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F936bce1a07c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-lora-low-rank-adaptation-for-finetuning-large-models-936bce1a07c6&user=Bhavin+Jawade&userId=11a205eeb0d3&source=-----936bce1a07c6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F936bce1a07c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-lora-low-rank-adaptation-for-finetuning-large-models-936bce1a07c6&source=-----936bce1a07c6---------------------bookmark_footer-----------)![](../Images/5703f134a85ea9f30a74e384e46bba18.png)

来源 — 图片由 DALLE-3 生成。提示：一个较小的机器人与一个较大的机器人握手。较小的机器人是紫色的，充满电，活跃而有活力，而较大的机器人则被冻结在冰中，呈灰色。

微调大型预训练模型在计算上具有挑战性，通常涉及调整数百万个参数。虽然这种传统的微调方法有效，但需要大量的计算资源和时间，这为将这些模型适应特定任务带来了瓶颈。LoRA通过在微调过程中分解更新矩阵，提出了一个有效的解决方案。为了研究LoRA，我们首先回顾一下传统的微调。

# （ Δ W ）的分解

在传统的微调中，我们会修改预训练神经网络的权重以适应新任务。这种调整涉及改变网络的原始权重矩阵（ W ）。微调过程中对（ W ）所做的更改统称为（ Δ W ），以便更新后的权重可以表示为（ W + Δ W ）。

现在，LoRA方法并不是直接修改（ W ），而是寻求分解（ Δ W ）。这种分解是减少与微调大模型相关的计算开销的关键步骤。

![](../Images/f82bfcfa1f214d4914baa83392d0f3e5.png)

传统的微调可以根据上述方法重新构想。在这里，W 是固定的，而 ΔW 是可训练的（图像来源于博客作者）

# 内在秩假设

内在秩假设表明，通过较低维度的表示可以捕捉到神经网络的显著变化。基本上，它认为（ Δ W ）的所有元素并不是同样重要的；相反，这些变化的一个较小子集可以有效地封装所需的调整。

# 引入矩阵（ A ）和（ B ）

在此假设基础上，LoRA建议将（ Δ W ）表示为两个较小矩阵（ A ）和（ B ）的乘积，且秩较低。更新后的权重矩阵（ W’ ）因此变为：

[ W’ = W + BA ]

在这个方程中，（ W ）保持不变（即，在训练过程中不更新）。矩阵（ B ）和（ A ）的维度较低，其乘积（ BA ）表示（ Δ W ）的低秩近似。

![](../Images/a57eb454cfa654b203ca8adb8fe7c233.png)

ΔW 被分解成两个矩阵 A 和 B，其中两个矩阵的维度都低于 d x d。（图像来源于博客作者）

# 低秩对可训练参数的影响

通过选择具有较低秩（ r ）的矩阵（ A ）和（ B ），可训练参数的数量显著减少。例如，如果（ W ）是一个（ d x d ）矩阵，传统上，更新（ W ）将涉及（ d² ）个参数。然而，使用大小为（ d x r ）和（ r x d ）的（ B ）和（ A ），总参数数量减少到（ 2dr ），当（ r << d ）时，这个数量要小得多。

通过低秩自适应（LoRA）方法实现的可训练参数数量的减少提供了几个重要的好处，特别是在微调大规模神经网络时：

1.  减少内存占用：LoRA通过降低需要更新的参数数量来减少内存需求，有助于管理大规模模型。

1.  更快的训练和适应：通过简化计算需求，LoRA加快了大型模型对新任务的训练和微调。

1.  对较小硬件的可行性：LoRA的较低参数数量使得可以在较不强大的硬件上，如普通GPU或CPU，进行大型模型的微调。

1.  扩展到更大的模型：LoRA使得AI模型的扩展无需相应增加计算资源，使得管理不断增长的模型规模变得更为实际。

在LoRA的背景下，秩的概念在确定适应过程的效率和有效性方面起着关键作用。值得注意的是，论文指出矩阵*A*和*B*的秩可以非常低，有时低至1。

尽管LoRA论文主要展示了在自然语言处理（NLP）领域的实验，但低秩适应的基本方法具有广泛的适用性，可能在训练不同领域的各种神经网络时有效。

# 结论

LoRA通过将（Δ W）分解为低秩矩阵的乘积，有效地平衡了将大型预训练模型适应新任务与保持计算效率的需求。内在的秩概念对这一平衡至关重要，确保了模型学习能力的本质在参数大大减少的情况下得以保留。

参考文献：

[[1] Hu, Edward J., et al. “Lora: Low-rank adaptation of large language models.” *arXiv预印本arXiv:2106.09685* (2021).](https://arxiv.org/abs/2106.09685)
