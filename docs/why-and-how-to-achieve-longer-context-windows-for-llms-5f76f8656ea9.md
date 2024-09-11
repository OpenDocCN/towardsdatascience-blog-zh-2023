# 为什么以及如何实现更长的 LLM 上下文窗口

> 原文：[https://towardsdatascience.com/why-and-how-to-achieve-longer-context-windows-for-llms-5f76f8656ea9?source=collection_archive---------1-----------------------#2023-10-13](https://towardsdatascience.com/why-and-how-to-achieve-longer-context-windows-for-llms-5f76f8656ea9?source=collection_archive---------1-----------------------#2023-10-13)

[](https://medium.com/@ddxzzx?source=post_page-----5f76f8656ea9--------------------------------)[![Davide Ghilardi](../Images/1643cb58465144e914c545bbca4359b4.png)](https://medium.com/@ddxzzx?source=post_page-----5f76f8656ea9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5f76f8656ea9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5f76f8656ea9--------------------------------) [Davide Ghilardi](https://medium.com/@ddxzzx?source=post_page-----5f76f8656ea9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2907a3374fa5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-and-how-to-achieve-longer-context-windows-for-llms-5f76f8656ea9&user=Davide+Ghilardi&userId=2907a3374fa5&source=post_page-2907a3374fa5----5f76f8656ea9---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5f76f8656ea9--------------------------------) ·7 min read·2023年10月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5f76f8656ea9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-and-how-to-achieve-longer-context-windows-for-llms-5f76f8656ea9&user=Davide+Ghilardi&userId=2907a3374fa5&source=-----5f76f8656ea9---------------------clap_footer-----------)

-- 

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5f76f8656ea9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-and-how-to-achieve-longer-context-windows-for-llms-5f76f8656ea9&source=-----5f76f8656ea9---------------------bookmark_footer-----------)

语言模型（LLMs）在过去几年中彻底改变了自然语言处理（NLP）领域，在各种任务中取得了最先进的成果。然而，开发和改进这些模型的一个关键挑战在于延长它们的上下文长度。这非常重要，因为它决定了模型在生成输出时可用的信息量。

然而，增加 LLM 的上下文窗口并非易事。实际上，这会带来计算复杂度的增加，因为注意力矩阵呈二次方增长。

一种解决方案可能是用相对较小的窗口（例如4K个令牌）在大量数据上训练模型，然后在更大的窗口（例如64K个令牌）上进行微调。这一操作并不简单，因为尽管上下文长度不会影响模型的权重数量，但它确实影响这些权重在令牌稠密表示中如何编码**位置的信息**。

这减少了模型在长上下文窗口下适应的能力，即使在微调之后也会导致性能不佳，因此需要新的技术来正确和动态地编码位置的信息，在训练和微调之间。

# 绝对位置编码

在transformers中，每个令牌的位置信息在令牌输入到注意力头之前被编码。这是一个关键步骤，因为与RNN不同，transformers不会跟踪令牌的位置。

原始transformer架构 [[1](#d8de)] 将位置编码为与令牌嵌入相同形状的向量，以便它们可以**相加**在一起。特别是，他们使用了一个cos/sin波的组合，其长度从低维到高维嵌入的顺序递增。

![](../Images/ba7885dd0e0a7826e17a67e0dc250355.png)

图片由作者提供

这种方法允许对所有令牌位置进行高效的唯一稠密表示。然而，它不能解决扩展上下文长度的挑战，因为这些表示不能有效地编码令牌的相对位置。

为了了解为什么会发生这种情况，我们来关注一对连续的维度。如果我们在图表上绘制它们，我们可以将令牌和位置嵌入表示为二维向量，如下图所示：

![](../Images/c2c045db49e24a0fde1c83c690ef7775.png)

*图片由作者提供*

上图包含两个图表：左侧是嵌入空间，右侧是键/查询空间。嵌入向量是黑色的，位置向量是蓝色的，它们的和用绿色表示。

为了从嵌入转到键（K）和查询（Q），transformer应用了由两个矩阵**W**q和**W**k定义的线性变换。由于线性性质，该变换可以在嵌入和位置向量之间分别应用。在KQ空间中，注意力作为键和查询之间的点积计算，图中用黄色区域表示在两个绿色向量之间。

由于旋转的混合方式不同，基于令牌和位置嵌入的原始方向，以及它们在变换中的缩放方式，模型无法直接访问令牌之间的相对距离。

同样重要的是，在这种情况下，加法是在变换之前应用的，而不是反过来，因为位置嵌入需要通过线性层进行缩放。

# 相对位置编码

为了高效编码标记之间的相对位置信息，已经提出了其他方法。我们将重点关注**RoPE** [[2](#d8de)]，即**Ro**tary **P**osition **E**mbedding，已经为更长的上下文窗口提出了扩展方法。

RoPE 的两个主要创新是：

+   使用 RoPE，我们可以使键和值嵌入之间的点积仅对它们的**相对距离**敏感。

+   位置嵌入被**乘以**标记的嵌入，而无需固定大小的查找表。

为了实现这一目标，首先，我们将标记的嵌入从实际的 D 维空间映射到一个复数 D/2 维空间，并在该空间中应用旋转。

再次考虑一对连续的维度。如果我们将它们绘制在复数平面上（将第一个设置在实轴上，第二个设置在虚轴上），我们可以将标记嵌入表示为复数向量，如下图所示：

![](../Images/0aee7722d74ac1e4feed129c77317423.png)

*图片来源：作者*

在这里，位置编码获得了新的含义，实际上，它们变成了直接应用于向量的旋转。这一变化使它们能够唯一地编码标记的位置，并在线性变换中得到保留。

这一最后的特性对于在注意力计算中纳入相对位置至关重要。实际上，如果我们考虑未旋转（黑色）和旋转（绿色）版本的**k**ₙ 和 **q**ₙ₊ₖ 之间的注意力，如右侧图表中分别由橙色和黄色角度表示，我们可以注意到一些有趣的现象：

![](../Images/8fdcabc8b8b3a1a6dec25f60b52eb584.png)

*图片来源：作者*

旋转的键和值之间的注意力仅因其位置差异成比例地不同于未旋转版本！

这一新特性使得使用 RoPE 训练的模型显示出**快速收敛**和**更低的损失**，并且是一些流行技术的核心，这些技术可以将上下文窗口扩展到训练集之外。

到目前为止，我们始终保持嵌入维度固定，但为了看到整个框架，还需要观察沿该方向发生的情况。

![](../Images/757e569819126fd9ce4a772d5b7975dc.png)

*图片来源：作者*

RoPE 公式定义了第 *d* 维的旋转角度，与 *exp(-d)* 成正比。因此，正如我们在上图中看到的，随着 *d* 的增加，应用于嵌入向量的旋转呈指数下降（对于固定的 n 和位置）。这一特性使模型能够通过从低维到高维，转变嵌入中编码的信息类型，从低频（近标记关联）到高频（远标记关联）。

# RoPE 扩展

一旦我们有效地将相对位置信息整合到模型中，增加我们LLM的上下文窗口L的最直接方法是通过**位置插值**（**PI**） [[3](#d8de)]进行微调。

这是一种简单的技术，将标记的位置缩放以适应新的上下文长度。例如，如果我们决定将其翻倍，所有位置将被缩小一半。特别地，对于我们想要实现的任何上下文长度L* > L，我们可以定义一个缩放因子*s* = L/ L* < 1。

尽管这种技术通过在相对较少的标记上进行微调成功地扩展了LLM的上下文，并显示了有希望的结果，但它也有其自身的缺点。

其中之一是，它在对较大上下文进行微调后，略微降低了短上下文大小的性能（例如，困惑度增加）。这个问题发生是因为通过缩放*s <* 1标记的位置（以及它们的相对距离），我们减少了对向量的旋转，导致高频信息的丧失。因此，模型较难识别小旋转，从而难以确定接近标记的位置信序。

为了解决这个问题，我们可以应用一种巧妙的机制，称为**NTK-aware** [[4](#d8de)]位置插值，它不是将每个RoPE维度均等地缩放*s*，而是通过对高频率缩放较少，对低频率缩放较多来分散插值压力。

其他PI扩展方法还包括**NTK-by-parts** [[5](#d8de)]和**dynamic NTK** [[6](#d8de)]方法。前者施加两个阈值，以限制在某些维度上的缩放；后者在推理过程中动态调整*s*。

最后，由于观察到随着标记数量的增加，注意力softmax分布变得越来越“尖锐”（注意力softmax的平均熵减少），**YaRN** [[7](#d8de)]（**Y**et **a**nother **R**oPE extensio**N** method）是一种通过在应用softmax之前将注意力矩阵乘以温度因子*t*来逆转这一过程的方法。

下面是这些方法在位置（旋转次数）和维度（每次旋转的度数）视角下的操作方式。

![](../Images/bf9c6d315ad5524bc56e483f555771e7.png)

*作者提供的图像*

## 其他方法

最后，如前所述，其他上下文扩展方法也存在，这里简要描述了最流行的一些方法及其操作方式：

+   Alibi [[8](#d8de)]：这是一种位置编码的另一种方法，根据键和值的距离来惩罚查询可以分配给键的注意力值。

+   XPos [[9](#d8de)]：另一种位置编码方法，它将RoPE推广到包括一个缩放因子。

# 参考文献

1.  Vaswani等人，2017年。《Attention Is All You Need》。 [link](https://arxiv.org/abs/1706.03762)

1.  Su 等人, 2022\. RoFormer：具有旋转位置嵌入的增强型变压器。[link](https://arxiv.org/abs/2104.09864)

1.  Chen 等人, 2023\. 通过位置插值扩展大型语言模型的上下文窗口。[link](https://arxiv.org/abs/2306.15595)

1.  bloc97, 2023\. NTK-Aware Scaled RoPE 使 LLaMA 模型在无需任何微调且最小化困惑度下降的情况下，能够拥有扩展（8k+）的上下文大小。[link](https://www.reddit.com/r/LocalLLaMA/comments/14lz7j5/ntkaware_scaled_rope_allows_llama_models_to_have/)

1.  bloc97, 2023\. 添加 NTK-Aware 插值“分段”修正。[link](https://github.com/jquesnelle/yarn/pull/1)

1.  emozilla, 2023\. 动态缩放的 RoPE 进一步提升了长上下文 LLaMA 的性能，无需任何微调。[link](https://www.reddit.com/r/LocalLLaMA/comments/14mrgpr/dynamically_scaled_rope_further_increases/)

1.  Peng 等人, 2023\. YaRN：大型语言模型的高效上下文窗口扩展。[link](https://arxiv.org/abs/2309.00071)

1.  Press 等人, 2022\. 训练短，测试长：具有线性偏差的注意力机制实现了输入长度的外推。[link](https://openreview.net/forum?id=R8sQPpGCv0)

1.  Sun 等人, 2022\. 一个长度可外推的变压器。[link](https://arxiv.org/abs/2212.10554)
