# Streamlit 和 MongoDB：在云端存储你的数据

> 原文：[`towardsdatascience.com/streamlit-and-mongodb-storing-your-data-in-the-cloud-c28663313ade`](https://towardsdatascience.com/streamlit-and-mongodb-storing-your-data-in-the-cloud-c28663313ade)

## **Streamlit Cloud 没有本地存储，因此在应用程序终止时创建的数据会丢失——除非你使用类似 MongoDB 的第三方存储**

[](https://medium.com/@alan-jones?source=post_page-----c28663313ade--------------------------------)![Alan Jones](https://medium.com/@alan-jones?source=post_page-----c28663313ade--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c28663313ade--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c28663313ade--------------------------------) [Alan Jones](https://medium.com/@alan-jones?source=post_page-----c28663313ade--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c28663313ade--------------------------------) ·12 分钟阅读·2023 年 8 月 25 日

--

![](img/f90175fb704850839f3927b973483025.png)

经典 NoSQL 数据库——照片由 [Jan Antonin Kolar](https://unsplash.com/@jankolar?utm_source=medium&utm_medium=referral) 提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Streamlit 允许你将公共应用程序部署到他们的云端，免费提供，但你在本地创建的任何文件或数据库将在应用结束时消失。这可能不是你想要的行为，因此我们将探讨使用 MongoDB 的解决方案。

对于许多应用程序来说，丢失这些数据并不成问题。例如，如果你设计了一个从外部源读取数据的仪表板，那么你生成的任何数据都可能是临时的，仅在应用运行期间需要。

但正如我在为文章开发应用程序时所提到的，[Simple Surveys with Streamlit](https://medium.com/towards-data-science/simple-surveys-with-streamlit-and-databutton-d027586f1c71)，如果应用程序本身生成需要存储的数据，这就不那么简单了。在那个应用程序中，我将数据存储在本地文件中，但在基于云的部署中，这些数据将在应用停止运行时消失——正确的解决方案是使用外部数据存储。

我们将看看如何通过 MongoDB 实现这一点，但也有其他选择。

## 有哪些选择？

在 Streamlit 文档中，有关于连接各种数据库和云存储供应商的指南。它们基本上分为三个领域：数据桶，例如 AWS S3 和 Google Cloud Storage，你可以在其中存储任何东西；SQL 数据库，如微软的 SQL Server、MySQL、PostgreSQL；以及 NoSQL 数据库，Firestore 和 MongoDB 就是其中的例子。对于每种类型，你显然需要访问托管该特定数据库的服务器。

坦白说，我不是 SQL 的最大粉丝。对我来说，SQL 代码和 Python 之间的脱节让人感到不适。（话虽如此，我确实欣赏 SQL 的强大功能和便利，并且在[这里](https://medium.com/towards-data-science/python-pandas-and-sqlite-a0e2c052456f)、[这里](https://medium.com/towards-data-science/duckdb-sqlite-for-data-analysis-a279bdf94399)和[这里](https://medium.com/towards-data-science/sql-pandas-or-both-analysing-the-uk-electoral-system-24fa01d33d05)写过相关内容。）

但 NoSQL 数据库如 MongoDB 感觉更符合 Python 的工作方式。

我相信关于速度、效率、易用性、安全性等各种争论都有。但我不打算讨论这些。我将使用 MongoDB，因为这是我的个人喜好和偏见，你可以自己决定它是否是一个好的选择。

## 调查应用程序

我将使用我开发的一个应用程序版本来演示在 Streamlit 中构建简单调查。我有意编写它以便于移植到不同的数据存储，因此所有的数据存储代码都在一个库中。其目的是，为了从使用本地数据存储转到基于云的数据库，你只需重新编写库即可。

但首先，我们需要访问 MongoDB 数据库。

## MongoDB Atlas

你可以下载 MongoDB 并在自己的服务器上运行一个实例。或者，你可以使用像 MongoDB Atlas 这样的托管服务。我们将使用后者。

[MongoDB Atlas](https://www.mongodb.com/) 是搜索中出现的第一个托管服务——虽然还有其他服务，但我不知道有哪个提供免费层，因此我们将使用这个。作为一种免费增值服务，你可以从零开始，这几乎满足了你对简单（或者实际上不那么简单）应用程序的所有需求，但限制你存储数据为 0.5 GB。

要使用 MongoDB，你需要注册一个帐户（使用上面的链接），并且还应阅读他们网页上资源标签中的全面入门指南。

一旦设置好，你将拥有创建连接字符串的信息，如下所示：

```py
"mongodb+srv://<user>:<password>@<cluster-url>?retryWrites=true&writeConcern=majority"
```

替换 `<user>`、`<password>` 和 `<cluster-url>` 为你自己的详细信息，并将其保留为 Streamlit 密钥。

## 一个 MongoDB 数据库

MongoDB 由 *集群* 和 *数据库* 组成。数据库内有 *集合*（有点像表），集合内有 *文档*（以类似 JSON 的格式存储的实际数据）。

为了本教程的方便，我创建了一个数据库和两个集合。这些集合将存储构成调查的问题以及用户生成的结果。我将它们命名为 `survey1` 和 `results1`——我知道这并不特别有创意。

在结果收集部分，每个文档将代表一个问题。在结果表中，每个文档将代表对调查中所有问题的单独回答。

![](img/39454eebce1fd58cfa907061c144fc20.png)

**调查** *数据库* 截图

点击其中一个集合名称将显示文档（如果有的话）。

![](img/f709b41a2864c7af6bb996964295fbdd.png)

调查 1 集合中的问题

如你所见，这些条目非常类似于 Python 字典。注意 `_id` 字段：这是一个唯一标识符，对于 MongoDB 文档来说是必需的；你可以在创建文档时指定它，否则 MongoDB 会自动为你创建一个。

## 应用程序

你可以在原始文章中阅读实现的详细信息。这里，我们将专注于数据库代码。（在本文发布后不久，将会有一个指向我[网页](http://alanjones2.github.io)上所有代码的链接。）但让我们快速回顾一下应用程序的功能。

原始应用程序由三个页面组成：

+   第一个页面允许用户通过指定单独的问题并存储它们来创建一个简单的问卷。

+   第二个页面向用户展示问卷，收集响应并将其添加到之前的响应中。

+   第三个页面展示结果；它读取响应并对其进行一些简单的分析 —— 绘制概览柱状图，并让用户选择一个问题以更详细地查看该问题的响应。数据也可以作为 CSV 文件下载。

每个页面使用一个库，*DButils*，来访问数据。在原始应用程序中，数据存储为 JSON 文件。

为了保持简单，我将专注于问卷的展示和收集的数据的展示。这意味着我们需要对数据库进行读写操作。（我的想法是，创建问卷可能最好在本地完成，并将结果存储在文件中，之后再将其完整上传到 MongoDB —— 因此，本文将不涉及应用程序的这一方面。）

## 数据库库

原始库使用了一些辅助函数，但该版本应用程序使用的函数有：

+   `get_survey()` — 这个函数用于检索整个问卷

+   `append_results(value)` — 这个函数将新的响应添加到现有响应中

+   `get_results()` — 这个函数用于检索所有的响应

对于 MongoDB 版本，我们需要一点前置准备。

```py
import streamlit as st
from pymongo.mongo_client import MongoClient

DB = "survey"
SURVEY_KEY = "survey1"
RESULTS_KEY = "results1"

uri = st.secrets['mongoURI']
client = MongoClient(uri)
```

我们显然需要导入 *Streamlit*；我们还需要从 *pymongo* 库中导入 *MongoClient*。然后，我们设置一些便利常量，将数据库和集合名称硬编码到代码中。这当然使得库特定于这个应用程序，如果我们希望在不同的应用程序中再次使用该库，可能会避免这种情况。我们更倾向于在应用程序中设置这些值，并将其传递给库。

变量 `uri` 是我们之前创建的连接字符串，通过它我们创建一个 MongoDB 客户端，从中可以访问我们的数据。

读取和写入数据非常简单。首先，你需要指定数据库以及该数据库中的集合。然后，你可以使用 `find()` 函数来读取数据，使用 `insert_one()` 或 `insert_many()` 函数将数据添加到集合中。

我们将在这里说明读取和写入函数的简单用法，但要想充分利用 MongoDB，你需要查阅他们网站上的文档，例如 [如何在 MongoDB 中使用 Python](https://www.mongodb.com/languages/python)。

## 读取数据

要从 MongoDB 集合中读取所有数据，我们使用 `find()` 函数。下面的代码将从我们其中一个集合中读取所有数据——`key` 参数应为 `SURVEY_KEY` 或 `RESULTS_KEY`。

```py
def get(key):
    coll = db[key]
    item_details = coll.find()
    return list(data)
```

从 `find()` 返回的值是一个游标，我们可以遍历它以检索数据。然而，由于我们在这里处理的数据量很小，将其转换为 Python 列表以便所有数据都保存在内存中是完全可以接受的。

但这并不是我们想要的。正如你会记得的，每个集合中的文档都有一个由 MongoDB 自动提供的唯一标识符。对于这个应用程序，我们不需要，也确实不想要那个属性。因此，让我们更仔细地查看这个函数。

## MongoDB 查询和过滤器

正如你所料，我们可以对 MongoDB 数据库进行查询。为了方便这一点，`find()` 函数可以接受修改其行为的参数。每个参数都是可选的，正如我们看到的，通过不指定这些参数，我们检索到所有的数据。

下面我们可以看到一个小调查的片段。这是集合中所有文档的数组，每个文档由 id 字段加上三个调查问题和每个问题的回答组成。

```py
[
  {
    "_id": "ObjectId('64de4b2425de44afbff94ba5')",
    "How many years of programming experience do you have?": "less than 1",
    "What programming language do you use most?": "Python",
    "Towards Data Science is one of the most useful publications on Medium": "Strongly agree"
  },
  {
    "_id": "ObjectId('64de4b3125de44afbff94ba6')",
    "How many years of programming experience do you have?": "1 to 5",
    "What programming language do you use most?": "Python",
    "Towards Data Science is one of the most useful publications on Medium": "Agree"
  },

...

]
```

如果我们只需要对某个问题的回答，可以添加一个查询参数。在下面的代码中，我们指定我们想要包含对问题“你使用最多的编程语言是什么？”的回答为 ‘R’ 的文档。该参数以 Python 字典的形式提供，你可以在代码下方立即看到响应。

```py
def get(key):
    coll = db[key]
    item_details = coll.find({'What programming language do you use most?':'R'})
    return list(item_details)
```

```py
[
  {
    "_id": "ObjectId('64df43224c943462625ec464')",
    "How many years of programming experience do you have?": "5 to 10",
    "Towards Data Science is one of the most useful publications on Medium": "Disagree",
    "What programming language do you use most?": "R"
  },
  {
    "_id": "ObjectId('64df43324c943462625ec465')",
    "How many years of programming experience do you have?": "1 to 5",
    "Towards Data Science is one of the most useful publications on Medium": "Neither agree not disagree",
    "What programming language do you use most?": "R"
  }
]
```

正如你所预期的，返回值是与查询匹配的文档集合——它相当于 SQL 中的 `SELECT * WHERE …`。

这为我们提供了 MongoDB 查询如何工作的有用见解，但并未解决省略 id 字段的问题。为此，我们需要一个第二个参数。

第二个参数也是 Python 字典的形式，并充当过滤器，例如 `{'_id':False}`。这告诉 MongoDB 我们不需要 id 字段。我们可以在此参数中指定字段列表，每个字段标记为 `True` 或 `False`，取决于我们是否希望包含它们。

如果我们将这个第二个参数添加到之前的查询中，结果将是这样：

```py
[
  {
    "How many years of programming experience do you have?": "5 to 10",
    "Towards Data Science is one of the most useful publications on Medium": "Disagree",
    "What programming language do you use most?": "R"
  },
  {
    "How many years of programming experience do you have?": "1 to 5",
    "Towards Data Science is one of the most useful publications on Medium": "Neither agree not disagree",
    "What programming language do you use most?": "R"
  }
]
```

与之前相同的条目，但省略了 id 字段。

好的，让我们回到我们真正想要的，即*所有*的文档，但省略 id 字段。我们仍然需要指定两个参数，但第一个参数需要告诉 MongoDB 我们想要所有文档，我们通过传递一个空字典来实现。

```py
def get(key):
    coll = db[key]
    item_details = coll.find({},{'_id':False})
    return list(item_details)
```

这给了我们想要的结果。

```py
[
  {
    "How many years of programming experience do you have?": "less than 1",
    "What programming language do you use most?": "Python",
    "Towards Data Science is one of the most useful publications on Medium": "Strongly agree"
  },
  {
    "How many years of programming experience do you have?": "1 to 5",
    "What programming language do you use most?": "Python",
    "Towards Data Science is one of the most useful publications on Medium": "Agree"
  },
  {
    "How many years of programming experience do you have?": "more than 10",
    "What programming language do you use most?": "Python",
    "Towards Data Science is one of the most useful publications on Medium": "Strongly agree"
  },
  {
    "How many years of programming experience do you have?": "5 to 10",
    "What programming language do you use most?": "Julia",
    "Towards Data Science is one of the most useful publications on Medium": "Neither agree not disagree"
  },
  {
    "How many years of programming experience do you have?": "more than 10",
    "What programming language do you use most?": "other",
    "Towards Data Science is one of the most useful publications on Medium": "Agree"
  },
  {
    "How many years of programming experience do you have?": "5 to 10",
    "Towards Data Science is one of the most useful publications on Medium": "Disagree",
    "What programming language do you use most?": "R"
  },
  {
    "How many years of programming experience do you have?": "1 to 5",
    "Towards Data Science is one of the most useful publications on Medium": "Neither agree not disagree",
    "What programming language do you use most?": "R"
  },
  {
    "How many years of programming experience do you have?": "more than 10",
    "Towards Data Science is one of the most useful publications on Medium": "Strongly agree",
    "What programming language do you use most?": "Python"
  },
  {
    "How many years of programming experience do you have?": "1 to 5",
    "Towards Data Science is one of the most useful publications on Medium": "Agree",
    "What programming language do you use most?": "Julia"
  }
] 
```

当我们读取调查问题时，我们想做的正是同样的事情——返回所有文档，但不包含 id 字段——因此这个函数用于读取两个数据库。我们只需要提供正确的键，无论是 `SURVEY_KEY` 还是 `RESULTS_KEY`。

## 追加数据

在这个版本的应用中，我们仅向结果集合中添加数据。具体方法如下，使用 `insert_one()` 函数完成。

```py
def append_results(value):
    coll = db[RESULTS_KEY]
    coll.insert_one(value)
```

再次，我们选择具有适当键的集合，并添加以 Python 字典列表形式表示的调查问题及其每个回答的值。正如我们之前提到的，MongoDB 会自动添加唯一的 id。我们最终得到的集合与我们上面看到的类似。

如果你需要向集合中追加一组文档，`insert_many()` 函数可以为你完成这项工作。

## 代码

库的完整代码如下——实际上非常简洁！

```py
import streamlit as st
from pymongo.mongo_client import MongoClient

DB = "survey"
SURVEY_KEY = "survey1"
RESULTS_KEY = "results1"

uri = st.secrets['mongoURI']
client = MongoClient(uri)
db = client[DB]

# Get functions
def get(key):
    coll = db[key]
    item_details = coll.find({},{'_id':False})
    return list(item_details)

def get_survey(key=SURVEY_KEY):
    return get(key)

def get_results(key=RESULTS_KEY):
    return get(key)

# Append results function
def append_results(value):
    coll = db[RESULTS_KEY]
    coll.insert_one(value)
```

## 结论

我希望你能看到，使用 MongoDB 是相当简单的，并且为 Streamlit 应用中的持久存储问题提供了一个简洁的解决方案。我们本可以使用托管的 SQL 解决方案，但正如我之前提到的，NoSQL 解决方案在我看来更符合 Python 的处理方式——将 Python 列表映射到 MongoDB 集合是完全自然的。

不过，我应该补充一点：本文中的代码是在 Streamlit 引入新的 `st.experimental_connection()` 特性之前编写的，这种方法或其他基于类的解决方案可能会为生产代码提供更好的前景，特别是当 MongoDB 将被用于多个不同的项目时。然而，对于像这样的临时项目，我在这里使用的方法似乎简单且足够。

感谢阅读，希望这对你有帮助。如果你对代码或我的方法有任何疑问，或有任何评论，或者只是想说“你好”，请在下面添加评论——我总是会回复。

当你阅读这篇文章时，我的网站上应该有指向完整应用代码的链接 [website](http://alanjones2.github.io)。在那里你可以找到其他关于 Streamlit、数据可视化、数据科学和编程的一些文章和书籍——请查看一下。
