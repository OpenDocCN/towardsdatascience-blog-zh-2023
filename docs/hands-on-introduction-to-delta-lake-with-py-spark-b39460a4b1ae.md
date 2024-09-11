# 实操介绍 Delta Lake 和 (py)Spark

> 原文：[https://towardsdatascience.com/hands-on-introduction-to-delta-lake-with-py-spark-b39460a4b1ae?source=collection_archive---------0-----------------------#2023-02-16](https://towardsdatascience.com/hands-on-introduction-to-delta-lake-with-py-spark-b39460a4b1ae?source=collection_archive---------0-----------------------#2023-02-16)

## 现代数据存储框架的概念、理论和功能

[](https://joaopedro214.medium.com/?source=post_page-----b39460a4b1ae--------------------------------)[![João Pedro](../Images/64a0e14527be213e5fde0a02439fbfa7.png)](https://joaopedro214.medium.com/?source=post_page-----b39460a4b1ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b39460a4b1ae--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b39460a4b1ae--------------------------------) [João Pedro](https://joaopedro214.medium.com/?source=post_page-----b39460a4b1ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb111eee95c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-introduction-to-delta-lake-with-py-spark-b39460a4b1ae&user=Jo%C3%A3o+Pedro&userId=b111eee95c&source=post_page-b111eee95c----b39460a4b1ae---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b39460a4b1ae--------------------------------) ·10 分钟阅读·2023年2月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb39460a4b1ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-introduction-to-delta-lake-with-py-spark-b39460a4b1ae&user=Jo%C3%A3o+Pedro&userId=b111eee95c&source=-----b39460a4b1ae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb39460a4b1ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-introduction-to-delta-lake-with-py-spark-b39460a4b1ae&source=-----b39460a4b1ae---------------------bookmark_footer-----------)![](../Images/303f0065d94538660d3d2564a8209807.png)

照片由 [Nick Fewings](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

我认为现在大家对数据的价值已经非常清楚。以一个热门的例子来说，像 ChatGPT 这样的模型只能基于多年来产生和收集的大量数据构建。

我想强调“可以”这个词，因为编程领域有一句话仍然适用，并且可能永远适用：垃圾进，垃圾出。数据本身没有价值，它需要组织、标准化和清理。治理是必要的。在这种情况下，组织中的数据管理是其涉及数据的项目成功的关键点。

正确的数据管理的一个主要方面是数据架构的定义。数据架构是满足特定组织数据需求的一套实践、技术和服务，包括技术需求（速度、容量、频率、可用性）和非技术需求（业务规则、数据立法遵从）。

如今，几乎默认情况下，组织必须处理不同格式的数据（CSV、pdf、视频、parquet 等），这就是像亚马逊的 S3 这样的 blob 存储取得成功的原因。然而，这种方法可能会带来一些问题，因为缺乏对原始文件（特别是表格数据）的管理工具，如模式强制、版本控制和数据血缘。

考虑到这一点（以及其他一些因素），开发了 Delta Lake，一个开源数据存储框架，实施/体现了 Lakehouse 架构，并且是今天文章的主题。

# 什么是 Delta Lake？

在深入讨论 Delta Lake 之前，我们需要记住数据湖的概念，所以让我们回顾一些历史。

数据湖架构是在数据量大幅增长的时期提出的，特别是非结构化和半结构化数据，当传统的数据仓库系统开始无法处理这种需求时。

提案很简单——“把你拥有的一切都扔进来，稍后再担心”。在第一个数据湖的背景下，主要的参与者是 Hadoop，一个分布式文件系统，配合 MapReduce，这是一个建立在最小数据移动和高并行性的理念上的处理范式。理论上，只需将所有数据扔进 Hadoop，然后编写作业处理数据以得到预期结果，从而摆脱复杂的数据仓库系统。

传说中，这一过程并不顺利。文件被随意丢弃，没有质量担忧，没有版本控制，也没有管理。数据变得毫无用处。问题如此严重，以至于创造了“数据沼泽”这一术语，调侃非常混乱的数据湖，以及“WORN paradigm”（写一次读永不再读）。实际上，传统数据仓库系统，特别是关系数据库管理系统（RDBMS）所施加的保障仍然需要，以确保数据质量。（那时我还很小，我最近才从现代文献中了解到这一切历史）

时间流逝，基于过去的成功与失败，提出了新的架构。其中之一就是 Lakehouse 架构。简而言之，它试图结合数据湖（灵活性）和数据仓库（保障）的优点。

Delta Lake 只是一个具有 Lakehouse 视角的存储框架/解决方案的实际实现。

让我们开始吧：

Delta Lake（即 Delta 表）中的表实际上就是一个包含 JSON 事务日志的 *parquet* 文件，该日志记录了文件上的所有变更历史。通过这种方式，即使数据存储在文件中，也可以完全控制所有发生的事件，包括读取以前的版本和恢复操作。Delta Lake 还使用 ACID 事务的概念，即防止因作业失败或读取不一致导致的部分写入。Delta Lake 还会拒绝格式不正确的数据写入（模式强制）并允许模式演变。最后，它还提供了通常在原始文件中不可用的 CRUD 功能（插入、更新、合并和删除）。

本文将以实际操作的方式使用 pyspark 讨论这些功能。

# **数据**

本文使用的数据是发生在巴西高速公路上的交通事故列表，由 PRF（巴西公路警察）收集，并在巴西开放数据门户网站上公开提供 [[Link](https://www.gov.br/prf/pt-br/acesso-a-informacao/dados-abertos/dados-abertos-acidentes)][[License — CC BY-ND 3.0].](https://creativecommons.org/licenses/by-nd/3.0/deed.pt_BR)

数据涵盖了 2007 年到 2021 年期间的各种事故信息：地点、高速公路、公里数、纬度和经度、涉及人员数量、事故类型等。

# 实现

## 0\. 设置环境

一如既往，该项目使用 docker 容器进行开发：

```py
version: '3'
services:
  spark:
    image: bitnami/spark:3.3.1
    environment:
      - SPARK_MODE=master
    ports:
      - '8080:8080'
      - '7077:7077'
    volumes:
      - ./data:/data
      - ./src:/src
  spark-worker:
    image: bitnami/spark:3.3.1
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark:7077
      - SPARK_WORKER_MEMORY=4G
      - SPARK_EXECUTOR_MEMORY=4G
      - SPARK_WORKER_CORES=4
    ports:
      - '8081:8081'
    volumes:
      - ./data:/data
      - ./src:/src
  jupyter:
    image: jupyter/pyspark-notebook:spark-3.3.1
    ports:
      - '8890:8888'
    volumes:
      - ./data:/data
```

所有代码均可在此 [GitHub 仓库](https://github.com/jaumpedro214/posts)中获取。

## 1\. 创建 Delta 表

首先需要实例化一个 Spark 会话，并将其配置为使用 Delta-Lake 依赖项。

```py
# Install the delta-spark package.
!pip install delta-spark
```

```py
from pyspark.sql import SparkSession
from pyspark.sql.types import StructField, StructType, StringType, IntegerType, DoubleType
import pyspark.sql.functions as F

from delta.pip_utils import configure_spark_with_delta_pip

spark = (
    SparkSession
    .builder.master("spark://spark:7077")
    .appName("DeltaLakeFundamentals")
    .config("spark.sql.extensions", "io.delta.sql.DeltaSparkSessionExtension")
    .config("spark.sql.catalog.spark_catalog", "org.apache.spark.sql.delta.catalog.DeltaCatalog")
)

spark = configure_spark_with_delta_pip(spark).getOrCreate()
```

创建 Delta 表非常简单，就像以特定格式编写一个新文件一样。以下代码读取 2020 年的事故数据，并将数据写入 delta 表。

```py
SCHEMA = StructType(
    [
        StructField('id', StringType(), True),          # ACCIDENT ID
        StructField('data_inversa', StringType(), True),# DATE
        StructField('dia_semana', StringType(), True),  # DAY OF WEEK
        StructField('horario', StringType(), True),     # HOUR
        StructField('uf', StringType(), True),          # BRAZILIAN STATE
        StructField('br', StringType(), True),          # HIGHWAY
        # AND OTHER FIELDS OMITTED TO MAKE THIS CODE BLOCK SMALL
    ]
)

df_acidentes = (
    spark
    .read.format("csv")
    .option("delimiter", ";")
    .option("header", "true")
    .option("encoding", "ISO-8859-1")
    .schema(SCHEMA)
    .load("/data/acidentes/datatran2020.csv")
)

df_acidentes.show(5)
```

![](../Images/d72cfddfa1e866eeb230fbc31680a2a5.png)

2020 年前 5 行。

编写 delta 表。

```py
df_acidentes\
    .write.format("delta")\
    .mode("overwrite")\
    .save("/data/delta/acidentes/")
```

就这些了。

如前所述，Delta-Lake 表（就文件而言）仅仅是传统的 *parquet* 文件，附带一个记录所有变更的 JSON 事务日志。

![](../Images/3c877b178787f54d87ff170643be3325.png)

带有 JSON 事务日志的 Delta 表。

## 2\. 从 Delta 表中读取数据

再次强调，读取 Delta 表并没有特别之处。

```py
df_acidentes_delta = (
    spark
    .read.format("delta")
    .load("/data/delta/acidentes/")
)
```

```py
df_acidentes_delta.select(["id", "data_inversa", "dia_semana", "horario", "uf"]).show(5)
```

![](../Images/1681c30993e3e05d2b8ee361a767d1db.png)

让我们计算行数

```py
df_acidentes_delta.count()

>> Output: 63576
```

## 3\. 向 Delta 表中添加新数据

Delta 表支持“追加”写入模式，因此可以将新数据添加到已存在的表中。让我们添加 2019 年的数据。

```py
# READING THE 2019 DATA
df_acidentes_2019 = (
    spark
    .read.format("csv")
    .option("delimiter", ";")
    .option("header", "true")
    .schema(SCHEMA)
    .load("/data/acidentes/datatran2019.csv")
)
```

向 Delta 表追加数据

```py
df_acidentes_2019\
    .write.format("delta")\
    .mode("append")\
    .save("/data/delta/acidentes/")
```

重要的是要强调：Delta表会执行模式强制，因此只能写入与已存在表具有相同模式的数据，否则，Spark会抛出错误。

让我们检查Delta表中的行数。

```py
df_acidentes_delta.count()

>> Output: 131132
```

## 4\. 查看Delta表的历史记录（日志）

Delta表的日志记录了所有对表执行的操作。它包含了对每个操作的详细描述，包括关于操作的所有元数据。

要读取日志，我们需要使用一个名为`DeltaTable`的特殊Python对象。

```py
from delta.tables import DeltaTable

delta_table = DeltaTable.forPath(spark, "/data/delta/acidentes/")
delta_table.history().show()
```

![](../Images/be4e3d84b827c8be00d4a2507942ae17.png)

历史对象是一个Spark数据框。

```py
delta_table.history().select("version", "timestamp", "operation", "operationParameters").show(10, False)
```

![](../Images/281f55740f79b0a5d92674832c0b3550.png)

正如我们所见，目前有两个表版本，每个操作都有一个版本：表创建时的覆盖写入和之前进行的追加写入。

## 5\. 读取Delta表的特定版本

如果没有指定任何内容，Spark将读取Delta表的最新版本。

```py
df_acidentes_latest = (
    spark
    .read.format("delta")
    .load("/data/delta/acidentes/")
)
df_acidentes_latest.count()

>> Output: 131132
```

但也可以通过仅添加一行代码从特定版本中读取数据：

```py
df_acidentes_version_0 = (
    spark
    .read.format("delta")
    .option("versionAsOf", 0)
    .load("/data/delta/acidentes/")
)
df_acidentes_version_0.count()

>> Output: 63576
```

计数下降了，因为我们正在从版本0读取，在2019年的数据插入之前。

## 6\. 恢复到先前版本

可以恢复到表的先前版本。这对于快速解决管道中出现的错误非常有用。此操作也是通过之前创建的DeltaTable对象执行的。

让我们将表恢复到版本0：

```py
delta_table.restoreToVersion(0)
```

现在，最新的计数将再次为=63576，因为我们恢复到了数据尚未包括2019年的版本。

```py
# Counting the number of rows in the latest version
df_acidentes_latest.count()
```

**RESTORE**操作也记录在日志中。因此，实际上没有信息丢失：

```py
delta_table.history().select("version", "timestamp", "operation", "operationParameters").show(10, False)
```

![](../Images/91cd2dbd7153e9e98eb26a74c366d2b9.png)

让我们恢复到版本1。

```py
delta_table.restoreToVersion(1)
```

## 7\. 更新

更新操作也可以通过`DeltaTable`对象完成，但我们将使用SQL语法来尝试一种新方法。

首先，让我们将2016年的数据写入增量表。这些数据中的“data_inversa”（日期）列格式错误：dd/MM/yy而不是yyyy-MM-dd。

```py
df_acidentes_2016 = (
    spark
    .read.format("csv")
    .option("delimiter", ";")
    .option("header", "true")
    .option("encoding", "ISO-8859-1")
    .schema(SCHEMA)
    .load("/data/acidentes/datatran2016.csv")
)

df_acidentes_2016.select("data_inversa").show(5)
```

![](../Images/01ea716ec1c9fa6aef3faddbfe900d64.png)

让我们保存数据：

```py
df_acidentes_2016\
    .write.format("delta")\
    .mode("append")\
    .save("/data/delta/acidentes/")

df_acidentes_latest.count()
>> Output: 227495
```

但由于我们的data_inversa字段是字符串类型，因此不会发生错误。现在，我们的表中插入了错误的数据，需要进行修复。当然，我们可以只需恢复这次操作，并再次正确插入数据，但让我们改用UPDATE操作。

以下SQL代码仅修复年份=2016的数据格式。

```py
df_acidentes_latest.createOrReplaceTempView("acidentes_latest")

spark.sql(
    """
    UPDATE acidentes_latest
    SET data_inversa = CAST( TO_DATE(data_inversa, 'dd/MM/yy') AS STRING)
    WHERE data_inversa LIKE '%/16'
    """
)
```

错误格式化数据的行数为0：

```py
df_acidentes_latest.filter( F.col("data_inversa").like("%/16") ).count()
>> Output: 0
```

## 8\. 合并

最后将介绍的操作是MERGE（也称为UPSERT）操作。它是INSERT和UPDATE的混合。

它会尝试将新行插入目标表格，将某些列视为关键列。如果要插入的行已经存在于目标表中（即行键已经在目标表中存在），它将仅更新该行（按照指定的一些逻辑），否则，它将插入新行。

总结来说：如果存在，则更新；如果不存在，则插入。

![](../Images/653b122d4d312b4a778daf114b76f102.png)

合并示例。图片由作者提供。

为了演示这种方法，让我们插入一些2018年的数据，所有行的*人* = 0（*pessoas —* 参与事故的人数），模拟一个包含不完整数据的部分报告。

```py
# FULL DATA FROM 2018
df_acidentes_2018 = (
    spark
    .read.format("csv")
    .option("delimiter", ";")
    .option("header", "true")
    .option("encoding", "ISO-8859-1")
    .schema(SCHEMA)
    .load("/data/acidentes/datatran2018.csv")
)

# SAMPLE WITH pessoas=0
df_acidentes_2018_zero = (
  df_acidentes_2018
  .withColumn("pessoas", F.lit(0))
  .limit(1000)
)

df_acidentes_2018_zero\
    .write.format("delta")\
    .mode("append")\
    .save("/data/delta/acidentes/")
```

如果我们现在想用2018年的完整数据更新表格，我们必须确保已经插入的行仅更新*人*列，并插入所有新的行。

这可以通过以下MERGE操作来完成，该操作将事故的id和日期视为关键：

```py
df_acidentes_latest.createOrReplaceTempView("acidentes_latest")
df_acidentes_2018.createOrReplaceTempView("acidentes_2018_new_counts")

spark.sql(
    """
    MERGE INTO acidentes_latest
    USING acidentes_2018_new_counts

    ON acidentes_latest.id = acidentes_2018_new_counts.id
    AND acidentes_latest.data_inversa = acidentes_2018_new_counts.data_inversa

    WHEN MATCHED THEN
        UPDATE SET pessoas = acidentes_latest.pessoas + acidentes_2018_new_counts.pessoas

    WHEN NOT MATCHED THEN
        INSERT *
    """
)
```

# 结论

定义数据架构对所有旨在创建数据驱动产品的组织（如BI报告和机器学习应用）至关重要。数据架构定义了将确保组织的技术和非技术数据需求得到满足的工具、技术和实践。

在私营公司中，这可以帮助加快此类产品的开发，提升其质量和效率，并带来转化为利润的商业优势。在公共组织中，数据架构的好处转化为更好的公共政策，更好地了解特定领域的现状，如交通、安全、预算，以及提高透明度和管理水平。

在过去几十年中，提出了许多架构，每种架构在不同背景下都有其自身的优势。Lakehouse范式试图将数据湖和数据仓库的优势结合起来。Delta Lake是基于Lakehouse范式的存储框架。简而言之，它将通常仅在经典RDBMS中可用的许多保证（ACID事务、日志、撤销操作、CRUD操作）带到基于*parquet*的文件存储之上。

在这篇文章中，我们使用巴西高速公路交通事故的数据探索了这些功能中的一些。我希望我能有所帮助，我对讨论的任何主题都不是专家，我强烈建议进一步阅读（见下方一些参考文献）和讨论。

感谢您的阅读！;)

# 参考文献

> *所有代码都可以在* [*这个 GitHub 仓库*](https://github.com/jaumpedro214/posts)*找到。*

[1] Chambers, B., & Zaharia, M. (2018). *Spark: The definitive guide: Big data processing made simple*. “O’Reilly Media, Inc.”

[2] Databricks. (2020年3月26日). *Tech Talk | Diving into Delta Lake Part 1: Unpacking the Transaction Log* [[视频](https://www.youtube.com/watch?v=F91G4RoA8is)]. YouTube.

[2] *如何使用恢复功能将Delta Lake表回滚到先前版本*。 (2022年10月3日)。Delta Lake。 [链接](https://delta.io/blog/2022-10-03-rollback-delta-lake-restore/)

[3] *Delta Lake 官方页面*。 (无日期)。Delta Lake。 [https://delta.io/](https://delta.io/)

[4] Databricks. (2020年3月12日). *简化和扩展使用Delta Lake的数据工程管道* [[视频](https://www.youtube.com/watch?v=qtCxNSmTejk)]。YouTube。

[5] Databricks. (2020年9月15日). *利用Delta Lake改进Apache SparkTM* [[视频](https://www.youtube.com/watch?v=LJtShrQqYZY)]。YouTube。

[6] Reis, J., & Housley, M. (2022年). *数据工程基础：规划和构建稳健的数据系统* (第1版)。O’Reilly Media。
