# 大型语言模型：RoBERTa——一种强健优化的BERT方法

> 原文：[https://towardsdatascience.com/roberta-1ef07226c8d8?source=collection_archive---------1-----------------------#2023-09-24](https://towardsdatascience.com/roberta-1ef07226c8d8?source=collection_archive---------1-----------------------#2023-09-24)

## 了解用于BERT优化的关键技术

[](https://medium.com/@slavahead?source=post_page-----1ef07226c8d8--------------------------------)[![Vyacheslav Efimov](../Images/db4b02e75d257063e8e9d3f1f75d9d6d.png)](https://medium.com/@slavahead?source=post_page-----1ef07226c8d8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1ef07226c8d8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1ef07226c8d8--------------------------------) [Vyacheslav Efimov](https://medium.com/@slavahead?source=post_page-----1ef07226c8d8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc8a0ca9d85d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Froberta-1ef07226c8d8&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=post_page-c8a0ca9d85d8----1ef07226c8d8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ef07226c8d8--------------------------------) ·5分钟阅读·2023年9月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1ef07226c8d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Froberta-1ef07226c8d8&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=-----1ef07226c8d8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1ef07226c8d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Froberta-1ef07226c8d8&source=-----1ef07226c8d8---------------------bookmark_footer-----------)![](../Images/0fcff885fed32012c5a3ec2976d5f60c.png)

# 介绍

**BERT**模型的出现带来了NLP领域的重大进展。BERT源自**Transformer**架构，在各种下游任务上实现了最先进的结果：语言建模、下一句预测、问答、命名实体识别标记等。

[](/bert-3d1bf880386a?source=post_page-----1ef07226c8d8--------------------------------) [## 大型语言模型：BERT——来自Transformer的双向编码表示

### 了解**BERT**如何构建最先进的嵌入表示

towardsdatascience.com](/bert-3d1bf880386a?source=post_page-----1ef07226c8d8--------------------------------)

尽管 BERT 的性能优秀，研究人员仍然继续尝试调整其配置，希望实现更好的指标。幸运的是，他们成功了，并提出了一种新的模型，称为 **RoBERTa**——鲁棒优化 BERT 方法。

在本文中，我们将引用官方的 [RoBERTa 论文](https://arxiv.org/pdf/1907.11692.pdf)，其中包含有关该模型的深入信息。简单来说，RoBERTa 包含了对原始 BERT 模型的几个独立改进——所有其他原则包括架构保持不变。所有的进展将会在本文中涵盖和解释。

# 1\. 动态掩蔽

从 BERT 的架构中，我们记得在预训练期间，BERT 通过尝试预测一定百分比的掩蔽标记来执行语言建模。原始实现的问题在于，对于给定的文本序列，在不同的批次中选择掩蔽的标记有时是相同的。

更准确地说，训练数据集重复了 10 次，因此每个序列仅以 10 种不同的方式进行掩蔽。考虑到 BERT 运行了 40 个训练周期，每个具有相同掩蔽的序列会传递给 BERT 四次。研究人员发现，使用动态掩蔽稍微更好，即每次将序列传递给 BERT 时掩蔽都是唯一生成的。总的来说，这在训练过程中减少了重复的数据，给模型提供了处理更多不同数据和掩蔽模式的机会。

![](../Images/5f0d5098a4da926aa2d2e4e5d5b760a7.png)

静态掩蔽与动态掩蔽

# 2\. 下一个句子预测

论文的作者进行了研究，以寻找建模下一个句子预测任务的最佳方法。因此，他们发现了几个有价值的见解：

+   **去除下一个句子预测损失会导致性能略有提升。**

+   **将单个自然语言句子传入 BERT 输入会降低性能，相较于传入由多个句子组成的序列**。解释这一现象的一个可能假设是模型仅依靠单个句子难以学习长距离依赖关系。

+   **从单个文档中采样连续的** **句子来构造输入序列比从多个文档中采样更有利。** 通常，序列总是从单个文档的连续完整句子中构造，这样总长度最多为 512 个标记。问题在于当我们到达文档末尾时。研究人员在这方面比较了是否值得停止采样句子，还是额外采样下一个文档的前几个句子（并在文档之间添加相应的分隔标记）。结果表明，第一个选项更好。

最终，对于 RoBERTa 的最终实现，作者选择保留前两个方面而省略第三个方面。尽管第三个见解带来了观察到的改进，但研究人员没有继续采用它，因为这会使之前实现之间的比较变得更加困难。这是因为达到文档边界并在此停止意味着输入序列将包含少于 512 个标记。为了在所有批次中保持类似的标记数量，在这种情况下需要增加批量大小。这会导致批量大小的变化和更复杂的比较，而研究人员希望避免这种情况。

![](../Images/49279eaf526bc460b7f3d52923f394a1.png)

# 3\. 增加批量大小

最近在自然语言处理（NLP）领域的进展表明，增加批量大小并适当减少学习率和训练步数通常会改善模型的性能。

提醒一下，BERT base 模型在 256 个序列的批量大小下训练了 100 万步。作者尝试了 2K 和 8K 的批量大小，并选择了后者来训练 RoBERTa。相应的训练步数和学习率值分别变为 31K 和 1e-3。

> 同时，需要注意的是，批量大小的增加通过一种叫做“***梯度累积***”的特殊技术可以更容易地进行并行化。

# 4\. 字节文本编码

在 NLP 中，存在三种主要的文本标记化类型：

+   字符级别的标记化

+   子词级别的标记化

+   单词级别的标记化

原始的 BERT 使用了子词级别的标记化，词汇表大小为 30K，该词汇表在输入预处理后使用多个启发式方法进行学习。RoBERTa 使用字节而非 Unicode 字符作为子词的基础，并将词汇表大小扩展到 50K，而无需任何预处理或输入标记化。这导致 BERT base 和 BERT large 模型分别增加了 15M 和 20M 的额外参数。RoBERTa 中引入的编码版本表现稍逊于之前的版本。

尽管如此，RoBERTa 中词汇表大小的增长使其能够编码几乎任何单词或子词，而无需使用未知标记，这相比于 BERT 是一个显著的优势。这使得 RoBERTa 可以更全面地理解包含稀有词汇的复杂文本。

![](../Images/cdee0913cccc1b2bf6291958373ee900.png)

# 预训练

除此之外，RoBERTa 采用与 BERT large 相同的架构参数，应用了上述四个方面。RoBERTa 的总参数数量为 355M。

RoBERTa 在五个大规模数据集的组合上进行了预训练，总共达到了 160 GB 的文本数据。相比之下，BERT large 只在 13 GB 的数据上进行了预训练。最后，作者将训练步数从 100K 增加到 500K。

结果是，RoBERTa 在最受欢迎的基准测试中超越了 BERT large 和 XLNet large。

# RoBERTa 版本

类似于 BERT，研究人员开发了两个版本的 RoBERTa。基础版和大型版中的大多数超参数是相同的。下图展示了主要的区别：

![](../Images/a46e3fd0b0d36e07965b858682fb8e91.png)

RoBERTa 的微调过程类似于 BERT 的过程。

# 结论

在本文中，我们探讨了 BERT 的改进版本，该版本通过引入以下几个方面修改了原始训练过程：

+   动态掩蔽

+   省略下一句预测目标

+   在更长的句子上进行训练

+   增加词汇表的大小

+   在数据上使用更大的批量进行更长时间的训练

结果显示，RoBERTa 模型在顶级基准测试中优于其前身。尽管配置更复杂，但 RoBERTa 仅增加了 1500 万个参数，同时保持了与 BERT 相当的推理速度。

# 资源

+   [RoBERTa: 一种稳健优化的 BERT 预训练方法](https://arxiv.org/pdf/1907.11692.pdf)

*除非另有说明，否则所有图片均为作者提供*
