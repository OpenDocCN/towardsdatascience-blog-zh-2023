# 使用Neo4j和APOC将LLM工作流与知识图谱集成

> 原文：[https://towardsdatascience.com/integrate-llm-workflows-with-knowledge-graph-using-neo4j-and-apoc-27ef7e9900a2?source=collection_archive---------0-----------------------#2023-06-07](https://towardsdatascience.com/integrate-llm-workflows-with-knowledge-graph-using-neo4j-and-apoc-27ef7e9900a2?source=collection_archive---------0-----------------------#2023-06-07)

## OpenAI和VertexAI端点现已作为APOC扩展程序提供

[](https://bratanic-tomaz.medium.com/?source=post_page-----27ef7e9900a2--------------------------------)[![Tomaz Bratanic](../Images/d5821aa70918fcb3fc1ff0013497b3d5.png)](https://bratanic-tomaz.medium.com/?source=post_page-----27ef7e9900a2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----27ef7e9900a2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----27ef7e9900a2--------------------------------) [Tomaz Bratanic](https://bratanic-tomaz.medium.com/?source=post_page-----27ef7e9900a2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F57f13c0ea39a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintegrate-llm-workflows-with-knowledge-graph-using-neo4j-and-apoc-27ef7e9900a2&user=Tomaz+Bratanic&userId=57f13c0ea39a&source=post_page-57f13c0ea39a----27ef7e9900a2---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----27ef7e9900a2--------------------------------) ·8 分钟阅读·2023年6月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F27ef7e9900a2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintegrate-llm-workflows-with-knowledge-graph-using-neo4j-and-apoc-27ef7e9900a2&user=Tomaz+Bratanic&userId=57f13c0ea39a&source=-----27ef7e9900a2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F27ef7e9900a2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintegrate-llm-workflows-with-knowledge-graph-using-neo4j-and-apoc-27ef7e9900a2&source=-----27ef7e9900a2---------------------bookmark_footer-----------)

或许每天都有新鲜激动人心的事情发生在大型语言模型（LLM）领域。任何公司都有众多机会和应用场景，可以利用LLM的力量来提升生产力，转化或处理数据，以及用于对话AI和问答系统。

为了让你更容易[将 LLM 与知识图谱集成](https://neo4j.com/generativeai/)，Neo4j 的团队已经开始了添加对 LLM 集成支持的旅程。这些集成作为 APOC 扩展过程提供。目前支持 OpenAI 和 VertexAI 端点，但我们计划添加更多的支持。

当我在头脑风暴展示新添加的 APOC 过程的酷炫用例时，我的朋友[Michael Hunger](https://medium.com/u/3865848842f9?source=post_page-----27ef7e9900a2--------------------------------)提出了一个激动人心的想法。如果我们利用图谱上下文或节点的邻域来丰富文本嵌入中的信息怎么办？这样，由于嵌入信息的丰富性增加，向量相似性搜索可能会产生更好的结果。这个想法简单但引人入胜，可能在许多用例中都很有用。

所有代码都可以在[GitHub](https://github.com/tomasonjo/blogs/blob/master/llm/Neo4jOpenAIApoc.ipynb)上找到。

## Neo4j 环境设置

在这个示例中，我们将同时使用 APOC 和图数据科学库。幸运的是，[Neo4j Sandbox](https://neo4j.com/sandbox/) 项目已经安装了这两个库，并且附带了一个预填充的数据库。因此，你可以通过几次点击设置环境。我们将使用[小型电影项目](https://sandbox.neo4j.com/?usecase=movies)以避免产生更高的 LLM API 成本。

![](../Images/75628fd51fac874fdfaa7a0be250c4b9.png)

电影图谱的示例。图像由作者提供。

数据集包含**电影**和**人物**节点。只有 38 部电影，因此我们处理的是一个微小的数据集。信息提供了电影的标题和标语、发布日期以及演员或导演。

## 构建文本嵌入值

我们将使用[OpenAI API 端点](https://openai.com/)。因此，如果你还没有 OpenAI 账户，你需要创建一个。

如前所述，这个想法是使用节点的邻域来构建其文本嵌入表示。由于图谱模型简单，我们没有太多的创造性自由。我们将通过使用电影的属性和邻居信息来创建电影的文本嵌入表示。在这种情况下，邻居信息仅包括其演员和导演。然而，我相信这个概念可以应用于更复杂的图谱模式，并用于改进你的向量相似性搜索应用。

![](../Images/8c63ba0a64729b916c5864a9117df353.png)

利用邻居节点的信息来丰富中心节点的文本嵌入表示。图像由作者提供。图标来自[flaticon](https://www.flaticon.com/)。

我们现在看到的典型方法是简单地分块并嵌入文档，但当寻找跨越多个文档的信息时，这种方法可能会失败。这种问题也被称为多跳问答。然而，多跳问答问题可以使用知识图谱来解决。看待知识图谱的一种方式是将其视为浓缩的信息存储。例如，可以使用[信息提取管道](https://medium.com/neo4j/creating-a-knowledge-graph-from-video-transcripts-with-gpt-4-52d7c7b9f32c)从各种记录中提取相关信息。使用知识图谱，您可以将跨越多个文档的高度连接的信息表示为各种实体之间的关系。

一种解决方案是使用[LLMs生成Cypher语句，以从数据库中检索连接的信息](/langchain-has-added-cypher-search-cb9d821120d5)。另一种解决方案是使用连接信息来丰富文本嵌入表示。此外，增强的信息可以在查询时检索，以向LLM提供额外的上下文，从而作为其回答的基础。

以下Cypher查询可用于从邻居节点中检索所有相关的电影信息。

```py
MATCH (m:Movie)
MATCH (m)-[r:ACTED_IN|DIRECTED]-(t)
WITH m, type(r) as type, collect(t.name) as names
WITH m, type+": "+reduce(s="", n IN names | s + n + ", ") as types
WITH m, collect(types) as contexts
WITH m, "Movie title: "+ m.title + " year: "+coalesce(m.released,"") +" plot: "+ coalesce(m.tagline,"")+"\n" +
       reduce(s="", c in contexts | s + substring(c, 0, size(c)-2) +"\n") as context
RETURN context LIMIT 1
```

查询返回了以下Matrix电影的上下文。

```py
Movie title: The Matrix year: 1999 plot: Welcome to the Real World
ACTED_IN: Emil Eifrem, Hugo Weaving, Laurence Fishburne, Carrie-Anne Moss, Keanu Reeves
DIRECTED: Lana Wachowski, Lilly Wachowski
```

根据您的领域，您可能还会使用自定义查询来检索多跳之外的信息，或者有时需要聚合一些结果。

我们将使用OpenAI的嵌入端点来生成表示电影及其上下文的文本嵌入，并将其存储为节点属性。

```py
CALL apoc.periodic.iterate(
  'MATCH (m:Movie) RETURN id(m) AS id',
  'MATCH (m:Movie)
   WHERE id(m) = id
   MATCH (m)-[r:ACTED_IN|DIRECTED]-(t)
   WITH m, type(r) as type, collect(t.name) as names
   WITH m, type+": "+reduce(s="", n IN names | s + n + ", ") as types
   WITH m, collect(types) as contexts
   WITH m, "Movie title: "+ m.title + " year: "+coalesce(m.released,"") +" plot: "+ coalesce(m.tagline,"")+"\n" +
        reduce(s="", c in contexts | s + substring(c, 0, size(c)-2) +"\n") as context
   CALL apoc.ml.openai.embedding([context], $apiKey) YIELD embedding
   SET m.embedding = embedding',
  {batchSize:1, retries:3, params: {apiKey: $apiKey}})
```

新增的`apoc.ml.openai.embedding`过程使使用OpenAI的API生成文本嵌入变得非常容易。我们用`apoc.periodic.iterate`来包装API调用，以批处理事务并引入重试策略。

## 检索增强型LLMs

看起来主流趋势是查询时向LLMs提供外部信息。我们甚至可以找到OpenAI的指南，了解如何[将相关信息作为提示的一部分来生成答案](https://platform.openai.com/docs/guides/gpt-best-practices/strategy-provide-reference-text)。

![](../Images/f5ef0f66b66657b98f3ad70c49cdc01a.png)

检索增强型LLMs的方法。图片由作者提供。

在这里，我们将使用向量相似度搜索来根据用户输入查找相关电影。工作流程如下：

+   我们使用与嵌入节点上下文信息相同的文本嵌入模型来嵌入用户问题

+   我们使用余弦相似度来找到前三个最相关的节点，并将它们的信息返回给LLM

+   LLM根据提供的信息构建最终答案

由于我们将使用**gpt-3.5-turbo**模型来生成最终答案，因此定义系统提示是一个良好的实践。为了使其更具可读性，我们将系统提示定义为Python变量，然后在执行Cypher语句时使用查询参数。

```py
system_prompt = """
You are an assistant that helps to generate text to form nice and human understandable answers based.
The latest prompt contains the information, and you need to generate a human readable response based on the given information.
Make the answer sound as a response to the question. Do not mention that you based the result on the given information.
Do not add any additional information that is not explicitly provided in the latest prompt.
I repeat, do not add any information that is not explicitly given.
"""
```

接下来，我们将定义一个函数，该函数根据用户问题和数据库中提供的上下文构建用户提示。

```py
def generate_user_prompt(question, context):
   return f"""
   The question is {question}
   Answer the question by using the provided information:
   {context}
   """
```

在要求语言模型生成答案之前，我们必须定义智能搜索工具，该工具将基于向量相似性搜索提供相关的上下文信息。如前所述，我们需要嵌入用户输入，然后使用余弦相似性来识别相关节点。通过图形，你可以决定你想要检索和提供作为上下文的信息类型。在这个示例中，我们将返回与生成文本嵌入相同的上下文信息，以及类似的电影信息。

```py
def retrieve_context(question, k=3):
  data = run_query("""
    // retrieve the embedding of the question
    CALL apoc.ml.openai.embedding([$question], $apiKey) YIELD embedding
    // match relevant movies
    MATCH (m:Movie)
    WITH m, gds.similarity.cosine(embedding, m.embedding) AS score
    ORDER BY score DESC
    // limit the number of relevant documents
    LIMIT toInteger($k)
    // retrieve graph context
    MATCH (m)--()--(m1:Movie)
    WITH m,m1, count(*) AS count
    ORDER BY count DESC
    WITH m, apoc.text.join(collect(m1.title)[..3], ", ") AS similarMovies
    MATCH (m)-[r:ACTED_IN|DIRECTED]-(t)
    WITH m, similarMovies, type(r) as type, collect(t.name) as names
    WITH m, similarMovies, type+": "+reduce(s="", n IN names | s + n + ", ") as types
    WITH m, similarMovies, collect(types) as contexts
    WITH m, "Movie title: "+ m.title + " year: "+coalesce(m.released,"") +" plot: "+ coalesce(m.tagline,"")+"\n" +
          reduce(s="", c in contexts | s + substring(c, 0, size(c)-2) +"\n") + "similar movies:" + similarMovies + "\n" as context
    RETURN context
  """, {'question': question, 'k': k, 'apiKey': openai_api_key})
  return data['context'].to_list()
```

目前，你需要使用`gds.similarity.cosine`函数来计算问题和相关节点之间的余弦相似性。在识别相关节点后，我们使用两个额外的`MATCH`子句来检索上下文。你可以查看 [Neo4j 的 GraphAcademy](https://graphacademy.neo4j.com/) 以了解更多关于 Cypher 查询语言的内容。

最后，我们可以定义一个函数，该函数接受用户问题并返回答案。

```py
def generate_answer(question):
    # Retrieve context
    context = retrieve_context(question)
    # Print context
    for c in context:
        print(c)
    # Generate answer
    response = run_query(
        """
  CALL apoc.ml.openai.chat([{role:'system', content: $system},
                      {role: 'user', content: $user}], $apiKey) YIELD value
  RETURN value.choices[0].message.content AS answer
  """,
        {
            "system": system_prompt,
            "user": generate_user_prompt(question, context),
            "apiKey": openai_api_key,
        },
    )
    return response["answer"][0]
```

让我们测试一下增强检索的语言模型工作流程。

![](../Images/f4ad9a6afe98b3ac53152fa3af87d238.png)

图片由作者提供

我们可以观察到，工作流程首先检索相关的电影，并使用这些信息生成答案。这里隐藏了一个彩蛋，Emil Eifrem 被认为出演了《黑客帝国》。

让我们再试一个。

![](../Images/0ebe8f13410cbda6aa3ac7502bee8955.png)

图片由作者提供

向量相似性搜索能够检索提到 Jack Nicholson 的电影，并使用这些信息构建答案。如前所述，我们可以从图形中检索未包含在节点文本嵌入生成中的信息。

![](../Images/8f14fe8b412d9c0081644345562a4358.png)

图片由作者提供

## 摘要

这就是将大型语言模型与知识图谱集成的迷人世界的一瞥。随着这一领域的不断发展，我们手头的工具和技术也会不断进步。随着 Neo4j 和 APOC 的不断进步，我们可以期待在数据处理和处理方面出现更大的创新。

一如既往，代码可以在 [GitHub](https://github.com/tomasonjo/blogs/blob/master/llm/Neo4jOpenAIApoc.ipynb) 上找到。
