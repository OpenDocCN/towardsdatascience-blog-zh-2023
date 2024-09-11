# 在Neo4j中高效的语义搜索

> 原文：[https://towardsdatascience.com/efficient-semantic-search-over-unstructured-text-in-neo4j-8179ad7ff451?source=collection_archive---------1-----------------------#2023-08-23](https://towardsdatascience.com/efficient-semantic-search-over-unstructured-text-in-neo4j-8179ad7ff451?source=collection_archive---------1-----------------------#2023-08-23)

## 将新添加的向量索引集成到LangChain中，以提升你的RAG应用程序

[](https://bratanic-tomaz.medium.com/?source=post_page-----8179ad7ff451--------------------------------)[![Tomaz Bratanic](../Images/d5821aa70918fcb3fc1ff0013497b3d5.png)](https://bratanic-tomaz.medium.com/?source=post_page-----8179ad7ff451--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8179ad7ff451--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8179ad7ff451--------------------------------) [Tomaz Bratanic](https://bratanic-tomaz.medium.com/?source=post_page-----8179ad7ff451--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F57f13c0ea39a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-semantic-search-over-unstructured-text-in-neo4j-8179ad7ff451&user=Tomaz+Bratanic&userId=57f13c0ea39a&source=post_page-57f13c0ea39a----8179ad7ff451---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8179ad7ff451--------------------------------) ·7分钟阅读·2023年8月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8179ad7ff451&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-semantic-search-over-unstructured-text-in-neo4j-8179ad7ff451&user=Tomaz+Bratanic&userId=57f13c0ea39a&source=-----8179ad7ff451---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8179ad7ff451&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-semantic-search-over-unstructured-text-in-neo4j-8179ad7ff451&source=-----8179ad7ff451---------------------bookmark_footer-----------)

自从六个月前ChatGPT问世以来，技术领域经历了变革性的转变。ChatGPT出色的概括能力减少了对专业深度学习团队和大量训练数据集的需求，从而使得创建定制的NLP模型变得更加容易。这使得诸如摘要和信息提取等NLP任务的获取变得比以往任何时候都更加容易。然而，我们很快意识到[类似ChatGPT模型的局限性](https://medium.com/neo4j/knowledge-graphs-llms-fine-tuning-vs-retrieval-augmented-generation-30e875d63a35)，如知识截止日期和无法访问私人信息。在我看来，紧接着出现的是生成性AI转型的第二波浪潮，即检索增强生成（RAG）应用的兴起，你可以在查询时向模型提供相关信息，以构建更好、更准确的答案。

![](../Images/83f907ab327cbc66de48a587422a5183.png)

RAG应用流程。图像由作者提供。图标来自[https://www.flaticon.com/](https://www.flaticon.com/)

如前所述，RAG应用程序需要一个智能搜索工具，能够根据用户输入检索额外信息，这使得LLMs能够生成更准确和最新的答案。最初，重点主要是使用语义搜索从非结构化文本中检索信息。然而，很快就明显看出，结构化和非结构化数据的组合是RAG应用程序的最佳方法，如果你想要[超越“与PDF对话”应用](https://medium.com/neo4j/knowledge-graphs-llms-real-time-graph-analytics-89b392eaaa95)。

Neo4j曾经并且现在仍然非常适合处理结构化信息，但由于其粗暴的方法，它在语义搜索方面有些吃力。然而，这种困难已经成为过去，因为Neo4j已经在[5.11版本中引入了新的向量索引](https://neo4j.com/blog/vector-search-deeper-insights/)，旨在高效地对非结构化文本或其他嵌入数据模式执行语义搜索。新添加的向量索引使Neo4j非常适合大多数RAG应用，因为它现在可以很好地处理结构化和非结构化数据。

在这篇博客文章中，我将向你展示如何在Neo4j中设置向量索引，并将其集成到[LangChain生态系统](https://www.langchain.com/)中。代码可以在[GitHub](https://github.com/tomasonjo/blogs/blob/master/llm/Neo4j_vector_index_%26_Langchain.ipynb)上找到。

## Neo4j环境设置

你需要设置Neo4j 5.11或更高版本，以跟随本博客中的示例。最简单的方法是启动一个免费的[Neo4j Aura](https://neo4j.com/cloud/platform/aura-graph-database/)实例，该平台提供Neo4j数据库的云实例。或者，你也可以通过下载[Neo4j Desktop](https://neo4j.com/download/)应用程序并创建一个本地数据库实例，来设置Neo4j数据库的本地实例。

实例化 Neo4j 数据库后，你可以使用 LangChain 库连接到它。

```py
from langchain.graphs import Neo4jGraph

NEO4J_URI="neo4j+s://1234.databases.neo4j.io"
NEO4J_USERNAME="neo4j"
NEO4J_PASSWORD="-"

graph = Neo4jGraph(
    url=NEO4J_URI,
    username=NEO4J_USERNAME,
    password=NEO4J_PASSWORD
)
```

## 设置向量索引

[Neo4j 向量索引由 Lucene 提供支持](https://neo4j.com/docs/cypher-manual/current/indexes-for-vector-search/)，其中 Lucene 实现了一个层次导航的小世界 (HNSW) 图，以在向量空间上执行近似最近邻 (ANN) 查询。

Neo4j 对向量索引的实现旨在索引节点标签的单个节点属性。例如，如果你想要对标签为 `Chunk` 的节点的 `embedding` 属性进行索引，你可以使用以下 Cypher 程序。

```py
CALL db.index.vector.createNodeIndex(
  'wikipedia', // index name
  'Chunk',     // node label
  'embedding', // node property
   1536,       // vector size
   'cosine'    // similarity metric
)
```

除了索引名称、节点标签和属性外，你还必须指定向量大小（嵌入维度）和相似度度量。我们将使用 OpenAI 的 text-embedding-ada-002 嵌入模型，该模型使用向量大小 **1536** 来表示嵌入空间中的文本。目前，仅提供 **余弦** 和 **欧几里得** 相似度度量。OpenAI 建议在使用其嵌入模型时使用余弦相似度度量。

## 填充向量索引

Neo4j 的设计是无模式的，这意味着它不强制对节点属性中的内容施加任何限制。例如，`embedding`属性可以存储整数、整数列表甚至字符串。我们来尝试一下。

```py
WITH [1, [1,2,3], ["2","5"], [x in range(0, 1535) | toFloat(x)]] AS exampleValues
UNWIND range(0, size(exampleValues) - 1) as index
CREATE (:Chunk {embedding: exampleValues[index], index: index})
```

这个查询为列表中的每个元素创建一个 `Chunk` 节点，并将元素用作 `embedding` 属性值。例如，第一个 `Chunk` 节点的 `embedding` 属性值为 **1**，第二个节点为 **[1,2,3]**，依此类推。Neo4j 对节点属性下可以存储的内容没有强制规则。然而，向量索引对它应该索引的值类型和嵌入维度有明确的指示。

我们可以通过执行向量索引搜索来测试哪些值已被索引。

```py
CALL db.index.vector.queryNodes(
  'wikipedia', // index name
   3, // topK neighbors to return
   [x in range(0,1535) | toFloat(x) / 2] // input vector
)
YIELD node, score
RETURN node.index AS index, score
```

如果你运行这个查询，你将只返回一个单独的节点，即使你请求返回前 **3** 个邻居。这是为什么呢？向量索引仅索引属性值，其中值是具有指定大小的浮点数列表。在这个示例中，只有一个 `embedding` 属性值具有浮点数列表类型，并且长度为 1536。

如果满足以下所有条件，则节点会按向量索引进行索引：

+   节点包含配置的标签。

+   节点包含配置的属性键。

+   相应的属性值类型为 `LIST<FLOAT>`。

+   相应值的 `[size()](https://neo4j.com/docs/cypher-manual/current/functions/scalar/#functions-size)` 与配置的维度是相同的。

+   该值是配置的相似度函数的有效向量。

## 将向量索引集成到 LangChain 生态系统中

现在我们将实现一个简单的自定义 LangChain 类，该类将使用 Neo4j 向量索引来检索相关信息，以生成准确和最新的答案。但首先，我们必须填充向量索引。

![](../Images/c74118be178b79f9f3609665c6658ed2.png)

使用Neo4j向量索引在RAG应用中的数据流。图像由作者提供。图标来自flaticons。

任务将包括以下步骤：

+   检索一篇维基百科文章

+   切分文本

+   在Neo4j中存储文本及其向量表示

+   实现一个自定义LangChain类以支持RAG应用

在这个例子中，我们将只获取一篇维基百科文章。我决定使用[Baldur’s Gate 3页面](https://en.wikipedia.org/wiki/Baldur%27s_Gate_3)。

```py
import wikipedia
bg3 = wikipedia.page(pageid=60979422)
```

接下来，我们需要对文本进行切分和嵌入。我们将通过双换行符分隔符按部分切分文本，然后使用OpenAI的嵌入模型为每个部分表示一个合适的向量。

```py
import os
from langchain.embeddings import OpenAIEmbeddings

os.environ["OPENAI_API_KEY"] = "API_KEY"

embeddings = OpenAIEmbeddings()

chunks = [{'text':el, 'embedding': embeddings.embed_query(el)} for
                  el in bg3.content.split("\n\n") if len(el) > 50]
```

在我们继续讲解LangChain类之前，需要将文本块导入Neo4j。

```py
graph.query("""
UNWIND $data AS row
CREATE (c:Chunk {text: row.text})
WITH c, row
CALL db.create.setVectorProperty(c, 'embedding', row.embedding)
YIELD node
RETURN distinct 'done'
""", {'data': chunks})
```

有一点值得注意的是，我使用了`db.create.setVectorProperty`过程来将向量存储到Neo4j中。此过程用于验证属性值确实是浮点数列表。此外，它还有助于将向量属性的存储空间减少约50%。因此，建议始终使用此过程将向量存储到Neo4j中。

现在我们可以实现自定义的LangChain类，用于从Neo4j向量索引中检索信息，并用它生成答案。首先，我们将定义用于检索信息的Cypher语句。

```py
vector_search = """
WITH $embedding AS e
CALL db.index.vector.queryNodes('wikipedia',$k, e) yield node, score
RETURN node.text AS result
"""
```

如您所见，我已硬编码了索引名称。如果您愿意，可以通过添加适当的参数使其动态化。

自定义的LangChain类实现得非常直接。

```py
class Neo4jVectorChain(Chain):
    """Chain for question-answering against a Neo4j vector index."""

    graph: Neo4jGraph = Field(exclude=True)
    input_key: str = "query"  #: :meta private:
    output_key: str = "result"  #: :meta private:
    embeddings: OpenAIEmbeddings = OpenAIEmbeddings()
    qa_chain: LLMChain = LLMChain(llm=ChatOpenAI(temperature=0), prompt=CHAT_PROMPT)

    def _call(self, inputs: Dict[str, str], run_manager, k=3) -> Dict[str, Any]:
        """Embed a question and do vector search."""
        question = inputs[self.input_key]

        # Embed the question
        embedding = self.embeddings.embed_query(question)

        # Retrieve relevant information from the vector index
        context = self.graph.query(
            vector_search, {'embedding': embedding, 'k': 3})
        context = [el['result'] for el in context]

        # Generate the answer
        result = self.qa_chain(
            {"question": question, "context": context},
        )
        final_result = result[self.qa_chain.output_key]
        return {self.output_key: final_result}
```

我省略了一些模板代码以使其更易读。实质上，当您调用Neo4jVectorChain时，会执行以下步骤：

1.  使用相关的嵌入模型对问题进行嵌入

1.  使用文本嵌入值从向量索引中检索最相似的内容

1.  使用类似内容提供的上下文生成答案

我们现在可以测试我们的实现。

```py
vector_qa = Neo4jVectorChain(graph=graph, embeddings=embeddings, verbose=True)
vector_qa.run("What is the gameplay of Baldur's Gate 3 like?")
```

*响应*

![](../Images/21517a69c90aea570d61228bfebc4243.png)

生成的响应。图像由作者提供。

通过使用`verbose`选项，您还可以评估从向量索引中检索的上下文，这些上下文用于生成答案。

## 总结

利用Neo4j的新向量索引功能，您可以创建一个统一的数据源，有效支持检索增强生成应用。这不仅使您能够实现“与PDF或文档聊天”解决方案，还能进行实时分析，所有这些都来自一个强大的数据源。这种多用途的实用工具可以简化您的操作并增强数据协同，使Neo4j成为管理结构化和非结构化数据的绝佳解决方案。

一如既往，代码可在[GitHub](https://github.com/tomasonjo/blogs/blob/master/llm/Neo4j_vector_index_%26_Langchain.ipynb)上找到。
