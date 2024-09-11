# 如何将表格时间序列数据发送到 Apache Kafka，使用 Python 和 Pandas

> 原文：[`towardsdatascience.com/how-to-send-tabular-time-series-data-to-apache-kafka-with-python-and-pandas-39e2055373c3?source=collection_archive---------19-----------------------#2023-01-24`](https://towardsdatascience.com/how-to-send-tabular-time-series-data-to-apache-kafka-with-python-and-pandas-39e2055373c3?source=collection_archive---------19-----------------------#2023-01-24)

## 现在学习如何使用在线零售交易的示例日志在 Kafka 中生成和消费数据

[](https://medium.com/@tomasatquix?source=post_page-----39e2055373c3--------------------------------)![Tomáš Neubauer](https://medium.com/@tomasatquix?source=post_page-----39e2055373c3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----39e2055373c3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----39e2055373c3--------------------------------) [Tomáš Neubauer](https://medium.com/@tomasatquix?source=post_page-----39e2055373c3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd620afda25db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-send-tabular-time-series-data-to-apache-kafka-with-python-and-pandas-39e2055373c3&user=Tom%C3%A1%C5%A1+Neubauer&userId=d620afda25db&source=post_page-d620afda25db----39e2055373c3---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----39e2055373c3--------------------------------) ·15 分钟阅读·2023 年 1 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F39e2055373c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-send-tabular-time-series-data-to-apache-kafka-with-python-and-pandas-39e2055373c3&user=Tom%C3%A1%C5%A1+Neubauer&userId=d620afda25db&source=-----39e2055373c3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F39e2055373c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-send-tabular-time-series-data-to-apache-kafka-with-python-and-pandas-39e2055373c3&source=-----39e2055373c3---------------------bookmark_footer-----------)![](img/33c2b7574c7e450bfa10a9e6192ff1ce.png)

图片来源于 [Tech Daily](https://unsplash.com/@techdailyca?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/ztYmIQecyH4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

时间序列数据有各种形式，通常以高频率生成，例如传感器数据和交易日志。它也以巨大的数据量产生，其中记录是以毫秒而非小时或天来分隔的。

但什么样的系统可以处理如此持续的数据流？一种较旧的方法是将原始数据转储到数据湖中，并通过长期运行的过程进行大批量处理。如今，许多公司更喜欢实时处理原始数据，并将汇总结果写入数据库。

例如，在线零售商可以通过产品和日期不断汇总交易数据，而不是按需运行昂贵的数据库查询。但这在实际中是如何运作的？让我们找出答案吧！

在本教程中，我们将使用 Python 和 Apache Kafka 来处理来自真实在线零售商的大量时间序列数据。

# 你将学到的内容

到本教程结束时，你将了解：

+   **初创公司和在线业务为何使用 Apache Kafka**

+   **时间序列数据的独特性质以及它如何与 Kafka 协作**

+   **如何在本地机器上安装和运行 Kafka**

+   **如何使用 Python 和 Pandas 库批量发送时间序列数据到 Kafka**

# 你应该已经了解的内容

本文针对数据科学家和工程师，因此我假设你具备以下条件：

+   你熟悉 Python 并且使用过 Pandas 库，或者至少知道它在数据科学中的用途。

+   你听说过 Apache Kafka，并大致了解它的用途。

但如果你不符合这些标准，也不必担心。这个教程足够简单易懂，我们会简要解释这些技术。请注意，它不适合绝对初学者。

# 为什么使用 Apache Kafka 处理时间序列数据？

Apache Kafka 是实时数据处理的行业标准。你可以用它来处理大量的数据流（只要你有足够的计算资源）。

例如，一些一级方程式赛车队将 Kafka 与 Kubernetes 结合使用，以处理每毫秒传入的大量传感器数据。这些数据被实时分析，以预测比赛结果，并为车队提供驾驶员的洞察。

除了处理数据流的能力外，还有其他关键原因说明在线业务可能选择使用 Apache Kafka：

+   **可扩展性**：Kafka 设计用于处理高数据量和低延迟，非常适合那些预期快速增长并需要一个可以随之扩展的解决方案的初创公司。

+   **解耦**：你听说过 [事件驱动架构](https://www.tibco.com/reference-center/what-is-event-driven-architecture) 吗？Kafka 是一种经常被用来促进这种模式的工具。它允许系统的解耦，这意味着架构的不同部分可以独立开发和部署。这对于快速迭代并需要能够在不影响其他架构部分的情况下修改系统的初创公司尤为有用。

+   **耐久性**：Kafka 会将所有发布的消息存储在可配置的时间内，这意味着它可以作为系统中所有数据的持久日志。这对于需要保持所有数据记录以满足审计或合规要求的初创公司非常有用。

+   **广泛采用**：Apache Kafka 在行业中得到了广泛的采用，这意味着它拥有庞大的用户基础和强大的工具及资源生态系统。这对于希望利用 Kafka 社区集体经验的初创公司来说非常有帮助。

这一点尤其重要，因为 Apache Kafka 有着 notoriously steep 的学习曲线。没有大量的教程和演示，许多初学者将难以使其正常运行。

# 为什么要在 Apache Kafka 中使用 Python？

因为 Python 是数据和机器学习社区中最受欢迎的语言。这些社区可以从 Apache Kafka 中获益良多，但目前还没有足够吸引他们技能水平的 Kafka 教程。

如果你是数据团队的一部分，你更有可能知道 Python 和 Pandas，而不是 Java。然而，大多数旧版 Kafka 教程都是为使用 Java 的软件工程师编写的。这是因为软件工程师传统上构建了与 Kafka 交互的组件（而 Kafka 本身是用 Java/Scala 编写的）。

然而，Kafka 的用户基础正在发生变化。软件和数据团队的职责开始趋于融合。这是由于数据在现代组织中的重要性日益增长，以及数据管理和处理任务的复杂性增加。今天，数据专业人员也会参与与 Kafka 交互的软件组件的开发，但他们仍然会遇到很多阻力，因为他们通常不熟悉 Java 生态系统中的技术。这就是为什么我们在这些示例中使用 Python 和 Pandas 的原因。

好了，前言部分就到这里——让我们进入教程。

# 本教程的先决条件

你需要的第一件事是时间——大约 30 分钟（在你安装了所需的软件后）。

说到这一点，请确保在继续之前已经安装了以下软件。

## 软件

1.  **Windows 或基于 Unix 的操作系统** 我们将提供 Windows 和基于 Unix 的操作系统（如 macOS 和 Ubuntu）的命令。

1.  [**Python 3.0+**](https://www.python.org/about/gettingstarted/) **和所需的库**

    —** 你可以从 [Python 下载页面](https://www.python.org/downloads/) 下载安装程序。

    — 可选地，你可能想要创建一个 [虚拟环境](https://docs.python.org/3/library/venv.html) 用于本教程，以避免依赖冲突。

    **所需库

    —** [Pandas](https://pypi.org/project/pandas/): 使用 `pip3 install pandas` 安装

    **—** [Kafka-python](https://pypi.org/project/kafka-python/): 使用 `pip3 install kafka-python` 安装

1.  **Java 8+** 这是 Apache Kafka 的前提条件。要安装它，请选择以下之一：

    **—Ubuntu**：使用 `sudo apt install default-jre` 安装

    **— macOS**：使用 `brew install java` 安装

    * *如果你使用的是带有 M1 芯片的 Macbook Pro，你可能需要按照本指南中概述的步骤安装 Java：“*[*如何在 Apple Mac M1 Pro 上设置 Java (Dev.to)*](https://dev.to/docker/how-to-setup-java-on-macos-124-monterey-3l10)*” — 这大约需要 2 分钟。*

    **— Windows**：下载并运行 [Windows 的 Java 可执行文件](https://www.java.com/en/download/)

1.  [**Apache Kafka**](https://kafka.apache.org/documentation/#quickstart) 你可以从 [Apache Kafka 下载页面](https://kafka.apache.org/downloads) 下载 Apache Kafka 二进制文件。

    将文件内容解压到一个方便的位置。例如，你可以为本教程创建一个项目文件夹并将其解压到那里。

# 主要步骤

在深入细节之前，让我们回顾一下我们将要覆盖的主要步骤。

1.  **设置 Kafka：** 我们首先将熟悉 Kafka 的命令行工具，并使用它们来：

    — 启动 Zookeeper 和 Kafka 服务器

    — 创建一个主题（我们将向其发送数据）

1.  **数据分析：** 使用 Python 和 Pandas 来：

    — 仔细查看在线零售数据集的结构。

    — 将其读入数据框并理解不同的数据类型。

    我们还将探讨使用数据框和时间序列数据与 Kafka 的好处。

1.  **使用生产者向 Kafka 发送数据：** 使用 kafka-python 库来：

    — 将 csv 文件读取到数据框中并初始化 Kafka python 生产者。

    — 遍历行并将其批量发送到 Kafka

1.  **从 Kafka 中读取数据：** 我们将再次使用 kafka-python 库来：

    — 从我们的 Kafka 主题中读取消息

    — 将批处理消息转换回数据框。

    — 对其进行一些简单的聚合操作。

你会在 ‘*tabular-timeseries-kafka*’ 子文件夹中找到代码。如果你想直接跳到代码部分，可以使用以下命令克隆我们的教程仓库：

git clone [`github.com/quixai/tuto...`](https://github.com/quixai/tutorial-code.git)

# 设置 Apache Kafka

如果你还没这样做，从 [Apache Kafka 下载页面](https://kafka.apache.org/downloads) 下载 Apache Kafka（例如，“*kafka_2.12–3.3.1.tgz*”）并将文件内容解压到一个方便的位置。

在你能对 Kafka 进行任何操作之前，你需要启动核心服务。每个服务都需要在单独的终端窗口中运行。这两个服务是：

1.  **Zookeeper 服务**负责管理所有不同服务、代理和客户端之间的协调，这些组成了 Kafka 集群。

1.  **Kafka 服务器服务**运行 Apache Kafka 的核心功能，如消息代理。

按照这些说明操作时，请在你解压 Kafka 的目录中启动每个终端窗口（例如，“*C:\Users\demo\Downloads\kafka_2.13–3.3.1\*”）。

**要启动核心 Kafka 服务，请按照以下步骤操作：**

**1.** 在 Kafka 目录中，打开一个终端窗口，并使用以下命令启动 zookeeper 服务：

+   **Linux / macOS**

```py
bin/zookeeper-server-start.sh config/zookeeper.properties
```

+   **Windows**

```py
bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
```

+   你应该会看到一堆日志消息，表明 zookeeper 启动成功。

    保持窗口打开。

**2.** 打开第二个终端窗口，并使用以下命令启动 Kafka 服务器：

+   **Linux / macOS**

```py
bin/kafka-server-start.sh config/server.properties
```

+   **Windows**

```py
bin\windows\kafka-server-start.bat .\config\server.properties
```

+   再次，你应该会看到类似的日志消息，表明服务器启动成功。

    也保持这个窗口打开。

**3.** 接下来，你需要创建一个名为“transactions”的主题来存储数据。

+   如果“主题”这个术语不太熟悉，可以把它看作是一个不断更新日志文件的过程。有很多方式来 [详细解释主题的作用](https://dattell.com/data-architecture-blog/what-is-a-kafka-topic/)，但目前可以简单理解为，它是一个记录特定类型数据（如：传入的交易）及数据本身的事件日志文件。

**要在 Kafka 中创建一个主题：**

打开第三个终端窗口并输入以下命令：

+   **Linux / macOS**

```py
bin/kafka-topics.sh --create --topic transactions --bootstrap-server localhost:9092
```

+   **Windows**

```py
bin\windows\kafka-topics.bat --create --topic transactions --bootstrap-server localhost:9092
```

+   你应该会看到确认消息 “*创建了主题 transactions*”。

Kafka 部分就这些了。希望你已经顺利设置好了。如果遇到任何问题，可能会有帮助的 [故障排除指南](https://www.codeproject.com/Articles/5276053/Troubleshoot-Kafka-Setup-on-Windows)。现在，让我们进入激动人心的部分。

# 分析数据

对于本练习，我们将使用一个跨国数据集，该数据集包含了 2010 年 01 月 12 日至 2011 年 09 月 12 日之间发生的所有交易，数据来自于一个位于英国的在线零售商。该数据集来源于加州大学主办的机器学习库。你可以在 UCI 机器学习库网站的 [专页](https://archive.ics.uci.edu/ml/datasets/online+retail) 上找到有关数据集的更多详细信息。

准备工作，按以下步骤操作：

+   为此项目创建一个文件夹（例如，“*tabular-timeseries-kafka*”）。

+   从 [这个存储位置](https://quixdocsdev.blob.core.windows.net/docsartifacts/online_retail_II.zip) 下载压缩的 CSV 文件，并将其解压到项目文件夹中。

我们将提供 Python 命令作为说明，但你也可以使用 [Pycharm Community Edition](https://www.jetbrains.com/pycharm/download/) 等 IDE。

第一个任务是检查文件中的数据，并查看 Pandas 默认如何解释这些数据：

**要检查数据，请按照以下步骤操作：**

**1.** 在你的项目目录中打开一个终端窗口并输入 *python* 启动 Python 控制台。

**2.** 通过输入以下命令将 CSV 读入 DataFrame：

```py
>>> import pandas as pd
>>> df = pd.read_csv("online_retail_II.csv", encoding="unicode_escape")
>>> print(df.info())
```

**3.** 检查 `df.info` 的输出：

```py
RangeIndex: 1067371 entries, 0 to 1067370
Data columns (total 8 columns):
# Column Non-Null Count Dtype
 - - - - - - - - - - - - - - -
0 Invoice 1067371 non-null object
1 StockCode 1067371 non-null object
2 Description 1062989 non-null object
3 Quantity 1067371 non-null int64
4 InvoiceDate 1067371 non-null object
5 Price 1067371 non-null float64
6 Customer ID 824364 non-null float64
7 Country 1067371 non-null object
dtypes: datetime64ns, float64(2), int64(1), object(4)
memory usage: 65.1+ MB
```

请注意，`InvoiceDate` 列已被读取为对象数据类型。

在 Pandas 中，对象数据类型用于表示字符串值或无法轻易转换为数值数据类型的数据。

为了使数据集成为真正的时间序列数据集，我们需要将记录创建的时间（在此例中为 `InvoiceDate`）转为合适的 DateTime 格式。我们稍后会解释原因。

**将 InvoiceDate 列转换为日期格式：**

+   在 Python 控制台中，输入以下命令：

```py
>>> df['InvoiceDate'] = pd.to_datetime(df['InvoiceDate'])
>>> df.set_index('InvoiceDate', inplace=True)
```

+   这种转换允许你利用 pandas 的广泛 [时间序列功能](https://pandas.pydata.org/docs/user_guide/timeseries.html#timeseries-overview)。

+   例如，将其设置为 DatetimeIndex 可以帮助你优化时间序列数据，包括预计算和缓存的日期范围；快速且轻松地选择日期、日期范围及其相关值；以及使用时间块（“年”、“月”）进行快速汇总。

# Kafka 可以对时间序列数据做什么

所以我们知道 Pandas 有许多与时间序列相关的功能，但 Kafka 呢？确实，Kafka 也提供了许多功能来优化时间序列数据的处理。这些包括：

+   **压缩**：Kafka 支持多种压缩算法，这些算法可以利用时间数据来减少数据流的大小，提高数据处理效率。

+   **数据分区**：Kafka 允许你按键和时间对数据流进行分区，这对于将数据处理分布到多个消费者实例中很有用。

+   **自定义序列化**：Kafka 提供了一个可插拔的序列化框架，允许你为数据指定自定义序列化器和反序列化器，这对于优化数据处理性能可能非常有用。

所有这些功能部分依赖于时间的正确格式。

请注意，在这个基础教程中我们不会使用这些功能。但是，如果你打算在生产环境中使用 Kafka，了解时间数据在与 Kafka 交互时所扮演的角色是很重要的。

# 使用 DataFrames 与 Kafka 的优点

Pandas DataFrames 在以表格格式存储数据时特别有用，因为数据集中的每个特征都可以作为一维形状或 Series 进行操作，同时仍然形成一个多维数据集。DataFrames 还配备了一些方便的内置函数，允许你快速操作和处理数据。这在使用 Kafka 时非常宝贵。

例如，在下一主要步骤中，我们将以批量的形式发送数据。我们将记录分批处理，以模拟生产场景，你可能每秒接收到数百条记录。

如果你为每条单独的记录发送一条消息，你可能会遇到瓶颈或系统故障。这就是为什么你以较低频率发送小批量数据（而不是以高频率发送大量小消息）的原因。

正如你所见，当我们使用 Pandas 分块功能时，这个过程非常简单。这是 Pandas DataFrames 提供的众多 Kafka 友好功能之一。

# 创建一个 Kafka 生产者来发送数据

现在，让我们将所学内容放入一个合适的 Python 文件中，并开始向 Kafka 发送数据。我们将使用[*kafka-python*](https://github.com/dpkp/kafka-python)库，这是多个用于将 Python 应用程序连接到 Kafka 的库之一（另一个是[*confluent-kafka-python*](https://github.com/confluentinc/confluent-kafka-python)库）。

**要创建一个生产者，请遵循以下步骤：**

1.  在你的项目目录中，创建一个名为 `producer.py` 的文件，并插入以下代码块：

```py
# Import packages
import pandas as pd
import json
import datetime as dt
from time import sleep
from kafka import KafkaProducer

# Initialize Kafka Producer Client
producer = KafkaProducer(bootstrap_servers=['localhost:9092'])
print(f'Initialized Kafka producer at {dt.datetime.utcnow()}')
```

这段代码导入了所有必需的库并初始化 Kafka 生产者，告诉它连接到你（希望）仍然在计算机上运行的服务器下的‘*localhost*’。

**2.** 接下来，添加一个‘`for`’循环，它将遍历文件并以批量的形式发送数据：

```py
# Set a basic message counter and define the file path
counter = 0
file = "online_retail_II.csv"

for chunk in pd.read_csv(file,encoding='unicode_escape',chunksize=10):

  # For each chunk, convert the invoice date into the correct time format
  chunk["InvoiceDate"] = pd.to_datetime(chunk["InvoiceDate"])

  # Set a counter as the message key
  key = str(counter).encode()

  # Convert the data frame chunk into a dictionary
  chunkd = chunk.to_dict()

      # Encode the dictionary into a JSON Byte Array
      data = json.dumps(chunkd, default=str).encode('utf-8')

      # Send the data to Kafka
      producer.send(topic="transactions", key=key, value=data)

      # Sleep to simulate a real-world interval
      sleep(0.5)

      # Increment the message counter for the message key
      counter = counter + 1

      print(f'Sent record to topic at time {dt.datetime.utcnow()}')
```

代码包括解释性的注释，但基本上它做了以下几件事：

+   按每 10 行批量读取 CSV 文件

+   将每个批次序列化为 JSON 并将 JSON 编码为字节数组

+   将 JSON 作为消息发送到 Kafka 主题“transactions”

你可以在我们的 Github 仓库中查看完整的文件。 [`github.com/quixai/tutorials/timeseries/producer.py`](https://github.com/quixai/tutorials/timeseries/producer.py)

**3.** 保存你的文件并使用以下命令运行代码：

`python producer.py`

+   在你的终端窗口中，你应该开始看到类似这样的确认信息：

    “*将记录发送到主题的时间是 2022–12–28 13:23:52.125664*”为每条发送的消息。

+   如果你收到缺少模块的错误，请确保你已安装 *kafka-python* 库（`pip3 install kafka-python`）。

一旦数据进入 Kafka 主题，它可以被多个消费者读取，并提取用于更多下游处理。

创建一个消费者来读取这些消息并对其进行基本操作。

# 创建一个 Kafka 消费者来读取数据

创建 Kafka 消费者的过程与前一步非常类似。在这种情况下，我们将读取每个批量的消息，并将其转换回 DataFrame。

我们将假设这个消费者是用于某种库存分析管道，它只关心每个库存项目的总销售额。因此，在数据中，我们只查看 StockCode、销售数量和价格。我们将计算每条记录的销售总值，以便按 StockCode 汇总销售数据。

1.  在你的项目目录中，创建一个名为`consumer.py`的文件，并插入以下代码块以初始化消费者。

```py
from kafka import KafkaConsumer
import json
import pandas as pd

# Consume all the messages from the topic but do not mark them as 'read' (enable_auto_commit=False)
# so that we can re-read them as often as we like.
consumer = KafkaConsumer('transactions',
                         group_id='test-consumer-group',
                         bootstrap_servers=['localhost:9092'],
                         value_deserializer=lambda m: json.loads(m.decode('utf-8')),
                         auto_offset_reset='earliest',
                         enable_auto_commit=False)
```

+   我们正在用比我们为生产者设置的更多选项来初始化消费者。

+   首先，我们告诉它从哪个主题读取，然后 Kafka 服务器运行在哪里，最后，我们提供了一个 lambda 函数用于将消息值反序列化回 Python 字典。

2. 接下来，添加‘for’循环，它将迭代消息并对其进行一些处理。

```py
for message in consumer:
    mframe = pd.DataFrame(message.value)

    # Multiply the quantity by the price and store in a new "revenue" column
    mframe['revenue'] = mframe.apply(lambda x: x['Quantity'] * x['Price'], axis=1)

    # Aggregate the StockCodes in the individual batch by revenue
    summary = mframe.groupby('StockCode')['revenue'].sum()

    print(summary)
```

+   正如你在代码注释中看到的，我们正在进行一个简单的计算，输出按`StockCode`的每个消息批次的收入汇总。

+   当然，最终目标是按`StockCode`保持总体收入的持续统计。这将需要一些进一步的处理，将汇总数据写入数据库，从而为某种仪表盘提供数据。

+   然而，为了本教程的目的，这里是一个很好的停止点。如果一切正常，你应该能看到每条消息的汇总结果被记录下来。它应该类似于这样：

```py
Name: revenue, dtype: float64
StockCode
16161P 10.50
16169N 10.50
21491 11.70
22065 17.40
22138 44.55
22139 44.55
22352 30.60
85014A 17.85
85014B 17.85
85183B 144.00
```

如果你看到记录的汇总，做得好！祝贺你完成了所有步骤。

# 总结一下

如果你只是把它做在本地机器上，这个过程可能显得平凡无奇，但当你考虑到这种模式如何扩展时，它会变得更加有趣。让我们稍微回顾一下你做了什么——你完成了两个关键任务：

## 1— 你生成了高频率的消息流，并将其流入一个 Kafka 主题

在这种情况下，你是在“重放”客户交易的历史日志，并人为地将每条消息延迟半秒。

**它如何扩展**：

+   在生产环境中，可能会有某种前端应用程序生成流数据，我们需要做一些额外的路由工作将其放入主题中（因为前端应用程序和 Kafka 集群将位于不同的服务器上）。

+   消息也可能以不规则的频率到达，而不是标准的半秒，并且序列化可能会以某种方式进行优化。

## 2 — 你从 Kafka 主题中消费了高频率的消息流，并对数据进行了汇总处理

对于这个教程，你只是通过同一台机器进行数据流处理和消费，这可能看起来没什么特别的——但实际上，会有许多不同机器的消费者。

**它如何扩展**：

+   你可以在不同的服务器上运行一整套应用程序，每个应用程序以不同的方式消费流数据。

+   一个消费者可能是一个欺诈检测应用程序，它会深入交易历史，寻找可疑的交易模式。

+   另一个消费者可能是一个订单履行管道，它只关心最新的未完成订单。它会读取新消息，并将订单发送去处理。

+   另一个消费者可能是一个数据聚合管道，它将数据与 CRM 中的数据结合，并将其放入数据仓库中供市场营销团队分析。

如你所见，当你使用 Apache Kafka 来利用高频时间序列数据时，它可以变得极其强大。表格时间序列数据在许多应用中都很常见，如金融分析、传感器数据分析和社交媒体分析。

按照本教程中概述的步骤，你现在应该具备了将表格时间序列数据发送到 Apache Kafka 并利用其能力进行实时数据处理和分析的坚实基础。无论你是在进行小规模还是大规模项目，Apache Kafka 都是你工具包中的一个重要工具，希望我们能让你更接近掌握它的终极目标。

+   你可以在我们的 [教程 GitHub 仓库](https://github.com/quixai/tutorial-code.git)中找到本教程及其他教程的源代码。

+   数据来源：*Daqing Chen, Sai Liang Sain, 和 Kun Guo, 数据挖掘在线零售行业：“基于 RFM 模型的客户细分案例研究”，《数据库营销与客户策略管理期刊》，第 19 卷，2012 年（在线出版日期：2012 年 8 月 27 日。doi: 10.1057/dbm.2012.17）*

## 关于作者

Tomáš Neubauer 是 [Quix](https://www.quix.io/company/) 的首席技术官——一个实时数据工程堆栈，帮助工程师管理越来越庞大和快速的数据流。它使数据和机器学习团队更容易访问流数据，这些团队喜欢用 Python 进行工作。

在业余时间，Tomáš 喜欢在布拉格周围的山丘中骑山地自行车，他还喜欢品尝捷克提供的最优质啤酒。如果你对教程有任何问题，可以联系他 [@Tomas Neubaue](https://stream-processing.slack.com/team/U035LUJJ4CQ)r，或在 [The Stream](https://quix.io/slack-invite?_ga=2.115746945.682056252.1672214079-749957754.1668468796)——一个针对实时数据爱好者的开放社区。

*本文最初发表于：* [*https://www.quix.io/blog/send-timeseries-data-to-kafka-python*](https://www.quix.io/blog/send-timeseries-data-to-kafka-python)
