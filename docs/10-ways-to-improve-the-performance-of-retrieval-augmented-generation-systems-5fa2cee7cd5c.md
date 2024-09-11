# 提升检索增强生成系统性能的10种方法

> 原文：[https://towardsdatascience.com/10-ways-to-improve-the-performance-of-retrieval-augmented-generation-systems-5fa2cee7cd5c?source=collection_archive---------0-----------------------#2023-09-18](https://towardsdatascience.com/10-ways-to-improve-the-performance-of-retrieval-augmented-generation-systems-5fa2cee7cd5c?source=collection_archive---------0-----------------------#2023-09-18)

## 从原型到生产的工具

[](https://mattambrogi.medium.com/?source=post_page-----5fa2cee7cd5c--------------------------------)[![Matt Ambrogi](../Images/60aec1bdc756f49ecf556056ab82f950.png)](https://mattambrogi.medium.com/?source=post_page-----5fa2cee7cd5c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5fa2cee7cd5c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5fa2cee7cd5c--------------------------------) [Matt Ambrogi](https://mattambrogi.medium.com/?source=post_page-----5fa2cee7cd5c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1e23ad8f92c5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-ways-to-improve-the-performance-of-retrieval-augmented-generation-systems-5fa2cee7cd5c&user=Matt+Ambrogi&userId=1e23ad8f92c5&source=post_page-1e23ad8f92c5----5fa2cee7cd5c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5fa2cee7cd5c--------------------------------) ·9分钟阅读·2023年9月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5fa2cee7cd5c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-ways-to-improve-the-performance-of-retrieval-augmented-generation-systems-5fa2cee7cd5c&user=Matt+Ambrogi&userId=1e23ad8f92c5&source=-----5fa2cee7cd5c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5fa2cee7cd5c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-ways-to-improve-the-performance-of-retrieval-augmented-generation-systems-5fa2cee7cd5c&source=-----5fa2cee7cd5c---------------------bookmark_footer-----------)![](../Images/abcb15689ece03425fe79b2d191b0ae0.png)

# 快速入门指南并不够

> “检索增强生成是补充用户对大型语言模型（LLM）如ChatGPT的输入，加入你（系统）从其他地方*检索*到的额外信息的过程。LLM然后可以利用这些信息来*增强*它所*生成*的响应。” — Cory Zue

LLMs是一个了不起的发明，但存在一个关键问题。它们会编造内容。RAG通过为LLMs提供事实背景，使其在回答查询时更有用。

使用 LangChain 或 LlamaIndex 等框架的快速入门指南，任何人都可以用大约五行代码构建一个简单的 RAG 系统，比如一个文档聊天机器人。

不过，用那五行代码构建的机器人效果不会很好。RAG 易于原型设计，但很难投入生产——即达到用户满意的程度。一个基础教程可能使 RAG 达到 80% 的效果，但弥补剩下的 20% 通常需要一些认真的实验。最佳实践尚未确定，并且可能因使用场景而异。但是，弄清楚最佳实践是非常值得我们花时间的，因为 RAG 可能是使用 LLMs 的最有效方式。

本文将探讨提高 RAG 系统质量的策略。它专为那些使用 RAG 构建系统、希望弥补基础设置与生产级性能之间差距的读者量身定制。就本文而言，提高意味着增加系统处理的查询比例，其中系统：1. 查找适当的上下文 2. 生成适当的响应。我将假设读者已经理解 RAG 的工作原理。如果没有，我建议阅读 [这篇文章](https://scriv.ai/guides/retrieval-augmented-generation-overview/) 作为一个很好的介绍。还假设读者对用于构建这些工具的常见框架有一些基本了解：[LangChain](https://www.langchain.com/) 和 [LlamaIndex](https://gpt-index.readthedocs.io/en/stable/getting_started/starter_example.html)。然而，本文讨论的思想与框架无关。

我不会深入探讨每种策略的具体实施细节，而是会尝试给出何时以及为何它可能有用的想法。鉴于这一领域的快速发展，提供一个详尽或完全最新的最佳实践清单是不可能的。相反，我旨在概述一些你在尝试改进检索增强生成应用程序时可以考虑和尝试的内容。

# 提高检索增强生成性能的 10 种方法

**1. 清理你的数据。**

RAG 将 LLM 的能力连接到你的数据上。如果你的数据在实质上或布局上令人困惑，那么你的系统将会受挫。如果你使用的数据包含冲突或冗余的信息，你的检索将难以找到正确的上下文。当它找到时，LLM 执行的生成步骤可能会不尽如人意。假设你正在为你的创业公司帮助文档构建一个聊天机器人，你发现它的效果不好。你首先应该查看的是你输入系统的数据。主题是否按逻辑分解？主题是否集中在一个地方还是多个分开的位置？如果你作为一个人，不能轻松判断需要查看哪个文档来回答常见问题，你的检索系统也无法做到。

这个过程可以像手动合并同一主题的文档一样简单，但你可以更进一步。我见过的一个更具创意的方法是使用LLM创建所有提供的文档的摘要。检索步骤可以首先在这些摘要上进行搜索，仅在必要时才深入详细信息。一些框架甚至将其作为内置抽象。

**2\. 探索不同的索引类型。**

索引是LlamaIndex和LangChain的核心支柱。它是承载检索系统的对象。RAG的标准方法涉及嵌入和相似性搜索。将上下文数据分块，嵌入所有内容，当有查询时，从上下文中找到相似的部分。这种方法效果很好，但并非所有用例的最佳方法。查询是否与特定项目有关，例如电子商务商店中的产品？你可能需要探索基于关键词的搜索。它不一定是非此即彼，许多应用程序使用混合方法。例如，你可以对特定产品的查询使用基于关键词的索引，但对一般客户支持则依赖嵌入。

**3\. 尝试不同的分块方法。**

分块上下文数据是构建RAG系统的核心部分。框架抽象了分块过程，让你无需考虑。但你应该考虑。分块大小很重要。你应该探索哪种方式最适合你的应用程序。一般来说，更小的块通常会改善检索，但可能会导致生成缺乏周围上下文。有很多方法可以处理分块。唯一不适用的是盲目处理。[这篇来自PineCone的文章](https://www.pinecone.io/learn/chunking-strategies/)列出了一些策略。我有一组测试问题。我通过[运行实验](https://www.mattambrogi.com/posts/chunk-size-matters/)来处理这些问题。我用小、中、大块大小循环了一遍，发现小块效果最好。

**4\. 玩转你的基础提示。**

在LlamaIndex中使用的一个基础提示的例子是：

*“上下文信息如下。根据上下文信息而非先前知识，回答查询。”*

你可以覆盖这个设置并尝试其他选项。你甚至可以破解RAG，使其在找不到好的答案时允许LLM依赖自己的知识。你还可以调整提示来帮助引导它接受的查询类型，例如，指示它对主观问题以某种方式回应。至少，覆盖提示是有帮助的，这样LLM可以了解自己正在做的工作。例如：

*“你是一个客户支持代表。你旨在尽可能提供有用的信息，同时只提供事实信息。你应该友好，但不要过于多话。上下文信息如下。根据上下文信息而非先前知识，回答查询。”*

**5\. 尝试元数据过滤。**

提高检索效果的一个非常有效的策略是为你的块添加元数据，然后利用它来帮助处理结果。日期是一个常见的元数据标签，因为它允许你进行[按时间过滤](https://www.mattambrogi.com/posts/recency-for-chatbots/)。假设你正在构建一个允许用户查询其电子邮件历史的应用程序。较新的电子邮件可能会更相关。但从嵌入角度来看，我们不能确定它们与用户查询的相似性最高。这提出了一个在构建RAG时需要记住的普遍概念：相似性 ≠ 相关性。你可以将每封电子邮件的日期附加到其元数据中，然后在检索时优先考虑最新的上下文。LlamaIndex有一个内置的Node Post-Processors类，专门帮助处理这个问题。

**6\. 使用查询路由。**

通常拥有多个索引是很有用的。然后在查询到达时，将它们路由到适当的索引。例如，你可能会有一个处理总结性问题的索引，一个处理指向性问题的索引，还有一个适用于与日期相关的问题的索引。如果你尝试优化一个索引以应对所有这些行为，你会发现它在所有这些行为上的表现都将有所折中。相反，你可以将查询路由到合适的索引。另一种用例是将一些查询定向到基于关键词的索引，如第2节所述。

一旦你构建了你的索引，你只需要在文本中定义每个索引的用途。然后在查询时，LLM将选择合适的选项。LlamaIndex和LangChain都提供了这样的工具。

**7\. 关注重新排名。**

重新排名是解决相似性与相关性之间差异问题的一种方法。通过重新排名，你的检索系统会像往常一样获取顶级节点作为上下文。然后它会根据相关性对这些节点进行重新排序。[Cohere Rereanker](https://docs.cohere.com/docs/reranking)通常用于此策略。这是我看到专家们经常推荐的策略。无论用例是什么，如果你在使用RAG，你应该尝试重新排名，看看它是否能改善你的系统。LangChain和LlamaIndex都提供了简化设置的抽象。

**8\. 考虑查询转换。**

你已经通过将用户的查询放入基本提示中来改变用户的查询。进一步改变它可能是有意义的。这里有几个例子：

重新表述：如果你的系统没有找到查询的相关上下文，你可以让LLM重新表述查询并再次尝试。对人类来说看似相同的两个问题，在嵌入空间中不总是看起来那么相似。

HyDE: [HyDE](http://boston.lti.cs.cmu.edu/luyug/HyDE/HyDE.pdf)是一种策略，它会获取一个查询，生成一个假设响应，然后使用这两者进行嵌入查找。研究发现，这可以显著提高性能。

子查询：LLM 在处理复杂查询时通常表现更好。你可以将这种功能集成到你的 RAG 系统中，以便将查询分解为多个问题。

LLamaIndex 有关于这些类型的 [查询转换](https://gpt-index.readthedocs.io/en/v0.6.9/how_to/query/query_transformations.html) 的文档。

**9\. 微调你的嵌入模型。**

基于嵌入的相似性是 RAG 的标准检索机制。你的数据被分解并嵌入到索引中。当查询到来时，它也会被嵌入以便与索引中的嵌入进行比较。但是，是什么在进行嵌入呢？通常是一个预训练模型，如 OpenAI 的 text-*embedding*-*ada*-*002*。

问题在于，预训练模型对嵌入空间中的相似性的理解可能与你实际上下文中的相似性不太一致。假设你在处理法律文件。你希望你的嵌入模型更多地依据你的领域特定术语，如“知识产权”或“合同违约”，而不是像“特此”和“协议”这样的通用术语。

你可以微调你的嵌入模型来解决这个问题。这样做可以提升你的检索指标 5%–10%。这需要更多的努力，但可以显著改善你的检索性能。这个过程比你想象的要简单，LlamaIndex 可以帮助你生成训练集。有关更多信息，你可以查看 Jerry Liu 关于 LlamaIndex 如何进行嵌入微调的 [这篇文章](https://medium.com/llamaindex-blog/fine-tuning-embeddings-for-rag-with-synthetic-data-e534409a3971)，或 [这篇文章](https://betterprogramming.pub/fine-tuning-your-embedding-model-to-maximize-relevance-retrieval-in-rag-pipeline-2ea3fa231149)，其中详细介绍了微调的过程。

**10\. 开始使用 LLM 开发工具。**

你可能已经在使用 LlamaIndex 或 LangChain 来构建你的系统。这两个框架都有有用的调试工具，允许你定义回调，查看使用了什么上下文，检索来自哪个文档等等。

如果你发现这些框架内置的工具不够完善，那么有一个不断扩展的工具生态系统可以帮助你深入了解你的RAG系统的内部工作。Arize AI 提供了一个 [notebook 内工具](https://arize.com/resource/arizeobserve-introducing-phoenix-ml-observability-in-your-notebook/)，允许你探索上下文是如何被检索的以及为什么。 [Rivet](https://news.ycombinator.com/item?id=37433218) 是一个提供可视化界面的工具，帮助你构建复杂的代理。它刚刚由法律科技公司 Ironclad 开源。新工具不断推出，值得尝试看看哪些工具对你的工作流程有帮助。

# 结论

使用 RAG 构建系统可能令人沮丧，因为它很容易上手，但要做到完美却非常困难。我希望以上策略能为你提供一些灵感，帮助你弥合这一差距。这些想法中的任何一个都不是全能的，整个过程充满了实验、尝试和错误。我在这篇文章中没有深入探讨评估，即如何衡量系统的性能。目前，评估更像是一门艺术而非科学，但设立某种可以持续检查的系统是很重要的。这是判断你所实施的变化是否有效的唯一方法。我之前写过关于[如何评估 RAG](https://www.mattambrogi.com/posts/evaluating-chatbots/)系统的文章。欲了解更多信息，你可以探索[LlamaIndex Evals](https://gpt-index.readthedocs.io/en/v0.7.2/how_to/evaluation/evaluation.html)、[LangChain Evals](https://blog.langchain.dev/evaluating-rag-pipelines-with-ragas-langsmith/)和一个非常有前景的新框架，[RAGAS](https://blog.langchain.dev/evaluating-rag-pipelines-with-ragas-langsmith/)。
