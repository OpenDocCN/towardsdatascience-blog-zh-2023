# 向量搜索并不是你所需的一切

> 原文：[`towardsdatascience.com/vector-search-is-not-all-you-need-ecd0f16ad65e`](https://towardsdatascience.com/vector-search-is-not-all-you-need-ecd0f16ad65e)

[](https://medium.com/@alcarazanthony1?source=post_page-----ecd0f16ad65e--------------------------------)![安东尼·阿尔卡拉兹](https://medium.com/@alcarazanthony1?source=post_page-----ecd0f16ad65e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ecd0f16ad65e--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----ecd0f16ad65e--------------------------------) [安东尼·阿尔卡拉兹](https://medium.com/@alcarazanthony1?source=post_page-----ecd0f16ad65e--------------------------------)

·发表于[数据科学前沿](https://towardsdatascience.com/?source=post_page-----ecd0f16ad65e--------------------------------) ·阅读时间 6 分钟·2023 年 9 月 18 日

--

*人工智能软件被用来提升本文文本的语法、流畅性和可读性。*

# 引言

检索增强生成（RAG）已经革新了开放领域问答，使系统能够对各种查询生成类似人类的响应。RAG 的核心是一个检索模块，它扫描大量语料库以找到相关的上下文段落，这些段落随后由神经生成模块（通常是预训练的语言模型，如 GPT-3）处理，以制定最终答案。

尽管这种方法非常有效，但也不是没有局限性。

最关键的组成部分之一，即对嵌入段落的向量搜索，存在固有的限制，这可能会妨碍系统以细致的方式进行推理。这在需要跨多个文档进行复杂的多跳推理时尤为明显。

向量搜索指的是使用数据的向量表示来搜索信息。这涉及两个关键步骤：

1.  **将数据编码为向量**

首先，被搜索的数据会被编码为数值向量表示。对于像段落或文档这样的文本数据，这通过诸如 BERT 或 RoBERTa 的嵌入模型来完成。这些模型将文本转换为表示语义意义的连续数字的稠密向量。图像、音频和其他格式也可以使用适当的深度学习模型编码为向量。

2\. **使用向量相似性搜索**

一旦数据被编码为向量，搜索涉及到找到与搜索查询的向量表示相似的向量。这依赖于距离度量，如余弦相似度，以量化两个向量的接近程度并对结果进行排序。距离最小（相似度最高）的向量被返回为最相关的搜索结果。

向量搜索的主要优势在于能够搜索语义相似性，而不仅仅是字面上的关键词匹配。向量表示捕捉了概念意义，从而能够识别出更相关但语言上不同的结果。这使得搜索质量比传统的关键词匹配更高。

然而，将数据转换为向量并在高维语义空间中进行搜索也存在局限性。平衡向量搜索的权衡是一个活跃的研究领域。

在本文中，我们将剖析向量搜索的局限性，探讨它为何难以捕捉文档之间的多样关系和复杂的相互联系。我们还将深入研究如知识图谱提示等替代技术，这些技术有望克服这些不足之处。

随着我们的生活中越来越多地整合 AI 工具，了解当前 AI 工具的优缺点是至关重要的。本文旨在提供对向量搜索在增强大语言模型推理能力方面的优缺点的全面视角。

# 问题与答案之间的语义差距

在向量搜索中，输入问题和语料库中的段落都被编码为密集的向量表示。通过找到与问题向量具有最高语义相似性的段落来检索相关的上下文。

然而，问题往往与它们寻求的实际答案存在间接关系。

“法国的首都是什么？”的向量可能不一定与陈述“巴黎是法国人口最多的城市”的段落具有高度相似性。

这种语义差距意味着包含答案的段落可能会被忽视。

嵌入无法捕捉问题与答案之间的推理联系。

# 段落粒度的重要性

在向量搜索系统中，段落通常由单一的嵌入向量表示。这些段落的粒度可以有所不同。

如果段落非常大，例如整个文档，它可能包含多个概念。段落的某些部分可能相关，而其他部分则不相关。

但由于单个向量代表整个段落，因此无法区分相关部分和无关部分。整个段落可能与问题向量只有微弱的相似性。

相反，使用句子级别的块可以帮助隔离概念。但这会增加索引中的向量数量，增加计算开销。

选择段落大小时存在精度与可处理性之间的固有权衡。

# 对复杂推理的挑战

有些问题需要综合多个文档中的事实。

例如，“酿酒的最早历史记录是什么？” 可能需要从不同来源拼凑日期。

向量搜索对于这种多跳推理能力不足。每个段落独立地对问题进行评分。没有机制可以共同分析或连接不同结果中的信息。

随着问题变得越来越复杂，简单的相似性搜索达到了极限。系统难以从不同的段落中收集和上下文化事实。

# 黑箱模型工作原理

在标准向量搜索流程中，如何选择初始检索的段落是不透明的。排名取决于语义相似性模型的内部工作。

这种缺乏透明性使得结果难以解释、验证和改进，也限制了在业务关键应用中的部署。

为了增加监督，排名算法应提供一些可解释性，以说明为什么某些段落被认为是相关的。

# **建模多样关系**

标准向量搜索的核心限制在于其单一关注语义相似性。

然而，现实世界的推理需要对内容之间的多样关系进行建模。

[## 知识图谱提示：一种用于多文档问答的新方法](https://blog.gopenai.com/knowledge-graph-prompting-a-new-approach-for-multi-document-question-answering-ab5c4006a429?source=post_page-----ecd0f16ad65e--------------------------------)

### 多文档问答（MD-QA）涉及回答需要综合多篇信息的问题…

[blog.gopenai.com](https://blog.gopenai.com/knowledge-graph-prompting-a-new-approach-for-multi-document-question-answering-ab5c4006a429?source=post_page-----ecd0f16ad65e--------------------------------)

知识图谱通过明确编码各种连接到互联图结构中来克服这一点。具体而言：

+   *主题关系* — 如果段落共享稀有或关键关键词，则这些段落会被链接。这捕捉到讨论主题的相似性。

+   *语义关系* — 段落嵌入被比较以连接那些语义上接近的段落，即使它们不共享相同的术语。

这超越了表面层次的主题匹配。

+   *结构关系* — 段落与它们出现的特定部分、页面或文档相连。

这编码了上下文层次结构。

+   *时间关系* — 讨论时间顺序事件的段落按时间顺序链接在一起。

这代表了事件的流动。

+   *实体关系* — 在引用相同现实世界实体的段落之间添加了指代链接。

这允许以实体为中心的推理。

通过结合这些超越语义相似性的多样信号，知识图谱提示（KGP）提供了一个更丰富的推理基础，用于关于互联信息的推理。

**结构关系**

相比之下，标准向量搜索没有这些结构关系的概念。段落被视为原子，没有任何周围的上下文。

知识图谱对结构关系的建模值得进一步讨论。通过将段落链接到它们出现的特定文档或部分，信息的上下文层次被编码。

这使得可以明确推理某一事实所在的部分、其来源的文档以及发布的网站。

对层次文档结构的编码为确定重要性、有效性和相关性提供了有用的归纳偏差，这在跨段落推理时尤为重要。

**时间关系**

在孤立的向量搜索中完全不存在这种归纳偏差。向量相似性评分没有考虑时间动态。检索到的段落是断裂的快照，缺乏叙事流。

KGP 中时间关系的明确建模也带来了显著的优势。根据所描述事件的时间顺序排列段落，使得对展开的叙事和时间线进行推理成为可能。

知识图谱通过根据相对时间链接事件来克服这一局限性。这解锁了更丰富的推理能力。

**实体关系**

在标准的向量搜索中，这些实体链接没有直接建模。有关实体的宝贵知识在段落嵌入中丢失。

知识图谱连接实体引用的能力是一项强大的资产。链接讨论相同现实世界实体、概念或人物的段落，可以围绕这些共享元素进行重点推理。

KGP 保留了这一信号，使得可以以实体为中心探索知识图谱。这在跨文档聚合关于特定实体的事实时提供了结构性优势。

# 结论

向量搜索基于语义相似性实现了高效的近似匹配。然而，在 RAG 系统的检索步骤中，单独使用时存在明显的局限性。

采用结合向量搜索与基于图谱的知识表示、多步推理模块和透明排名算法的混合方法可以帮助克服这些弱点。

一如既往，没有单一的解决方案——利用多样化的技术工具包是实现现实世界问答系统的强大检索的关键。

![](img/45a8af157d31577aa016dfeff183c404.png)

此图像是使用 AI 图像生成模型创建的。

## 来源：

[`medium.com/thirdai-blog/understanding-the-fundamental-limitations-of-vector-based-retrieval-for-building-llm-powered-48bb7b5a57b3`](https://medium.com/thirdai-blog/understanding-the-fundamental-limitations-of-vector-based-retrieval-for-building-llm-powered-48bb7b5a57b3)

[`labelbox.com/blog/how-vector-similarity-search-works/`](https://labelbox.com/blog/how-vector-similarity-search-works/)

[`www.elastic.co/what-is/vector-search`](https://www.elastic.co/what-is/vector-search)

[`kaushikshakkari.medium.com/open-domain-question-answering-series-part-7-the-rise-of-vector-databases-in-the-world-of-9d848a3f47d5`](https://kaushikshakkari.medium.com/open-domain-question-answering-series-part-7-the-rise-of-vector-databases-in-the-world-of-9d848a3f47d5)

[`medium.com/@PolonioliAI/limitations-of-vectors-and-neural-search-4d81fd64482f`](https://medium.com/@PolonioliAI/limitations-of-vectors-and-neural-search-4d81fd64482f)

[`medium.com/vector-database/frustrated-with-new-data-our-vector-database-can-help-e5c430b29be7`](https://medium.com/vector-database/frustrated-with-new-data-our-vector-database-can-help-e5c430b29be7)

[`www.singlestore.com/blog/why-your-vector-database-should-not-be-a-vector-database/`](https://www.singlestore.com/blog/why-your-vector-database-should-not-be-a-vector-database/)

[`clickhouse.com/blog/vector-search-clickhouse-p1`](https://clickhouse.com/blog/vector-search-clickhouse-p1)

[`www.searchenginejournal.com/semantic-search-with-vectors/467574/`](https://www.searchenginejournal.com/semantic-search-with-vectors/467574/)

[`www.usenix.org/system/files/osdi23-zhang-qianxi_1.pdf`](https://www.usenix.org/system/files/osdi23-zhang-qianxi_1.pdf)

[`www.infoworld.com/article/3651360/solving-complex-problems-with-vector-databases.html`](https://www.infoworld.com/article/3651360/solving-complex-problems-with-vector-databases.html)

[`people.eecs.berkeley.edu/~matei/papers/2020/sigir_colbert.pdf`](https://people.eecs.berkeley.edu/~matei/papers/2020/sigir_colbert.pdf)

[`blog.futuresmart.ai/gpt-4-semantic-search-and-vector-databases-revolutionizing-question-answering`](https://blog.futuresmart.ai/gpt-4-semantic-search-and-vector-databases-revolutionizing-question-answering)

[`blog.vespa.ai/constrained-approximate-nearest-neighbor-search/`](https://blog.vespa.ai/constrained-approximate-nearest-neighbor-search/)

[`www.pinecone.io/learn/vector-search-filtering/`](https://www.pinecone.io/learn/vector-search-filtering/)
