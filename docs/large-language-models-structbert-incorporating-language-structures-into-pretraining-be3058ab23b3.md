# 大型语言模型，StructBERT — 将语言结构融入预训练

> 原文：[https://towardsdatascience.com/large-language-models-structbert-incorporating-language-structures-into-pretraining-be3058ab23b3?source=collection_archive---------3-----------------------#2023-11-22](https://towardsdatascience.com/large-language-models-structbert-incorporating-language-structures-into-pretraining-be3058ab23b3?source=collection_archive---------3-----------------------#2023-11-22)

## 通过融入更好的学习目标来使模型更智能

[](https://medium.com/@slavahead?source=post_page-----be3058ab23b3--------------------------------)[![Vyacheslav Efimov](../Images/db4b02e75d257063e8e9d3f1f75d9d6d.png)](https://medium.com/@slavahead?source=post_page-----be3058ab23b3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----be3058ab23b3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----be3058ab23b3--------------------------------) [Vyacheslav Efimov](https://medium.com/@slavahead?source=post_page-----be3058ab23b3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc8a0ca9d85d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flarge-language-models-structbert-incorporating-language-structures-into-pretraining-be3058ab23b3&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=post_page-c8a0ca9d85d8----be3058ab23b3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----be3058ab23b3--------------------------------) ·5分钟阅读·2023年11月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbe3058ab23b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flarge-language-models-structbert-incorporating-language-structures-into-pretraining-be3058ab23b3&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=-----be3058ab23b3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbe3058ab23b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flarge-language-models-structbert-incorporating-language-structures-into-pretraining-be3058ab23b3&source=-----be3058ab23b3---------------------bookmark_footer-----------)![](../Images/9950addc5beece240f634d42ca38618b.png)

# 介绍

自BERT首次出现以来，它在各种NLP任务中显示了惊人的效果，包括情感分析、文本相似度、问答等。从那时起，研究人员就尝试通过修改架构、增加训练数据、扩充词汇量或改变层的隐藏大小等方式，使BERT变得更加高效。

[](/bert-3d1bf880386a?source=post_page-----be3058ab23b3--------------------------------) [## 大型语言模型：BERT — 双向编码器表示转换器

### 了解 BERT 如何构建最先进的嵌入

[towardsdatascience.com](/bert-3d1bf880386a?source=post_page-----be3058ab23b3--------------------------------)

尽管创建了其他强大的基于 BERT 的模型如 RoBERTa，研究人员发现了另一种提升 BERT 性能的有效方法，这将在本文中讨论。这导致了新模型 **StructBERT** 的发展，该模型在顶级基准上自信地超越了 BERT。

> StructBERT 的想法相对简单，重点在于略微修改 BERT 的预训练目标。

在本文中，我们将深入探讨 StructBERT 论文的主要细节，并理解最初修改的目标。

# 预训练

在很大程度上，StructBERT 具有与 BERT 相同的架构原则。然而，StructBERT 提出了两个新的预训练目标，以扩展 BERT 的语言知识。该模型在此目标下进行训练，配合掩码语言建模。让我们看看下面的这两个目标。

# 1\. 单词句子目标

> 实验表明，掩码语言建模（MSM）任务在 BERT 设置中发挥了关键作用，帮助其获得广泛的语言知识。预训练后，BERT 可以以高准确率正确猜测掩码词。然而，它无法正确重建单词被打乱的句子。为实现这一目标，StructBERT 开发者通过部分打乱输入标记来修改 MSM 目标。

与原始 BERT 一样，输入序列会被标记化、掩码处理，然后映射到标记、位置和段嵌入。这些嵌入会被求和以产生组合嵌入，然后输入到 BERT。

在掩码处理过程中，与 BERT 一样，15% 的随机选择标记会被掩盖并用于语言建模。但在掩码处理后，StructBERT 随机选择 5% 的 K 个连续未掩盖标记，并在每个子序列内对其进行打乱。默认情况下，StructBERT 对三元组（K = 3）进行操作。

![](../Images/ae1dfe76bcdacacb333330b5f75ccc0c.png)

三元组打乱示例

当计算最后一层隐藏层时，掩码和打乱标记的输出嵌入被用于预测原始标记，同时考虑其初始位置。

> 最终，单词句子目标与 MLM 目标以相等的权重结合。

# 2\. 句子结构目标

> 下一句预测，作为 BERT 的另一个预训练任务，被认为相对简单。掌握它并不会显著提升 BERT 在大多数下游任务上的表现。因此，StructBERT 研究人员通过让 BERT 预测句子顺序来增加这一目标的难度。

通过在文档中取一对连续句子 S₁ 和 S₂，StructBERT 使用它们以三种可能的方式之一构建训练样本。每种方式发生的概率均为 1 / 3：

+   S₂ 后跟 S₁ *(标签 1);*

+   S₁ 后跟 S₂ *(标签 2);*

+   从随机文档中抽取另一个句子 S₃，并且 S₁ *(标签 0)* 跟在其后。

这三种过程中的每一种都会生成一个有序的句子对，然后将它们连接起来。在第一个句子开始前添加了 [CLS] 标记，并使用 [SEP] 标记标记每个句子的结束。BERT 将该序列作为输入，并输出最后隐藏层的嵌入集。

[CLS] 嵌入的输出，最初在 BERT 中用于下一句预测任务，现在在 StructBERT 中用于正确识别与输入序列构建方式相对应的三种可能标签之一。

![](../Images/53c9a4372539ab5637e1fef508e2bc7d.png)

训练样本的组成

# 最终目标

最终目标由词和句子结构目标的线性组合组成。

![](../Images/4d5d17bd852192d75c360c85a07f28c6.png)

BERT 预训练包括词和句子结构目标

# StructBERT 设置

BERT 和 StructBERT 的所有主要预训练细节相同：

+   StructBERT 使用与 BERT 相同的预训练语料库：英语维基百科（2500M 单词）和 BookCorpus（800M 单词）。分词由 WordPiece 分词器完成。

+   优化器：Adam（学习率 *l* = 1e-4，权重衰减 L₂ = 0.01，β₁ = 0.9，β₂ = 0.999）。

+   学习率热身在总步骤的前 10% 中进行，然后线性减少。

+   在所有层上使用 Dropout（α = 0.1）。

+   激活函数：GELU。

+   预训练过程运行 40 个周期。

# StructBERT 版本

与原始 BERT 类似，StructBERT 提供了基础版和大型版。所有主要设置，如层数、注意力头、隐藏层大小和参数数量，都分别对应 BERT 的基础版和大型版。

![](../Images/554d81d8503b1480c90ba7c32d3130c4.png)

StructBERT 基础版与 StructBERT 大型版的比较

# 结论

通过引入一对新的训练目标，StructBERT 在 NLP 中达到新的极限，在各种下游任务上始终超越 BERT。研究表明，这两个目标在 StructBERT 设置中发挥了不可或缺的作用。词结构目标主要提高了模型在单句问题上的性能，使 StructBERT 能够重建词序，句子结构目标则改善了对句间关系的理解，这对句子对任务特别重要。

# 资源

+   [StructBERT：将语言结构纳入预训练以实现深度语言理解](https://arxiv.org/pdf/1908.04577.pdf)

*除非另有说明，否则所有图片均为作者提供*
