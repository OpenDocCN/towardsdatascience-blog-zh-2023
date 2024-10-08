# 填空自监督在自然语言处理中的应用

> 原文：[`towardsdatascience.com/fill-in-the-blanks-self-supervision-in-nlp-f0afb16dc7fd`](https://towardsdatascience.com/fill-in-the-blanks-self-supervision-in-nlp-f0afb16dc7fd)

## 为什么它强大以及如何解决

[](https://jagota-arun.medium.com/?source=post_page-----f0afb16dc7fd--------------------------------)![Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----f0afb16dc7fd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f0afb16dc7fd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0afb16dc7fd--------------------------------) [Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----f0afb16dc7fd--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0afb16dc7fd--------------------------------) ·阅读时间 14 分钟·2023 年 5 月 10 日

--

![](img/03be7b5832a9d134961e51b08dd7a5a7.png)

图片来源 [Patrick Tomasso](https://unsplash.com/@impatrickt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/Oaqk7qqNh_c?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

预测下一个词在语言建模中有着悠久而成功的历史，尤其是在大型语言模型中。它利用了大量的文本语料库：维基百科、分布在全球的公共网页等。相比其他类型的无监督学习，这种方法在这些语料库上的效果更强，因为学习是有监督的。此外，由于它是自监督的，因此不需要人工创建标记数据。

考虑

> 曝露在阳光下会导致 __

从一个足够丰富的英语句子语料库中，机器学习应该能够学习一个语言模型，能够合理地填补这些空白，在这个例子中是“皮肤癌”。

我们为什么称之为监督？因为我们从一个完整的标记序列开始，模糊掉最后一个词，要求模型预测它是什么，如果模型预测正确则奖励它，预测错误则惩罚它。所以这就是有监督学习。

最近，这种自监督方法以一种简单而强大的方式得到了扩展。具体而言，要填补的空白可以出现在文本的任何位置。因此，我们可能称之为文本重构任务，而不仅仅是文本补全任务。

以下是一些例子。每个实例是一个词的序列（以**粗体**标识），后面跟着一个或多个这些词被掩蔽的序列。模型的目标是正确填充这些空白。

```py
**exposure to sunlight causes skin cancer.**
exposure to sunlight causes ___________.
____________________ causes skin cancer.
________ to ________ causes skin cancer.
exposure to sunlight ______ skin cancer.
```

在这篇文章的副标题中，我们提到这种扩展大大提升了预测下一个词的能力。这里我们解释一下为什么我们这么说。

当然，预测下一个单词可以迫使模型从各种场景中学习，因为很容易组装一个包含数十亿个真实单词序列的数据集。也就是说，将其推广到预测序列中任何掩蔽单词的子集可以大大扩展学习场景的范围。这就是为什么这种掩蔽在 BERT [2] 中扮演了核心角色。

考虑一个包含 *n* 个单词的序列。如果我们只允许最后一个单词被掩蔽，我们只会从中生成一个标记实例——填入最后一个单词。如果我们允许任何后缀被掩蔽，我们可以从中生成最多 *n* 个标记实例——填入一个后缀。如果我们允许任何子集的单词被掩蔽，我们可以从中生成约 2^*n* 个标记实例。这是因为有这么多可以被掩蔽的子集。

此外，正如我们在上面的例子中所见，掩蔽不同的单词将迫使模型学习剩余单词的表示，从而在可能的情况下预测被掩蔽的单词。在我们的例子中，填补右尾的空白迫使模型理解暴露在阳光下的影响，而掩蔽左尾的两个单词则迫使模型学习导致皮肤癌的原因。

这是另一个例子，我们在[3]中覆盖过。

想象一下我们有一个汽车名称的列表。例如：

> 本田思域、丰田*Celica*、福特*Mustang*、吉普*Wrangler*……

预测下一个单词无疑有助于模型学习特定品牌的模型。例如，*Cherokee* 和 *Wrangler*（以及其他）是*Jeep*的模型。然而，通过掩蔽任何单词的额外自我监督也有助于模型学习到模型名称往往能强烈预测品牌。例如，*Celica* 是 *Toyota*，*Mustang* 是 *Ford*，*Wrangler* 是 *Jeep*，等等。

这种学习将更好地服务于下游应用场景。例如回答问题。

> *Celica* 的品牌是什么？

在本文的其余部分，我们将介绍几种不同的建模方法，并讨论它们的优点和局限性。

我们将讨论的方法在处理这种任务的细微版本时，远不如最先进的大型语言模型。例如，整个段落被掩蔽。

尽管如此，它们更容易理解，也许对一些读者更为熟悉，并且可以使用广泛可用的 NLP 工具包尝试，或者在某些情况下从头开始构建。

**数据**

假设我们有一组文本文档。文档是一个令牌（单词、标点符号等）的序列。在这篇文章中，我们将假设我们定义的学习任务不会跨越文档边界。

大部分情况下，我们将关注由单个英文句子组成的文档。我们可以想象，这些句子语料库是通过使用 NLP 将较长文档分割成句子从较长的文档语料库中得出的。也就是说，我们讨论的方法同样适用于更长的多句文档。只是对单句文档的描述更为方便。

在这篇文章中，我们将描述一种概念上简单易懂、实现简单且在训练和推断过程中速度较快的方法。

也就是说，它的范围仅限于填补单个遮罩区域的空白，无论是在左尾、右尾还是中间。大型语言模型可以填补多个遮罩区域的空白。此外，LLM 通常会更准确，尤其是在长序列上，如完整段落或甚至整页文本。

**方法**

我们的方法将包括两个 tries，一个前向 trie 和一个后向 trie，以特定的方式融合在一起。我们将这种数据结构称为前向-后向 trie（FB-Trie）。

我们将使用以下示例来说明前向-后向 trie 数据结构。假设我们的训练集中只有两个句子来构建 trie。

```py
work causes stress.
work is good for you.
```

首先，让我们看看普通的前向 Trie。

![](img/e89c30bc4a0035dfb07eba8f6e495aac.png)

图 1（作者提供）：我们的数据上的前向 Trie

这个 Trie 只捕捉了两个根到叶子的路径上显示的两个单词序列。

Trie 中的一个节点隐式地表示在根到节点路径中以该节点结尾的单词序列。

让我们填补图 1 中未显示的重要细节。一个节点存储训练集中前缀与其匹配的序列数量。因此，在我们的示例中，根节点将存储 2，因为我们的训练集中有两个序列。假设有一个更丰富的训练集，表示 [*work*, *causes*] 的节点将存储 15，如果有这么多的序列以 *work causes* 开头的话。

令 *i* 表示一个节点，*n*(i) 表示它的计数。令 *j* 表示 *i* 的一个子节点。从 *i* 到 *j* 的弧的概率是 *n*(*j*)/*n*(*i*)，我们将其称为 *P*(*i*, *j*)。

从这个 Trie 中，如果存在明显的高概率扩展，我们可以在右尾填补单个空白。让我们通过示例来说明这一点。

考虑 *work ____*。我们不会填补这些空白，因为我们可以想象有很多不同的句子以 *work* 开头。另一方面，如果我们有 *work causes* ___*，*用 stress 填补空白将更为合理*。

**填补左尾的空白**

如果填空查询是 ___ *causes stress* 呢？为了扩展我们的模型以回答这种查询，我们将添加一个后向 Trie。

后向 Trie 是如果我们反转训练集中序列而得到的 trie。它的结构与前向 trie 相同（图 1）。它还包括节点上的计数。

左尾空白现在可以通过相同的推理过程进行填充（如果可能的话），只不过我们查找的是空白需要填充的序列的反向字典树。

在填空输入中，我们将知道空白是位于左尾还是右尾，因此我们将知道是查看输入的前向字典树还是输入反向的后向字典树。

**填充中间的空白**

考虑当待填充的空白不在任何尾部时。例如*工作 __ 压力*。

对于这个任务，我们将设计一个新的数据结构，这是前向和后向字典树的特殊融合，向两个字典树添加了父节点弧线，并添加了桥接边以从一个字典树跳到另一个。

让我们从向前向字典树添加父节点弧线开始。

![](img/5c647f09fa299380c643294711c5dc7e.png)

图 2（作者提供）：带有父节点弧线的我们的数据上的前向字典树

我们能够添加父节点弧线，因为根据定义，字典树中的每个非根节点都有唯一的父节点。

接下来，我们将向后向字典树添加父节点弧线。

**融合前向字典树和后向字典树**

接下来，我们将融合这两个字典树。我们下面展示了我们示例中的融合字典树。

![](img/d7862aa84681368de6c41c5711c653a0.png)

图 3（作者提供）：我们示例中的前向-后向字典树

前向字典树的节点和弧线用实线表示，而后向字典树的节点和弧线用虚线表示。前向字典树中的父节点弧线用虚线曲线表示。为了减少混乱，后向字典树中不显示父节点弧线，但它们确实存在。

两个字典树通过桥接边融合在一起。这些是没有箭头的虚线边。每条边可以实现为两个节点之间的一对弧线，覆盖两个方向（这未被描绘）。

在图 3 中，只展示了许多桥接边中的几个——实际上是两个。

现在让我们解释这些桥接边是如何添加的。一旦理解了这一点，读者可以想象其余桥接边的位置。

想象一下在一个新序列上进行训练。为了具体说明，假设这是*工作导致压力*的标记化版本。首先，我们将这个序列插入前向字典树和后向字典树。现在我们考虑这个序列的所有可能的（*前缀*，*后缀*）分割。接下来，我们反转后缀。

以下是我们将获得的（*前缀*，*后缀*，*反向后缀*）三元组，*排除前缀或后缀为空的情况*。

```py
**prefix            suffix              reverse suffix**
[work]            [causes, stress]    [stress, causes]
[work, causes]    [stress]            [stress]
```

现在我们按照如下方法处理每个三元组（*前缀*，*后缀*，*反向后缀*）。我们在前向字典树中查找前缀以找到它结束的节点。同样地，我们在后向字典树中查找反向后缀以找到它结束的节点。我们现在通过一条桥接边连接这两个节点。

**模型大小和训练时间**

在描述推理之前，让我们花些时间反思模型的大小以及训练所需的时间。

首先，由于我们有两个字典树——一个前向字典树和一个反向字典树。此外，我们还有桥接边。

在这篇文章中，我们认为模型的大小本身不是问题。最先进的技术可以轻松容纳巨大的模型。

更有趣的问题是训练模型需要多长时间。

首先，训练可以是增量的。也就是说，可以随时向 FB-Trie 添加新序列。事实证明，训练实际上非常快速。这是因为（i）将新序列插入前向字典树及其逆序到反向字典树是很快的，和（ii）枚举所有这些序列的分裂，进行查找以发现要桥接的节点，并添加各种桥接边也是很快的。

**使用前向-反向字典树填充空白的说明**

让我们用*work __ stress*来说明这一点。首先，我们查找前向字典树中的节点，称之为*u*，这是[*work*]结束的地方。接着我们查找反向字典树中的节点，称之为*v*，这是[*stress*]结束的地方。然后，我们沿着从反向字典树到前向字典树的桥接边走。接着，我们沿着[ *work*, *causes* ]在前向弧中的父节点走，最终到达[*work*]。现在我们在两个方向上都到达了相同的节点。在这个过程中我们沿着父弧走的标签序列，经过反转之后，将成为空白的值。在我们的示例中，我们只走了一条反向弧，这条弧标记为*causes*。所以我们的答案是*“causes”*。

**推断：更一般的描述**

在我们的*work __ stress*示例中，空白有一个唯一的解决方案。在一般情况下，*L* __ *R*，可能会有多个解决方案*。

（我们略微调整了术语，因为在本节中会有所帮助。L 和 R 是标记序列，而不是字符串。）

下面我们讨论如何处理一般情况。

首先，我们在前向字典树中查找 L 的最大前缀，称之为 L'。接着，我们在反向字典树中查找 R 的最大后缀，称之为 R'。让*u*表示前向字典树中 L'结束的节点，*v*表示反向字典树中 R'的逆序结束的节点。

这种情况如下图所示。

![](img/0ea12264b8dd2b4c9dd51f0790753e84.png)

图 4（作者提供）：一般推断——多解情况

由于 v 在反向字典树中不是叶子节点——否则就没有空白可以填充——因此必须至少有一条桥接边接触到*v*，如图 4 中的标注所示。

考虑任何一条这样的桥接边，并让*w*表示在前向字典树另一端的节点。我们按照前向字典树中的父弧顺序跟踪，从*w*开始。当我们首次到达一个节点，称之为*x*，这个节点位于前向字典树中的路径 L'上时，我们停止。这种情况总是会发生（见下一段）。

最佳情况是*x*等于*v*。最坏情况是*x*是前向字典树的根。

逆转从 *w* 到 *x* 的路径上的标签顺序会产生一种推断的解决方案。更准确地说，这个解决方案是针对填空问题 *L’’* __ *R’*，这可能比我们开始时的问题 *L* __ *R* 更宽泛。

**进一步扩展解决方案**

每条触及 *v* 的桥接边都会生成一个唯一的解决方案。因此，形式为 *L*’’ __ *R*’ 的解决方案的数量就是触及 *v* 的桥接边的数量。请注意 *L*” 取决于 *w*。

我们可以通过从 *u* 开始而不是 *v* 来反向推断，从 *u* 到 *w* 走一条桥接边。这次 *w* 是反向字典树中的一个节点。然后我们在反向字典树中从 *w* 顺序地沿父节点弧走，直到第一次遇到反向字典树中的 *R’* 节点，我们将其称为 *x*。 (这次 *x* 是反向字典树中的一个节点，而不是正向字典树中的。) 我们得到另一组形式为 *L*’ __ *R*’’ 的解决方案。注意 *R’’* 取决于 *w*。

两组解决方案可能会重叠。这些集合容易去重，因为解决方案是由表示填充部分的路径的端点唯一确定的。请注意，路径本身可能不同，但端点（忽略起点和终点）将是相同的。

让我们通过图 3 中的“*work* __ *stress*”的例子来说明这一点。

![](img/e99a6dd7e2a062d131ec69be8b99e437.png)

图 5（作者提供）：两种方法来填入“causes”在“work __ stress”中。

如果我们按照所描述的算法进行，有两种方法可以将“causes”填入“work __ stress”中。一种方法是从反向字典树中的[*stress*]的桥接边开始，另一种方法是从正向字典树中的[*work*]的桥接边开始。尽管这两种解决方案是通过不同的路径找到的，但在忽略起始点和终点后，两条路径上的端点是相同的：正向字典树中的[*work*]和反向字典树中的[*stress*]。

如果我们仅允许从其中一个字典树开始，我们不会得到重复。但我们不能排除可能会遗漏某些解决方案的可能性。

这是一个例子。假设我们在以下两个字符串的分词版本上进行了训练。

```py
work causes burnout.
work causes stress.
```

考虑提示“*work* __ *stress*”。从反向字典树中触及[*stress*]的桥接边开始，我们会得到解决方案“*work* ***causes*** *stress*”。从正向字典树中触及[*work*]的桥接边开始，我们会得到解决方案“*work* ***causes burnout***”。

很合理地怀疑第二个答案是否应该被认为是一个有效的解决方案，因为它忽略了词语*stress*。这是一个需要建模者做出的决定。这两种观点都有其合理性。

在这篇文章中，我们的观点是，如果有人想将第二个答案视为有效的解决方案，这可以通过枚举两个方向上的所有解决方案然后去重来实现。

**评分解决方案**

当填空题有多个解决方案时，为了能够按优劣顺序呈现它们，对它们进行评分是合理的。

我们已经看到一个评分有意义的例子。对于提示“*工作* __ *压力”*，如果我们认为“*工作* ***导致倦怠”*** *也是一个解决方案，那么它应该比*“工作* ***导致*** *压力”*的评分低。这是因为前者忽略了提示中的单词压力，而后者则没有*。

在这个例子中，前一个解决方案的粗体字比后一个多这一事实在此*没有作用*。答案中的单词数量本身并不重要，重要的是被忽略的提示信息的量。

现在考虑这个提示：“*工作* __”。假设训练集仅包含这两个字符串

```py
work causes burnout.
work causes stress.
```

“*工作* ***导致倦怠”*** 和 “*工作* ***导致压力”***都是不错的解决方案。

现在想象一下，训练集中实际上包含了这两个字符串的多个副本，并且字符串“*工作导致压力*”的出现频率远高于*工作导致倦怠*。这就意味着，将“*工作* ***导致压力”***评为比*“工作 __”*更好的解决方案是合理的，而不是*“工作* ***导致倦怠”***。

对于上一段的场景，提供所需排名顺序的评分很容易实现。从前向查找树上的节点[*工作*, *导致*]开始，选择标记为“*压力*”的弧的概率高于选择标记为*“倦怠”*的弧的概率。这些概率可以从之前讨论的前向查找树中存储的信息中轻松计算出来。

对于填空在左尾的提示，我们可以类似地对解决方案进行排名。这次我们使用的是反向查找树，而不是前向查找树。

唯一剩下的情况是当空白在中间，并且两个解决方案使用相同的提示信息时。也就是说，两个解决方案的形式为 L M1 R 和 L M2 R，其中 L 和 R 在两个解决方案中是相同的，但 M1 和 M2 不同。

为了覆盖这种情况，我们将定义解决方案 L M R 的评分如下。写作 M = [*m*1] M’ [*m*2]，其中*m*1 和*m*2 是 M 中的第一个和最后一个标记。M’可能为空，这种情况下*m*1 等于*m*2。

我们将定义 L M R 的评分为前向查找树中 P(m1|L)与反向查找树中 P(m2|R)的乘积。

**总结**

在这篇文章中，我们首先讨论了填空自监督问题作为训练大型语言模型的一种强大方法。然后，我们提出了使用自定义融合的前向和反向查找树来解决这个问题的方法。

尽管这种解决方案在与现代深度学习和基于变换器的大型语言模型相比时并不具备竞争力，但在较小版本的问题中，特别是在回答单句填空题时，它可能非常有效。

从零实现相对容易理解且快速训练，同时也支持增量训练。

**参考文献**

1.  `towardsdatascience.com/contextual-text-correction-using-nlp-81a1363c5fc3`

1.  [`arxiv.org/pdf/1810.04805.pdf`](https://arxiv.org/pdf/1810.04805.pdf) [BERT]

1.  [`medium.com/towards-data-science/multi-task-learning-4531eb32d77b`](https://medium.com/towards-data-science/multi-task-learning-4531eb32d77b)
