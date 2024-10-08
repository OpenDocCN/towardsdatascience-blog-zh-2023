# AI 的句子嵌入，揭密

> 原文：[`towardsdatascience.com/ais-sentence-embeddings-demystified-7c9cce145dd2`](https://towardsdatascience.com/ais-sentence-embeddings-demystified-7c9cce145dd2)

## 弥合计算机与语言之间的差距：AI 句子嵌入如何革新自然语言处理

[](https://medium.com/@dataemporium?source=post_page-----7c9cce145dd2--------------------------------)![Ajay Halthor](https://medium.com/@dataemporium?source=post_page-----7c9cce145dd2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7c9cce145dd2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7c9cce145dd2--------------------------------) [Ajay Halthor](https://medium.com/@dataemporium?source=post_page-----7c9cce145dd2--------------------------------)

·发表于[数据科学前沿](https://towardsdatascience.com/?source=post_page-----7c9cce145dd2--------------------------------) ·阅读时长 10 分钟·2023 年 8 月 1 日

--

![](img/c42af0ad915c5feb7d8468cc3195ef2d.png)

图片由[Steve Johnson](https://unsplash.com/@steve_j?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇博客文章中，让我们揭示计算机如何理解句子和文档。为了开始这个讨论，我们将回顾最早使用 n-gram 向量和 TF-IDF 向量表示句子的方法。后续章节将讨论从神经袋词到今天看到的句子变换器和语言模型的方法。还有很多有趣的技术要介绍。让我们从简单优雅的 n-gram 开始我们的旅程。

# 1\. N-gram 向量

计算机不理解单词，但它们理解数字。因此，我们需要将单词和句子转换为向量，以便计算机处理。句子作为向量的最早表示之一可以追溯到[克劳德·香农于 1948 年的论文](https://people.math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf)，信息理论的奠基人。在这项开创性的工作中，句子被表示为一个单词的 n-gram 向量。*这意味着什么？*

![](img/adde47738399c0b060b5a05fc6910ffe.png)

图 1：从句子生成 n-gram 向量。（作者提供的图片）

以句子“This is a good day”为例。我们可以将这个句子分解成以下 n-gram：

+   **单字组**：This, is, a, good, day

+   **双字组**：This is, is a, a good, good day

+   **三字组**：this is a, is a good, a good day

+   以及更多内容……

通常，一个句子可以分解为其组成的 n-gram，从 1 到 n 迭代。在构建向量时，这个向量中的每个数字表示 n-gram 是否在句子中出现。一些方法则可以使用句子中 n-gram 的出现次数。*图 1* 上展示了一个句子的示例向量表示。

# 2\. TF-IDF

另一种早期且流行的句子和文档表示方法是确定句子的 TF-IDF 向量，即“词频—逆文档频率”向量。在这种情况下，我们会计算一个词在句子中出现的次数来确定文本频率 (TF)。然后我们会计算包含该词的句子数量来确定逆文档频率 (IDF)。将这些值相乘得到每个词的 TF-IDF 乘积。对句子中每个单词重复这一计算，可以生成该句子的 TF-IDF 向量。*图 2* 显示了句子的一个 TF-IDF 表示示例。

![](img/a4a440ddd75570d0adc296f3c8b05636.png)

图 2：从句子生成 TF-IDF 向量。（图片来源：作者）

因此，每个句子（文档）由一个等于语言词汇表的向量表示。每个位置将是该句子中每个词的 TF-IDF 分数。如果你感兴趣的话，TF-IDF 是在这个 [1972 年的论文](https://www.emerald.com/insight/content/doi/10.1108/eb026526/full/html) 中引入的。

# 3\. 评估早期句子向量

## 优点

+   **简单**：基于计数的表示方法非常容易理解和手动计算。

+   **固定长度向量**：具有不同单词数量的句子可以由相同长度的向量表示。这种统一性使得处理更加方便。

## 缺点

+   **稀疏性**：生成的句子向量较大，大小级别与词汇表的规模相当。对于 n-gram 的情况，向量的大小是可能的 n-gram 数量，随着 *n* 值的增长，这个值可能变得异常大，即使 *n* 是一个适中的大小。句子相似性通常通过计算其向量之间的距离来评估。对于这样大的稀疏向量，句子 A 和 B 之间的距离可能与另一对句子 A 和 C 的距离难以区分。这意味着我们无法确定句子 A 是否更类似于 B 还是 C，因为维度数量变得太大。这个问题通常被称为 **维度灾难**，困扰着许多统计学和人工智能领域。

显然，这些句子表示变得难以处理。为了解决这个问题，我们退后一步。与其将句子表示为向量，不如 *当词本身被表示为向量时会发生什么？*

# 4\. 密集词表示

一篇[2001 年的神经概率语言模型论文](https://www.jmlr.org/papers/volume3/bengio03a/bengio03a.pdf)提出了用稠密和连续的固定长度向量表示词的想法。这种方法通过确保每个词可以用一个可处理大小的向量（~64、128 或 256）表示，而不是与词汇表大小（~100,000）成比例，从而克服了维度灾难。

![](img/9db69c57c74cbf82d32ee605199e8eb9.png)

图 3：2001 年，这种语言模型在训练期间学习了稠密的词表示（[图像来源](https://www.jmlr.org/papers/volume3/bengio03a/bengio03a.pdf)）

在*图 3*中，*C(.)*是表示单个词的向量。理想情况下，我们希望计算机对这些词有一定的语义理解。一种衡量方法是确保学习到的对应于相似词的向量在空间中彼此更靠近，如*图 4*所示。NLP 领域致力于改进这些词嵌入。

![](img/68e681ab7694b422101565d5590bf4c2.png)

图 4：这是一个嵌入空间，每个点代表一个词的向量表示。如果相似的词彼此靠得更近，则表示向量效果良好，如此图所示（图像由作者提供）

# 5\. Word2Vec

实现进一步学习词嵌入的目标的一个进展是[2013 年的 Word2Vec 框架](https://arxiv.org/abs/1301.3781)。通过非常简单的前馈神经网络架构，学习到了比以往更高质量的词向量，如*图 5*所示。

![](img/31749ebf457fa5e7e39836c58c8e96b8.png)

图 5：2 种 Word2Vec 架构学习了每个词的有效连续稠密表示（[图像来源](https://arxiv.org/abs/1301.3781)）

要了解更多关于 Word2Vec 和其他类似架构的信息，[请查看我关于此主题的博客，其中我们解释了这些架构](https://medium.com/towards-data-science/word2vec-glove-and-fasttext-explained-215a5cd4c06f)。本质上，到此为止，词可以通过固定长度的向量有效地表示，并且这些向量可以由计算机处理。*但是句子呢*，这是本博客关注的主题。

# 6\. 神经词袋

一种简单的方法是对句子中的词向量取平均。这种方法在*图 6*中展示。一个句子本质上被视为一个*词袋*，不考虑排序或上下文信息。从这个固定大小的向量中，我们可以添加前馈层以执行诸如情感分析或主题分类等机器学习任务。这种方法的优点是确保生成的向量尺寸更小，同时保留其组成词的一些意义。然而，它的表示缺乏对上下文和语法的理解。

![](img/6dd0ca9b6881d598c73da6a4862eb388.png)

图 6：神经词袋架构简单地平均单词向量以获得对应的句子向量（作者提供的图片）

多年来，已经取得了进展，以更好地保留句子向量中的上下文和语法。

# 7\. 自然语言处理中的卷积神经网络

卷积神经网络（CNNs）允许不同长度的序列使用卷积和池化操作的组合表示为固定长度的向量。卷积操作使句子中的单词能够从周围的单词中获取上下文，从而在最终向量中编码意义。这在[1989 年的时间延迟神经网络用于音素检测](https://www.cs.toronto.edu/~fritz/absps/waibelTDNN.pdf)中就已应用。该网络的架构如*图 7*所示，其中，给定一个音频波形，我们将语音的音素分类为“b”、“d”或“g”。

![](img/8332afc1d25166a60aaad89f961750c6.png)

图 7：使用卷积处理序列信息的初始时间延迟神经网络架构（[图片来源](https://www.cs.toronto.edu/~fritz/absps/waibelTDNN.pdf)）

在这篇开创性的论文之后，卷积在自然语言处理中的进展有限。然而，它们在 2010 年代得到了复兴，性能得到改善，并有跟进研究，如[动态卷积神经网络（DCNNs）](https://arxiv.org/pdf/1404.2188.pdf)，原因如下：

+   **更多数据可用**：训练具有大量参数的大型模型需要更多的数据。

+   **软件进展**：这些是前述段落中讨论的密集词表示学习的进展。

+   **硬件进展**：引入了 GPU 和 CUDA，以加速模型训练并利用并行化。

动态卷积神经网络具有根据序列长度动态变化的架构。因此，即使是长句子和文档也可以适当地处理，而无需更改网络的配置。

![](img/31ca0e4b58e95e2c79454d098f517de8.png)

图 8：根据句子输入长度具有动态最大 k 池化的 DCNN 架构（[图片来源](https://arxiv.org/pdf/1404.2188.pdf)）

欲了解有关这些卷积架构及其如何塑造自然语言处理领域的更多信息，[请查看这个关于该主题的其他 Medium 博客](https://medium.com/@dataemporium/convolution-in-nlp-573d0329cc37)。与卷积一样，近年来也出现了一些架构，它们获得了更多的关注。*它们是什么？*

# 8\. 循环神经网络

递归神经网络（RNNs）是时间上展开的前馈网络。因此，它们可以处理序列数据。尽管自 1980 年代以来就有人谈论 RNN，[Rumelhart 的这篇论文是最早的间接提及之一](https://apps.dtic.mil/dtic/tr/fulltext/u2/a164453.pdf)，但早期并未使用，因为训练困难。这一情况随着[Ilya Sutskever 的 2013 年博士论文](https://www.cs.utoronto.ca/~ilya/pubs/ilya_sutskever_phd_thesis.pdf)的出现而改变，论文展示了如何稳定地训练 RNN。从那时起，我们见证了 RNN 在处理序列数据方面的爆发性增长。

他们顺序处理输入单词，使每个单词都能参考前面的单词。引入双向 RNN 后，单词的上下文可以基于其在句子中前后的单词进行理解。

然而，使用 RNN 时，长句子和文档输入可能导致训练网络时梯度消失或爆炸。这正是 LSTM（长短期记忆）网络展现其处理长序列能力的地方。有关这个主题的更多信息，[请查看我配套的 YouTube 视频](https://youtu.be/QciIcRxJvsM)。

![](img/77baf6de9426431566f7cee5eb9c18da.png)

图 9: LSTM 单元以更好地理解长句子 ([image source](https://colah.github.io/posts/2015-08-Understanding-LSTMs/))

然而，这些递归神经网络的一个问题是训练速度非常慢，以至于它们使用了截断版本的反向传播来学习参数。这种慢速归因于 RNN 逐个处理数据的方式。因此，它们不能很好地利用现代 GPU。

# 9\. Transformer 神经网络

Transformer 神经网络解决了 RNN 的慢速问题。在[2017 年的论文《Attention is all you need》](https://arxiv.org/abs/1706.03762)中，Transformer 架构被用于解决诸如机器翻译、问答等序列到序列的问题。该网络由一个编码器和解码器组成，其中包含注意力单元，如*图 10*所示。这些注意力单元使网络能够学习高质量的单词向量表示，同时考虑到长期依赖关系。除此之外，它还优于 RNN，因为输入单词现在可以并行处理，从而实现比 RNN 更快的训练速度。有关 Transformer 及其构建方法的更多信息，[请查看这个视频播放列表](https://www.youtube.com/playlist?list=PLTl9hO2Oobd97qfWC40gOSU8C0iu0m2l4)。

![](img/1d808e6bbe3bacb4794106514f4684e9.png)

图 10: Transformer 神经网络的架构 ([image source](https://arxiv.org/abs/1706.03762))

尽管有这些优秀的特性，但这种变换器网络也有其局限性。每当需要学习一个新问题时，我们需要从头开始训练模型。这需要为每个任务准备大量量身定制的数据。例如，如果我想建立一个从英语到卡纳达语的翻译器，我可能需要数万个例子。这些数据对许多任务来说可能很难获取。*我们如何解决这个问题？*

# 10\. BERT

BERT 代表双向编码器表示来自变换器。作为这种设计，它是一组变换器编码器，旨在处理语言任务。许多自然语言处理任务都有“语言理解”这一共同元素。因此，如果我们有一个对语言有基本理解的 AI，那么微调这个模型以理解语言翻译、问答或任何其他语言任务不应该需要太多的数据。基于这一理念，[BERT 于 2019 年推出](https://arxiv.org/pdf/1810.04805.pdf)，其训练阶段分为两个部分：

+   **预训练**：模型从头学习语言理解

+   **微调**：模型学会用较少的数据解决特定的自然语言处理任务。

![](img/4a0082a8259bf5ef50d09197a56b390c.png)

图 11：BERT 的预训练和微调 ([图片来源](https://arxiv.org/pdf/1810.04805.pdf))

从本质上讲，你可以下载一个预训练的 BERT 模型，并使用你拥有的数据对其进行微调，即使你没有太多数据。因此，BERT 利用迁移学习快速训练模型来解决语言问题。有关 BERT 及其训练和推理的更多信息，[请查看这个 YouTube 视频](https://youtu.be/xI0HHN5XKDo)。

然而，BERT 是一组变换器编码器。而变换器编码器内部学习高质量的词嵌入。那么回到这个问题，*我们如何得到句子嵌入？*

# 11\. 句子转换器

最简单的方法是将句子传递给 BERT 以生成高质量的词嵌入。然后对这些词嵌入取平均值以获得句子嵌入。这是最基本的句子转换器。

![](img/2d20b1f22bbc2fc26444cb48fd4021b7.png)

图 12：一个基本的句子转换器，它接受一个句子并生成一个向量 ([图片来源](https://arxiv.org/pdf/1908.10084.pdf))

句子转换器的流程非常简单，即将句子通过 BERT 和池化层，以获得该句子的向量表示。[但这会导致比 GloVe 嵌入更差的嵌入](https://arxiv.org/pdf/1908.10084.pdf)。因此，我们对 BERT 架构进行微调，针对以下一个或两个任务：

+   **自然语言推理** [(NLI)](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fwww.sbert.net%2Fdocs%2Fpretrained-models%2Fnli-models.html&link_redirector=1)

+   **句子测试相似性** [(STS)](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fwww.sbert.net%2Fdocs%2Fpretrained-models%2Fsts-models.html&link_redirector=1)

下图*图 13*展示了每个部分的微调过程。

![](img/008214a995416a71ba5f85a0cb30a474.png)

图 13：左侧是对 NLI 训练的 Siamese 网络，右侧是对 STS 训练的 Siamese 网络 ([图像来源](https://arxiv.org/pdf/1908.10084.pdf))

一旦这种架构的某一种形式被训练好，我们就可以使用其中一个 BERT 和池化单元进行推理，将句子转换为更高质量的向量表示。

# 结论

在这篇文章中，我们了解了将句子表示为向量的进展。在 NLP 的早期，我们开始使用基于计数的方法，如 n-gram 向量和 TF-IDF 向量。然而，这些方法产生了稀疏向量，计算机难以处理。但随着 2000 年代初期密集向量表示的引入、数据的增加以及硬件的进步，我们能够训练出具有高质量词向量理解的模型。这也可以有效地用于创建句子向量，如前面关于句子转换器的部分所示。

当前的语言模型对单词、句子和文档的理解越来越好。我希望你和我一样，对学习更多关于 AI 未来的内容感到兴奋。

*快乐学习！*
