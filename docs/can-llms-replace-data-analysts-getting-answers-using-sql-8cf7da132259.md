# LLMs 能否替代数据分析师？使用 SQL 获取答案

> 原文：[https://towardsdatascience.com/can-llms-replace-data-analysts-getting-answers-using-sql-8cf7da132259?source=collection_archive---------0-----------------------#2023-12-22](https://towardsdatascience.com/can-llms-replace-data-analysts-getting-answers-using-sql-8cf7da132259?source=collection_archive---------0-----------------------#2023-12-22)

## 第 2 部分：深入探讨 LLM 代理

[](https://miptgirl.medium.com/?source=post_page-----8cf7da132259--------------------------------)[![Mariya Mansurova](../Images/b1dd377b0a1887db900cc5108bca8ea8.png)](https://miptgirl.medium.com/?source=post_page-----8cf7da132259--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8cf7da132259--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8cf7da132259--------------------------------) [Mariya Mansurova](https://miptgirl.medium.com/?source=post_page-----8cf7da132259--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F15a29a4fc6ad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-llms-replace-data-analysts-getting-answers-using-sql-8cf7da132259&user=Mariya+Mansurova&userId=15a29a4fc6ad&source=post_page-15a29a4fc6ad----8cf7da132259---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8cf7da132259--------------------------------) ·31 分钟阅读·2023 年 12 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8cf7da132259&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-llms-replace-data-analysts-getting-answers-using-sql-8cf7da132259&user=Mariya+Mansurova&userId=15a29a4fc6ad&source=-----8cf7da132259---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8cf7da132259&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-llms-replace-data-analysts-getting-answers-using-sql-8cf7da132259&source=-----8cf7da132259---------------------bookmark_footer-----------)![](../Images/a9a7ac47dced35c371073a2c4bd70565.png)

图片由 DALL-E 3 提供

在 [上一篇文章](https://medium.com/towards-data-science/can-llms-replace-data-analysts-building-an-llm-powered-analyst-851578fa10ce) 中，我们开始构建一个由 LLM 驱动的分析师。我们决定专注于描述性分析和报告任务，因为这些任务是分析师最常见的。大多数分析师的职业生涯从这些任务开始，大多数公司也从报告和 BI 工具开始构建分析功能。

我们的第一个原型可以使用现成的工具来回答与定义指标相关的问题，如下面的示例所示。

![](../Images/37dd8da1463d0133009b5fea8137b792.png)

作者插图

下一步是教我们的 LLM 驱动分析师获取任何指标。分析师通常使用 SQL 来获取数据。因此，对 LLM 分析师来说，最有用的技能是与 SQL 数据库互动。

我们已经讨论了 OpenAI 功能，并了解了 LLM 如何使用工具与世界进行集成。在这篇文章中，我想专注于 LLM 代理，并详细讨论它们。我们将学习如何使用 LangChain 构建代理，并尝试不同类型的代理。

# 设置数据库

首先，让我们设置一个我们将要交互的数据库。我选择的是 ClickHouse。ClickHouse 是一个开源的列式 SQL 数据库管理系统，适用于在线分析处理（OLAP）。它是大数据和分析任务的良好选择。

如果你想了解更多关于 ClickHouse 的内容，请查看[我的文章](https://towardsdev.com/clickhouse-tips-tricks-i-wish-i-knew-f575f0371cd3)。不过，你可以使用任何数据库。你只需调整获取数据的函数代码即可。

安装 ClickHouse 只需一行代码。初始命令会执行 ClickHouse 团队提供的脚本，以下载适合你平台的二进制文件。然后，你需要启动一个服务器，就完成了。

```py
curl https://clickhouse.com/ | sh # downloads appropriate binary file
./clickhouse server # starts clickhouse server
```

你可以通过 HTTP API 访问 ClickHouse。默认情况下，它监听 8123 端口。

```py
CH_HOST = 'http://localhost:8123' # default address 

def get_clickhouse_data(query, host = CH_HOST, connection_timeout = 1500):
    r = requests.post(host, params = {'query': query}, 
      timeout = connection_timeout)

    return r.text
```

我通常检查 `r.status_code = 200` 以确保请求已成功完成，否则会引发错误。然而，我们将把此函数的结果传递给 LLM。因此，无论输出 DB 返回什么，都没关系，无论是否为错误，LLM 都能够妥善处理。

我为这个例子生成了合成数据。如果你想了解更多关于数据模拟的内容，可以在[这里](https://github.com/miptgirl/miptgirl_medium/blob/main/analyst_agent/generate_synthetic_data_for_sql.ipynb)找到代码。我使用了保留曲线来为客户建模会话，考虑了账户创建以来的天数和每周的季节性。这可能现在有些复杂，因为我们还不太使用数据。但我希望在未来的原型中，我们能利用 LLM 从这些数据中获得一些见解。

我们只需要几个表来表示基本的电子商务产品数据模型。我们将处理用户列表（`ecommerce.users`）及其会话（`ecommerce.sessions`）。

让我们来看一下 `ecommerce.sessions` 表。

![](../Images/8430e5082dd3450b91d9779b4faf0aa1.png)

作者截图

在这里，你可以看到我们为用户提供了哪些功能。

![](../Images/faf4589845750ce8ac3151e06172aced.png)

作者截图

现在，我们有了可以使用的数据，准备继续深入讨论 LLM 代理的细节。

# 代理概述

LLM 代理的核心思想是将 LLM 作为推理引擎来定义要执行的操作集合。在经典方法中，我们硬编码了一系列操作，但使用代理时，我们给模型提供工具和任务，让它决定如何实现这些任务。

关于LLM代理的最基础论文之一是[“ReAct: Synergizing Reasoning and Acting in Language Models”](https://arxiv.org/abs/2210.03629)，作者是Shunyu Yao等。ReAct（**Re**asoning + **Act**ing）方法建议结合：

+   帮助制定计划并在出现例外情况时更新计划的推理，

+   允许模型利用外部工具或从外部来源获取数据的操作。

这种方法在不同任务上的表现更好。下面是来自论文的一个示例。

![](../Images/35ba2a3b5a9028a1189160df6725e4e1.png)

来自[Yao等人的论文](https://arxiv.org/abs/2210.03629)的示例

实际上，这就是人类智能的工作方式：我们将内在的推理与任务导向的行动结合起来。假设你需要做晚餐。你将使用推理来制定计划（“客人将在30分钟内到达，我只有时间做意大利面”），调整计划（“本变成了素食者，我应该为他订购一些食物”）或决定委派任务（相当于外部工具，“意大利面没了，我需要问我的伴侣去买”）。同时，你会使用行动来使用一些工具（询问伴侣帮助或使用搅拌机）或获取一些信息（查找互联网了解煮意大利面所需的时间，以使其达到“ al dente”）。所以，使用类似的方法对LLM是合理的，因为它对人类有效（人类无疑是AGI）。

现在，自ReAct以来，LLM代理有许多不同的方法。它们在用于设置模型推理的提示、如何定义工具、输出格式、处理中间步骤的记忆等方面有所不同。

一些最受欢迎的方法包括：

+   OpenAI函数，

+   AutoGPT，

+   BabyAGI，

+   计划并执行的代理。

我们稍后会使用这些方法来完成我们的任务，并查看它们的工作原理及其差异。

# 从零开始构建代理

让我们开始构建一个代理。我们将从零开始，以了解其内部工作原理。然后，如果你不需要任何自定义，我们将使用LangChain的工具来加快原型制作。

LLM代理的核心组件包括：

+   引导模型推理的提示。

+   模型可以使用的工具。

+   记忆 — 一个将先前迭代传递给模型的机制。

对于LLM代理的第一个版本，我们将使用OpenAI函数作为构建代理的框架。

## 定义工具

让我们开始定义我们机器人的工具。让我们考虑一下我们的LLM驱动分析师可能需要哪些信息来回答问题：

+   表格中的列表 — 我们可以将其放入系统提示中，以便模型了解我们拥有的数据，并且每次都不需要执行工具。

+   表格的列列表，以便模型可以理解数据模式，

+   表格中列的前几个值，以便模型可以查找过滤器的值，

+   SQL查询执行的结果，以便获取实际数据。

要在 LangChain 中定义工具，我们需要对函数使用 `@tool` 装饰器。我们将使用 Pydantic 来指定每个函数的参数模式，以便模型知道传递给函数的内容。

我们在 [上一篇文章](https://medium.com/towards-data-science/can-llms-replace-data-analysts-building-an-llm-powered-analyst-851578fa10ce) 中详细讨论了工具和 OpenAI 功能。所以如果你需要复习这个主题，请不要犹豫去阅读它。

下面的代码定义了三个工具：`execute_sql`、`get_table_columns` 和 `get_table_column_distr`。

```py
from langchain.agents import tool
from pydantic import BaseModel, Field
from typing import Optional

class SQLQuery(BaseModel):
    query: str = Field(description="SQL query to execute")

@tool(args_schema = SQLQuery)
def execute_sql(query: str) -> str:
    """Returns the result of SQL query execution"""
    return get_clickhouse_data(query)

class SQLTable(BaseModel):
    database: str = Field(description="Database name")
    table: str = Field(description="Table name")

@tool(args_schema = SQLTable)
def get_table_columns(database: str, table: str) -> str:
    """Returns list of table column names and types in JSON"""

    q = '''
    select name, type
    from system.columns 
    where database = '{database}'
        and table = '{table}'
    format TabSeparatedWithNames
    '''.format(database = database, table = table)

    return str(get_clickhouse_df(q).to_dict('records'))

class SQLTableColumn(BaseModel):
    database: str = Field(description="Database name")
    table: str = Field(description="Table name")
    column: str = Field(description="Column name")
    n: Optional[int] = Field(description="Number of rows, default limit 10")

@tool(args_schema = SQLTableColumn)
def get_table_column_distr(database: str, table: str, column: str, n:int = 10) -> str:
    """Returns top n values for the column in JSON"""

    q = '''
    select {column}, count(1) as count
    from {database}.{table} 
    group by 1
    order by 2 desc 
    limit {n}
    format TabSeparatedWithNames
    '''.format(database = database, table = table, column = column, n = n)

    return str(list(get_clickhouse_df(q)[column].values))
```

> 值得注意的是，上面的代码使用了 Pydantic v1。在 2023 年 6 月，Pydantic 发布了 v2，这与 v1 不兼容。所以如果你看到验证错误，请检查你的版本。你可以在 [文档](https://python.langchain.com/docs/guides/pydantic_compatibility) 中找到有关 Pydantic 兼容性的更多详细信息。

我们将使用 OpenAI 的功能，需要转换我们的工具。此外，我已经将工具包保存在一个字典中。在执行工具以获取观察结果时，这将非常方便。

```py
from langchain.tools.render import format_tool_to_openai_function

# converting tools into OpenAI functions
sql_functions = list(map(format_tool_to_openai_function, 
    [execute_sql, get_table_columns, get_table_column_distr]))

# saving tools into a dictionary for the future
sql_tools = {
    'execute_sql': execute_sql,
    'get_table_columns': get_table_columns,
    'get_table_column_distr': get_table_column_distr
}
```

## 定义一个链

我们已经为模型创建了工具。现在，我们需要定义代理链。我们将使用最新的 GPT 4 Turbo，它也经过了微调以便与这些功能一起使用。让我们初始化一个聊天模型。

```py
from langchain.chat_models import ChatOpenAI

llm = ChatOpenAI(temperature=0.1, model = 'gpt-4-1106-preview')\
  .bind(functions = sql_functions)
```

下一步是定义一个由系统消息和用户问题组成的提示。我们还需要一个 `MessagesPlaceholder` 来设置模型将要处理的观察列表的位置。

```py
from langchain.prompts import ChatPromptTemplate, MessagesPlaceholder

system_message = '''
You are working as a product analyst for the e-commerce company. 
Your work is very important, since your product team makes decisions based on the data you provide. So, you are extremely accurate with the numbers you provided. 
If you're not sure about the details of the request, you don't provide the answer and ask follow-up questions to have a clear understanding.
You are very helpful and try your best to answer the questions.

All the data is stored in SQL Database. Here is the list of tables (in the format <database>.<table>) with descriptions:
- ecommerce.users - information about the customers, one row - one customer
- ecommerce.sessions - information about the sessions customers made on our web site, one row - one session
'''

analyst_prompt = ChatPromptTemplate.from_messages(
    [
        ("system", system_message),
        ("user", "{question}"),
        MessagesPlaceholder(variable_name="agent_scratchpad"),
    ]
)
```

正如我们讨论过的，我已经将数据库中的表列表添加到提示中，以便模型对我们的数据有至少一些了解。

我们拥有所有构建块，并准备好设置代理链。输入参数包括用户消息和中间步骤（之前的消息、函数调用和观察）。我们使用 `format_to_openai_function_messages` 将输入参数传递到提示中，以转换成期望的格式。然后，我们将所有内容传递给 LLM，最后使用输出解析器 `OpenAIFunctionsAgentOutputParser` 以方便操作。

```py
 from langchain.agents.format_scratchpad import format_to_openai_function_messages
from langchain.agents.output_parsers import OpenAIFunctionsAgentOutputParser

analyst_agent = (
    {
        "question": lambda x: x["question"],
        "agent_scratchpad": lambda x: format_to_openai_function_messages(x["intermediate_steps"]),
    }
    | analyst_prompt
    | llm
    | OpenAIFunctionsAgentOutputParser()
)
```

我们已经定义了我们的主要代理链。让我们尝试调用它。我传递了一个空列表，因为开始时没有中间步骤。

```py
analyst_agent.invoke({"question": "How many active customers from the United Kingdom do we have?", 
    "intermediate_steps": []})

# AgentActionMessageLog(
#    tool='execute_sql', 
#    tool_input={'query': "SELECT COUNT(DISTINCT user_id) AS active_customers_uk FROM ecommerce.sessions WHERE country = 'United Kingdom' AND active = TRUE"}, 
#    log='\nInvoking: `execute_sql` with `{\'query\': "SELECT COUNT(DISTINCT user_id) AS active_customers_uk FROM ecommerce.sessions WHERE country = \'United Kingdom\' AND active = TRUE"}`\n\n\n', 
#    message_log=[AIMessage(content='', additional_kwargs={'function_call': {'arguments': '{"query":"SELECT COUNT(DISTINCT user_id) AS active_customers_uk FROM ecommerce.sessions WHERE country = \'United Kingdom\' AND active = TRUE"}', 'name': 'execute_sql'}})]
# )
```

我们得到了一个 `AgentActionMessageLog` 对象，这意味着模型想要调用 `execute_sql` 函数。当模型准备好将最终答案返回给用户时，它会返回 `AgentFinish` 对象。

如果我们查看 `tool_input`，可以看到模型想要执行以下查询。

```py
SELECT COUNT(DISTINCT user_id) AS active_customers_uk 
FROM ecommerce.sessions 
WHERE country = 'United Kingdom' AND active = TRUE
```

查询看起来很好，但使用了错误的列名：`active` 而不是 `is_active`。有趣的是，LLM 是否能够从这个错误中恢复并返回结果。

我们可以一步一步地手动执行这些步骤，但自动化会更方便。

+   如果返回 `AgentActionMessageLog` 对象，我们需要调用一个工具，将观察结果添加到 `agent_scratchpad` 中，并再次调用链。

+   如果我们得到了 `AgentFinish` 对象，我们可以终止执行，因为我们已经得到了最终答案。

我还会在十次迭代后添加一个断点，以避免潜在的无限循环。

```py
from langchain_core.agents import AgentFinish

# setting initial parameters
question = "How many active customers from the United Kingdom do we have?"
intermediate_steps = []
num_iters = 0

while True:
    # breaking if there were more than 10 iterations
    if num_iters >= 10:  
        break

    # invoking the agent chain
    output = analyst_agent.invoke(
        {
            "question": question,
            "intermediate_steps": intermediate_steps,
        }
    )
    num_iters += 1

    # returning the final result if we got the AgentFinish object
    if isinstance(output, AgentFinish):
        model_output = output.return_values["output"]
        break
    # calling tool and adding observation to the scratchpad otherwise
    else:
        print(f'Executing tool: {output.tool}, arguments: {output.tool_input}')
        observation = sql_tools[output.tool](output.tool_input)
        print(f'Observation: {observation}')
        print()
        intermediate_steps.append((output, observation))

print('Model output:', model_output)
```

我在输出中添加了一些工具使用的日志，以查看执行情况。此外，你可以随时使用 LangChain 调试模式查看所有调用。

执行结果如下。

```py
Executing tool: execute_sql, arguments: {'query': "SELECT COUNT(*) AS active_customers_uk FROM ecommerce.users WHERE country = 'United Kingdom' AND active = TRUE"}
Observation: Code: 47\. DB::Exception: Missing columns: 'active' 
while processing query: 'SELECT count() AS active_customers_uk 
FROM ecommerce.users WHERE (country = 'United Kingdom') AND (active = true)', 
required columns: 'country' 'active', maybe you meant: 'country'. 
(UNKNOWN_IDENTIFIER) (version 23.12.1.414 (official build))

Executing tool: get_table_columns, arguments: {'database': 'ecommerce', 'table': 'users'}
Observation: [{'name': 'user_id', 'type': 'UInt64'}, {'name': 'country', 'type': 'String'}, 
{'name': 'is_active', 'type': 'UInt8'}, {'name': 'age', 'type': 'UInt64'}]

Executing tool: execute_sql, arguments: {'query': "SELECT COUNT(*) AS active_customers_uk FROM ecommerce.users WHERE country = 'United Kingdom' AND is_active = 1"}
Observation: 111469

Model output: We have 111,469 active customers from the United Kingdom.
```

> 注意：不能保证代理不会对你的数据库执行 DML 操作。因此，如果在生产环境中使用，请确保 LLM 要么没有更改数据的权限，要么你的工具实现不允许这样做。

所以，模型尝试执行 SQL，但收到错误提示，没有 `active` 列。然后，它决定查看表的 schema，随后相应地修正了查询，并获得了结果。

这是相当不错的性能。我自己也是如此。我通常会先尝试回忆或猜测列名，只有在第一次尝试失败时才会查看文档。

但在大多数情况下，我们不需要自己编写执行代码。我们可以使用 LangChain 的 `AgentExecutor` 类。查看文档以了解该类的所有可能的 [参数](https://api.python.langchain.com/en/latest/agents/langchain.agents.agent.AgentExecutor.html)。

你只有在想要自定义某些内容时，才需要编写自己的执行器。例如，添加一些条件以终止执行或逻辑来使用工具。

你可以在下面找到使用 `AgentExecutor` 的相同代码。

```py
from langchain.agents import AgentExecutor

analyst_agent_executor = AgentExecutor(
    agent=analyst_agent, 
    tools=[execute_sql, get_table_columns, get_table_column_distr], 
    verbose=True,
    max_iterations=10, # early stopping criteria
    early_stopping_method='generate', 
    # to ask model to generate the final answer after stopping
)

analyst_agent_executor.invoke(
  {"question": "How many active customers from the United Kingdom do we have?"}
)
```

结果是我们得到了一个易于追踪的输出，且结果相同。你可以注意到 LangChain 对代理输出的格式化非常方便。

![](../Images/9af57b26a48a6cae3401a95f913919fd.png)

图片由作者提供

我们从头开始构建了 LLM 代理。现在，我们了解它是如何工作的，并知道如何自定义它。然而，LangChain 提供了一个高级函数 `initialize_agent`，可以仅通过一次调用完成。你可以在 [文档](https://api.python.langchain.com/en/latest/agents/langchain.agents.initialize.initialize_agent.html?highlight=initialize_agent#langchain.agents.initialize.initialize_agent) 中找到所有详细信息。

```py
from langchain.agents import AgentType, Tool, initialize_agent
from langchain.schema import SystemMessage

agent_kwargs = {
    "system_message": SystemMessage(content=system_message)
}

analyst_agent_openai = initialize_agent(
    llm = ChatOpenAI(temperature=0.1, model = 'gpt-4-1106-preview'),
    agent = AgentType.OPENAI_FUNCTIONS, 
    tools = [execute_sql, get_table_columns, get_table_column_distr], 
    agent_kwargs = agent_kwargs,
    verbose = True,
    max_iterations = 10,
    early_stopping_method = 'generate'
) 
```

> 请注意，我们传递了 ChatOpenAI 模型，但未绑定任何函数。我们单独传递了工具，因此不需要将它们链接到模型。

# 不同的代理类型

我们从头开始构建了一个基于 OpenAI 函数的 LLM 代理。然而，还有很多其他方法。因此，我们也来尝试一下这些方法。

我们将查看 ReAct 方法（来自我们之前讨论的论文中的初始版本）以及 LangChain 提供的几种实验方法：Plan-and-execute、BabyAGI 和 AutoGPT。

## ReAct 代理

让我们从查看 ReAct 代理开始。通过当前的实现，我们可以轻松地更改代理类型，并尝试论文中描述的 ReAct 方法。

最通用的ReAct实现是[零样本ReAct](https://python.langchain.com/docs/modules/agents/agent_types/react)。它不适用于我们，因为它只支持单一字符串作为输入。我们的工具需要多个参数，因此我们需要使用[结构化输入ReAct](https://python.langchain.com/docs/modules/agents/agent_types/structured_chat)。

我们可以利用使用模块化框架的优势：我们只需更改一个参数`agent = AgentType.STRUCTURED_CHAT_ZERO_SHOT_REACT_DESCRIPTION`，就可以了。

```py
system_message = system_message + '''\nYou have access to the following tools:'''

agent_kwargs = {
    "prefix": system_message
}

analyst_agent_react = initialize_agent(
    llm = ChatOpenAI(temperature=0.1, model = 'gpt-4-1106-preview'),
    agent = AgentType.STRUCTURED_CHAT_ZERO_SHOT_REACT_DESCRIPTION, 
    tools = [execute_sql, get_table_columns, get_table_column_distr], 
    agent_kwargs = agent_kwargs,
    verbose = True,
    max_iterations = 10,
    early_stopping_method = 'generate'
)
```

你可能会想知道如何找到可以为代理指定的参数。不幸的是，这些没有文档说明，因此我们需要深入源码来理解它。让我们一步步来讨论。

+   我们可以看到`analyst_agent_react`是`AgentExecutor`类的一个对象。

+   这个类有一个代理字段。在我们的例子中，它是`StructuredChatAgent`类的一个对象。这个类依赖于指定的代理类型。

+   让我们找到一个`StructuredChatAgent`类的实现并看看它是如何工作的。在这种情况下，LangChain [创建](https://github.com/langchain-ai/langchain/blob/133971053a0b84a034fb0bc78cd1150cdb7f5dbf/libs/langchain/langchain/agents/structured_chat/base.py#L91) 了一个由前缀、工具描述、格式化的指令和后缀组成的提示。

+   你可以在[代码](https://github.com/langchain-ai/langchain/blob/133971053a0b84a034fb0bc78cd1150cdb7f5dbf/libs/langchain/langchain/agents/structured_chat/base.py#L103)中找到你可以作为`agent_kwargs`传递的所有参数的完整列表。

+   所以，我们可以从[这里](https://github.com/langchain-ai/langchain/blob/133971053a0b84a034fb0bc78cd1150cdb7f5dbf/libs/langchain/langchain/agents/structured_chat/prompt.py#L2)覆盖默认的`PREFIX`值，并将其作为`agent_kwargs`中的`prefix`传递。如果你感兴趣，你可以在这里阅读默认的ReAct提示，并考虑如何根据你的任务对其进行调整。

如果你感兴趣，你可以使用以下调用查看最终提示。

```py
for message in analyst_agent_react.agent.llm_chain.prompt.messages:
    print(message.prompt.template)
```

让我们调用我们的方法，看看结果。

```py
analyst_agent_react.run("How many active customers from the United Kingdom do we have?")
```

我们可以注意到模型采用了稍微不同的推理框架。模型开始时写下思考（推理），然后转到动作（函数调用）和观察（函数调用的结果）。然后，迭代重复。最后，模型返回`动作 = 最终答案`。

```py
> Entering new AgentExecutor chain...
Thought: To answer this question, I need to define what is meant by 
"active customers" and then query the database for users from 
the United Kingdom who meet this criteria. I will first need to know 
the structure of the `ecommerce.users` table to understand what columns 
are available that could help identify active customers and their location.

Action:
```

{

"动作": "获取表列",

"动作输入": {

    "数据库": "电子商务",

    "表": "用户"

}

}

```py

Observation: [{'name': 'user_id', 'type': 'UInt64'}, {'name': 'country', 'type': 'String'}, {'name': 'is_active', 'type': 'UInt8'}, {'name': 'age', 'type': 'UInt64'}]
Thought:The `ecommerce.users` table contains a column named `is_active` 
which likely indicates whether a customer is active or not, and a `country` 
column which can be used to filter users by their location. Since we 
are interested in active customers from the United Kingdom, we can use 
these two columns to query the database.

Action:
```

{

"动作": "执行SQL",

"动作输入": {

    "查询": "SELECT COUNT(*) AS active_customers_uk FROM ecommerce.users WHERE country = 'United Kingdom' AND is_active = 1"

}

}

```py
Observation: 111469

Thought:I have the information needed to respond to the user's query.
Action:
```

{

"动作": "最终答案",

"动作输入": "来自英国的活跃客户有111,469个。"

}

```py

> Finished chain.
'There are 111,469 active customers from the United Kingdom.'
```

尽管模型采用了不同的路径（从理解表格模式开始，然后执行SQL），但它得出了相同的结果。

现在，让我们继续实验方法。在 LangChain 中，有实验性的代理类型。它们尚不建议用于生产。然而，尝试使用它们并查看它们的工作效果会很有趣。

## 计划与执行代理

*下面的代码基于来自 LangChain 厨房书的* [*示例*](https://github.com/langchain-ai/langchain/blob/master/cookbook/plan_and_execute_agent.ipynb) *。*

该代理遵循“计划与执行”方法，与我们之前查看的“行动”代理不同。这一方法受 BabyAGI 框架和 [论文《计划与解决提示》](https://arxiv.org/abs/2305.04091) 的启发。

这种方法的特点是代理首先尝试规划下一步，然后执行它们。

这种方法有两个组成部分：

+   规划器 — 一个常规的大型语言模型，其主要目标是进行推理和规划，

+   执行者 — 行动代理，是一个拥有可以用来执行的工具集合的 LLM。

这种方法的优势在于你有一个分离：一个模型专注于规划（推理），另一个模型专注于执行（行动）。它更具模块化，可能你可以使用针对特定任务微调的小型且便宜的模型。然而，这种方法也会产生更多的 LLM 调用，因此如果我们使用 ChatGPT，会更昂贵。

让我们初始化规划器和执行者。

```py
from langchain_experimental.plan_and_execute import PlanAndExecute, load_agent_executor, load_chat_planner

model = ChatOpenAI(temperature=0.1, model = 'gpt-4-1106-preview')
planner = load_chat_planner(model)
executor = load_agent_executor(model, 
    tools = [execute_sql, get_table_columns, get_table_column_distr], 
    verbose=True)
```

目前没有办法为执行者指定自定义提示，因为你不能将其传递给 [该函数](https://github.com/langchain-ai/langchain/blob/master/libs/experimental/langchain_experimental/plan_and_execute/executors/agent_executor.py)。然而，我们可以破解提示，将我们的初始系统消息添加到默认提示的开头，以提供一些关于任务的背景。

> 免责声明：覆盖对象字段是一种不好的做法，因为我们可能会绕过一些提示验证。我们现在这样做只是为了实验这种方法。这样的解决方案不适用于生产。

```py
executor.chain.agent.llm_chain.prompt.messages[0].prompt.template = system_message + '\n' + \
    executor.chain.agent.llm_chain.prompt.messages[0].prompt.template
```

现在，是时候定义一个代理并执行我们之前询问的相同查询了。

```py
analyst_agent_plan_and_execute = PlanAndExecute(
    planner=planner, 
    executor=executor
)
analyst_agent_plan_and_execute.run("How many active customers from the United Kingdom do we have?")
```

调用返回了一个错误：`RateLimitError: Error code: 429 — {'error': {'message': '请求超出 gpt-4–1106-preview 组织内基于令牌使用的每分钟限制：限制 150000，要求 235832。', 'type': 'tokens_usage_based', 'param': None, 'code': 'rate_limit_exceeded'}}`。看起来模型尝试向 OpenAI 发送了过多的令牌。

让我们通过查看模型的输出（可以在下面找到）来尝试理解发生了什么：

+   首先，模型决定查看 `ecommerce.users` 和 `ecommerce.sessions` 列，以确定“活跃”客户的标准。

+   它意识到需要在 `ecommerce.users` 表中使用 `is_active`。然而，模型决定还应该使用会话数据来定义客户的活动。

+   然后，模型深入挖掘试图定义`ecommerce.sessions`中近期活动的标准。它查看了`action_date`、`session_duration`和`revenue`的分布情况。

+   最终，它将活跃客户定义为在过去30天内有过会话的客户，并且会话时长和收入高于某些阈值，而忽略了可以直接使用`is_active`。模型计划查看数据以定义这些阈值。

+   最终错误是由于尝试加载所有客户的user_ids，因为ChatGPT的令牌太多。

尽管有很多文本，我鼓励你阅读一下，看看模型是如何思考问题的。

```py
> Entering new AgentExecutor chain...
Action:
```

{

"action": "Final Answer",

"action_input": "有关客户的数据存储在SQL数据库中，特别是在名为'ecommerce.users'的表中。"

}

```py

> Finished chain.

> Entering new AgentExecutor chain...
Thought: To determine the criteria for an "active" customer, I need 
to understand what data is available in the 'ecommerce.users' and 
'ecommerce.sessions' tables. This will help me identify which columns 
could be used to define an "active" customer based on recent purchases, 
account activity, or other engagement metrics.

Action:
```

{

"action": "get_table_columns",

"action_input": {

    "database": "ecommerce",

    "table": "users"

}

}

```py

Observation: [{'name': 'user_id', 'type': 'UInt64'}, {'name': 'country', 'type': 'String'}, {'name': 'is_active', 'type': 'UInt8'}, {'name': 'age', 'type': 'UInt64'}]
Thought:The 'ecommerce.users' table contains columns for 'user_id', 'country',
 'is_active', and 'age'. The 'is_active' column seems to directly indicate 
whether a user is considered active or not. However, to understand 
the criteria behind this designation, we need to see if there are 
any engagement metrics in the 'ecommerce.sessions' table that could 
correlate with the 'is_active' status.

Action:
```

{

"action": "get_table_columns",

"action_input": {

    "database": "ecommerce",

    "table": "sessions"

}

}

```py

Observation: [{'name': 'user_id', 'type': 'UInt64'}, {'name': 'session_id', 'type': 'UInt64'}, {'name': 'action_date', 'type': 'Date'}, {'name': 'session_duration', 'type': 'UInt64'}, {'name': 'os', 'type': 'String'}, {'name': 'browser', 'type': 'String'}, {'name': 'is_fraud', 'type': 'UInt8'}, {'name': 'revenue', 'type': 'Float32'}]
Thought:The 'ecommerce.sessions' table includes columns such as 'user_id', 
'session_id', 'action_date', 'session_duration', 'os', 'browser', 'is_fraud', 
and 'revenue'. The 'action_date' could be used to determine recent activity, 
and 'session_duration' might indicate engagement level. 'Revenue' could 
be used to identify recent purchases. To define an "active" customer, 
we could consider users with recent 'action_date', longer 'session_duration', 
and positive 'revenue'. However, to set specific criteria, we might need 
to understand the distribution of these values, such as 
the most recent 'action_date' or average 'session_duration'. 
Let's first check the distribution of 'action_date' to see how recent 
the sessions are.

Action:
```

{

"action": "get_table_column_distr",

"action_input": {

    "database": "ecommerce",

    "table": "sessions",

    "column": "action_date",

    "n": 10

}

}

```py
Observation: ['2023-12-25', '2023-12-26', '2023-12-27', '2023-12-18', '2023-12-28', '2023-12-21', '2023-12-20', '2023-12-04', '2023-12-19', '2023-12-14']
Thought:The distribution of 'action_date' shows recent dates, with 
the latest being '2023-12-28'. This suggests that the data includes 
very recent sessions. To define an "active" customer, we could consider 
a timeframe such as the last 30 days. However, before finalizing 
this criterion, it would be beneficial to also understand 
the distribution of 'session_duration' and 'revenue' to see if there 
are common thresholds that could further define an "active" customer. 
Let's check the distribution of 'session_duration' next.

Action:
```

{

"action": "get_table_column_distr",

"action_input": {

    "database": "ecommerce",

    "table": "sessions",

    "column": "session_duration",

    "n": 10

}

}

```py
Observation: [772, 860, 887, 678, 405, 671, 670, 707, 982, 907]
Thought:The distribution of 'session_duration' shows a range of values, 
with durations varying from 405 to 982 seconds. These values could 
be used to set a threshold for engagement level, but without context, 
it's difficult to determine what would be considered a high or 
low session duration. To get a better understanding, we should look 
at the distribution of 'revenue' to see if there's a common revenue 
threshold that could be used in conjunction with 'action_date' and 
'session_duration' to define an "active" customer. Let's check 
the distribution of 'revenue' next.

Action:
```

{

"action": "get_table_column_distr",

"action_input": {

    "database": "ecommerce",

    "table": "sessions",

    "column": "revenue",

    "n": 10

}

}

```py

Observation: [0.0, 1556.9, 7803.2, 4936.5, 619.6, 9947.7, 646.2, 3251.1, 2860.9, 4376.5]
Thought:The distribution of 'revenue' shows a range of values including zero, 
which could indicate sessions without purchases, and values up to 9947.7,
 which suggests significant purchases. To define an "active" customer, 
we could consider those with non-zero revenue within the last 30 days 
and with a session duration above a certain threshold. However, to set 
a precise threshold for 'session_duration' and 'revenue', we would need 
to analyze the data further, possibly calculating averages or percentiles. 
For now, we can propose a preliminary definition of an "active" customer
 as one who has had a session within the last 30 days, with a session duration 
and revenue above certain thresholds to be determined.

Action:
```

{

"action": "Final Answer",

"action_input": "根据'ecommerce.users'和'ecommerce.sessions'表中可用的数据，可以初步定义一个'活跃'客户为在过去30天内有过会话的客户，其会话时长和收入高于某些阈值。'users'表中的'is_active'列可能已经反映了这种定义或类似定义，但需要进一步分析以设定'session_duration'和'revenue'的具体阈值。这些阈值可以通过计算数据分布的平均值或百分位数来确定。"

}

```py

> Finished chain.

> Entering new AgentExecutor chain...
Action:
```

{

"action": "get_table_columns",

"action_input": {

    "database": "ecommerce",

    "table": "users"

}

}

```py

Observation: [{'name': 'user_id', 'type': 'UInt64'}, {'name': 'country', 'type': 'String'}, {'name': 'is_active', 'type': 'UInt8'}, {'name': 'age', 'type': 'UInt64'}]
Thought:The 'ecommerce.users' table contains a 'country' column which 
can be used to filter the customer records based on the location 
being the United Kingdom. I will write and execute an SQL query 
to retrieve the user IDs of customers located in the United Kingdom.

Action:
```

{

"action": "execute_sql",

"action_input": {

    "query": "SELECT user_id FROM ecommerce.users WHERE country = 'United Kingdom'"

}

}

```py

Observation: 
1000001
1000011
1000021
1000029
1000044
... <many more lines...>
```

这是一个很好的例子，说明代理在问题上过于复杂化并进入了过多细节。人类分析师也会不时犯这样的错误。因此，看到LLM行为中的类似模式是很有趣的。

如果我们反思一下如何潜在地解决这个问题，有几种方法：

+   首先，我们可以防止在尝试从数据库获取过多数据时出现问题，如果`execute_sql`函数的输出超过1K行，则返回错误。

+   另一个我会考虑的是允许LLM提出后续问题，并指示它不要做出假设。

让我们继续研究启发当前方法的 BabyAGI 方法。

## 带工具的 BabyAGI 代理

*下面的代码基于* [*示例*](https://github.com/langchain-ai/langchain/blob/master/cookbook/baby_agi_with_agent.ipynb) *来自 LangChain 的食谱。*

与之前的方法类似，我们的另一个实验方法 BabyAGI 尝试先计划然后执行。

这种方法使用检索，因此我们需要设置向量存储和嵌入模型。我使用开源且轻量级的 [Chroma](https://python.langchain.com/docs/integrations/vectorstores/chroma) 进行存储和 OpenAI 嵌入。

```py
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.vectorstores import Chroma

embedding = OpenAIEmbeddings()
persist_directory = 'vector_store'

vectordb = Chroma(
    persist_directory=persist_directory,
    embedding_function=embedding
)
```

检索允许模型长期存储所有结果，并提取和传递仅最相关的结果。如果你想了解更多关于检索的内容，请阅读 [我关于 RAG 的文章](https://medium.com/towards-data-science/rag-how-to-talk-to-your-data-eaf5469b83b0)（检索增强生成）。

首先，我们将创建一个待办事项链，稍后将用作我们执行器的工具。

```py
from langchain.chains import LLMChain
from langchain.prompts import PromptTemplate

todo_prompt_message = '''
You are a planner who is an expert at coming up with a todo list for 
a given objective. Come up with a todo list for this objective: {objective}
'''

todo_prompt = PromptTemplate.from_template(todo_prompt_message)
todo_chain = LLMChain(llm=OpenAI(temperature=0.1, 
    model = 'gpt-4-1106-preview'), prompt=todo_prompt)
```

然后，我们将创建一个指定工具和提示的代理。

```py
from langchain.agents import AgentExecutor, Tool, ZeroShotAgent
from langchain.prompts import PromptTemplate

tools = [
    execute_sql, 
    get_table_columns, 
    get_table_column_distr,
    Tool(
        name="TODO",
        func=todo_chain.run,
        description="useful for when you need to come up with todo lists. Input: an objective to create a todo list for. Output: a todo list for that objective. Please be very clear what the objective is!",
    )
]

prefix = """
You are an AI who performs one task based on the following objective: {objective}. Take into account these previously completed tasks: {context}.

You are asked questions related to analytics for e-commerce product.
Your work is very important, since your product team makes decisions based on the data you provide. So, you are extremely accurate with the numbers you provided. 
If you're not sure about the details of the request, you don't provide the answer and ask follow-up questions to have a clear understanding.
You are very helpful and try your best to answer the questions.

All the data is stored in SQL Database. Here is the list of tables (in the format <database>.<table>) with descriptions:
- ecommerce.users - information about the customers, one row - one customer
- ecommerce.sessions - information about the sessions customers made on our web site, one row - one session
"""

suffix = """Question: {task}
{agent_scratchpad}"""

prompt = ZeroShotAgent.create_prompt(
    tools,
    prefix=prefix,
    suffix=suffix,
    input_variables=["objective", "task", "context", "agent_scratchpad"],
)

llm = OpenAI(temperature=0.1)
llm_chain = LLMChain(llm=llm, prompt=prompt)
tool_names = [tool.name for tool in tools]
analyst_agent_babyagi = ZeroShotAgent(llm_chain=llm_chain, allowed_tools=tool_names)
analyst_agent_babyagi_executor = AgentExecutor.from_agent_and_tools(
    agent=analyst_agent_babyagi, tools=tools, verbose=True
)
```

最后一步是定义 BabyAGI 执行器并运行它。

```py
from langchain_experimental.autonomous_agents import BabyAGI
baby_agi = BabyAGI.from_llm(
    llm=llm,
    vectorstore=vectordb,
    task_execution_chain=analyst_agent_babyagi_executor,
    verbose=True,
    max_iterations=10
)
baby_agi("Find, how many active customers from the United Kingdom we have.")
```

再次，模型未能返回结果，因为它无法遵循工具的输入模式。

而且，令人惊讶的是，模型决定不使用待办事项功能来创建待办列表，而是直接跳入 SQL 查询。然而，第一个查询不正确。模型尝试恢复并调用`get_table_columns`函数以获取列名，但未能遵循模式。

让我们查看日志。

```py
*****TASK LIST*****

1: Make a todo list

*****NEXT TASK*****

1: Make a todo list

> Entering new AgentExecutor chain...
Thought: I need to find out how many active customers from the United Kingdom 
we have
Action: execute_sql
Action Input: SELECT COUNT(*) FROM ecommerce.users WHERE country = 'UK' AND active = 1
Observation: Code: 47\. DB::Exception: Missing columns: 'active' while processing query: 
'SELECT count() FROM ecommerce.users WHERE (country = 'UK') AND (active = 1)', 
required columns: 'country' 'active', maybe you meant: 'country'. 
(UNKNOWN_IDENTIFIER) (version 23.12.1.414 (official build))

Thought: I need to get the columns of the ecommerce.users table
Action: get_table_columns
Action Input: ecommerce.users
```

所以，我们看到另一个问题，这在未使用 OpenAI 函数的代理中非常常见——它们无法遵循结构。

## 带工具的 AutoGPT 代理

*下面的代码基于* [*示例*](https://github.com/langchain-ai/langchain/blob/master/cookbook/autogpt/marathon_times.ipynb) *来自 LangChain 的食谱。*

让我们看看另一种实验方法——使用 LangChain 框架实现的 [AutoGPT](https://github.com/Significant-Gravitas/AutoGPT)。

再次，我们需要为中间步骤设置向量存储。

```py
embedding = OpenAIEmbeddings()
from langchain.vectorstores import Chroma
persist_directory = 'autogpt'

vectordb = Chroma(
    persist_directory=persist_directory,
    embedding_function=embedding
)
```

在这种情况下，我们再次无法向模型指定任何提示。让我们尝试在没有任何具体指导的情况下使用它。但我们将添加 `get_tables` 工具，以便模型可以查看所有可用的表。我希望这能帮助模型编写正确的 SQL 查询。

```py
@tool()
def get_tables() -> str:
    """Returns list of tables in the format <database>.<table>"""

    return ['ecommerce.users', 'ecommerce.sessions']
```

让我们创建一个 AutoGPT 代理。这只需一个函数调用即可完成。然后，我们执行它，看看它是如何工作的。

```py
 from langchain_experimental.autonomous_agents import AutoGPT

analyst_agent_autogpt = AutoGPT.from_llm_and_tools(
    ai_name="Harry",
    ai_role="Assistant",
    tools= [execute_sql, get_table_columns, 
        get_table_column_distr, get_tables],
    llm=ChatOpenAI(temperature=0.1, model = 'gpt-4-1106-preview'),
    memory=vectordb.as_retriever(),
)

analyst_agent_autogpt.chain.verbose = True

analyst_agent_autogpt.run(["Find how many active customers from the United Kingdom we have."])
```

模型能够给出正确的答案：“来自英国的活跃客户数量是 111,469。”

阅读提示是有趣的，因为我们使用了默认提示。你可以通过 `analyst_agent_autogpt.chain.prompt` 访问它。

```py
System: You are Harry, Assistant
Your decisions must always be made independently without seeking user 
assistance.
Play to your strengths as an LLM and pursue simple strategies with 
no legal complications.
If you have completed all your tasks, make sure to use the "finish" command.

GOALS:

1\. Find how many active customers from the United Kingdom we have.

Constraints:
1\. ~4000 word limit for short term memory. Your short term memory is short, 
so immediately save important information to files.
2\. If you are unsure how you previously did something or want to recall 
past events, thinking about similar events will help you remember.
3\. No user assistance
4\. Exclusively use the commands listed in double quotes e.g. "command name"

Commands:
1\. execute_sql: execute_sql(query: str) -> str - Returns the result of SQL query execution, args json schema: {"query": {"title": "Query", "description": "SQL query to execute", "type": "string"}}
2\. get_table_columns: get_table_columns(database: str, table: str) -> str - Returns list of table column names and types in JSON, args json schema: {"database": {"title": "Database", "description": "Database name", "type": "string"}, "table": {"title": "Table", "description": "Table name", "type": "string"}}
3\. get_table_column_distr: get_table_column_distr(database: str, table: str, column: str, n: int = 10) -> str - Returns top n values for the column in JSON, args json schema: {"database": {"title": "Database", "description": "Database name", "type": "string"}, "table": {"title": "Table", "description": "Table name", "type": "string"}, "column": {"title": "Column", "description": "Column name", "type": "string"}, "n": {"title": "N", "description": "Number of rows, default limit 10", "type": "integer"}}
4\. get_tables: get_tables() -> str - Returns list of tables in the format <database>.<table>, args json schema: {}
5\. finish: use this to signal that you have finished all your objectives, args: "response": "final response to let people know you have finished your objectives"

Resources:
1\. Internet access for searches and information gathering.
2\. Long Term memory management.
3\. GPT-3.5 powered Agents for delegation of simple tasks.
4\. File output.

Performance Evaluation:
1\. Continuously review and analyze your actions to ensure you are 
performing to the best of your abilities.
2\. Constructively self-criticize your big-picture behavior constantly.
3\. Reflect on past decisions and strategies to refine your approach.
4\. Every command has a cost, so be smart and efficient. Aim to complete 
tasks in the least number of steps.

You should only respond in JSON format as described below 
Response Format: 
{
    "thoughts": {
        "text": "thought",
        "reasoning": "reasoning",
        "plan": "- short bulleted\n- list that conveys\n- long-term plan",
        "criticism": "constructive self-criticism",
        "speak": "thoughts summary to say to user"
    },
    "command": {
        "name": "command name",
        "args": {
            "arg name": "value"
        }
    }
} 
Ensure the response can be parsed by Python json.loads
System: The current time and date is Sat Dec 16 18:39:49 2023
System: This reminds you of these events from your past:
[]
```

所以，我们可以看到这个模型通过向量存储具有长期记忆。此外，它还具备全面的推理，包括思考、理由、计划、批评和总结。

让我们看看模型的推理（我只过滤了模型的回答）。所以，它遵循了与之前成功的代理相同的方法。

```py
{
    "thoughts": {
        "text": "To find the number of active customers from the United Kingdom, I need to identify the relevant database and table that contains customer information. Once I have that, I can look for columns that might indicate customer activity and country. The first step is to list all the available tables.",
        "reasoning": "Understanding the structure of the database is essential before I can execute a query to find the active customers. Listing tables is a logical first step to identify where customer data might be stored.",
        "plan": "- Use the get_tables command to list all available tables.\n- Identify the table that likely contains customer information.\n- Use get_table_columns to understand the structure of the customer table.\n- Execute an SQL query to count active UK customers.",
        "criticism": "I need to ensure that I'm efficient with the commands I use, aiming to gather the necessary information in as few steps as possible.",
        "speak": "I am now going to list all the available tables to find where customer information is stored."
    },
    "command": {
        "name": "get_tables",
        "args": {}
    }
}

{
    "thoughts": {
        "text": "The 'ecommerce.users' table is likely to contain information about the customers. To find the number of active customers from the United Kingdom, I need to understand the structure of this table, specifically looking for columns that might indicate whether a customer is active and their country.",
        "reasoning": "Knowing the structure of the 'users' table will help me determine which columns to use in an SQL query to count active UK customers.",
        "plan": "- Use the get_table_columns command to get the structure of the 'ecommerce.users' table.\n- Identify columns that indicate customer activity and country.\n- Formulate an SQL query to count active UK customers based on the identified columns.",
        "criticism": "I must continue to be efficient with my commands, ensuring that each step brings me closer to the goal without unnecessary actions.",
        "speak": "I will now retrieve the structure of the 'ecommerce.users' table to identify the relevant columns for active customers and their country."
    },
    "command": {
        "name": "get_table_columns",
        "args": {
            "database": "ecommerce",
            "table": "users"
        }
    }
}

{
    "thoughts": {
        "text": "With the structure of the 'ecommerce.users' table known, I can see that there are columns for 'country' and 'is_active', which are necessary to identify active customers from the United Kingdom.",
        "reasoning": "The 'country' column will allow me to filter customers by their country, and the 'is_active' column will allow me to filter customers by their activity status. With this information, I can construct an SQL query to count the number of active customers from the UK.",
        "plan": "- Execute an SQL query using the 'execute_sql' command to count the number of active customers from the United Kingdom.\n- The query will count rows where 'country' is 'United Kingdom' and 'is_active' is true (1).",
        "criticism": "I need to ensure that the SQL query is correctly formulated to retrieve the exact information needed without any errors.",
        "speak": "I am now ready to execute an SQL query to count the number of active customers from the United Kingdom."
    },
    "command": {
        "name": "execute_sql",
        "args": {
            "query": "SELECT COUNT(*) FROM ecommerce.users WHERE country = 'United Kingdom' AND is_active = 1"
        }
    }
}

{
    "thoughts": {
        "text": "I have successfully executed the SQL query and obtained the number of active customers from the United Kingdom, which is 111,469.",
        "reasoning": "The goal was to find the number of active customers from the UK, and the SQL query has provided this information.",
        "plan": "- Since the goal has been achieved, the next step is to use the 'finish' command to signal that I have completed all my objectives.",
        "criticism": "There is no need for further action or commands as the objective has been met.",
        "speak": "I have found the number of active customers from the United Kingdom and am ready to finish the task."
    },
    "command": {
        "name": "finish",
        "args": {
            "response": "The number of active customers from the United Kingdom is 111,469."
        }
    }
} 
```

总体来说，AutoGPT看起来是一种可行的方法。然而，我仍然相信，目前构建代理的最可靠方法是通过OpenAI的函数。

# 我们需要从头开始构建一切吗？

我们在构建集成了SQL数据库的代理时度过了一段美好的学习时光。然而，我必须提到LangChain有自己实现的SQL代理——[SQLDatabaseChain](https://python.langchain.com/docs/integrations/toolkits/sql_database)。

这种方法使用SQL Alchemy与数据库进行交互。因此，我们需要安装`clickhouse-sqlalchemy`包以连接到ClickHouse。

```py
pip install clickhouse-sqlalchemy
```

我们可以建立与数据库的连接并初始化工具包。

```py
uri = 'clickhouse+native://localhost/ecommerce'
db = SQLDatabase.from_uri(uri)
toolkit = SQLDatabaseToolkit(db=db, llm=OpenAI(temperature=0))
```

工具包是与某个主题相关的有用工具的集合。你可以在[文档](https://python.langchain.com/docs/integrations/toolkits)中找到许多示例。

我们可以看到工具包中拥有的工具列表。这里有用于进行SQL查询或获取与数据库相关信息的工具。

```py
toolkit.get_tools()
```

然后，我们可以快速创建并运行一个基于OpenAI函数的代理。

```py
agent_executor = create_sql_agent(
    llm=ChatOpenAI(temperature=0.1, model = 'gpt-4-1106-preview'),
    toolkit=toolkit,
    verbose=True,
    agent_type=AgentType.OPENAI_FUNCTIONS
)

agent_executor.run("How many active customers from the United Kingdom do we have?")
```

我们在这边没有多大麻烦就得到了正确的答案。

```py
> Entering new AgentExecutor chain...

Invoking: `sql_db_list_tables` with ``

sessions, users
Invoking: `sql_db_schema` with `users`

CREATE TABLE users (
 user_id UInt64, 
 country String, 
 is_active UInt8, 
 age UInt64
) ENGINE = Log

/*
3 rows from users table:
user_id country is_active age
1000001 United Kingdom 0 70
1000002 France 1 87
1000003 France 1 88
*/
Invoking: `sql_db_query` with `SELECT COUNT(*) FROM users WHERE country = 'United Kingdom' AND is_active = 1`

[(111469,)]We have 111,469 active customers from the United Kingdom.

> Finished chain.
'We have 111,469 active customers from the United Kingdom.'
```

我们可以使用`langchain.debug = True`来查看使用了什么提示。

```py
System: You are an agent designed to interact with a SQL database.
Given an input question, create a syntactically correct clickhouse query 
to run, then look at the results of the query and return the answer.
Unless the user specifies a specific number of examples they wish to obtain, 
always limit your query to at most 10 results.
You can order the results by a relevant column to return the most interesting 
examples in the database.
Never query for all the columns from a specific table, only ask for 
the relevant columns given the question.
You have access to tools for interacting with the database.
Only use the below tools. Only use the information returned 
by the below tools to construct your final answer.
You MUST double check your query before executing it. If you get 
an error while executing a query, rewrite the query and try again.

DO NOT make any DML statements (INSERT, UPDATE, DELETE, DROP etc.) 
to the database.

If the question does not seem related to the database, just return 
"I don't know" as the answer.

Human: How many active customers from the United Kingdom do we have? 
AI: I should look at the tables in the database to see what I can query.  
Then I should query the schema of the most relevant tables.
```

因此，我们有了一个相当方便且有效的SQL分析实现。如果你不需要任何自定义更改，你可以直接使用LangChain的实现。

此外，你可以稍作调整，例如，通过将提示传递给`create_sql_agent`函数（[文档](https://api.python.langchain.com/en/latest/agent_toolkits/langchain_community.agent_toolkits.sql.base.create_sql_agent.html?highlight=create_sql_agent#langchain_community.agent_toolkits.sql.base.create_sql_agent)）。

# 摘要

今天，我们学习了如何创建不同类型的代理。我们实现了一个完全从零开始的LLM驱动代理，可以处理SQL数据库。然后，我们利用高层次的LangChain工具，通过几个函数调用实现了相同的结果。

所以，现在我们的LLM驱动分析师可以使用你的数据库中的数据并回答问题。这是一个重大改进。我们可以将SQL数据库代理作为工具添加到我们的LLM驱动分析师中。这将是我们的第一个技能。

代理现在可以回答与数据相关的问题并自行工作。然而，分析工作的基石是合作。因此，在接下来的文章中，我们将添加记忆功能和学习代理，以便提出后续问题。敬请关注！

> 非常感谢你阅读这篇文章。我希望它对你有所启发。如果你有任何后续问题或评论，请在评论区留言。
