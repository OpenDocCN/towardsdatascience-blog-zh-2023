# 使用 LangChain 和 LLMs 的业务分析

> 原文：[`towardsdatascience.com/business-analytics-with-langchain-and-llms-c8e902446073`](https://towardsdatascience.com/business-analytics-with-langchain-and-llms-c8e902446073)

## 生成式 AI

## 一个关于用人类语言查询 SQL 数据库的分步教程

[](https://tamimi-naser.medium.com/?source=post_page-----c8e902446073--------------------------------)![Naser Tamimi](https://tamimi-naser.medium.com/?source=post_page-----c8e902446073--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c8e902446073--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----c8e902446073--------------------------------) [Naser Tamimi](https://tamimi-naser.medium.com/?source=post_page-----c8e902446073--------------------------------)

·发表于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----c8e902446073--------------------------------) ·7 分钟阅读·2023 年 12 月 19 日

--

![](img/5540ac3aed1e97360f2bf8286e9d3663.png)

图片由作者提供（通过 Midjourney 生成）

许多企业在其数据库中存储了大量专有数据。如果有一个能够理解人类语言并查询这些数据库的虚拟代理，这将为这些企业打开巨大的机会。想想客户服务聊天机器人，它们就是一个常见的例子。这些代理可以接受客户请求，向数据库询问信息，并向客户提供他们所需的内容。

这些代理的好处不仅限于外部客户互动。许多企业主或公司中的人员，即使是在科技公司，也可能不知道 SQL 或类似的语言，但他们仍然需要向数据库查询信息。这就是像 LangChain 这样的框架派上用场的地方。这些框架使得创建这些有用的代理/应用程序变得容易。那些可以与人类对话，同时与数据库、API 等进行交互的代理。

# LLM 支持的应用程序

LangChain 是一个开源框架，用于构建使用大型语言模型（LLMs）的互动应用程序。这是一个帮助 LLMs 连接其他信息来源，并使其能够与周围世界对话的工具。在这些框架中，一个重要的概念是链（Chain）。让我们来看看这个概念。

## 什么是链（Chains）？

链接是这个框架中的高级工具，它们将 LLM 与其他工具结合起来，以执行更复杂的任务。具体来说，链是使用 LLM 序列和其他工具（如 SQL 数据库、API 调用、bash 操作符或数学计算器）来完成复杂工作的接口。例如，我们的应用程序接收来自用户的输入并将其传递给我们的 LLM 模型；然后，LLM 调用一个 API。API 对 LLM 进行响应，LLM 使用响应执行另一个任务，如此循环往复。正如你所看到的，这是一系列输入和输出的链条，其中在许多部分，我们有 LLM 模型处理情况。

# 开始动手实践

现在是时候开始动手编写一个简单的 LLM 支持的应用程序了。对于这个应用程序，我们将制作一个简单的问答代理，它接收我们的问题并查询 SQL 数据库以为我们找到答案。

# 设置 PostgreSQL 示例数据库

我们使用了来自 postgresqltutorial.com 的 [DVD Rental Sample Database](https://www.postgresqltutorial.com/postgresql-getting-started/postgresql-sample-database/)（[许可证信息](https://dev.mysql.com/doc/sakila/en/sakila-license.html)）。要在你的本地系统上安装数据库，你需要安装 PostgreSQL。在终端中简单地输入 `*psql*` 看看它是否运行。

然后你应该按照 [这里](https://www.postgresqltutorial.com/postgresql-getting-started/load-postgresql-sample-database/) 的说明进行操作。这里我将快速带你完成数据库安装过程。

在你的终端中通过以下命令启动 PostgreSQL：

```py
psql -U <USERNAME>
```

你必须将 `<USERNAME>` 替换为你的实际用户名。系统会询问你的密码。输入你的密码后，你将进入你的 Postgres 数据库。

首先，我们应该创建一个数据库以加载所有的表。

```py
postgres=# CREATE DATABASE dvdrental;
```

创建数据库后，你可以通过输入 `*\list*` 命令来检查它，你应该会在返回的列表中看到你的 dvdrental 数据库。然后简单地退出 Postgres（通过 `\q`）。

现在我们需要下载表和数据。你可以在这里下载所有内容的 tar 文件：[下载 DVD Rental Sample Database](https://www.postgresqltutorial.com/wp-content/uploads/2019/05/dvdrental.zip)

进入你下载 tar 文件的文件夹，使用 `pg_restore`，我们可以将表加载到我们的 Postgres 数据库中。

```py
pg_restore -U postgres -d dvdrental dvdrental.tar
```

在继续之前，让我们检查表是否已加载。通过 `psql -U <USERNAME>` 启动 Postgres 并输入你的密码。

然后输入以下命令以列出 dvdrental 数据库中的表。

```py
postgres=# \c dvdrental
dvdrental=# \dt
```

你应该看到一个如下所示的表列表：

```py
 List of relations
 Schema |     Name      | Type  |  Owner
--------+---------------+-------+----------
 public | actor         | table | postgres
 public | address       | table | postgres
 public | category      | table | postgres
 public | city          | table | postgres
 public | country       | table | postgres
 public | customer      | table | postgres
 public | film          | table | postgres
 public | film_actor    | table | postgres
 public | film_category | table | postgres
 public | inventory     | table | postgres
 public | language      | table | postgres
 public | payment       | table | postgres
 public | rental        | table | postgres
 public | staff         | table | postgres
 public | store         | table | postgres
```

# 设置 .env 文件

在构建示例数据库之后，我们需要创建一个 `.env` 文件。我们使用此文件来存储我们的秘密、密钥以及环境变量。当然，我们可以将所有这些信息放在代码中，但即使是小项目也必须遵循最佳工程实践。意外的 git push 可能会将我们的 API 密钥和秘密暴露给公众。以下是我们的 `.env` 文件：

```py
OPENAI_API_KEY = 'YOUR-OPENAI-API-KEY'
PSQL_HOST = 'localhost'
PSQL_PORT = 'YOUR-POSTGRES-PORT'
PSQL_USERNAME = 'YOUR-POSTGRES-USERNAME'
PSQL_PASSWORD = 'YOUR-POSTGRES-PASSWORD'
PSQL_DB = 'dvdrental'
```

正如你所见，你需要获得自己的 OpenAI API 密钥进行测试。你可以按照 [这里](https://medium.com/@rithin_9167/how-to-obtain-openai-api-key-30df0b8bd114) 的说明从 Open AI 获取 API 密钥。

# 创建一个 LangChain 应用程序

在将 .env 文件保存到你的项目文件夹后，我们可以开始实际的代码。首先，我们将所需的库导入到 Python 代码中。

```py
pip install langchain langchain-experimental openai psycopg2
```

对于本教程，我建议使用 Jupyter Notebook 逐步测试它。

我们需要在开始时导入所需的库。

```py
from os import environ
from dotenv import load_dotenv

from langchain.chat_models import ChatOpenAI
from langchain.utilities import SQLDatabase
from langchain_experimental.sql import SQLDatabaseChain

load_dotenv()
```

使用 load_dotenv()，我们加载了在 `.env` 文件中定义的环境变量。现在，我们可以安全地在代码中访问它们。

对于我们的链，我们需要一个具有聊天功能的语言模型。为简单起见，我们选择了来自 OpenAI 的 gpt-3.5-turbo，该模型可以通过 API 访问。你可以使用任何其他公共或私有模型。

```py
# Instantiating Our LLM

OPENAI_API_KEY = environ.get("OPENAI_API_KEY")
llm = ChatOpenAI(model_name='gpt-3.5-turbo', temperature=0, openai_api_key=OPENAI_API_KEY)
```

除了模型，我们还需要一个 SQL 数据库连接。该连接使我们的链能够对数据库进行查询并获取结果。如前所述，我们使用的是 PostgreSQL 数据库，但根据 LangChain 的说法，任何具有 JDBC 连接的 SQL 引擎都应该容易使用（例如 MySQL、Presto、Databricks）。

```py
# Database Setup

PSQL_HOST = environ.get("PSQL_HOST")
PSQL_PORT = environ.get("PSQL_PORT")
PSQL_USERNAME = environ.get("PSQL_USERNAME")
PSQL_PASSWORD = environ.get("PSQL_PASSWORD")
PSQL_DB = environ.get("PSQL_DB")
psql_uri = f"postgresql+psycopg2://{PSQL_USERNAME}:{PSQL_PASSWORD}@{PSQL_HOST}:{PSQL_PORT}/{PSQL_DB}"

db = SQLDatabase.from_uri(psql_uri, sample_rows_in_table_info=3)
db.get_usable_table_names()
```

如果你对我们在 SQL 数据库连接中使用的 `sample_rows_in_table_info` 参数感到好奇，Rajkumar 等人在他们的论文中（[`arxiv.org/abs/2204.00498`](https://arxiv.org/abs/2204.00498)）展示了从表中包含几行样本数据可以提高模型在创建模式下查询数据的性能。在 LangChain 中，你可以简单地设置 `sample_rows_in_table_info`，并确定从每个表中提取的样本行数，这些样本行将被附加到每个表的描述中。

为了测试 SQL 数据库连接，我使用 `db.get_usable_table_names()` 打印了可用表的列表。它也应该返回以下表的列表给你。

```py
['actor',
 'address',
 'category',
 'city',
 'country',
 'customer',
 'film',
 'film_actor',
 'film_category',
 'inventory',
 'language',
 'payment',
 'rental',
 'staff',
 'store']
```

下一步是最重要的步骤。借助我们的 LLM 模型和 SQL 数据库连接，我们现在应该能够实例化我们的链。在展示实例化之前，让我们先对链有更多了解。

现在，我们正在使用来自 LangChain 的 SQL 链来获取人类问题，将其转换为 SQL 查询，在数据库上运行它，并检索结果。

```py
# Definining Our SQL Dabatabe Chain

db_chain = SQLDatabaseChain.from_llm(llm, db, verbose=True)
```

最后，我们使用 `run()` 方法将输入/问题传递给链，并获取最终响应。

```py
# Define a Function To Run the Chain with Our Query

def query(input: str) -> str:
    response = db_chain.run(input)
    return response
```

现在是测试代码的时候了。我们从一个简单的查询开始，该查询只需查询一个表。

```py
# Example 1:

query("What is the average length of films released in 2006")
```

该链接收我们的自然语言查询，并首先将其转换为 SQL 查询。然后，使用 SQL 数据库连接，它在数据库上运行 SQL 查询。返回的响应为 LLM 提供了上下文，并与原始人类查询一起触发了响应。在这种情况下，你可以看到最终的响应：

```py
> Entering new SQLDatabaseChain chain...
What is the average length of films released in 2006
SQLQuery:SELECT AVG(length) FROM film WHERE release_year = 2006
SQLResult: [(Decimal('115.2720000000000000'),)]
Answer:The average length of films released in 2006 is 115.272 minutes.
> Finished chain.
**'The average length of films released in 2006 is 115.272 minutes.'**
```

首先，由于 `verbose=True`（我们在其中定义了 db_chain），我们可以获得更多关于构建的 SQL 查询和返回的 SQL 结果的信息。如果你关闭 verbose，你只能看到最终结果（用粗体标记）。最终答案正确显示了 2006 年上映的电影的平均时长是 115.272 分钟（你可以在 Postgres 上使用你自己的 SQL 查询进行验证）。

第二个问题稍微复杂一些，需要连接三个表。

```py
# Example 2:

query("Which actor played in movies with total longer duration? And how much was the duration?")
```

这个问题询问的是在电影总时长超过其他任何演员的演员。这个问题需要连接三个表：actor、film_actor 和 film。以下是链响应：

```py
> Entering new SQLDatabaseChain chain...
Which actor played in movies with total longer duration? And how much was the duration?
SQLQuery:SELECT a.first_name, a.last_name, SUM(f.length) AS total_duration
FROM actor a
JOIN film_actor fa ON a.actor_id = fa.actor_id
JOIN film f ON fa.film_id = f.film_id
GROUP BY a.actor_id
ORDER BY total_duration DESC
LIMIT 1;
SQLResult: [('Mary', 'Keitel', 4962)]
Answer:Mary Keitel played in movies with a total duration of 4962 minutes.
> Finished chain.
**'Mary Keitel played in movies with a total duration of 4962 minutes.'**
```

如你所见，我们超简单的 LangChain 应用成功地理解了这些表之间的关系，并构建了 SQL 查询。

# 总结

在本文中，我们介绍了一款强大的开源工具，叫做 LangChain，它使我们能够构建基于 LLM 的应用程序。

然后我们使用 SQLDatabaseChain 构建了一个应用程序，该应用程序根据用户的问题查询 SQL 数据库并返回结果。

这个简单的问答应用可以扩展成一个更复杂的业务分析助手，供企业内部的任何人每天使用，从专有数据中获取最新的洞察。
