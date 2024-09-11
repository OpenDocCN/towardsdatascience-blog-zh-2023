# 使用 LangChain 和 Azure OpenAI 与您的 SQL 数据库“对话”

> 原文：[https://towardsdatascience.com/talk-to-your-sql-database-using-langchain-and-azure-openai-bb79ad22c5e2?source=collection_archive---------0-----------------------#2023-09-28](https://towardsdatascience.com/talk-to-your-sql-database-using-langchain-and-azure-openai-bb79ad22c5e2?source=collection_archive---------0-----------------------#2023-09-28)

## 探索使用 LLM 处理数据库查询的自然语言处理的强大功能

[](https://medium.com/@cleancoder?source=post_page-----bb79ad22c5e2--------------------------------)[![Satwiki De](../Images/868c965bef6bae0c451744bc325eed10.png)](https://medium.com/@cleancoder?source=post_page-----bb79ad22c5e2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bb79ad22c5e2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bb79ad22c5e2--------------------------------) [Satwiki De](https://medium.com/@cleancoder?source=post_page-----bb79ad22c5e2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd4bbc4d61092&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftalk-to-your-sql-database-using-langchain-and-azure-openai-bb79ad22c5e2&user=Satwiki+De&userId=d4bbc4d61092&source=post_page-d4bbc4d61092----bb79ad22c5e2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb79ad22c5e2--------------------------------) · 10 分钟阅读 · 2023年9月28日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbb79ad22c5e2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftalk-to-your-sql-database-using-langchain-and-azure-openai-bb79ad22c5e2&user=Satwiki+De&userId=d4bbc4d61092&source=-----bb79ad22c5e2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbb79ad22c5e2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftalk-to-your-sql-database-using-langchain-and-azure-openai-bb79ad22c5e2&source=-----bb79ad22c5e2---------------------bookmark_footer-----------)

LangChain 是一个开源框架，用于开发可以使用 LLM（大型语言模型）处理自然语言的应用程序。

LangChain 的 ***Agent*** 组件是一个 LLM 的封装器，它决定解决问题的最佳步骤或行动。Agent 通常可以访问一组称为 ***Tools***（或工具包）的功能，并且可以根据用户输入决定使用哪个工具。每个代理都可以执行各种 NLP 任务，例如解析、计算、翻译等。

***Agent Executor*** 是 Agent 及其工具集的可运行接口。Agent 执行器负责调用 Agent、获取操作和操作输入、调用操作引用的工具及其对应输入、获取工具的输出，然后将所有这些信息传回 Agent 以获取下一步操作。通常这是一个迭代过程，直到 Agent 达到最终答案或输出。

在这篇文章中，我将展示如何使用 ***LangChain Agent*** 和 ***Azure OpenAI gpt-35-turbo 模型*** 通过自然语言查询你的 SQL 数据库（无需编写任何 SQL！）并获取有用的数据洞察。我们将使用 [SQL Database Toolkit and Agent](https://python.langchain.com/docs/integrations/toolkits/sql_database)，它可以将用户输入转换为适当的 SQL 查询，并在数据库中执行以获取答案。

> 这是一篇探索性文章。它旨在提供对当前可用工具的概述，并在过程中识别任何挑战。

![](../Images/58a5e9d9807e7f7a8770e0f7fe6397fe.png)

作者提供的图片（使用 Bing 图像创作者创建）

# 需求范围

在这次探索中，我们只读取数据库中的数据，避免任何插入、更新或删除操作。这是为了保持数据库中的数据完整性。我们将重点关注如何利用数据库中可用的数据来回答问题。

*不过，SQL Agent 不保证它不会根据特定的问题对你的数据库执行任何 DML 操作。确保不发生任何意外 DML 操作的一种方法是创建一个仅具有* `*read*` *权限的数据库用户，并在以下代码中使用它。*

让我们以一个电子零售公司的订单和库存系统数据库为例。库存跟踪多个类别的产品，例如厨房、园艺、文具、浴室等。订单系统记录每个产品的购买历史，包括订单状态、交货日期等。

以下可能是该应用程序的最终用户提出的一些问题：

1.  上个月销售的厨房产品数量。

1.  有多少订单尚未发货？

1.  上个月有多少订单延迟交付？

1.  上个月销售的前三大产品是什么？

# 设置

1.  **Python>=3.8** 和一个 IDE 进行我们的探索。我使用 VS Code。

1.  我在这里使用 [Azure OpenAI gpt-35-turbo](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models#gpt-35) 作为 LLM。这个模型是 GPT-3.5 系列的一部分，可以理解和生成自然语言及代码。要跟随本指南，你需要一个启用了 OpenAI 服务的 Azure 订阅。了解更多 [here](https://azure.microsoft.com/en-in/products/cognitive-services/openai-service)。

1.  我在这里使用一个 Azure SQL 数据库。然而，你也可以使用本地 SQL 数据库。

## 数据库

我创建了一个名为`retailshopdb`的数据库，包含以下表格和关系：

1.  类别

1.  产品

1.  订单

![](../Images/0a01abef276c66b45260e815e1eea53a.png)

作者提供的图片

除了‘Id’列作为每个表的主键外，这些表还有相互之间的外键关系，例如 CategoryId 是 Product 表中的外键，ProductId 是 Orders 表中的外键。这些关系对于 LangChain 代理根据最终用户的问题构造 SQL 查询至关重要。

## Azure OpenAI

如果你在订阅中创建了 Azure OpenAI 资源，请导航到 Azure OpenAI Studio。为 `gpt-35-turbo` 模型创建一个 `deployment`。

![](../Images/996e8596678ca87575eeff03859929cd.png)

作者提供的图片

# 代码与输出分析

让我们从一些基础代码开始，以便在 VS Code Python 笔记本中访问 LLM。

1.  通过安装所需的库并设置所需的环境变量来初始化笔记本。

```py
%pip install langchain openai sqlalchemy
```

```py
import os
from dotenv import load_dotenv

os.environ["OPENAI_API_TYPE"]="azure"
os.environ["OPENAI_API_VERSION"]="2023-07-01-preview"
os.environ["OPENAI_API_BASE"]="" # Your Azure OpenAI resource endpoint
os.environ["OPENAI_API_KEY"]="" # Your Azure OpenAI resource key
os.environ["OPENAI_CHAT_MODEL"]="gpt-35-turbo-16k" # Use name of deployment

os.environ["SQL_SERVER"]="" # Your az SQL server name
os.environ["SQL_DB"]="retailshop"
os.environ["SQL_USERNAME"]="" # SQL server username 
os.environ["SQL_PWD"]="{<password>}" # SQL server password 
```

2\. 连接到数据库。

```py
from sqlalchemy import create_engine

driver = '{ODBC Driver 17 for SQL Server}'
odbc_str = 'mssql+pyodbc:///?odbc_connect=' \
                'Driver='+driver+ \
                ';Server=tcp:' + os.getenv("SQL_SERVER")+'.database.windows.net;PORT=1433' + \
                ';DATABASE=' + os.getenv("SQL_DB") + \
                ';Uid=' + os.getenv("SQL_USERNAME")+ \
                ';Pwd=' + os.getenv("SQL_PWD") + \
                ';Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;'

db_engine = create_engine(odbc_str)
```

3\. 初始化 LangChain `chat_model` 实例，它提供了使用聊天 API 调用 LLM 提供者的接口。选择聊天模型的原因是 `gpt-35-turbo` 模型已针对聊天进行了优化，因此我们在这里使用 `AzureChatOpenAI` 类来初始化实例。

```py
from langchain.chat_models import AzureChatOpenAI

llm = AzureChatOpenAI(model=os.getenv("OPENAI_CHAT_MODEL"),
                      deployment_name=os.getenv("OPENAI_CHAT_MODEL"),
                      temperature=0)
```

> 请注意，`temperature` 设置为 0。温度是一个控制生成文本的“创造力”或随机性的参数。较低的温度（0 为最低）使输出更“专注”或确定。由于我们处理的是数据库，LLM 生成的响应必须是事实性的。

4\. 创建提示模板。

***提示*** 是我们发送给 LLM 以生成输出的输入。***提示也可以设计为包含指令、上下文、示例（单次或少次）***，这些都对生成准确的输出至关重要，同时也可以设置语气和格式化输出数据。

使用提示模板是结构化这些属性的好方法，包括提供给 LLM 的最终用户输入。我们在这里使用 LangChain 的 `ChatPromptTemplate` 模块，它基于 ChatML（聊天标记语言）。

这是一个基础提示模板，供我们开始使用。随着时间的推移，我们会根据需要更新此模板 —

```py
from langchain.prompts.chat import ChatPromptTemplate

final_prompt = ChatPromptTemplate.from_messages(
    [
        ("system", 
         """
          You are a helpful AI assistant expert in querying SQL Database to find answers to user's question about Categories, Products and Orders.
         """
         ),
        ("user", "{question}\n ai: "),
    ]
)
```

现在初始化 `create_sql_agent`，它设计用于与 SQL 数据库交互，如下所示。该代理配备了与 SQL 数据库连接并读取表的元数据和内容的工具包。

```py
from langchain.agents import AgentType, create_sql_agent
from langchain.sql_database import SQLDatabase
from langchain.agents.agent_toolkits.sql.toolkit import SQLDatabaseToolkit

db = SQLDatabase(db_engine)

sql_toolkit = SQLDatabaseToolkit(db=db, llm=llm)
sql_toolkit.get_tools()

sqldb_agent = create_sql_agent(
    llm=llm,
    toolkit=sql_toolkit,
    agent_type=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True
)
```

请注意，我们在这里使用 `ZERO_SHOT_REACT_DESCRIPTION` 作为 `agent_type` 参数的值，这指示代理 *不使用记忆*。

一切准备就绪进行测试 —

```py
sqldb_agent.run(final_prompt.format(
        question="Quantity of kitchen products sold last month?"
  ))
```

> 注意在以下单元格输出中，LangChain Agent Executor 如何以迭代的方式使用 `Action`、`Observation` 和 `Thought` 流，直到达到 `Final Answer`。

![](../Images/dc80bc8bb758a01f49c71be0e5e9e208.png)

作者提供的图片

*输出: 10*

这是一个正确的答案。

让我们来点乐趣吧？将问题中的‘Quantity’替换为‘how many’，这应该会产生相同的答案。

```py
sqldb_agent.run(final_prompt.format(
        question="How many kitchen products were sold last month?"
  ))
```

但这就是我们得到的结果 —

![](../Images/6df046e7da8416c6f52e628bf429474d.png)

作者提供的图片

*输出: ‘本月销售了 2 件厨房产品。’*

***这个输出不正确！*** 代理在创建 SQL 查询时犯了错误。它没有执行 `SUM(ProductOrderedQuantity)` 来获取输出，而是对 JOIN 结果执行了 `COUNT(*)`，这导致了错误的输出。

> **为什么稍微改变提示输入会产生不同的输出？**
> 
> OpenAI 模型是非确定性的，这意味着相同的输入可能会产生不同的输出。将温度设置为 0 会使输出大多是确定性的，但由于 GPU 浮点运算，可能仍会存在少量的变异。

使用不同的输入运行另一个测试 —

```py
sqldb_agent.run(final_prompt.format(
        question="How many orders have not been shipped yet?"
  ))
```

![](../Images/b17c40ba6c5169f621273cd1cb3666e5.png)

图片由作者提供

*输出：‘还有 15 个订单尚未发货。’*

***这又是错误的结果。*** *代理也考虑了‘已完成’的订单，而我们的提问仅涉及尚未发货的订单。*

让我们看看可以做哪些修改以生成准确的输出。

## 玩转 Prompt Engineering

LangChain 代理可以使用其工具包从 SQL 数据库中读取表格元数据，并且在一定程度上可以解释列名。但是仍然存在一些推理上的差距，我们可以尝试使用 Prompt Engineering 技术来弥补这些差距。

我们从一个基础提示模板开始，该模板只有一行指令。让我们添加一些额外的信息，以便向 LLM 提供更多关于我们用例的上下文，以形成更好的 SQL 查询。以下是我在系统消息中添加的高层次信息：

1.  表格列的信息

1.  订单状态值的‘含义’

1.  最具体的信息在最后

```py
from langchain.prompts.chat import ChatPromptTemplate

final_prompt = ChatPromptTemplate.from_messages(
    [
        ("system", 
         """
         You are a helpful AI assistant expert in identifying the relevant topic from user's question about Categories, Products and Orders and then querying SQL Database to find answer.
         Use following context to create the SQL query. Context:
         Product table contains information about products including product name, description, product price and product category.
         Category table contains information about categories including category name and description. Each Product is mapped to a Category.
         Orders table contains information about orders placed by customers including 
         quantity or number of products ordered,
         expected delivery date and actual delivery date of the Order in the location and the status of the order. 
         Order status = 'Processing' means the order is being processed by seller and not yet shipped,
         Order status = 'Shipped' means the order is shipped by the seller and is on the way to the customer,
         Order status = 'Completed' means the order is delivered to the customer, and 
         Order status = 'Cancelled' means the order is cancelled by the customer.

         If the question is about number of products in an order, then look for column names with 'quantity' in the tables and use 'sum' function to find the total number of products.
        """
         ),
        ("user", "{question}\n ai: "),
    ]
)
```

现在重新运行第一个输入 —

```py
sqldb_agent.run(final_prompt.format(
        question ="How many Kitchen products were sold in current month?"
  ))
```

![](../Images/df0a705e4289d08e6deddb162df978fd.png)

图片由作者提供

*输出：10*

推理得到了改善！通过向 LLM 提供额外的上下文，我们能够获得准确的输出。

现在测试所有用户输入 —

![](../Images/80a87b16e47fddd1f271be3bfc8b9171.png)

# 思考之后 . . .

这一切在测试时看起来都很不错，但如果我们想实际构建一个解决方案并将其发布供最终用户使用呢？这是一个很好的想法，适用于类似于在自己数据库上运行聊天机器人的用例，但像任何典型的软件开发一样，在构建基于 LLM 的系统之前，我们还需要考虑和决定一些关键的设计方面。

## 可扩展性

在这个例子中，我使用了 3 个表，总共约 30 行。涉及连接所有 3 个表的输出的平均延迟约为 5 秒。截至目前，我在官方文档中没有找到关于我们可以使用的数据库最大大小的信息。然而，我们可以考虑一些参数来确定我们的需求：

1.  您的应用程序需要的延迟是多少？如果您正在构建一个聊天机器人，那么您的预期延迟可能不会超过某个数值。

1.  您的数据库大小是多少？还要考虑您希望用于查询的各个表的大小。

> 请注意，你不需要将整个数据库传递给 Agent Toolkit。可以选择特定的表与 Toolkit 一起使用。一个好的选择是为不同的用例识别表的子集，并创建多个指向不同表子集的 Agent。

3\. Azure OpenAI 资源的速率和配额限制。如果你使用的是其他 LLM 提供商，也请查看那里可能存在的限制/约束。

## 可靠性

我们如何确保始终获得准确的响应？如何确保系统不会出现幻觉或产生完全意外的内容？

当前正在研究如何提高 LLM 的可靠性和鲁棒性。通过使用特定用例的提示，我们可以帮助改善我们的用例的推理，这有时也被称为“临时学习”或“上下文学习”。

> 请记住，我们在这里不是在训练 LLM。从使用预训练 LLM 构建产品的角度，我们只能有针对性地调整我们的代码、模型参数和构建在这些 LLM 上的提示。

以迭代方式进行开发，并在过程中进行评估，可以将我们引向正确的方向，开发一个整体有效的系统。

## 日志记录和监控

像其他典型的软件开发一样，启用日志记录和持续监控 LLM 基础的应用程序是一种好习惯。监控不仅可以捕获系统相关的指标，如性能、延迟、请求-响应率，还可以捕获我们系统的输入和输出，这可以帮助我们确定系统的一致性。从监控中收集的一些有用信息可以用来改进我们的系统：

1.  终端用户频繁提出类似问题，以及这些问题生成的输出

1.  LLM 的幻觉率

## 结论

软件工程领域正在迅速变化，LLM 的巨大生成能力带来了许多解决方案。我们有机会采用这项技术，利用其力量创建产品，同时保持对 LLM 支持系统的可靠性的检查。通常，从小处开始，建立一个概念验证应用程序，看看它是否符合你的需求，总是一个好主意。

# 参考文献：

[https://community.openai.com/t/run-same-query-many-times-different-results/140588](https://community.openai.com/t/run-same-query-many-times-different-results/140588)

[https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai_api)

[https://mlops.community/concepts-for-reliability-of-llms-in-production/](https://mlops.community/concepts-for-reliability-of-llms-in-production/)

*如果你想阅读更多关于新兴技术的内容，请关注我。请在评论区留下你的反馈。*
