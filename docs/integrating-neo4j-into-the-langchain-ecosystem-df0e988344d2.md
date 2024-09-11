# 将 Neo4j 集成到 LangChain 生态系统中。

> 原文：[https://towardsdatascience.com/integrating-neo4j-into-the-langchain-ecosystem-df0e988344d2?source=collection_archive---------1-----------------------#2023-04-17](https://towardsdatascience.com/integrating-neo4j-into-the-langchain-ecosystem-df0e988344d2?source=collection_archive---------1-----------------------#2023-04-17)

## 了解如何开发一个可以通过多种方式与 Neo4j 数据库交互的 LangChain 代理。

[](https://bratanic-tomaz.medium.com/?source=post_page-----df0e988344d2--------------------------------)[![Tomaz Bratanic](../Images/d5821aa70918fcb3fc1ff0013497b3d5.png)](https://bratanic-tomaz.medium.com/?source=post_page-----df0e988344d2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----df0e988344d2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----df0e988344d2--------------------------------) [Tomaz Bratanic](https://bratanic-tomaz.medium.com/?source=post_page-----df0e988344d2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F57f13c0ea39a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintegrating-neo4j-into-the-langchain-ecosystem-df0e988344d2&user=Tomaz+Bratanic&userId=57f13c0ea39a&source=post_page-57f13c0ea39a----df0e988344d2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----df0e988344d2--------------------------------) ·15分钟阅读·2023年4月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdf0e988344d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintegrating-neo4j-into-the-langchain-ecosystem-df0e988344d2&user=Tomaz+Bratanic&userId=57f13c0ea39a&source=-----df0e988344d2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdf0e988344d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintegrating-neo4j-into-the-langchain-ecosystem-df0e988344d2&source=-----df0e988344d2---------------------bookmark_footer-----------)![](../Images/79e7c6ccc5293a96177b339b15d4db39.png)

图片由 [Alex Knight](https://unsplash.com/@agk42?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

*更新：所谓的 Cypher 搜索，即 LLM 生成一个 Cypher 语句以查询 Neo4j 数据库，现已直接集成到 LangChain 库中。了解更多* [*这里*](/langchain-has-added-cypher-search-cb9d821120d5)

*第二次更新：现在 Neo4j 的向量索引直接支持向量搜索，因此我已将向量搜索代码更改为使用 5.11 中引入的新索引*

ChatGPT 启发了全球，并引发了新的 AI 革命。然而，最新的趋势似乎是提供 ChatGPT 外部信息，以提高其准确性并使其能够回答公共数据集中不存在答案的问题。关于大型语言模型（LLMs）的另一个趋势是将它们转变为代理，使它们能够通过各种 API 调用或其他集成与环境互动。

由于增强LLMs相对较新，目前还没有很多开源库。然而，看起来用于构建围绕像 ChatGPT 这样的LLMs的应用程序的首选库被称为 [LangChain](https://python.langchain.com/en/latest/index.html)。该库通过给予其访问各种工具和外部数据源的能力来增强LLM。它不仅可以通过访问外部数据来改善其响应，还可以作为代理通过外部端点操作其环境。

我偶然发现了由 [Ibis Prevedello](https://medium.com/u/fd610570f1c7?source=post_page-----df0e988344d2--------------------------------) 开发的 LangChain 项目，该项目通过提供额外的外部上下文来增强 LLMs 的图搜索。

[](https://github.com/ibiscp/LLM-IMDB?source=post_page-----df0e988344d2--------------------------------) [## GitHub - ibiscp/LLM-IMDB: 使用 LangChain 和 LLMs 从图形中检索信息的概念验证应用…

### 使用 LangChain 和 LLMs 构建 IMDB 数据集的概念验证应用程序 - GitHub…

github.com](https://github.com/ibiscp/LLM-IMDB?source=post_page-----df0e988344d2--------------------------------)

Ibis 的项目使用 [NetworkX 库](https://networkx.org/) 来存储图形信息。我非常喜欢他的方法以及将图搜索轻松整合到 LangChain 生态系统中的方式。因此，我决定开发一个项目，将图数据库 [Neo4j](https://neo4j.com/) 集成到 LangChain 生态系统中。

[](https://github.com/tomasonjo/langchain2neo4j?source=post_page-----df0e988344d2--------------------------------) [## GitHub - tomasonjo/langchain2neo4j: 将 Neo4j 数据库集成到 LangChain 生态系统中

### Langchain2Neo4j 是一个概念验证应用程序，演示了如何将 Neo4j 集成到 Langchain 生态系统中。这…

github.com](https://github.com/tomasonjo/langchain2neo4j?source=post_page-----df0e988344d2--------------------------------)

编码两周后，项目现在允许 LangChain 代理以三种不同模式与 Neo4j 进行交互：

+   生成用于查询数据库的 Cypher 语句

+   全文关键词搜索相关实体

+   向量相似度搜索

在这篇博客文章中，我将带领您了解我开发的每种方法的推理和实施过程。

## 环境设置

首先，我们将配置 Neo4j 环境。我们将使用 Neo4j 沙盒中作为 [recommendations project](https://sandbox.neo4j.com/?usecase=recommendations) 提供的数据集。最简单的解决方案是通过 [这个链接](https://sandbox.neo4j.com/?usecase=recommendations) 创建一个 Neo4j 沙盒实例。然而，如果你更倾向于使用本地 Neo4j 实例，你还可以恢复一个 [在 GitHub 上可用的数据库转储](https://github.com/neo4j-graph-examples/recommendations/tree/main/data)。该数据集是 [MovieLens datasets](https://grouplens.org/datasets/movielens/) [1] 的一部分，具体是小版本。

Neo4j 数据库实例化后，我们应该拥有一个包含以下模式的图表。

![](../Images/7c83401d58a5c1a58e6cdc6d5294af0b.png)

图谱模式。图像由作者提供。

接下来，你需要通过执行以下命令来克隆 langchain2neo4j 仓库：

```py
git clone https://github.com/tomasonjo/langchain2neo4j
```

在下一步中，你需要创建一个 `.env` 文件，并按照 `.env.example` 文件中的说明填写 neo4j 和 OpenAI 凭证。

最后，你需要在 Neo4j 中创建一个全文索引，并通过运行以下命令导入电影标题嵌入：

```py
sh seed_db.sh
```

如果你是 Windows 用户，`seed_db` 脚本可能无法正常工作。在这种情况下，我准备了一个 Jupyter 笔记本，可以作为替代 shell 脚本来帮助你初始化数据库。

现在，让我们跳到 LangChain 集成部分。

## LangChain 代理

根据我所见，使用 LangChain 代理回答用户问题的最常见数据流如下：

![](../Images/7f905a41e80ab8cd9127f4d3ce7b1e7c.png)

LangChain 代理流程。图像由作者提供。

代理数据流在接收到用户输入时启动。代理随后向 LLM 模型发送一个请求，该请求包含用户问题以及代理提示，代理提示是一组代理应遵循的自然语言指令。接着，LLM 会向代理发送进一步的指令。通常情况下，第一个响应是使用任何可用工具从外部来源获取额外的信息。然而，工具不仅限于只读操作。例如，你可以使用它们来更新数据库。在工具返回额外的上下文后，将再次调用包含新获得信息的 LLM。LLM 现在可以选择生成返回给用户的最终答案，或者决定通过其可用工具执行更多操作。

LangChain 代理使用 LLM 进行推理。因此，第一步是定义使用哪个模型。目前，langchain2neo4j 项目仅支持 OpenAI 的聊天完成模型，特别是 GPT-3.5-turbo 和 GPT-4 模型。

```py
if model_name in ['gpt-3.5-turbo', 'gpt-4']:
    llm = ChatOpenAI(temperature=0, model_name=model_name)
else:
    raise Exception(f"Model {model_name} is currently not supported")
```

我还没有探索除 OpenAI 之外的其他 LLM。然而，使用 LangChain 应该很简单，因为它集成了十多种其他 LLM。我不知道竟然存在这么多。

[## 集成

### 编辑描述

python.langchain.com](https://python.langchain.com/en/latest/modules/models/llms/integrations.html?source=post_page-----df0e988344d2--------------------------------)

接下来，我们需要添加一个对话记忆，使用以下行：

```py
memory = ConversationBufferMemory(
    memory_key="chat_history", return_messages=True)
```

LangChain 支持[多种类型的代理](https://python.langchain.com/en/latest/modules/agents/agents.html)。例如，一些代理可以使用记忆组件，而其他的则不能。由于目标是构建一个聊天机器人，我选择了[对话代理（针对聊天模型）代理](https://python.langchain.com/en/latest/modules/agents/agents/examples/chat_conversation_agent.html)类型。LangChain 库有趣的是，一半的代码用 Python 编写，另一半则是提示工程。我们可以探索[对话代理使用的提示](https://github.com/hwchase17/langchain/blob/master/langchain/agents/conversational_chat/prompt.py)。例如，代理有一些必须遵循的基本指令：

> Assistant 是一个由 OpenAI 训练的大型语言模型。Assistant 旨在能够协助处理各种任务，从回答简单问题到提供深入解释和讨论广泛话题。作为一个语言模型，Assistant 能够根据接收到的输入生成类似人类的文本，使其能够进行自然流畅的对话，并提供与主题相关的连贯回应。Assistant 不断学习和改进，其能力也在不断发展。它能够处理和理解大量文本，并利用这些知识提供准确和有用的回应。此外，Assistant 能够根据接收到的输入生成自己的文本，从而参与讨论并提供解释和描述。总体而言，Assistant 是一个强大的系统，可以帮助处理各种任务，并提供关于广泛话题的宝贵见解和信息。无论你是需要帮助回答具体问题还是只是想讨论某个特定话题，Assistant 都在这里提供帮助。

此外，代理有指令在需要时使用任何指定的工具。

```py
Assistant can ask the user to use tools to look up information 
that may be helpful in answering the users original question.
The tools the human can use are:
{{tools}}
{format_instructions}
USER'S INPUT - - - - - - - - - - 
Here is the user's input 
(remember to respond with a markdown code snippet of a 
json blob with a single action, and NOTHING else):
{{{{input}}}}
```

有趣的是，提示中指出助手可以要求用户使用工具查找额外信息。然而，用户不是人类，而是构建在 LangChain 库之上的应用程序。因此，整个寻找额外信息的过程是自动完成的，没有任何人类参与。当然，如果需要，我们可以更改提示。提示还包括 LLMs 应该使用的与代理沟通的格式。

*请注意，代理提示中没有包括代理在工具返回的上下文中没有提供答案时不应回答问题的内容。*

现在，我们只需定义可用的工具。正如所提到的，我准备了三种与Neo4j数据库交互的方法。

```py
tools = [
    Tool(
        name="Cypher search",
        func=cypher_tool.run,
        description="""
        Utilize this tool to search within a movie database, 
        specifically designed to answer movie-related questions.
        This specialized tool offers streamlined search capabilities
        to help you find the movie information you need with ease.
        Input should be full question.""",
    ),
    Tool(
        name="Keyword search",
        func=fulltext_tool.run,
        description="Utilize this tool when explicitly told to use 
        keyword search.Input should be a list of relevant movies 
        inferred from the question.",
    ),
    Tool(
        name="Vector search",
        func=vector_tool.run,
        description="Utilize this tool when explicity told to use 
        vector search.Input should be full question.Do not include 
        agent instructions.",
    ),

]
```

工具的描述用于指定工具的能力以及通知代理何时使用它。此外，我们需要指定工具所期望的输入格式。例如，Cypher和向量搜索都期望完整的问题作为输入，而关键字搜索则期望相关电影的列表作为输入。

LangChain与我在编码中习惯的情况大相径庭。它使用提示来指示LLMs为你完成工作，而不是你自己编码。例如，关键字搜索指示ChatGPT提取相关电影并使用这些作为输入。我花了2小时调试工具输入格式，才意识到可以使用自然语言来指定格式，LLM会处理剩下的工作。

记得我提到过代理没有指示不回答那些信息未在上下文中提供的问题吗？让我们来看看以下对话。

![](../Images/379650f0d2d96d1a4aefa9d50b1d3db7.png)

作者提供的图片。

LLM（大型语言模型）决定基于工具描述，它无法使用任何工具来检索相关的上下文。然而，LLM默认知道很多东西，由于代理没有约束条件必须依赖外部来源，LLM可以独立形成答案。如果我们想强制执行不同的行为，我们需要更改代理提示。

## 生成Cypher语句

我已经开发了一个与Neo4j数据库交互的聊天机器人，通过生成Cypher语句使用OpenAI的[对话模型，如GPT-3.5-turbo和GPT-4](https://medium.com/neo4j/context-aware-knowledge-graph-chatbot-with-gpt-4-and-neo4j-d3a99e8ae21e)。因此，我可以借用大部分想法来实现一个工具，允许LangChain代理通过构造Cypher语句从Neo4j数据库中检索信息。

像text-davinci-003和GPT-3.5-turbo这样的旧模型在作为几-shot Cypher生成器时表现更好，我们提供几个Cypher示例供模型生成新的Cypher语句。然而，似乎GPT-4在仅展示图谱模式时表现良好。因此，由于图谱模式可以通过Cypher查询提取，理论上GPT-4可以用于任何图谱模式，而无需人工干预。

我不会详细讲解LangChain在底层是如何工作的。我们只会查看当LangChain代理决定使用Cypher语句与Neo4j数据库交互时执行的函数。

```py
def _call(self, inputs: Dict[str, str]) -> Dict[str, str]:
    chat_prompt = ChatPromptTemplate.from_messages(
        [self.system_prompt] + inputs['chat_history'] + [self.human_prompt])
    cypher_executor = LLMChain(
        prompt=chat_prompt, llm=self.llm, callback_manager=self.callback_manager
    )
    cypher_statement = cypher_executor.predict(
        question=inputs[self.input_key], stop=["Output:"])
    # If Cypher statement was not generated due to lack of context
    if not "MATCH" in cypher_statement:
        return {'answer': 'Missing context to create a Cypher statement'}
    context = self.graph.query(cypher_statement)

    return {'answer': context}
```

Cypher生成工具将问题和聊天历史作为输入。LLM的输入则结合了**系统**消息、**聊天历史**和当前问题。我为Cypher生成工具准备了以下**系统**消息提示。

```py
SYSTEM_TEMPLATE = """
You are an assistant with an ability to generate Cypher queries based off 
example Cypher queries. Example Cypher queries are:\n""" + examples + """\n
Do not response with any explanation or any other information except the 
Cypher query. You do not ever apologize and strictly generate cypher statements
based of the provided Cypher examples. Do not provide any Cypher statements 
that can't be inferred from Cypher examples. Inform the user when you can't 
infer the cypher statement due to the lack of context of the conversation 
and state what is the missing context.
"""
```

提示工程感觉更像是一门艺术而非科学。在这个示例中，我们给 LLM 提供了几个 Cypher 语句示例，让它基于这些信息生成 Cypher 语句。此外，我们设置了一些约束，比如只允许构造可以从训练示例中推断出的 Cypher 语句。此外，我们不让模型道歉或解释它的想法（不过，GPT-3.5-turbo 不会听从这些指令）。最后，如果问题缺乏上下文，我们允许模型用这些信息进行回答，而不是强迫它生成 Cypher 语句。

在 LLM 构建 Cypher 语句后，我们简单地使用它查询 Neo4j 数据库，并将结果返回给代理。这里是一个示例流程。

![](../Images/d1b1003ba12bf55aa2646189c90598a5.png)

使用 Cypher 生成工具的代理流程。图片由作者提供。

当用户输入问题时，它会连同代理提示一起发送到 LLM。在这个示例中，LLM 反应说它需要使用**Cypher 搜索**工具。Cypher 搜索工具构造一个 Cypher 语句并用它来查询 Neo4j。查询结果随后传递回代理。接着，代理将另一个请求连同新上下文发送到 LLM。由于上下文包含构造答案所需的信息，LLM 形成最终答案并指示代理将其返回给用户。

当然，我们现在可以提出后续问题。

![](../Images/dd80db68f71b28fd477cb57d6e86231a.png)

后续问题。图片由作者提供。

由于代理具有记忆，它知道第二个参与者是谁，因此可以将信息传递给 Cypher 搜索工具，以构造合适的 Cypher 语句。

## 相关三元组的关键词搜索

我从现有的知识图谱索引实现中获得了关键词搜索的想法，这些实现包括 [LangChain](https://python.langchain.com/en/latest/modules/chains/index_examples/graph_qa.html?highlight=graph) 和 [GPT-index libraries](https://gpt-index.readthedocs.io/en/latest/reference/indices/kg_query.html)。这两种实现相当相似。它们要求 LLM 从问题中提取相关实体，并在图中搜索包含这些实体的三元组。因此，我认为我们可以在 Neo4j 中做类似的事情。然而，尽管我们可以使用简单的 **MATCH** 语句搜索实体，但我决定使用 Neo4j 的全文索引会更好。在使用全文索引找到相关实体后，我们返回三元组，并希望那里有回答问题的相关信息。

```py
def _call(self, inputs: Dict[str, str]) -> Dict[str, Any]:
    """Extract entities, look up info and answer question."""
    question = inputs[self.input_key]
    params = generate_params(question)
    context = self.graph.query(
        fulltext_search, {'query': params})
    return {self.output_key: context}
```

请记住，代理程序已经有指示来解析出相关的电影标题并将其作为输入传递给关键词搜索工具。因此，我们不需要处理这个问题。然而，由于问题中可能存在多个实体，我们必须构造适当的 Lucene 查询参数，因为全文索引是基于 Lucene 的。然后，我们只需查询全文索引，并希望返回相关的三元组。我们使用的 Cypher 语句如下：

```py
CALL db.index.fulltext.queryNodes("movie", $query) 
YIELD node, score
WITH node, score LIMIT 5
CALL {
  WITH node
  MATCH (node)-[r:!RATED]->(target)
  RETURN coalesce(node.name, node.title) + " " + type(r) + " " + coalesce(target.name, target.title) AS result
  UNION
  WITH node
  MATCH (node)<-[r:!RATED]-(target)
  RETURN coalesce(target.name, target.title) + " " + type(r) + " " + coalesce(node.name, node.title) AS result
}
RETURN result LIMIT 100
```

因此，我们从全文索引中提取出前五个相关实体。接下来，我们通过遍历它们的邻居来生成三元组。我特别排除了**RATED** 关系，因为它们包含无关的信息。我没有进一步探索，但我有一种好感觉，我们还可以指示 LLM 提供一个待调查的相关关系列表以及适当的实体，这将使我们的关键词搜索更加集中。关键词搜索可以通过明确指示代理程序来启动。

![](../Images/56fabe719ae3f80dcaaab4b30fd7d113.png)

关键词搜索流程。作者提供的图片。

LLM 被指示使用**关键词搜索**工具。此外，代理程序被告知向关键词搜索工具提供一个相关实体列表作为输入，在这个例子中只有**Pokemon**。然后使用 Lucene 参数查询 Neo4j。这种方法撒网更广，希望提取的三元组包含相关信息。例如，检索到的上下文包括关于 Pokemon 类型的信息，这些信息并不相关。但它也包含有关谁参演了电影的信息，这使代理程序能够回答用户的问题。

*如前所述，我们可以指示 LLM 生成一个相关关系类型的列表以及适当的实体，这可以帮助代理程序检索到更相关的信息。*

## 向量相似度搜索

向量相似度搜索是我们将要检查的与 Neo4j 数据库交互的最后一种模式。向量搜索目前非常流行。例如，LangChain 提供了[与十多个向量数据库的集成](https://python.langchain.com/en/latest/modules/indexes/vectorstores.html)。此外，Neo4j 在其 5.11 版本中[增加了一个向量索引](https://neo4j.com/blog/vector-search-deeper-insights/)，我们将在这个例子中使用。向量相似度搜索的核心思想是将问题嵌入到嵌入空间中，并根据问题和文档嵌入的相似性来查找相关文档。我们只需要小心使用相同的嵌入模型来生成文档和问题的向量表示。我在向量搜索实现中使用了 OpenAI 的嵌入。

```py
def _call(self, inputs: Dict[str, str]) -> Dict[str, Any]:
    """Embed a question and do vector search."""
    question = inputs[self.input_key]
    embedding = self.embeddings.embed_query(question)
    context = self.graph.query(
        vector_search, {'embedding': embedding})
    return {self.output_key: context}
```

所以，我们做的第一件事是将问题嵌入。接下来，我们使用嵌入在数据库中查找相关电影。通常，向量数据库会返回相关文档的文本。然而，我们处理的是图数据库。因此，我决定使用三元组结构生成相关信息。使用的 Cypher 语句是：

```py
WITH $embedding AS e
CALL db.index.vector.queryNodes('movies',5, e) yield node as m, score
CALL {
  WITH m
  MATCH (m)-[r:!RATED]->(target)
  RETURN coalesce(m.name, m.title) + " " + type(r) + " " + coalesce(target.name, target.title) AS result
  UNION
  WITH m
  MATCH (m)<-[r:!RATED]-(target)
  RETURN coalesce(target.name, target.title) + " " + type(r) + " " + coalesce(m.name, m.title) AS result
}
RETURN result LIMIT 100
```

Cypher 语句类似于关键字搜索示例。唯一的区别是我们使用向量索引，而不是全文索引来识别相关电影。

![](../Images/670b64f57b90aa96243df90b31c1dc60.png)

向量搜索流程。图片由作者提供。

当指示一个代理使用**向量搜索**工具时，第一步是将问题嵌入为一个向量。OpenAI 的嵌入模型生成的向量表示的维度为 1536。因此，下一步是使用构建的向量，并通过向量索引在数据库中搜索相关信息。同样，由于我们处理的是图数据库，我决定将信息以三元组的形式返回给代理。

向量搜索有趣之处在于，即使我们指示代理搜索**指环王**系列电影，向量相似性搜索也返回了**霍比特人**系列电影的信息。这看起来指环王和霍比特人电影在嵌入空间中是接近的，这也是可以理解的。

## 总结

看起来，能够访问外部工具和信息的聊天机器人和生成代理是继原始 ChatGPT 热潮之后的下一个浪潮。具备为大型语言模型提供额外上下文的能力可以大大改善其结果。此外，代理的工具不仅限于只读操作，这意味着它们可以更新数据库甚至在亚马逊上下订单。大部分时间，LangChain 库似乎是目前用于实现生成代理的主要库。当你开始使用 LangChain 时，你可能需要稍微调整编码过程，因为你需要将大型语言模型提示与代码结合起来完成任务。例如，LLM 与工具之间的消息可以通过自然语言指令作为提示进行塑造和重塑，而不是 Python 代码。我希望这个项目能帮助你将像 Neo4j 这样的图数据库的功能融入到你的 LangChain 项目中。

一如既往，代码可以在 [GitHub](https://github.com/tomasonjo/langchain2neo4j) 上找到。

## 参考文献

[1] *F. Maxwell Harper 和 Joseph A. Konstan. 2015\. The MovieLens 数据集：历史与背景。ACM 交互式智能系统交易（TiiS）5, 4: 19:1–19:19\.* [*https://doi.org/10.1145/2827872*](https://doi.org/10.1145/2827872)
