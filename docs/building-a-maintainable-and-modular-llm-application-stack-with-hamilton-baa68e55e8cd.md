# 用 Hamilton 在 13 分钟内构建一个可维护且模块化的 LLM 应用堆栈

> 原文：[https://towardsdatascience.com/building-a-maintainable-and-modular-llm-application-stack-with-hamilton-baa68e55e8cd?source=collection_archive---------10-----------------------#2023-07-13](https://towardsdatascience.com/building-a-maintainable-and-modular-llm-application-stack-with-hamilton-baa68e55e8cd?source=collection_archive---------10-----------------------#2023-07-13)

## LLM 应用是数据流，使用专门设计的工具来表达它们

[](https://medium.com/@stefan.krawczyk?source=post_page-----baa68e55e8cd--------------------------------)[![Stefan Krawczyk](../Images/150405abaad9590e1dc2589168ed2fa3.png)](https://medium.com/@stefan.krawczyk?source=post_page-----baa68e55e8cd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----baa68e55e8cd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----baa68e55e8cd--------------------------------) [Stefan Krawczyk](https://medium.com/@stefan.krawczyk?source=post_page-----baa68e55e8cd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F193628e26f00&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-maintainable-and-modular-llm-application-stack-with-hamilton-baa68e55e8cd&user=Stefan+Krawczyk&userId=193628e26f00&source=post_page-193628e26f00----baa68e55e8cd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----baa68e55e8cd--------------------------------) ·13 分钟阅读·2023年7月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbaa68e55e8cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-maintainable-and-modular-llm-application-stack-with-hamilton-baa68e55e8cd&user=Stefan+Krawczyk&userId=193628e26f00&source=-----baa68e55e8cd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbaa68e55e8cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-maintainable-and-modular-llm-application-stack-with-hamilton-baa68e55e8cd&source=-----baa68e55e8cd---------------------bookmark_footer-----------)![](../Images/84bcb5f1c47c2c34ce7c753f621da5a8.png)

LLM 堆栈。使用合适的工具，如 Hamilton，可以确保你的堆栈不会变得难以维护和管理。图片来源于 [pixabay](https://pixabay.com/photos/books-stack-book-store-1163695/)。

*此文章与* [*Thierry Jean*](https://medium.com/u/cf12dc7f8440) *合作撰写，最初发布于* [*此处*](https://blog.dagworks.io/p/building-a-maintainable-and-modular)。

在这篇文章中，我们将分享如何使用[Hamilton](https://github.com/dagWorks-Inc/hamilton)这一开源框架来编写模块化和可维护的代码，以支持你的大型语言模型（LLM）应用程序堆栈。Hamilton 非常适合描述任何类型的[数据流](https://en.wikipedia.org/wiki/Dataflow)，这正是你在构建 LLM 驱动应用程序时所做的。通过 Hamilton，你可以获得强大的[软件维护人机工程学](https://ceur-ws.org/Vol-3306/paper5.pdf)，同时还能轻松地交换和评估应用程序组件的不同提供者/实现。免责声明：我是 Hamilton 包的作者之一。

我们将演示的示例将镜像你用于填充向量数据库的典型 LLM 应用程序工作流程。具体来说，我们将涵盖从网络中提取数据、创建文本嵌入（向量）并将其推送到向量存储中。

![](../Images/3af6ce8a4bc5b6f967b1e28e6085bcc0.png)

堆栈概述。作者提供的图像。

# LLM 应用程序数据流

首先，让我们描述一下典型的 LLM 数据流的组成。应用程序将接收一个小的数据输入（例如文本、命令），并在更大的上下文中进行操作（例如聊天记录、文档、状态）。这些数据将通过不同的服务（LLM、向量数据库、文档存储等）进行操作、生成新的数据工件，并返回最终结果。大多数用例会在迭代不同输入的过程中重复这一流程多次。

一些常见的操作包括：

+   将文本转换为嵌入

+   存储 / 搜索 / 检索嵌入

+   查找嵌入的最近邻

+   检索用于嵌入的文本

+   确定传递到提示中的上下文

+   使用相关文本中的上下文提示模型

+   将结果发送到其他服务（API、数据库等）

+   …

+   并将它们串联起来！

现在，让我们在生产环境中深入探讨上述内容，假设用户对你的应用程序的输出不满意，并且你想找到问题的根源。你的应用程序记录了提示和结果。你的代码允许你找出操作的顺序。然而，你不知道问题出在哪里，系统产生了不理想的输出……为了解决这个问题，我们认为跟踪数据工件及生成它们的代码是关键，这样你才能快速调试类似的情况。

由于许多操作是非确定性的，这增加了你的LLM应用程序数据流的复杂性，这意味着你不能重新运行或逆向工程操作以重现中间结果。例如，即使你拥有相同的输入和配置，生成文本或图像响应的API调用可能也是不可重复的（你*可以*通过如[temperature](https://platform.openai.com/docs/api-reference/chat/create#chat/create-temperature)这样的选项*缓解部分问题*）。这也扩展到某些向量数据库操作，如“查找最近”——其结果取决于数据库中当前存储的对象。在生产环境中，快照数据库状态以使调用可重复几乎是不现实的。

基于这些原因，采用灵活的工具以创建稳健的数据流很重要，这样可以让你：

1.  轻松地插入各种组件。

1.  了解组件之间如何连接。

1.  添加和定制常见的生产需求，如缓存、验证和可观察性。

1.  根据你的需求调整流结构，而不需要强大的工程技能

1.  插件集成到传统的数据处理和机器学习生态系统中。

在这篇文章中，我们将概述Hamilton如何满足第1、2和4点。有关第3和5点的信息，请参阅我们的[文档](https://hamilton.dagworks.io/en/latest/)。

# 当前的LLM应用程序开发工具

LLM领域仍处于起步阶段，使用模式和工具正在快速演变。虽然LLM框架可以让你入门，但当前的选项并未经过生产环境测试；据我们了解，目前没有成熟的科技公司在生产中使用当前流行的LLM框架。

别误解我们的意思，有些工具确实非常适合快速建立概念验证！然而，我们认为它们在两个特定领域存在不足：

**1\. 如何建模LLM应用程序的数据流。** 我们强烈认为“动作”的数据流建模更适合用函数来表示，而不是通过面向对象的类和生命周期。函数更容易推理、测试和更改。面向对象的类可能变得相当晦涩，并带来更多的思维负担。

> 当出现错误时，面向对象的框架需要你深入到对象的源代码中以理解它。而使用Hamilton函数时，清晰的依赖关系谱能告诉你在哪里查找，并帮助你推理发生了什么（更多信息请见下文）！

**2\. 定制/扩展。** 不幸的是，一旦你超出框架提供的“简单”功能，你需要强大的软件工程技能来修改当前框架。如果这不是一个选项，这意味着你可能会在特定的自定义业务逻辑上脱离框架，这可能会导致你维护更多的代码面积，而不是如果你一开始就不使用框架的话。

关于这两点的更多信息，我们推荐你查看这些讨论线程（[hacker news](https://news.ycombinator.com/item?id=36645575#36647985), [reddit](https://old.reddit.com/r/LangChain/comments/13fcw36/langchain_is_pointless/)），其中有用户详细讨论。

虽然Hamilton并不是当前LLM框架的完整替代品（例如，没有“代理”组件），但它*确实*拥有满足LLM应用程序需求的所有构建模块，并且两者可以协同工作。如果你想要一种干净、清晰且可定制的方式来编写生产代码、集成多个LLM技术栈组件，并对你的应用程序进行观察，那么让我们继续进入接下来的几个部分吧！

# 使用Hamilton构建

Hamilton是一个声明式微框架，用于在Python中描述[数据流](https://en.wikipedia.org/wiki/Dataflow)。它不是一个新框架（已有3.5年以上历史），并且在生产建模数据和机器学习数据流中使用多年。它的优势在于以一种直观易创建和维护的方式表达数据和计算流（类似于DBT对SQL的作用），这非常适合支持建模LLM应用程序的数据和计算需求。

![](../Images/ac83c0914a4533f8f7429b752d511120.png)

Hamilton范式的示意图。与其使用过程性赋值，不如将其建模为一个函数。函数名称是你可以获得的“输出”，而函数输入参数声明了计算所需的依赖关系。图片由作者提供。

Hamilton的基础知识很简单，而且可以通过多种方式扩展；你不必了解Hamilton就能从这篇文章中获得价值，但如果你感兴趣，可以查看：

+   [tryhamilton.dev](https://www.tryhamilton.dev/) – 在浏览器中的互动教程！

+   [在5分钟内使用Hamilton进行Pandas数据转换](/how-to-use-hamilton-with-pandas-in-5-minutes-89f63e5af8f5)

+   [10分钟内了解Lineage + Hamilton](https://blog.dagworks.io/p/lineage-hamilton-in-10-minutes-c2b8a944e2e6)

+   [Hamilton + Airflow用于生产](https://blog.dagworks.io/publish/post/130538397)

# 进入我们的示例

为了帮助建立一些心理背景，想象一下。你是一个小型数据团队，负责创建一个LLM应用程序，与组织的文档进行“聊天”。你认为评估候选架构在功能、性能配置、许可证、基础设施要求和成本方面是很重要的。最终，你知道你组织的主要关注点是提供最相关的结果和良好的用户体验。评估这些的最佳方法是构建一个原型，测试不同的技术栈，并比较它们的特性和输出。然后，当你过渡到生产环境时，你会希望确保系统能够轻松维护和检查，以始终提供优质的用户体验。

有鉴于此，在这个示例中，我们将实现 LLM 应用程序的一部分，特别是数据摄取步骤，用于索引知识库，其中我们将文本转换为嵌入并存储在向量数据库中。我们使用几种不同的服务/技术以模块化的方式实现这一点。广泛的步骤包括：

1.  从 HuggingFace Hub 加载 [SQuAD 数据集](https://huggingface.co/datasets/squad)。你可以将其替换为你的预处理文档的语料库。

1.  使用 [Cohere API](https://docs.cohere.com/reference/embed)、[OpenAI API](https://platform.openai.com/docs/api-reference/embeddings) 或 [SentenceTransformer 库](https://www.sbert.net/examples/applications/computing-embeddings/README.html) 嵌入文本条目。

1.  将嵌入存储在一个向量数据库中，如 [LanceDB](https://lancedb.github.io/lancedb/)、[Pinecone](https://docs.pinecone.io/docs/overview) 或 [Weaviate](https://weaviate.io/developers/weaviate)。

如果你需要了解更多关于嵌入和搜索的信息，我们推荐以下链接：

+   [文本嵌入解释 - Weaviate](https://weaviate.io/blog/vector-embeddings-explained)

+   [如何使用 Pinecone 进行语义搜索](https://docs.pinecone.io/docs/semantic-text-search)

在我们讲解这个示例时，考虑以下几点会对你有帮助：

+   **将我们展示的内容与当前做的事情进行比较。** 看到 Hamilton 如何使你能够策划和结构化一个项目，而无需明确的 LLM 重点框架。

+   **项目和应用结构。** 了解 Hamilton 如何强制执行一种结构，使你能够构建和维护模块化堆栈。

+   **迭代中的信心和项目的持久性。** 结合上述两点，Hamilton 使你能够更轻松地维护生产中的 LLM 应用程序，无论它的作者是谁。

让我们从一个可视化开始，以便你能对我们谈论的内容有一个概览：

![](../Images/149c01e23dbcb8ef11df1a74517ca8f3.png)

Hamilton DAG 可视化 Pinecone + 句子变换器堆栈。图片由作者提供。

当使用 pinecone 和句子变换器时，LLM 应用程序的数据流将如下所示。借助 Hamilton，了解事物的连接就像在 [Hamilton 驱动程序对象](https://hamilton.dagworks.io/en/latest/reference/drivers/Driver/#hamilton.driver.Driver.display_all_functions)上调用`display_all_functions()`一样简单。

# 模块化代码

让我们解释一下使用 Hamilton 实现模块化代码的两种主要方式，以我们的示例为背景。

# @config.when

Hamilton 关注可读性。虽然没有解释`@config.when`的作用，你可能已经可以判断这是一个条件语句，且仅在满足条件时*包括*。下面你将找到使用 OpenAI 和 Cohere API 将文本转换为嵌入的实现。

Hamilton 将识别两个函数作为替代实现，因为`@config.when`装饰器和相同的函数名称`embeddings`位于双下划线（`__cohere`、`__openai`）之前。它们的函数签名不必完全相同，这意味着采纳不同实现是简单且清晰的。

embedding_module.py

对于这个项目，将所有嵌入服务实现放在同一个文件中并使用`@config.when`装饰器是合理的，因为每个服务只有 3 个函数。然而，随着项目复杂性的增长，函数也可以移动到单独的模块中，并采用下一节的模块化模式。另一个要点是这些函数都是独立可单元测试的。如果你有特定的需求，将其封装到函数中并进行测试是很简单的。

# 更换 Python 模块

下面你将看到 Pinecone 和 Weaviate 的向量数据库操作实现。请注意，这些代码片段来自`pinecone_module.py`和`weaviate_module.py`，并观察函数签名的相似之处和不同之处。

pinecone_module.py 和 weaviate_module.py

使用 Hamilton 时，数据流通过函数名称和函数输入参数连接在一起。因此，通过共享类似操作的函数名称，这两个模块可以轻松互换。由于 LanceDB、Pinecone 和 Weaviate 实现分别存在于不同的模块中，这减少了每个文件的依赖数量，使文件更短，从而提高了可读性和可维护性。每个实现的逻辑都清晰地封装在这些命名函数中，因此针对每个模块进行单元测试是直接可行的。分离的模块强化了它们不应同时加载的概念。当发现多个相同名称的函数时，Hamilton 驱动程序实际上会抛出错误，这有助于加强这一概念。

# 驱动程序的影响

运行 Hamilton 代码的关键部分是`Driver`对象，它在`run.py`中找到。排除 CLI 和一些参数解析的代码，我们得到：

run.py 的代码片段

Hamilton 驱动程序负责协调执行，并且是你通过它操控数据流的工具，通过上述代码片段中看到的三种机制实现了模块化：

1.  **Driver 配置。** 这是一个字典，驱动程序在实例化时接收该字典，包含应该保持不变的信息，例如使用哪个 API 或嵌入服务 API 密钥。这与可以传递 JSON 或字符串的命令平面（例如 Docker 容器、[Airflow](https://blog.dagworks.io/publish/posts/detail/130538397)、[Metaflow](https://outerbounds.com/blog/developing-scalable-feature-engineering-dags/) 等）很好地集成。具体来说，这里是我们指定要更换哪个嵌入 API 的地方。

1.  **驱动程序模块。** 驱动程序可以接收任意数量的独立 Python 模块来构建数据流。在这里，`vector_db_module` 可以替换为我们连接的所需向量数据库实现。还可以通过 [importlib](https://docs.python.org/3/library/importlib.html#importlib.import_module) 动态导入模块，这在开发与生产环境中可能很有用，同时也能实现通过配置驱动的方式来改变数据流实现。

1.  **驱动程序执行。** `final_vars` 参数决定了应该返回什么输出。你无需重构代码来改变想要获得的输出。举个例子，如果你想调试数据流中的某些内容，可以通过将函数的名称添加到 `final_vars` 来请求任何函数的输出。例如，如果你有一些中间输出需要调试，可以很容易地请求它，或者完全在那个点停止执行。请注意，驱动程序在调用 `execute()` 时可以接收 `inputs` 和 `overrides` 值；在上面的代码中，`class_name` 是一个执行时的 `input`，指示我们要创建的嵌入对象以及将其存储在向量数据库中的位置。

# 模块化总结

在 Hamilton 中，使组件可互换的关键是：

1.  定义具有相同名称的函数，然后，

1.  使用 `@config.when` 对它们进行注解，并通过传递给驱动程序的配置选择使用哪一个，或者，

1.  将它们放在不同的 Python 模块中，并将所需的模块传递给驱动程序。

所以我们刚刚展示了如何使用 Hamilton 插件、交换和调用各种 LLM 组件。我们无需解释什么是面向对象的层次结构，也不要求你具备广泛的软件工程经验（我们希望如此！）。为了实现这一点，我们只需匹配函数名称及其输出类型。因此，我们认为这种编写和模块化代码的方式比当前 LLM 框架所允许的更加可访问。

# Hamilton 代码的实际应用

为了支持我们的主张，这里有一些我们观察到的将 Hamilton 代码应用于 LLM 工作流的实际影响：

# CI/CD

模块/`@config.when` 的可互换性也意味着在 CI 系统中的集成测试非常容易思考，因为你可以根据需要灵活地交换或隔离数据流的部分。

# 协作

1.  Hamilton 实现的模块化可以轻松跨团队边界镜像。函数名称及其输出类型成为合同，确保可以进行有针对性的更改并对更改充满信心，还可以通过 Hamilton 的 [可视化和血统功能](https://blog.dagworks.io/p/lineage-hamilton-in-10-minutes-c2b8a944e2e6) 了解下游依赖关系（就像我们看到的初始可视化一样）。例如，如何与向量数据库交互并进行消费就非常清晰。

1.  代码更改更易于审查，因为流程由声明式函数定义。更改是自包含的；由于没有面向对象的层次结构需要学习，只需修改一个函数。任何“自定义”的内容都被Hamilton默认支持。

# 调试

当Hamilton出现错误时，很清楚它映射到的代码是什么，并且由于函数的定义，你知道它在数据流中的位置。

以使用cohere的embeddings函数为简单示例。如果发生超时或解析响应时出错，将清楚地映射到这段代码，并且通过函数定义你会知道它在流程中的位置。

```py
@config.when(embedding_service="cohere")
def embeddings__cohere(
    embedding_provider: cohere.Client,
    text_contents: list[str],
    model_name: str = "embed-english-light-v2.0",
) -> list[np.ndarray]:
    """Convert text to vector representations (embeddings) using Cohere Embed API
    reference: https://docs.cohere.com/reference/embed
    """
    response = embedding_provider.embed(
        texts=text_contents,
        model=model_name,
        truncate="END",
    )
    return [np.asarray(embedding) for embedding in response.embeddings]
```

![](../Images/ed4bd30c315330f5fcec9bd4984202c4.png)

可视化显示`embeddings`在数据流中的位置。图像由作者提供。

# 创建模块化LLM堆栈的技巧

在结束之前，这里有一些想法来指导你构建应用程序。某些决策可能没有明显的最佳选择，但正确的模块化方法将使你能够随着需求的变化高效迭代。

1.  在编写任何代码之前，绘制你的工作流的DAG。这为定义通用步骤和接口奠定了基础，这些步骤和接口不是特定于服务的。

1.  确定可以交换的步骤。通过有目的地设置配置点，你将减少[投机泛化](https://refactoring.guru/smells/speculative-generality)的风险。具体来说，这将导致具有较少参数、默认值且按主题模块分组的函数。

1.  将数据流的部分切分成依赖较少的模块（如有相关）。这将导致更短的Python文件，减少包依赖，提高可读性和可维护性。Hamilton对此不在意，可以从多个模块构建其DAG。

# 结论与未来方向

感谢你阅读到这里。我们相信Hamilton在帮助每个人表达他们的数据流方面有一定作用，而LLM应用程序只是其中一个用例！总结我们在这篇文章中的信息，可以归纳为：

1.  将LLM应用程序视为数据流是有用的，因此非常适合使用Hamilton。

1.  面向对象的LLM框架可能不透明且难以扩展和维护以满足生产需求。相反，应该使用Hamilton简单的声明式风格编写自己的集成。这样可以提高代码的透明度和可维护性，具有清晰的可测试函数、明确的运行时错误映射到函数的方式，以及内置的数据流可视化。

1.  使用Hamilton所规定的模块化将使协作更高效，并为你提供必要的灵活性，以便按照该领域的进展速度修改和更改LLM工作流。

现在邀请你在[这里](https://github.com/DAGWorks-Inc/hamilton/tree/main/examples/LLM_Workflows/modular_llm_stack)玩转、尝试和修改完整的示例。这里有一个`README`文件会解释如何运行命令和开始使用。否则，我们正在思考以下内容来提升 Hamilton + LLM 应用体验：

1.  **代理。** 我们能否为代理提供与常规 Hamilton 数据流相同的可视性？

1.  **并行化。** 我们如何简化在文档列表上运行数据流的表达方式。请参见这个[进行中的 PR](https://github.com/DAGWorks-Inc/hamilton/pull/216)了解我们的意思。

1.  **缓存和可观察性的插件。** 目前已经可以在 Hamilton 上实现自定义的缓存和可观察性解决方案。我们正在致力于为常见组件提供更多的标准选项，例如 redis。

1.  **用户贡献的数据流部分。** 我们看到可以在特定 LLM 应用用例上标准化常见名称的可能性。在这种情况下，我们可以开始聚合 Hamilton 数据流，并允许人们根据自己的需求下载。

# 我们想听听你的意见！

如果你对这些内容感到兴奋，或有强烈的看法，欢迎访问我们的 Slack 频道或在这里留下评论！一些可以帮助你的资源：

📣 加入我们的[Slack](https://hamilton-opensource.slack.com/join/shared_invite/zt-1bjs72asx-wcUTgH7q7QX1igiQ5bbdcg#/shared-invite/email)社区 — 我们很乐意帮助解答你可能遇到的问题或帮助你入门。

⭐️ 在[GitHub](https://github.com/DAGWorks-Inc/hamilton)上给我们点赞

📝 如果你发现了问题，请给我们留下一个[issue](https://github.com/DAGWorks-Inc/hamilton/issues)

你可能感兴趣的其他 Hamilton 文章：

+   [tryhamilton.dev](https://www.tryhamilton.dev/) – 一个在浏览器中进行交互式教程的平台！

+   [5分钟内在 Hamilton 中进行 Pandas 数据转换](https://blog.dagworks.io/p/how-to-use-hamilton-with-pandas-in-5-minutes-89f63e5af8f5)

+   [10分钟内了解 Lineage + Hamilton](https://blog.dagworks.io/p/lineage-hamilton-in-10-minutes-c2b8a944e2e6)

+   [Hamilton + Airflow 生产环境](https://blog.dagworks.io/publish/post/130538397)
