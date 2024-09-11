# 解密 GQA — 高效 LLM 预训练的分组查询注意力

> 原文：[https://towardsdatascience.com/demystifying-gqa-grouped-query-attention-3fb97b678e4a?source=collection_archive---------0-----------------------#2023-12-27](https://towardsdatascience.com/demystifying-gqa-grouped-query-attention-3fb97b678e4a?source=collection_archive---------0-----------------------#2023-12-27)

## 驱动像 LLaMA-2、Mistral7B 等大语言模型的多头注意力变体

[](https://bhavinjawade.medium.com/?source=post_page-----3fb97b678e4a--------------------------------)[![Bhavin Jawade](../Images/d10f204b2d0a8a3fd6887048ecaa307d.png)](https://bhavinjawade.medium.com/?source=post_page-----3fb97b678e4a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3fb97b678e4a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3fb97b678e4a--------------------------------) [Bhavin Jawade](https://bhavinjawade.medium.com/?source=post_page-----3fb97b678e4a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F11a205eeb0d3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemystifying-gqa-grouped-query-attention-3fb97b678e4a&user=Bhavin+Jawade&userId=11a205eeb0d3&source=post_page-11a205eeb0d3----3fb97b678e4a---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3fb97b678e4a--------------------------------) ·6分钟阅读·2023年12月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3fb97b678e4a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemystifying-gqa-grouped-query-attention-3fb97b678e4a&user=Bhavin+Jawade&userId=11a205eeb0d3&source=-----3fb97b678e4a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3fb97b678e4a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemystifying-gqa-grouped-query-attention-3fb97b678e4a&source=-----3fb97b678e4a---------------------bookmark_footer-----------)![](../Images/b93c237b4616df1eddea573e0336115d.png)

一组“驼鹿”（来源 — 作者使用 Dalle-3 创建的图像）

在前一篇关于训练大规模模型的文章中，我们探讨了[LoRA](/understanding-lora-low-rank-adaptation-for-finetuning-large-models-936bce1a07c6)。在这篇文章中，我们将研究另一种被不同大语言模型采用的高效训练策略——分组查询注意力（GQA）。简而言之，分组查询注意力（GQA）是多头注意力（MHA）和多查询注意力（MQA）的推广——它们都是GQA的特例。因此，在深入探讨分组查询注意力之前，我们先回顾一下Vaswani等人提出的经典“Attention is All You Need”论文中的传统多头注意力。接下来，我们将探索多查询注意力及其如何解决MHA面临的挑战。最后，我们将回答“什么是GQA？”和“它如何使我们兼得两全？”的问题。

多头注意力是变换器模型的关键组件，使其能够高效地处理和理解复杂序列任务，如语言翻译、摘要生成等。要掌握其复杂性，我们必须深入研究其数学基础，并理解注意力机制中多个头的功能。

基本的注意力机制计算值的加权和，加权依赖于查询和一组键。在数学上，这表示为：

![](../Images/21426445ffa7878019ea64e4e511a990.png)

这被称为**缩放点积注意力**。在这个方程中，Q（查询）和K（键）是表示查询和键的矩阵。V（值）是值的矩阵。“d_k”是键的维度，用于缩放。

## 扩展到多头注意力（MHA）

多头注意力使用多个‘头’的注意力层，使模型能够关注来自不同表示子空间的信息。在每个头中，有一组独立的线性层（投影矩阵）用于查询、键和值（这是一个重要的点，我们将在GQA中重述）。对于每个头（编号为h）：

> headʰ = Attention(*Q.Wq*ʰ,*K.Wk*ʰ,*V.Wv*ʰ)

## 连接头输出

各个头的输出被连接起来，然后进行线性变换。

> MultiHead(*Q*,*K*,*V*) = Concat(head¹,head²,…,headʰ) .*W*ᵒ

Wᵒ是另一个权重矩阵，用于将连接向量线性变换为最终输出维度。

多头注意力的直观理解是，通过并行地多次应用注意力机制，模型可以捕捉数据中不同类型的关系。

![](../Images/8bcc53ddc81a6a101601a80e2d0e7798.png)

插图描绘了缩放点积注意力、多头注意力在变换器编码器块中的应用。（来源——图示部分来自《Attention is All You Need》论文 [https://arxiv.org/abs/1706.03762](https://arxiv.org/abs/1706.03762)，作者编排）

然而，MHA 使得对输入的不同部分之间关系的理解更加细致。尽管如此，这种复杂性也带来了代价 — 对内存带宽的巨大需求，尤其是在解码器推理期间。

## 多头注意力中的内存带宽挑战

问题的关键在于内存开销。像 Transformers 这样的自回归模型中的每个解码步骤都需要加载解码器权重以及所有注意力键和值。这一过程不仅计算密集，而且内存带宽密集。随着模型大小的增长，这种开销也会增加，使得扩展变得越来越困难。

# 多查询注意力（MQA）的出现

多查询注意力（MQA）作为缓解这一瓶颈的解决方案出现。这个想法简单却有效：使用多个查询头，但只有一个键和值头。这种方法显著减少了内存负担，提高了推理速度。它已经在多个大规模模型中得到应用，如 PaLM、StarCoder 和 Falcon。

在多查询注意力中，我们对键和值的头进行平均，以便所有查询头共享相同的键和值头。这是通过将平均池化的“头”复制 H 次来实现的，其中 H 是查询头的数量。

![](../Images/682067ebcba8140c3452c1127219ae42.png)

左侧 — 多头注意力，中间 — 多查询注意力，右侧 — 将现有的 MHA 检查点转换为 MQA（来源 — [https://arxiv.org/pdf/2305.13245.pdf](https://arxiv.org/pdf/2305.13245.pdf)）

这里一个有趣的问题是 — 如何将现有的预训练多头注意力模型转换为多查询注意力模型（MQA）？从现有的多头模型创建一个多查询注意力模型涉及两个步骤：模型结构的转换和随后的预训练。[1]

检查点转换：这一步将多头模型的结构转换为多查询模型。通过将原始模型中多个头的键和值的投影矩阵（线性层）合并（均值池化）为单个键和值的投影矩阵来实现。这种均值池化的方法被发现比选择现有的一个键和值头或从头初始化新的键和值头更有效。结果结构具有整合的键和值投影，具有多查询模型的特征。

预训练转换后的模型：在结构转换后，模型经历额外的训练。这次训练没有原始模型训练那么广泛；它是原始模型训练步骤的一部分（记作 α）。这个预训练阶段的目的是让模型根据其新的简化注意力机制调整和优化性能。训练遵循与原始模型相同的步骤，以确保学习动态的一致性。

然而，MQA 也并非没有缺点。减少的复杂性可能导致质量下降和训练不稳定。

# 分组查询注意力

分组查询注意力（GQA）是一种简单的方法，它结合了多头注意力（MHA）和多查询注意力（MQA）的元素，以创建一个更高效的注意力机制。GQA 的数学框架可以理解如下：

分组：在 GQA 中，传统多头模型中的查询头（Q）被分成 G 组。每组分配一个单独的键（K）和值（V）头。这种配置被称为 GQA-G，其中 G 代表组的数量。

GQA 的特殊情况：

+   GQA-1 = MQA：当只有一个组（G = 1）时，GQA 等同于 MQA，因为所有查询头只有一个键和值头。

+   GQA-H = MHA：当组的数量等于头的数量（G = H）时，GQA 的行为类似于传统的 MHA，每个查询头都有其独特的键和值头。

![](../Images/b26048bc6686cff8d049e63552eb7825.png)

MHA、GQA 和 MQA 的区别（来源 — [https://arxiv.org/pdf/2305.13245.pdf](https://arxiv.org/pdf/2305.13245.pdf)）

我们对每组内原始头的键和值投影矩阵进行均值池化，将多头模型转换为 GQA 模型。这种技术对每组中每个头的投影矩阵进行平均，从而为该组生成一个单独的键和值投影。

通过利用 GQA，模型在 MHA 的质量和 MQA 的速度之间保持平衡。由于键值对较少，内存带宽和数据加载需求被最小化。G 的选择呈现一种折中：更多的组（接近 MHA）会导致更高的质量但较慢的性能，而较少的组（接近 MQA）则提高了速度，但可能牺牲质量。此外，随着模型规模的增长，GQA 允许内存带宽和模型容量按比例减少，与模型的规模相匹配。相比之下，对于更大的模型，在 MQA 中将其减少到一个单独的键和值头可能会过于严苛。

# 结论

在这篇文章中，我们首先介绍了传统的多头注意力（MHA）及其变体多查询注意力。然后我们探讨了一个更通用的公式 GQA，它被许多 LLM 模型用于有效的预训练。GQA 将多头注意力（MHA）与多查询注意力（MQA）结合起来，在质量和速度之间提供了一个公平的折中。GQA 通过将查询头分组来最小化内存带宽需求，使其适合于模型的扩展。GQA 已被用于取代典型的多头注意力，在最近的模型中如 LLaMA-2 和 Mistral7B。

**参考文献：** [1] GQA：从多头检查点训练通用化的多查询 Transformer 模型 — [https://arxiv.org/pdf/2305.13245.pdf](https://arxiv.org/pdf/2305.13245.pdf)

[2] MQA：快速 Transformer 解码：一个写头就足够了 — [https://arxiv.org/abs/1911.02150](https://arxiv.org/abs/1911.02150)

[3] MHA: 注意力即一切: [https://arxiv.org/abs/1706.03762](https://arxiv.org/abs/1706.03762)
