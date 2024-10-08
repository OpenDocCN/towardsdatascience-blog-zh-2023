# 大数据文件格式解释

> 原文：[`towardsdatascience.com/big-data-file-formats-explained-275876dc1fc9`](https://towardsdatascience.com/big-data-file-formats-explained-275876dc1fc9)

## Parquet 与 ORC 与 AVRO 与 JSON。该选择哪一个，如何使用？

[](https://mshakhomirov.medium.com/?source=post_page-----275876dc1fc9--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----275876dc1fc9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----275876dc1fc9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----275876dc1fc9--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----275876dc1fc9--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----275876dc1fc9--------------------------------) ·阅读时间 9 分钟·2023 年 2 月 28 日

--

![](img/ce5abf8987056d730cd51c36eda44998.png)

照片由 [James Lee](https://unsplash.com/@picsbyjameslee?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我是采用 ELT（Extract-Load-Transform）设计的数据仓库（DWH）解决方案的忠实粉丝。然而，在某些时候，我面临了*处理*原始事件数据的要求，并且必须**选择文件格式**用于数据文件。

> *这是一种典型的场景，当机器学习工程师被要求创建行为数据集以训练模型并生成更好的产品推荐或预测客户流失时。*

为我们的机器学习管道选择正确的文件格式至关重要，因为这可能显著改变数据的 I/O 时间，并且肯定会对我们的模型训练性能产生更广泛的影响。

[](https://pub.towardsai.net/supercharge-your-data-engineering-skills-with-this-machine-learning-pipeline-b69d159780b7?source=post_page-----275876dc1fc9--------------------------------) [## 利用这个机器学习管道提升你的数据工程技能

### 数据建模、Python、DAGs、大数据文件格式、成本……应有尽有。

[pub.towardsai.net](https://pub.towardsai.net/supercharge-your-data-engineering-skills-with-this-machine-learning-pipeline-b69d159780b7?source=post_page-----275876dc1fc9--------------------------------)

另一个需要考虑的因素是数据的大小，因为我们已经为文件存储支付了过多费用。

本文旨在探讨这些重要问题及其他选项，以找到数据管道的最佳大数据文件格式。

*需求很简单，强调了使用数据湖的想法，并减少与数据仓库中数据存储相关的成本。*

我创建了一个**归档**存储桶，这是最便宜的存储类别，并准备*提取*我在 DWH 中的一些非常大的表的数据。我不得不说，那些表非常庞大，包含了大量的用户参与数据原始事件。因此，它们是整个数据平台中最昂贵的存储部分。

然后我不得不停下来决定使用哪种文件格式，因为存储大小并不是唯一的考虑因素。

> *确实，在某些场景下，读写时间或更好的模式支持可能更重要*

# Parquet vs ORC vs AVRO vs JSON

随着数据网格的兴起以及**Hadoop** 生态系统中大量数据处理工具的出现，**在数据湖中处理原始事件数据**可能更有效。

> *现代数据仓库解决方案非常棒，但有些* ***定价模型*** *比其他模型更贵，而且肯定比数据湖工具更贵*

大数据 **文件格式** 的 **最大优势** 之一是它们携带了模式信息。因此，加载数据、拆分数据以更高效地处理等操作要容易得多。JSON 无法提供此功能，我们需要在每次读取、加载或验证数据时定义模式。

# 列式 ORC 和 Parquet

**Parquet** 是一种 ***面向列*** 的数据存储格式，专为 *Apache Hadoop* 生态系统设计（由 Cloudera 支持，与 Twitter 合作）。它在使用**Spark**的数据显示科学家和数据工程师中非常受欢迎。当处理大量数据时，你会开始注意到列式数据的主要优势，这是在表中行数远多于列数时实现的。

![](img/45fc6ec974c1bc982924ea6d8d9a1976.png)

Parquet 文件布局。来源：[apache.org](http://apache.org)（Apache 2.0 许可证）

> *Spark 扩展性很好，这也是为什么大家喜欢它*

实际上，Parquet 是 Spark 的默认数据文件格式。**Parquet** 在查询和处理分析工作负载时表现出色。

> *列式格式更适合 OLAP 分析查询。具体来说，我们希望仅检索执行分析查询所需的列。这对内存有一定的影响，因此对性能和速度也有影响。*

**ORC（优化行列式）** 也是一种列式数据存储格式，与 Parquet 类似，并且携带了模式信息。这意味着像 Parquet 一样，它是**自描述**的，我们可以用它将数据加载到不同的磁盘或节点中。

![](img/b7301da75334cf83797ce798485045a3.png)

ORC 文件布局。来源：[apache.org](http://apache.org)（Apache 2.0 许可证）

我做了一个小测试，结果显示 **Parquet 和 ORC** 提供了类似的压缩比。然而，有一种观点认为 ORC 的压缩效率更高。

> *10 Mb 使用* ***SNAPPY*** *算法压缩后会变成 2.4Mb 的* ***Parquet***

不过，我感觉**ORC**的支持项目数量少于 Parquet，例如 Hive 和 Pig。

如果我们希望在数据湖中运行更广泛的 OLAP 分析工具，我建议使用**Parquet**。它有更广泛的项目支持，尤其是**Spark**。

> *实际上，PARQUET 和 ORC 的列式架构有所不同。*

两者都非常有效地节省了大量的磁盘空间。

不过，**ORC** 文件中的数据组织成独立的*条带*。每个条带都有自己独立的索引、行数据和页脚。这使得从 HDFS 进行大规模高效读取成为可能。

数据以页面形式存储在**Parquet**中，每页包含头部信息、定义级别信息和重复级别信息。

当支持复杂的**嵌套**数据结构时，它非常有效，并且在执行 IO 类型的数据操作时似乎更高效。这里的一个巨大优势是，如果我们只选择所需的列，**读取时间**可以显著减少。

我的个人经验表明，只选择几列可以…将数据读取速度提高至 30 倍（！），比读取包含完整模式的相同文件快。

> 读取数据速度快 30 倍（！）

不错吧？数据工程师进行的其他测试也证实了这一点。我会在底部添加那个链接。

> *长话短说，如果你在 HIVE 上工作，选择 ORC 更佳。它对 HIVE 更优化。而选择 Parquet 如果你与 Spark 工作，因为这将是 Spark 的默认存储格式。*

在其他方面，ORC 和 Parquet 的架构相似，KPIs 看起来大致相同。

# AVRO

**AVRO** 是一种**基于行**的存储格式，数据经过索引以提高查询性能。

它使用 JSON 数据定义数据类型和模式，并以二进制格式（压缩）存储数据，以帮助节省磁盘空间。

![](img/6535c558a34c0ed7ca5047e18c4ffd04.png)

avro 文件结构。来源：[apache.org](http://apache.org)（Apache 2.0 许可证）

与 Parquet 和 ORC 相比，AVRO 似乎提供了较低效的压缩但更快的**写入**速度。

+   *10 Mb* ***Parquet*** *用* ***SNAPPY*** *算法压缩后将变为 2.4Mb*

+   *10 Mb* ***AVRO*** *用* ***SNAPPY*** *算法压缩后将变为 6.2Mb*

+   *相同的 10 Mb* ***AVRO*** *用* ***DEFLATE*** *算法压缩后将需要 4Mb 存储空间*

AVRO 的主要优势是**模式演进支持**。换句话说，随着数据模式的演变，***AVRO*** 通过容忍添加、删除或更改的字段来支持这些变化。

> *AVRO 在处理如新增字段、变更字段和缺失字段时表现非常好。*

AVRO 在处理***写入密集型***大数据操作时更高效。

它是存储数据于数据平台的`source`层或`landing`区域的一个优秀选择，因为这些*文件通常会被作为整体读取*以进行进一步转换，具体取决于数据管道。任何模式演变更改都能轻松处理。

[](/data-pipeline-design-patterns-100afa4b93e3?source=post_page-----275876dc1fc9--------------------------------) ## 数据管道设计模式

### 选择合适的架构并附带示例

towardsdatascience.com

即使是用不同语言编写的应用程序也可以共享使用 AVRO 保存的数据。数据交换服务可能需要一个**编码生成器**来解读数据规格并生成访问数据的代码。AVRO 是脚本语言的理想选择，因为它[不需要这个](https://avro.apache.org/docs/1.11.1/getting-started-python/#serializing-and-deserializing-without-code-generation)。

# JSON

对于在线应用程序，结构化数据通常以 JavaScript 对象表示法（JSON）格式进行序列化和发送。

这意味着大部分大数据以 JSON 格式收集和存储。

然而，由于 JSON 既不高度类型化也没有丰富的模式，处理大数据技术如 Hadoop 中的 JSON 文件可能会很缓慢。

基本上，它对大数据处理框架来说是不可行的。这也是 Parquet 和 ORC 格式被创建的主要原因。

# 压缩

> *它是如何工作的？*

如果我们的主要目标是提高数据处理性能，那么选择**可拆分**压缩类型是快速答案；如果存储成本减少是我们的主要目标，那么选择非可拆分的如**GZIP**和**DEFLATE**。

> *什么是可拆分的？*

***可拆分***意味着 Hadoop Mapper 可以拆分或划分大型文件并并行处理它。压缩编解码器在块级别应用压缩，当它在块级别应用时，即使是非常大的文件，映射器也可以同时读取单个文件。

话虽如此，*可拆分的*压缩类型有**LZO**、**LZ4**、**BZIP2**和**SNAPPY**，而**GZIP**不是。这些压缩器和解压器非常快。因此，如果我们可以容忍较大的存储，那么它比**GZIP**更好的选择。

**GZIP**平均具有 30%的更好压缩率，对于不需要频繁访问（冷存储）的数据来说是一个不错的选择。

# 外部数据在数据湖和数据仓库中

> *我们可以使用数据仓库查询外部数据存储吗？*

通常，所有现代数据仓库都提供外部分区表功能，因此我们可以对数据湖中的数据进行分析。

当然有一些限制，但一般来说，它足够将两个世界连接起来，以便理解它的工作原理。

例如，在 BigQuery 中，我们可以这样在数据仓库中创建一个外部表：

```py
CREATE OR REPLACE EXTERNAL TABLE source.parquet_external_test OPTIONS (

 format = 'PARQUET',

 uris = ['gs://events-export-parquet/public-project/events-*']

);

select * from source.parquet_external_test;
```

你会注意到，在任何数据仓库中，外部表的速度都较慢，并且在许多方面受限，例如并发查询数量有限、不支持数据操作语言（DML）和通配符支持，并且如果在查询运行期间底层数据发生变化，会出现不一致的问题。

所以我们可能想要像这样*加载*它：

```py
LOAD DATA INTO source.parquet_external_test
 FROM FILES(
   format='PARQUET',
   uris = ['gs://events-export-parquet/*']
 )
```

> 本地表更快。

如果有其他数据处理工具，我们可能需要使用 Hive 分区布局。这通常是为了启用外部分区。例如：

```py
gs://events-export-parquet/public-project/parquet_external_test/dt=2018-10-01/lang=en/partitionKey
gs://events-export-parquet/public-project/parquet_external_test/dt=2018-10-02/lang=fr/partitionKey
```

在这里，分区键必须始终保持相同的顺序。对于分区，我们将使用 `key = value` 对，这些对也将作为存储文件夹。

我们可以在任何其他支持 *HIVE* 布局的数据处理框架中使用它。

[](https://mydataschool.com/blog/hive-partitioning-layout/?source=post_page-----275876dc1fc9--------------------------------) [## Hive 分区布局

### Lakehouse 设计是我最喜欢的设计之一，因为它融合了两者的优点。在数据仓库中，我们可以管理…

mydataschool.com](https://mydataschool.com/blog/hive-partitioning-layout/?source=post_page-----275876dc1fc9--------------------------------)

# 结论

如果我们需要对复杂嵌套数据结构进行高效查询的平面列式格式支持，那么我们应该使用 Parquet。它与 Apache Spark 高度集成，并且实际上是该框架的默认文件格式。

如果我们的数据平台架构依赖于使用 Hive 或 Pig 构建的数据管道，那么 ORC 数据格式是更好的选择。它具备列式格式的所有优点，在分析数据湖查询中表现出色，但与 Hive 和 Pig 更好地集成。因此，与 Parquet 相比，它的性能可能更好。

当我们需要更高的压缩比和更快的读取时间时，Parquet 或 ORC 会更合适。与 AVRO 相比，读取时间可能快多达 3 倍。如果我们减少选择读取的列数，还可以进一步提高性能。

然而，我们不太担心输入操作的速度，AVRO 可能是更好的选择，它提供了合理的压缩（使用可拆分的 SNAPPY）、模式演变支持，并且写入速度比其他格式快得多（记住，它是基于行的）。AVRO 在*写入密集型*的大数据操作中绝对领先。因此，它更适合用于我们数据平台源层的数据存储。

`GZIP`、`DEFLATE` 和其他不可拆分的压缩类型在冷存储中效果最好，这里不需要频繁访问数据。

# 推荐阅读

[1\. https://cwiki.apache.org/confluence/display/hive/languagemanual+orc](https://cwiki.apache.org/confluence/display/hive/languagemanual+orc)

[2\. https://parquet.apache.org/](https://parquet.apache.org/)

[3\. https://avro.apache.org/](https://avro.apache.org/)

[4\. https://avro.apache.org/docs/1.11.1/getting-started-python/#serializing-and-deserializing-without-code-generation](https://avro.apache.org/docs/1.11.1/getting-started-python/#serializing-and-deserializing-without-code-generation)

[5\. https://avro.apache.org/docs/1.11.1/specification/](https://avro.apache.org/docs/1.11.1/specification/)

[6\. https://stackoverflow.com/questions/14820450/best-splittable-compression-for-hadoop-input-bz2](https://stackoverflow.com/questions/14820450/best-splittable-compression-for-hadoop-input-bz2)

[7\. https://stackoverflow.com/questions/35789412/spark-sql-difference-between-gzip-vs-snappy-vs-lzo-compression-formats](https://stackoverflow.com/questions/35789412/spark-sql-difference-between-gzip-vs-snappy-vs-lzo-compression-formats)

[`medium.com/ssense-tech/csv-vs-parquet-vs-avro-choosing-the-right-tool-for-the-right-job-79c9f56914a8`](https://medium.com/ssense-tech/csv-vs-parquet-vs-avro-choosing-the-right-tool-for-the-right-job-79c9f56914a8)
