# 使用 MNR 损失微调句子变换器

> 原文：[`towardsdatascience.com/fine-tuning-sentence-transformers-with-mnr-loss-cd6a26685b81`](https://towardsdatascience.com/fine-tuning-sentence-transformers-with-mnr-loss-cd6a26685b81)

## [语义搜索的 NLP](https://jamescalam.medium.com/list/nlp-for-semantic-search-d3a4b96a52fe)

## 多负样本排名损失的下一代句子嵌入

[](https://jamescalam.medium.com/?source=post_page-----cd6a26685b81--------------------------------)![James Briggs](https://jamescalam.medium.com/?source=post_page-----cd6a26685b81--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cd6a26685b81--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cd6a26685b81--------------------------------) [James Briggs](https://jamescalam.medium.com/?source=post_page-----cd6a26685b81--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----cd6a26685b81--------------------------------) ·8 分钟阅读·2023 年 2 月 3 日

--

![](img/1281f41ecb283e0989e5a4230b2c20f6.png)

图片由[Lars Kienle](https://unsplash.com/@larskienle?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。[文章最初发布于 pinecone.io](https://www.pinecone.io/learn/fine-tune-sentence-transformers-mnr/)，作者在该网站工作。

Transformer 生成的句子嵌入在很短的时间内取得了长足的进展。从 BERT 交叉编码器的缓慢但准确的相似度预测开始，句子嵌入的世界在 2019 年随着 SBERT 的出现而被点燃[1]。自那时以来，许多新的句子变换器被引入，这些模型迅速使原始 SBERT 成为过时的技术。

这些新型句子变换器是如何如此迅速超越 SBERT 的？答案是*多负样本排名（MNR）损失*。

本文将介绍 MNR 损失是什么，它所需的数据，以及如何实施以微调我们自己高质量的句子变换器。

实施将涵盖两种训练方法。第一种方法更为复杂，详细说明了微调模型的具体步骤。第二种方法则利用了`sentence-transformers`库出色的微调工具。

# NLI 训练

我们关于[softmax 损失](https://www.pinecone.io/learn/train-sentence-transformers-softmax/)的文章解释了如何使用**自然语言推理**（NLI）数据集来微调句子变换器。

这些数据集包含许多句子对，有些句子对*暗示*彼此，而其他句子对*不暗示*彼此。与 softmax 损失文章一样，我们将使用两个数据集：斯坦福自然语言推理（SNLI）和多领域 NLI（MNLI）语料库。

这两个语料库总共有 943K 句子对。每对包括一个`premise`和一个`hypothesis`句子，并且被分配一个`label`：

+   **0** — *蕴涵*，例如，`premise`暗示了`hypothesis`。

+   **1** — *中立*，`premise`和`hypothesis`可能都是真的，但它们不一定相关。

+   **2** — *矛盾*，`premise`和`hypothesis`相互矛盾。

在使用 MNR 损失进行微调时，我们将丢弃所有标记为*中立*或*矛盾*的行——仅保留正样本的*蕴涵*对。

我们将每一步将句子 A（`premise`，称为*锚点*）和句子 B（`hypothesis`，当标签为**0**时，这称为*正样本*）输入 BERT。与 softmax 损失不同，我们不使用`label`特征。

这些训练步骤是分批进行的。这意味着同时处理多个锚点-正样本对。

然后对模型进行优化，以在对之间生成相似的嵌入，同时对非对保持不同的嵌入。我们会很快更深入地解释这一点。

# 数据准备

让我们来看看数据准备过程。我们首先需要下载并合并两个 NLI 数据集。我们将使用 Hugging Face 的`datasets`库。

由于我们使用的是 MNR 损失，我们只需要锚点-正样本对。我们可以应用过滤器来移除所有其他对（包括错误的`-1`标签）。

数据集的准备方式现在取决于我们的训练方法。我们将继续准备更复杂的 PyTorch 方法。如果你更愿意直接训练模型而不关心涉及的步骤，可以跳过到下一部分。

对于 PyTorch 方法，我们必须对自己的数据进行分词。为此，我们将使用`transformers`库中的`BertTokenizer`并在我们的`dataset`上应用`map`方法。

完成这些后，我们就准备初始化我们的`DataLoader`，它将在训练期间用于将数据批次加载到模型中。

这样，我们的数据就准备好了。让我们继续训练。

# PyTorch 微调

训练 SBERT 模型时，我们不是从零开始，而是从已经预训练的 BERT 开始——我们只需*微调*它来构建句子嵌入。

MNR 和 softmax 损失训练方法在微调过程中使用*‘siamese’*–BERT 架构。这意味着在每一步中，我们将*句子 A*（我们的*锚点*）输入 BERT，然后是*句子 B*（我们的*正样本*）。

![](img/6a40444126cf3a5ef5105b792e645664.png)

Siamese-BERT 网络中，*锚点*和*正样本*句子对被分别处理。一个均值池化层将 token 嵌入转换为句子嵌入。句子 A 是我们的*锚点*，句子 B 是*正样本*。

因为这两个句子是*分别*处理的，所以它创建了一个类似*孪生*的网络，两个相同的 BERT 并行训练。实际上，每一步只使用一个 BERT。

我们可以进一步扩展到*三元组*网络。对于 MNR 的三元组网络，我们会传递三个句子，一个*锚点*，一个*正样本*和一个*负样本*。然而，我们*没有*使用三元组网络，因此我们已从数据集中移除了负样本行（`label` 为 `2` 的行）。

![](img/da2d651eb4d13417cfdb094d6d663e13.png)

Triplet 网络使用相同的逻辑，但增加了一个句子。对于 MNR 损失，这个句子是*负样本*对的*锚点*。

BERT 输出 512 个 768 维的嵌入。我们通过*均值池化*将这些嵌入转换为*平均*句子嵌入。使用孪生网络方法，每一步生成两个这样的嵌入——一个是我们称之为`a`的*锚点*，另一个是称之为`p`的*正样本*。

在`mean_pool`函数中，我们取这些 token 级别的嵌入（512）和句子 `attention_mask` 张量。我们调整`attention_mask`以匹配 token 嵌入的更高 `768` 维度。

调整后的掩码`in_mask`应用于 token 嵌入，以排除均值池化操作中的填充 token。均值池化计算每个维度的值的平均激活，但*排除*那些填充值，以免减少平均激活。此操作将我们的 token 级别嵌入（形状为`512*768`）转换为句子级别嵌入（形状为`1*768`）。

这些步骤是在*批次*中执行的，即我们同时处理许多（*锚点，正样本*）对。这在接下来的步骤中很重要。

首先，我们计算每个锚点嵌入（`a`）与同一批次中*所有*正样本嵌入（`p`）之间的余弦相似度。

从这里，我们为每个锚点嵌入`a_i`生成一个余弦相似度分数向量（大小为`batch_size`）（或大小为`*2 * batch_size*`的*三元组*）。每个锚点应与其正样本`p_i`共享最高分数。

![](img/5730faa4fa2216092158c79e73367056.png)

使用五对/三元组的三元组网络进行余弦相似度评分（使用 `(a, p, n)`）。孪生网络相同，但排除了深蓝色的 `n` 块（`n`）。

为了优化，我们使用一组递增的标签值来标记每个`a_i`的最高分数位置，并使用分类 [交叉熵损失](https://apps-showcase--optimistic-curran-b817a8.netlify.app/learn/cross-entropy-loss/)。

这就是我们用于 MNR 损失微调所需的所有组件。让我们把这些整合起来，设置一个训练循环。首先，我们将模型和层移动到支持 CUDA 的 GPU *（如果有的话）*。

然后我们设置优化器和训练计划。我们使用 Adam 优化器，并对总步数的 10% 进行线性预热。

现在，我们使用相同的训练过程定义训练循环。

就这样，我们使用 MNR 损失微调了我们的 BERT 模型。现在我们将其保存到文件中。

现在可以使用`SentenceTransformer`或 HF `from_pretrained`方法加载它。在我们开始测试模型性能之前，让我们看看如何使用*更简单的* `sentence-transformers`库来复制微调逻辑。

# 快速微调

如前所述，使用 MNR 损失微调模型有一个更简单的方法。`sentence-transformers`库允许我们使用预训练的句子变换器，并附带一些方便的训练工具。

我们将从预处理数据开始。这与我们之前进行的前几步相同。

之前，我们将数据标记化，然后加载到 PyTorch `DataLoader`中。这次我们遵循*稍微不同的格式*。我们*不*标记化；我们重新格式化为`sentence-transformers` `InputExample`对象的列表，并使用略有不同的`DataLoader`。

我们的`InputExample`仅包含我们的`a`和`p`句子对，然后将其输入到`NoDuplicatesDataLoader`对象中。该数据加载器确保每个批次没有重复——这是在使用 MNR 损失对*随机*抽样对进行排名时的一个有用功能。

现在我们定义模型。`sentence-transformers` 库允许我们使用*模块*来构建模型。我们只需要一个变换器模型（我们将再次使用`bert-base-uncased`）和一个平均池化模块。

我们现在有了一个初始化的模型。在训练之前，剩下的就是损失函数——MNR 损失。

这样，我们的数据加载器、模型和损失函数就准备好了。剩下的就是微调模型！与之前一样，我们将训练一个周期，并在训练步骤的前 10%进行热身。

几个小时后，我们有了一个使用 MNR 损失训练的新句子变换器模型。毫无疑问，使用`sentence-transformers`训练工具使生活*轻松得多*。为了结束这篇文章，让我们看看我们的 MNR 损失 SBERT 与其他句子变换器的性能。

# 比较句子变换器

我们将使用语义文本相似性（STS）数据集来测试*四个模型*的性能；我们的*MNR 损失* SBERT（使用 PyTorch 和`sentence-transformers`）、*原始* SBERT，以及一个在[1B+样本数据集](https://huggingface.co/spaces/flax-sentence-embeddings/sentence-embeddings)上用 MNR 损失训练的 MPNet 模型。

首先我们需要下载 STS 数据集。我们将再次使用 Hugging Face 的`datasets`。

STSb（或 STS 基准）包含特征`sentence1`和`sentence2`中的句子对，并分配了从*0 -> 5*的相似性分数。

STSb 验证集中的三个样本。

由于相似性分数范围为 0 -> 5，我们需要将其归一化到 0 -> 1 的范围。我们使用`map`来完成这项工作。

我们将使用`sentence-transformers`的评估工具。我们首先需要使用`InputExample`类重新格式化 STSb 数据——将句子特征作为`texts`，将相似度分数传递给`label`参数。

要评估模型，我们需要初始化适当的评估对象。由于我们在评估连续相似度分数，我们使用`EmbeddingSimilarityEvaluator`。

这样，我们就准备好开始评估了。我们将模型加载为`SentenceTransformer`对象，并将模型传递给我们的`evaluator`。

评估器输出*斯皮尔曼等级相关系数*，用于计算模型输出嵌入与 STSb 提供的相似度分数之间的余弦相似度。两个值之间的高相关性输出接近*+1*，没有相关性则输出*0*。

对于使用`sentence-transformers`微调的模型，我们输出了*0.84*的相关性，这意味着我们的模型根据 STSb 分配的分数输出了良好的相似度分数。让我们将其与其他模型进行比较。

排名前两的模型使用 MNR 损失训练，其次是原始 SBERT。

这些结果支持了`sentence-transformers`作者给出的建议，即使用 MNR 损失训练的模型在构建高性能句子嵌入方面优于使用 softmax 损失训练的模型[2]。

另一个关键点是，尽管我们尽了最大努力，并且构建这些 PyTorch 模型的复杂性很高，但*每个*使用易用的`sentence-transformers`工具训练的模型都远远超越了它们。

简而言之，用 MNR 损失微调你的模型，并使用`sentence-transformers`库进行微调。

这就是本次关于使用多负样本排名损失微调句子变换器模型的教程和指南——当前构建高性能模型的最佳方法。

我们首先处理了两个最受欢迎的 NLI 数据集——斯坦福 NLI 和多类别 NLI 语料库——以便使用 MNR 损失进行微调。随后，我们深入探讨了使用 PyTorch 进行这种微调方法的细节，然后利用了`sentence-transformers`库提供的出色训练工具。

最后，我们学习了如何使用语义文本相似性基准（STSb）评估我们的句子变换器模型——识别表现最佳的模型。

# 参考文献

[1] N. Reimers, I. Gurevych, [Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks](https://arxiv.org/abs/1908.10084) (2019), ACL

[2] N. Reimers, [Sentence Transformers NLI Training Readme](https://github.com/UKPLab/sentence-transformers/tree/master/examples/training/nli), GitHub
