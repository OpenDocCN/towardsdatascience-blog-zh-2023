# 大型语言模型：SBERT — Sentence-BERT

> 原文：[https://towardsdatascience.com/sbert-deb3d4aef8a4?source=collection_archive---------1-----------------------#2023-09-12](https://towardsdatascience.com/sbert-deb3d4aef8a4?source=collection_archive---------1-----------------------#2023-09-12)

## 学习如何使用 siamese BERT 网络准确地将句子转换为嵌入

[](https://medium.com/@slavahead?source=post_page-----deb3d4aef8a4--------------------------------)[![Vyacheslav Efimov](../Images/db4b02e75d257063e8e9d3f1f75d9d6d.png)](https://medium.com/@slavahead?source=post_page-----deb3d4aef8a4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----deb3d4aef8a4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----deb3d4aef8a4--------------------------------) [Vyacheslav Efimov](https://medium.com/@slavahead?source=post_page-----deb3d4aef8a4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc8a0ca9d85d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsbert-deb3d4aef8a4&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=post_page-c8a0ca9d85d8----deb3d4aef8a4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----deb3d4aef8a4--------------------------------) ·8 分钟阅读·2023年9月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdeb3d4aef8a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsbert-deb3d4aef8a4&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=-----deb3d4aef8a4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdeb3d4aef8a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsbert-deb3d4aef8a4&source=-----deb3d4aef8a4---------------------bookmark_footer-----------)![](../Images/213e63865ea083fbbc857bd1cdbdd7a5.png)

# 介绍

众所周知，**transformers** 在自然语言处理（NLP）领域取得了进化性的进展。在 transformers 的基础上，许多其他机器学习模型也得到了发展。其中之一是**BERT**，它主要由几个堆叠的 transformer **编码器** 组成。除了用于情感分析或问答等各种问题外，BERT 还因构建单词**嵌入**（即表示单词语义的数字向量）而变得越来越受欢迎。

将单词表示为嵌入形式带来了巨大的优势，因为机器学习算法无法处理原始文本，但可以操作向量的向量。这使得通过使用标准度量（如欧几里得距离或余弦距离）来比较不同单词的相似性成为可能。

问题在于，实际上，我们经常需要为整个句子而非单个单词构建嵌入。然而，基本的 BERT 版本仅在单词级别构建嵌入。因此，后续开发了几种类似 BERT 的方法来解决这个问题，本文将对此进行讨论。通过逐步讨论这些方法，我们将最终达到被称为**SBERT**的最先进模型。

> 为了深入了解 SBERT 的内部工作原理，建议您已经熟悉 BERT。如果没有，本文系列的前一部分会详细解释。

[](/bert-3d1bf880386a?source=post_page-----deb3d4aef8a4--------------------------------) [## 大型语言模型：BERT

### 了解 BERT 如何构建最先进的嵌入

towardsdatascience.com](/bert-3d1bf880386a?source=post_page-----deb3d4aef8a4--------------------------------)

# BERT

首先，让我们回顾一下 BERT 如何处理信息。作为输入，它接受一个 *[CLS]* 标记和两个由特殊 *[SEP]* 标记分隔的句子。根据模型配置，这些信息由多头注意力块处理 12 或 24 次。然后将输出汇总并传递到一个简单的回归模型中以获取最终标签。

![](../Images/7cec45ad8f1df3ed285b4332b79b587d.png)

BERT 架构

有关 BERT 内部工作原理的更多信息，您可以参考本文系列的前一部分：

## 交叉编码器架构

可以使用 BERT 来计算一对文档之间的相似度。考虑在一个大型集合中找到最相似的句子对。为了解决这个问题，每对可能的句子都放入 BERT 模型中。这会导致推理时的平方复杂度。例如，处理 *n = 10 000* 个句子需要 *n * (n — 1) / 2 = 49 995 000* 次 BERT 推理计算，这并不具备可扩展性。

## 其他方法

分析交叉编码器架构的低效性时，似乎合理的是独立地预计算每个句子的嵌入。之后，我们可以直接计算所有文档对之间选择的距离度量，这比将平方数量的句子对送入 BERT 要快得多。

不幸的是，这种方法在 BERT 中不可行：BERT 的核心问题在于，每次处理两个句子时同时进行，使得难以获得能够仅独立表示单个句子的嵌入。

研究人员尝试通过使用 *[CLS]* 标记嵌入来消除这个问题，希望它包含足够的信息来表示一个句子。然而，* [CLS]* 结果发现对于这个任务毫无用处，因为它最初在 BERT 中预训练用于下一个句子预测。

另一种方法是将单个句子传递给BERT，然后平均输出的token嵌入。然而，得到的结果甚至比简单平均GLoVe嵌入还要差。

> 导出独立的句子嵌入是BERT的主要问题之一。为了解决这一问题，开发了SBERT。

# SBERT

**SBERT**引入了**孪生网络**的概念，这意味着每次两个句子*独立地*通过相同的BERT模型。在讨论SBERT架构之前，让我们先参考一个关于孪生网络的细微说明：

> 在科学论文中，通常会展示一个孪生网络架构，其中多个模型接收许多输入。实际上，它可以被认为是一个具有相同配置和权重的单一模型，这些权重在多个并行输入之间共享。每当对单个输入更新模型权重时，其他输入的权重也会同步更新。

![](../Images/c26e287d0cd1471cdba937f37da6edc3.png)

左侧显示的是非孪生（交叉编码器）架构，而右侧是孪生（双编码器）架构。主要区别在于左侧模型同时接受两个输入。而右侧模型以并行方式接受两个输入，因此两个输出彼此不依赖。

回到SBERT，在通过BERT处理句子之后，会对BERT嵌入应用池化层，以获得其低维表示：最初的512个768维向量被转换为一个768维的向量。对于池化层，SBERT的作者建议默认选择均值池化层，尽管他们也提到可以使用最大池化策略或直接使用*[CLS]* token的输出。

当两个句子通过池化层时，我们会得到两个768维的向量*u*和*v*。利用这两个向量，作者提出了三种优化不同目标的方法，下面将进行讨论。

## 分类目标函数

这个问题的目标是将给定的句子对正确分类到几个类别中的一个。

在生成嵌入*u*和*v*之后，研究人员发现生成另一个从这两个向量派生出的向量作为元素级绝对差*|u-v|*是有用的。他们还尝试了其他特征工程技术，但这种方法显示了最佳结果。

最终，三个向量*u*、*v*和*|u-v|*被连接在一起，乘以一个可训练的权重矩阵*W*，然后将乘积结果输入到softmax分类器中，输出不同类别的句子的标准化概率。交叉熵损失函数用于更新模型的权重。

![](../Images/2f055f8250f01acf463b4258b35ae33d.png)

用于分类目标的SBERT架构。参数n表示嵌入的维度（BERT base的默认值为768），而k表示标签的数量。

一个常用的现有问题是 [**NLI**（自然语言推理）](https://paperswithcode.com/task/natural-language-inference)，对于给定的句子对 A 和 B（定义了假设和前提），需要预测假设是否为真（*entailment*）、假（*contradiction*）或未确定（*neutral*）。对于这个问题，推理过程与训练过程相同。

正如 [论文](https://arxiv.org/pdf/1908.10084.pdf) 中所述，SBERT 模型最初在两个数据集 SNLI 和 MultiNLI 上进行训练，这两个数据集包含一百万对句子及其对应的标签 *entailment*、*contradiction* 或 *neutral*。之后，论文中的研究人员提到 SBERT 调优参数的细节：

> “我们用一个三分类 softmax-分类器目标函数对 SBERT 进行微调，训练一个周期。我们使用了 16 的批量大小，Adam 优化器，学习率为 2e−5，并对 10% 的训练数据进行线性学习率热身。我们的默认池化策略是平均值。”

## 回归目标函数

在这种表述中，在获得向量 u 和 v 后，它们之间的相似度得分通过选择的相似度度量直接计算。预测的相似度得分与真实值进行比较，模型通过使用 MSE 损失函数进行更新。默认情况下，作者选择余弦相似度作为相似度度量。

![](../Images/b381c23229d4443516e6efdc035f46ab.png)

SBERT 回归目标的架构。参数 n 代表嵌入的维度（BERT base 的默认值为 768）。

在推理过程中，这种架构可以用两种方式之一：

+   对于给定的句子对，可以计算相似度得分。推理工作流程与训练过程完全相同。

+   对于给定的句子，可以提取其句子嵌入（在应用池化层之后）以供后续使用。这在我们需要计算大量句子对之间的相似度得分时特别有用。通过只对每个句子运行一次 BERT，我们提取了所有必要的句子嵌入。之后，我们可以直接计算所有向量之间的选定相似度度量（虽然这仍需要二次数量的比较，但同时我们避免了之前使用 BERT 进行的二次推理计算）。

## 三元组目标函数

Triplet 目标引入了一个三元组损失，该损失基于三个句子进行计算，通常称为*anchor*、*positive* 和 *negative*。假设 *anchor* 和 *positive* 句子彼此非常接近，而 *anchor* 和 *negative* 则差异很大。在训练过程中，模型评估 *(anchor, positive)* 对的相似度与 *(anchor, negative)* 对的相似度的差异。数学上，最小化以下损失函数：

![](../Images/caed12398bb6eb3f194cf4667f551f76.png)

[原始论文](https://arxiv.org/pdf/1908.10084.pdf)中的三元组损失函数。变量 sₐ、sₚ、sₙ 分别表示锚点、正面和负面嵌入。符号 ||s|| 是向量 s 的范数。参数 ε 称为 margin。

Margin ε 确保一个 *正面* 句子与 *锚点* 之间的距离至少比 *负面* 句子与 *锚点* 之间的距离多 ε。否则，损失将大于0。默认情况下，在这个公式中，作者选择了欧几里得距离作为向量范数，并将参数 ε 设为 1。

三元组 SBERT 架构与前两者的不同之处在于模型现在同时接受三个输入句子（而不是两个）。

![](../Images/1805aee36bb9bf02b5d1a338870b5da2.png)

SBERT 的回归目标架构。参数 n 代表嵌入的维度（BERT base 默认是 768）。

# 代码

[SentenceTransformers](https://www.sbert.net/index.html) 是一个用于构建句子嵌入的最先进的 Python 库。它包含了多个适用于不同任务的 [预训练模型](https://www.sbert.net/docs/pretrained_models.html)。使用 SentenceTransformers 构建嵌入非常简单，下面的代码片段展示了一个示例。

![](../Images/c38b4d4eb9eaf3a226ab4b4efd9e05c7.png)

使用 SentenceTransformers 构建嵌入

构建的嵌入可以用于相似性比较。每个模型都是为特定任务训练的，因此选择适当的相似性度量进行比较非常重要，可以参考文档。

# 结论

我们已经深入了解了一种用于获得句子嵌入的高级 NLP 模型。通过将 BERT 推理执行的二次数量减少到线性，SBERT 实现了速度的大幅提升，同时保持了高准确性。

要最终了解这种差异有多么显著，只需参考论文中描述的例子，其中研究人员尝试在 *n = 10000* 个句子中找到最相似的对。在现代 V100 GPU 上，这个过程使用 BERT 约需 65 小时，而使用 SBERT 仅需 5 秒！这个例子表明，SBERT 是 NLP 的巨大进步。

# 资源

+   [Sentence-BERT: 使用 Siamese BERT 网络的句子嵌入](https://arxiv.org/pdf/1908.10084.pdf)

+   [SentenceTransformers 文档 | SBERT.net](https://www.sbert.net/index.html)

+   [自然语言推理 | Papers with code](https://paperswithcode.com/task/natural-language-inference)

*除非另有说明，所有图像均由作者提供*
