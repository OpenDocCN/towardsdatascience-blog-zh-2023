# 使用 Softmax Loss 训练句子变换器

> 原文：[`towardsdatascience.com/training-sentence-transformers-with-softmax-loss-c114e74e1583`](https://towardsdatascience.com/training-sentence-transformers-with-softmax-loss-c114e74e1583)

## [NLP For Semantic Search](https://jamescalam.medium.com/list/nlp-for-semantic-search-d3a4b96a52fe)

## 原始句子变换器（SBERT）的构建方式

[](https://jamescalam.medium.com/?source=post_page-----c114e74e1583--------------------------------)![James Briggs](https://jamescalam.medium.com/?source=post_page-----c114e74e1583--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c114e74e1583--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c114e74e1583--------------------------------) [James Briggs](https://jamescalam.medium.com/?source=post_page-----c114e74e1583--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c114e74e1583--------------------------------) ·阅读时间 9 分钟·2023 年 1 月 12 日

--

![](img/e9e629a36ae10fd37e92fbd2fa3327d2.png)

图片由[Possessed Photography](https://unsplash.com/@possessedphotography?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。最初发布于[Pinecone 的《NLP for Semantic Search ebook》](https://www.pinecone.io/learn/sentence-embeddings/)（作者所在公司）。

搜索正在进入一个黄金时代。得益于*“句子嵌入”*和专门训练的模型*“句子变换器”*，我们现在可以通过概念而不是关键词匹配来搜索信息，从而解锁类似人类的信息发现过程。

本文将探讨第一个句子变换器的训练过程，即*sentence-BERT* — 更常被称为*SBERT*。我们将探讨**自然语言推理**（NLI）训练方法以及*softmax loss*如何用于微调模型，以生成句子嵌入。

请注意，softmax loss 已不再是训练句子变换器的首选方法，它已被其他方法如 MSE margin 和[多负样本排序损失](https://www.pinecone.io/learn/fine-tune-sentence-transformers-mnr/)所取代。但我们仍将介绍这种训练方法，因为它是句子嵌入不断发展的一个重要里程碑。

本文还涵盖了*两种方法*的微调。第一种展示了如何使用 softmax loss 进行 NLI 训练。第二种利用了`sentence-transformers`库提供的优秀训练工具——这种方法更为抽象，使得构建好的句子变换器模型*容易得多*。

# NLI 训练

训练句子变换器有几种方法。最流行的一种（也是我们将讨论的方法）是使用自然语言推断（NLI）数据集。

NLI 专注于识别那些*推断*或*不推断*彼此的句子对。我们将使用两个数据集：斯坦福自然语言推断（SNLI）和多领域 NLI（MNLI）语料库。

合并这两个语料库后，我们得到 943K 句子对（SNLI 550K，MNLI 393K）。所有对都包括一个 `premise` 和一个 `hypothesis`，每对都分配一个 `label`：

+   **0** — *蕴含*，例如，`premise` 暗示了 `hypothesis`。

+   **1** — *中立*，`premise` 和 `hypothesis` 都可以为真，但它们不一定相关。

+   **2** — *矛盾*，`premise` 和 `hypothesis` 互相矛盾。

训练模型时，我们将首先将句子 A（即 `premise`）输入 BERT，然后在下一步输入句子 B（即 `hypothesis`）。

从那里，模型使用 `label` 字段通过 *softmax 损失* 进行优化。我们将很快更深入地解释这一点。

现在，让我们下载并合并这两个数据集。我们将使用 Hugging Face 的 `datasets` 库，可以通过 `!pip install datasets` 下载。要下载和合并，我们写：

两个数据集的 `label` 特征中包含 `-1` 值，表示无法分配自信的类别。我们使用 `filter` 方法将其移除。

我们必须将人类可读的句子转换为变换器可读的令牌，因此我们继续对句子进行分词。`premise` 和 `hypothesis` 特征必须分成各自的 `input_ids` 和 `attention_mask` 张量。

现在，我们需要做的就是准备数据，以便输入到模型中。为此，我们首先将 `dataset` 特征转换为 PyTorch 张量，然后初始化一个数据加载器，该加载器将在训练期间将数据输入到我们的模型中。

数据准备完成了。让我们继续讨论训练方法。

# Softmax 损失

使用 *softmax 损失* 是 Reimers 和 Gurevych 在原始 SBERT 论文 [1] 中使用的主要方法。

尽管这用于训练第一个句子变换器模型，但它不再是首选的训练方法。现在，[MNR 损失方法](https://www.pinecone.io/learn/fine-tune-sentence-transformers-mnr/) 是最常见的。我们将在另一篇文章中介绍这种方法。

然而，我们希望解释 softmax 损失有助于揭示训练句子变换器时应用的不同方法。我们在文章末尾包含了与 MNR 损失的比较。

## 模型准备

当我们训练 SBERT 模型时，不需要从头开始。我们从一个已经预训练好的 BERT 模型（及其分词器）开始。

在训练过程中，我们将使用一种称为*‘siamese’*-BERT 架构。这意味着，对于一个句子对，我们首先将*句子 A* 输入 BERT，然后在 BERT 处理完第一个句子后，再输入*句子 B*。

这会产生一个 *类似 Siamese* 的网络，我们可以想象有两个相同的 BERT 正在并行处理句子对。实际上，只有一个模型在处理两个句子。

![](img/efe79a46a78a8721758ea20b8be06f21.png)

Siamese-BERT 处理一对句子，然后将大规模的令牌嵌入张量池化为一个单一的密集向量。

BERT 将输出 512 个 768 维的嵌入。我们将这些嵌入转换为 *平均* 嵌入，使用 *均值池化*。这个 *池化输出* 是我们的句子嵌入。每步我们会有两个 —— 一个是句子 A，称为 `u`，另一个是句子 B，称为 `v`。

要执行此均值池化操作，我们将定义一个名为 `mean_pool` 的函数。

在这里，我们获取 BERT 的令牌嵌入输出（我们很快会全面看到）和句子的 `attention_mask` 张量。然后我们调整 `attention_mask` 的大小，以对齐到令牌嵌入的更高 `768` 维度。

我们将调整大小的掩码`in_mask`应用于这些令牌嵌入，以排除平均池化操作中的填充令牌。我们的平均池化操作在每个维度上取值的平均激活值，以产生一个单一值。这将我们的张量大小从 `(512*768)` 变为 `(1*768)`。

下一步是连接这些嵌入。论文中提出了几种不同的方法：

句子嵌入 `u` 和 `v` 的连接方法及其在 STS 基准测试中的表现。

其中，表现最佳的是通过连接向量 `u`、`v` 和 `|u-v|` 构建的。将它们全部连接起来会生成一个长度是每个原始向量三倍的向量。我们将这个连接向量标记为 `(u, v, |u-v|)`。其中 `|u-v|` 是向量 `u` 和 `v` 之间的逐元素差异。

![](img/e10b0acdcbb05e26e8860e333aa0b99e.png)

我们将 `(u, v, |u-v|)` 连接起来，以合并句子 A 和 B 的句子嵌入。

我们将使用 PyTorch 执行此连接操作。一旦我们获得了均值池化的句子向量 `u` 和 `v`，我们将其连接为：

向量 `(u, v, |u-v|)` 输入到前馈神经网络 (FFNN)。FFNN 处理这个向量并输出三个激活值。每个 `label` 类别的一个值：*蕴涵*、*中性* 和 *矛盾*。

由于这些激活和 `label` 类别对齐，我们现在计算它们之间的 softmax 损失。

![](img/ce06be3cfb369530c14702cc5e7e8765.png)

训练的最后步骤。将连接后的 `(u, v, |u-v|)` 向量输入到前馈神经网络中以生成三个输出激活值。然后我们计算这些预测和真实标签之间的 softmax 损失。

Softmax 损失通过对三个激活值（或节点）应用 softmax 函数来计算，从而生成预测标签。然后我们使用 [交叉熵损失](https://www.pinecone.io/learn/cross-entropy-loss/)来计算预测标签与真实`label`之间的差异。

然后，使用这个损失对模型进行优化。我们使用学习率为`2e-5`的 Adam 优化器，并且优化函数的线性预热期为总训练数据的*10%*。为此，我们使用标准的 PyTorch `Adam`优化器以及 HF transformers 提供的学习率调度器：

现在，让我们将所有内容结合到一个 PyTorch 训练循环中。

我们这里只训练一个 epoch。现实中这应该足够（并且与原始 SBERT 论文中描述的情况相符）。最后，我们需要做的是保存模型。

现在让我们将到目前为止所做的所有工作与`sentence-transformers`训练工具进行比较。我们将在文章末尾对比这个和其他句子变换模型。

# 使用 Sentence Transformers 进行微调

正如我们之前提到的，`sentence-transformers`库对那些希望训练模型而不必担心底层训练机制的人提供了很好的支持。

我们只需要做一些数据预处理（但比我们之前做的少）。所以让我们继续完成相同的微调过程，但使用`sentence-transformers`。

## 训练数据

我们再次使用相同的 SNLI 和 MNLI 语料库，但这次我们将它们转换为`sentence-transformers`所需的格式，使用它们的`InputExample`类。在此之前，我们需要像之前一样下载并合并这两个数据集。

现在我们准备好将数据格式化为`sentence-transformers`所需的格式。我们只需将当前的`premise`、`hypothesis`和`label`格式转换为与`InputExample`类*几乎*匹配的格式即可。

我们也像之前一样初始化了一个`DataLoader`。接下来，我们要开始设置模型。在`sentence-transformers`中，我们使用不同的*模块*来构建模型。

我们只需要 transformer 模型模块，然后是均值池化模块。transformer 模型从 HF 加载，因此我们像之前一样定义`bert-base-uncased`。

我们有数据、模型，现在定义如何优化我们的模型。Softmax 损失*非常*容易初始化。

现在我们准备好训练模型了。我们训练一个 epoch，并像之前一样将训练数据的 10% 用于预热。

完成后，新的模型保存到`./sbert_test_b`。我们可以使用`SentenceTransformer`或 HF 的`from_pretrained`方法从该位置加载模型！让我们继续将其与其他 SBERT 模型进行比较。

# 比较 SBERT 模型

我们将对一组随机句子进行模型测试。我们将使用*四*个模型构建每个句子的均值池化嵌入；*softmax-loss* SBERT、*multiple-negatives-ranking-loss* SBERT、原始 SBERT `sentence-transformers/bert-base-nli-mean-tokens` 和 BERT `bert-base-uncased`。

生成句子嵌入后，我们将计算所有可能句子对之间的余弦相似度，进行一个简单但有见地的语义文本相似性（STS）测试。

我们定义了两个新函数；`sts_process`用于构建句子嵌入并用余弦相似度进行比较，以及`sim_matrix`用于从所有可能的对构建相似性矩阵。

然后，我们只是通过`sim_matrix`函数运行每个模型。

处理完所有对之后，我们在热图可视化中展示结果。

![](img/ed5615ebbabe4878af807caf5bdab6da.png)

四种 BERT/SBERT 模型的相似性分数热图。

在这些热图中，我们理想情况下希望所有不相似的对具有非常低的分数（接近白色），而相似的对则产生明显更高的分数。

让我们来讨论这些结果。左下角和右上角的模型生成了正确的前三对，而 BERT 和*softmax 损失* SBERT 返回了 2/3 的正确对。

如果我们关注标准 BERT 模型，我们会看到方块颜色变化极小。这是因为几乎每一对的相似性分数在*0.6*到*0.7*之间。这种缺乏变化使得区分相似对和不相似对变得困难。然而，这也是可以预期的，因为 BERT*没有*针对语义相似性进行微调。

我们的 PyTorch *softmax 损失* SBERT（左上角）漏掉了*9–1*句子对。尽管如此，它产生的对比不相似对要明显更具区分性，比原始 BERT 模型有所改进。`sentence-transformers`版本表现更好，并且*没有*漏掉*9-1*对。

接下来，我们有 Reimers 和 Gurevych 在 2019 年论文中训练的 SBERT 模型（左下角）[1]。它比我们的 SBERT 模型表现更好，但相似对和不相似对之间仍然差异不大。

最后，我们有一个使用*MNR 损失*训练的 SBERT 模型。这个模型无疑是性能最优的。大多数不相似的对产生的分数*非常*接近*零*。最高的非对分数返回*0.28*——大约是实际对分数的一半。

这些结果表明 SBERT MNR 模型是性能最优的。它为真实对产生的激活值（相对于平均值）远高于其他模型，使得相似性更容易识别。SBERT 使用 softmax 损失显然比 BERT 有所改进，但不太可能比使用 MNR 损失的 SBERT 模型提供更多益处。

这就是关于微调 BERT 以构建句子嵌入的文章了！我们深入探讨了 SNLI 和 MNLI 数据集的预处理细节以及如何使用*softmax 损失*方法微调 BERT。

最后，我们将这个*softmax 损失* SBERT 与原始 BERT、原始 SBERT 以及[一个*MNR 损失* SBERT](https://www.pinecone.io/learn/fine-tune-sentence-transformers-mnr/)进行了比较，使用了一个简单的 STS 任务。我们发现，尽管使用*softmax 损失*进行微调确实能产生有价值的句子嵌入，但与较新的训练方法相比，其质量仍然欠佳。

我们希望这次对变换器如何微调以构建句子嵌入的探索能够带来深入的见解和兴奋感。

# 参考文献

[1] N. Reimers, I. Gurevych, [Sentence-BERT: 使用 Siamese BERT 网络的句子嵌入](https://arxiv.org/abs/1908.10084) (2019), ACL

**除非另有说明，否则所有图像均由作者提供*
