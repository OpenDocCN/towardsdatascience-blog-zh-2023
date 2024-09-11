# LangChain 增加了 Cypher 搜索功能

> 原文：[https://towardsdatascience.com/langchain-has-added-cypher-search-cb9d821120d5?source=collection_archive---------1-----------------------#2023-05-24](https://towardsdatascience.com/langchain-has-added-cypher-search-cb9d821120d5?source=collection_archive---------1-----------------------#2023-05-24)

## 使用 LangChain 库，你可以方便地生成 Cypher 查询，从而高效地从 Neo4j 中检索信息。

[](https://bratanic-tomaz.medium.com/?source=post_page-----cb9d821120d5--------------------------------)[![Tomaz Bratanic](../Images/d5821aa70918fcb3fc1ff0013497b3d5.png)](https://bratanic-tomaz.medium.com/?source=post_page-----cb9d821120d5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cb9d821120d5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cb9d821120d5--------------------------------) [Tomaz Bratanic](https://bratanic-tomaz.medium.com/?source=post_page-----cb9d821120d5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F57f13c0ea39a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flangchain-has-added-cypher-search-cb9d821120d5&user=Tomaz+Bratanic&userId=57f13c0ea39a&source=post_page-57f13c0ea39a----cb9d821120d5---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cb9d821120d5--------------------------------) ·8 分钟阅读·2023年5月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcb9d821120d5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flangchain-has-added-cypher-search-cb9d821120d5&user=Tomaz+Bratanic&userId=57f13c0ea39a&source=-----cb9d821120d5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcb9d821120d5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flangchain-has-added-cypher-search-cb9d821120d5&source=-----cb9d821120d5---------------------bookmark_footer-----------)![](../Images/4a327bfd5a51c910e0ac40a24bbff57d.png)

图像由 Midjourney 付费订阅生成。 [Midjourney 服务条款](https://docs.midjourney.com/docs/terms-of-service)。

如果你开发了或计划实施任何使用大型语言模型的解决方案，你很可能听说过 [LangChain 库](https://python.langchain.com/en/latest/index.html)。 LangChain 库是最广泛使用的 Python 库，用于开发在不同能力下使用 LLM 的应用程序。它设计为模块化的，允许我们在任何可用模块中使用任何 LLM，例如链、工具、内存或代理。

一个月前，我花了一周时间研究并实现了一种解决方案，使任何人都可以直接从[Neo4j](https://neo4j.com/)数据库中检索信息，并在他们的LLM应用程序中使用。我了解了很多关于LangChain库内部的知识，并在[博客文章](/integrating-neo4j-into-the-langchain-ecosystem-df0e988344d2)中记录了我的经历。

我的一个同事向我展示了一个LangChain的功能请求，用户请求将我实现的从Neo4j数据库中检索信息的选项作为一个模块直接添加到LangChain库中，以便不需要额外的代码或外部模块就能将Neo4j集成到LangChain应用中。由于我已经熟悉LangChain的内部工作原理，我决定自己尝试实现Cypher搜索功能。我花了一个周末研究和编码解决方案，并确保其符合贡献标准以便添加到库中。幸运的是，LangChain的维护者们非常响应并且开放新想法，Cypher搜索功能已经在LangChain库的最新版本中添加了。感谢[Harrison Chase](https://medium.com/u/bbfa018ac706?source=post_page-----cb9d821120d5--------------------------------)维护如此出色的库，并且对新想法非常响应。

在这篇博客文章中，我将展示如何使用LangChain库中新添加的Cypher搜索功能从Neo4j数据库中检索信息。

代码可在[GitHub](https://github.com/tomasonjo/blogs/blob/master/llm/langchain_neo4j.ipynb)上获取。

## 知识图谱是什么

LangChain已经与Vector和SQL数据库集成，那么为什么我们还需要与像Neo4j这样的图形数据库集成呢？

![](../Images/fc7b909b07a0cfe91479086bff8ccf91.png)

[WikiData知识图谱。图片许可协议：CC BY-SA 4.0。](https://www.wikidata.org/wiki/Q33002955#/media/File:Wikidata_knowledge_graph_-_Christine_Choy.png)

知识图谱非常适合存储异质且高度关联的数据。例如，上面的图像包含了关于人物、组织、电影、网站等的信息。虽然能够直观地建模和存储多样化的数据集非常惊人，但我认为使用图谱的主要好处在于通过其关系分析数据点的能力。图谱使我们能够发现那些在传统数据库和分析方法中可能会被忽视的联系和关联，因为这些方法常常忽视个别数据点的上下文。

当处理复杂系统时，图形数据库的强大之处真正显现出来，因为相互依赖和互动对于理解系统至关重要。

这些功能使我们能够超越单个数据点，深入探讨定义其上下文的复杂关系。这提供了数据的更深层次、更全面的视图，有助于更好的决策和知识发现。

## 设置 Neo4j 环境

如果你有一个现有的 Neo4j 数据库，你可以用它来尝试新添加的 Cypher 查询模块。Cypher 查询模块使用图谱模式信息生成 Cypher 语句，意味着你可以将其插件化到任何 Neo4j 数据库中。

如果你还没有 Neo4j 数据库，你可以使用 [Neo4j Sandbox](https://neo4j.com/sandbox/)，它提供了一个免费的 Neo4j 数据库云实例。你需要注册并实例化任何一个现有的预填充数据库。在这篇博客中，我将使用 [**ICIJ Paradise Papers** 数据集](https://sandbox.neo4j.com/?usecase=icij-paradise-papers)，但如果你愿意，可以使用其他任何数据集。该数据集由 [国际调查记者联盟](https://www.icij.org/) 提供，作为他们 [离岸泄露数据库](https://offshoreleaks.icij.org/pages/database) 的一部分。

![](../Images/89fed8d3e032975331f4a222209f0fc5.png)

Paradise Papers 数据集图谱模式。图片由作者提供。

图谱包含四种类型的节点：

+   `**Entity**` - 离岸法律实体。它可以是公司、信托、基金会或其他在低税区创建的法律实体。

+   `**Officer**` - 在离岸实体中扮演角色的个人或公司，例如受益人、董事或股东。图中的关系只是所有现有关系的一部分。

+   `**Intermediary**` - 寻找离岸公司和离岸服务提供商之间的中介——通常是一个律师事务所或要求离岸服务提供商创建离岸公司的中介。

+   `**Address**` - 在 ICIJ 获取的原始数据库中出现的注册地址。

## 知识图谱 Cypher 查询

Cypher 查询的名称源自 Cypher，它是一种用于与像 Neo4j 这样的图数据库互动的查询语言。

![](../Images/7e9d4ec8b6a7974e5904bc5e443260f2.png)

知识图谱 Cypher 链工作流。图片由作者提供。

为了让 LangChain 从图数据库中检索信息，我实现了一个可以将自然语言转换为 Cypher 语句的模块，使用它从 Neo4j 中检索数据，并以自然语言形式将检索到的信息返回给用户。这种自然语言和数据库语言之间的双向转换过程不仅提高了数据检索的整体可访问性，还大大改善了用户体验。

LangChain 库的美在于其简洁性。我们只需几行代码即可使用自然语言从 Neo4j 中检索信息。

```py
from langchain.chat_models import ChatOpenAI
from langchain.chains import GraphCypherQAChain
from langchain.graphs import Neo4jGraph

graph = Neo4jGraph(
    url="bolt://54.172.172.36:7687", 
    username="neo4j", 
    password="steel-foreheads-examples"
)

chain = GraphCypherQAChain.from_llm(
    ChatOpenAI(temperature=0), graph=graph, verbose=True,
)
```

在这里，我们使用**gpt-3.5-turbo**模型来生成Cypher语句。Cypher语句是根据图形模式生成的，这意味着理论上，你可以将Cypher链插入任何Neo4j实例，它应该能够回答自然语言问题。不幸的是，我还没有测试其他LLM提供商生成Cypher语句的能力，因为我没有任何它们的访问权限。尽管如此，如果你愿意尝试，我很想听听你对其他LLM生成Cypher语句的评估。当然，如果你想摆脱对LLM云提供商的依赖，你可以[随时微调一个开源LLM以生成Cypher语句](/fine-tuning-an-llm-model-with-h2o-llm-studio-to-generate-cypher-statements-3f34822ad5)。

让我们从一个简单的测试开始。

```py
chain.run("""
Which intermediary is connected to most entites?
""")
```

*结果*

![](../Images/a7015803caa34f67ea96a440067ce01a.png)

生成的答案。图片由作者提供。

我们可以观察生成的Cypher语句和从Neo4j中检索的信息，这些信息用来形成答案。这是最简单的设置了。让我们继续下一个例子。

```py
chain.run("""
Who are the officers of ZZZ-MILI COMPANY LTD.?
""")
```

*结果*

![](../Images/ea7c4cfdbe32c1f36026cff5bd7c9093.png)

生成的答案。图片由作者提供。

由于我们使用的是图形，让我们构造一个可以利用图形数据库优势的问题。

```py
chain.run("""
How are entities SOUTHWEST LAND DEVELOPMENT LTD. and 
Dragon Capital Markets Limited related?
""")
# Generated Cypher statement
# MATCH (e1:Entity {name: 'SOUTHWEST LAND DEVELOPMENT LTD.'})-
# [:CONNECTED_TO|OFFICER_OF|INTERMEDIARY_OF|SAME_NAME_AS*]-(e2:Entity {name: 'Dragon Capital Markets Limited'})
```

生成的Cypher语句乍看之下没有问题。然而，存在一个问题，因为Cypher语句使用了可变长度路径查找语法，并且将关系视为无向的。因此，这种类型的查询高度未优化，会导致行数爆炸。

**gpt-3.5-turbo**的一个优点是它遵循我们在输入中留下的提示和指示。例如，我们可以要求它仅查找最短路径。

```py
chain.run("""
How are entities SOUTHWEST LAND DEVELOPMENT LTD. and 
Dragon Capital Markets Limited connected?
Find a shortest path.
""")
```

![](../Images/ec99f25f8d358965e67cc7269b1b4073.png)

生成的答案。图片由作者提供。

现在我们给出了一个只应检索最短路径的提示，我们不再遇到基数爆炸的问题。然而，我注意到的一件事是，如果返回的是路径对象，LLM有时不会提供最佳结果。生成的Cypher语句在Neo4j浏览器中返回以下可视化结果。

![](../Images/2d1dcdd2ce07e0f8564e60101dec422e.png)

答案的图形可视化。图片由作者提供。

生成的自然语言响应并没有真正提到这两家公司注册在同一个地址，而是根据节点属性生成了自己的最短路径。然而，我们也可以通过指导模型使用哪些信息来解决这个问题。

```py
chain.run("""
How are entities SOUTHWEST LAND DEVELOPMENT LTD. and Dragon Capital Markets Limited connected?
Find a shortest path.
Return only name properties of nodes and relationship types
""")
```

*结果*

![](../Images/9c225b05215012338afdc382473c497c.png)

生成的答案。图片由作者提供。

现在我们可以得到更好的响应和更合适的响应。你给LLM的提示越多，期望得到的结果就越好。例如，你还可以指导它可以遍历哪些关系。

```py
chain.run("""
How are entities SOUTHWEST LAND DEVELOPMENT LTD. and Dragon Capital Markets Limited connected?
Find a shortest path and use only officer, intermediary, and connected relationships.
Return only name properties of nodes and relationship types
""")
```

*结果*

![](../Images/510392f2207ab79a80e1507e1bb32378.png)

生成的答案。图片由作者提供。

你可以看到，生成的Cypher语句仅允许遍历**OFFICER_OF**、**INTERMEDIARY_OF**和**CONNECTED_TO**关系。相同的Cypher语句生成了以下图形化展示。

![](../Images/78a18b1af5a6fc1f286c787361feae4e.png)

答案的图形化展示。图像由作者提供。

## 总结

图形数据库是检索或分析各种实体之间的连接（如人员和组织）的绝佳工具。在这篇博客文章中，我们查看了一个简单的最短路径用例，其中关系数量和关系类型的顺序事先是未知的。这些类型的查询在向量数据库中几乎是不可能的，并且在SQL数据库中也可能相当复杂。

我对将Cypher Search添加到LangChain库中感到非常兴奋。请试用一下，并告诉我它对你是否有效，特别是如果你在其他LLM模型上进行测试或有有趣的用例。同时，记得订阅，因为我还有几篇博客文章将探讨LangChain库中的Cypher Search。

一如既往，代码可以在[GitHub](https://github.com/tomasonjo/blogs/blob/master/llm/langchain_neo4j.ipynb)上找到。
