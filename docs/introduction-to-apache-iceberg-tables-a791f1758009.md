# Apache Iceberg 表介绍

> 原文：[`towardsdatascience.com/introduction-to-apache-iceberg-tables-a791f1758009`](https://towardsdatascience.com/introduction-to-apache-iceberg-tables-a791f1758009)

## 选择 Apache Iceberg 作为数据湖的几个令人信服的理由

[](https://mshakhomirov.medium.com/?source=post_page-----a791f1758009--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----a791f1758009--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a791f1758009--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a791f1758009--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----a791f1758009--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a791f1758009--------------------------------) ·8 分钟阅读·2023 年 4 月 10 日

--

![](img/8757d57a9e2fc3aae50cadf617507568.png)

由 [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=medium&utm_medium=referral) 提供的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**Apache Iceberg**：这是什么？Apache Iceberg——它是新的数据湖文件格式吗？还是表格格式？它为何如此优秀？数据湖的时间旅行？

我将在这个故事中尝试回答所有这些问题。

> 事务一致的数据湖表以及时间点快照隔离是我们所需的一切。

选择表格格式是任何追求数据湖或数据网格策略的人都必须做出的关键选择。选择数据湖平台时需要考虑的一些重要功能包括**模式演变支持**、**读写时间**、**可扩展性**（数据处理是否可以 Hadoop 分割？）、**压缩**效率以及**时间旅行**等。

根据业务需求选择正确的文件和表格格式将决定你的数据平台的速度和成本效益。

**Iceberg** 是一种***表格格式***，引擎和文件格式无关。Iceberg 通常不是像**Apache Hive**这样的旧技术的开发。这是件好事，因为从旧技术中开发可能会有局限性。一个好的例子是模式如何随时间变化，而 Iceberg 可以像重命名列一样简单地处理这种变化。***它被设计并证明在全球最苛刻的工作负载中大规模的数据湖平台上表现良好。***

## 越来越多的数据工具开始引入冰山表的支持。

例如，我们可以在 AWS Athena（必须是引擎 3）中这样创建一个 Apache Iceberg 表：

谷歌的 BigQuery 现在也支持 Iceberg 表：

## 冰山表和数据平台类型

Netflix 最初创建了 Iceberg，最终它得到了 Apache 软件基金会的支持和捐赠。现在，Iceberg 独立开发，它是一个完全非盈利的开源项目，专注于处理复杂的数据平台架构。

它支持多种大数据文件格式，包括 Apache Avro、Apache Parquet 和 Apache ORC。

在设计数据平台时，将数据保存在数据湖中是最简单的解决方案之一。

> 相比于现代数据仓库解决方案，它需要的维护要少得多。

数据湖通常用于存储所有类型的数据——结构化和非结构化——无论大小。数据湖传统上与 Apache Hadoop 分布式文件系统（HDFS）连接。然而，组织越来越多地使用对象存储解决方案，如 Amazon S3、Google Cloud Storage 或 Microsoft Azure Data Lake Storage。

> 简而言之，数据湖通过集中管理数据来简化数据管理。

与此相反，当每次访问都通过一个单一系统（数据仓库）进行时，它简化了并发管理和更新，但限制了灵活性并增加了成本。我之前在这里写过：

[](/data-platform-architecture-types-f255ac6e0b7?source=post_page-----a791f1758009--------------------------------) ## 数据平台架构类型

### 它在多大程度上满足了您的业务需求？选择的困境。

towardsdatascience.com

一切都很棒，但在构建数据湖数据平台时，我们可能还会遇到其他一些问题，例如没有时间旅行、没有模式演变支持以及数据转换和定义的复杂性。

> 当我们构建数据湖数据平台时，外部表通常会带来与该架构相关的一整套缺点……但 Iceberg 有助于解决这些问题。

**传统外部表的一些已知限制：**

+   例如，我们**不能通过 DML 语句修改它们，且数据一致性不能得到保证**。话虽如此，如果在处理过程中底层数据发生变化，我们可能会得到不一致的结果。

+   现代数据仓库中通常有**有限的并发查询数**，例如 BigQuery 中为 4。

+   外部表**不支持集群**，也不允许从中导出数据。

+   不允许使用通配符引用表名。

+   **没有时间旅行功能**

根据我的经验，数据一致性是大规模分析所需的最重要的特性之一。Iceberg 解决了这个问题，现在多个引擎（如 Spark、Hive、Presto、Dremio 等）可以同时操作同一张表。

它还提供了许多其他出色的功能，例如回滚到先前的表版本（以悄悄解决问题），以及在处理海量数据时的高级数据过滤能力。

## 数据一致性和改进的处理效率

一个好的例子是，当 ETL 过程通过从存储中添加和删除文件来修改数据集时，另一个读取数据集的应用程序可能会处理数据集的部分或不一致的表示，从而产生不准确的结果。

Iceberg 通常通过利用大量清单（元数据）文件来缓解这些风险，以便在数据处理过程中进行快照。它将捕获模式并维护增量，包括文件信息和分区数据，以保证一致性和完全隔离。

Iceberg 还会自动以层次结构的方式排列快照元数据，这确保了快速高效的表格修改，无需重新定义所有数据集文件，从而在数据湖规模下实现最佳性能。

Iceberg 提供 SQL 命令，允许你合并新数据（MERGE）、更改旧行和删除特定行。

Iceberg 可以积极重建数据文件以提高读取性能，或者使用删除增量加快更新速度。

一个好的合并示例可以在这里找到：

//advanced-sql-techniques-for-beginners-211851a28488?source=post_page-----a791f1758009-------------------------------- ## 初学者的高级 SQL 技巧

### 在 1 到 10 的范围内，你的数据仓库技能有多好？

[towardsdatascience.com

另一个可以应用于上述 AWS Athena 表格的示例如下：

![](img/2ddfd9ebb83759959e340d702256c0e3.png)

图片由作者提供

## 时间旅行功能

现代数据仓库允许你在数据中进行时间旅行，即我们可以转到特定的时间戳以获取表格中的特定数据状态。例如，在 Google Cloud BigQuery 中，我们可以运行以下 SQL 来实现：

```py
SELECT *
FROM `mydataset.mytable`
  FOR SYSTEM_TIME AS OF TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 1 HOUR);
```

> 这也可能是数据湖平台的一个“不错的附加功能”。

如果你围绕文件设计数据架构，例如 Apache ORC 或 Apache Parquet，你将受益于实现的简便，但你也会遇到上述提到的问题。时间旅行不被支持。模式演变一直是个问题。当字段随时间变化时可能会出现问题。例如，AVRO 文件格式支持模式演变，我之前写过关于大数据文件格式优缺点的文章：

//big-data-file-formats-explained-275876dc1fc9?source=post_page-----a791f1758009-------------------------------- ## 大数据文件格式详解

### Parquet 与 ORC 与 AVRO 与 JSON。该选择哪个以及如何使用它们？

[towardsdatascience.com

> Iceberg 将整个历史记录保存在 Iceberg 表格格式中，没有存储系统依赖。

Iceberg 表格用户可以在任何 Iceberg 快照或历史时间点查询过去的状态，以获得一致的结果、进行比较或回滚以修复错误，因为历史状态是不可变的。

```py
SELECT count(*) FROM mydatabase.user_transactions FOR TIMESTAMP AS OF TIMESTAMP '2023-01-01 00:00:00.000000 Z'
```

## 模式演进支持

数据湖文件以及它们的模式随着时间的推移可能会发生变化，这并不是秘密。现在在 Iceberg 表中添加一列不会返回“无效”数据。

列名和顺序可以更改。最棒的是，模式更新从不需要重建你的表。

```py
ALTER TABLE mydatabase.user_transactions ALTER COLUMN total_cost_gbp AFTER user_id;
```

Iceberg 允许就地修改表，并确保正确性，即添加的新列永远不会从其他列中读取现有值。当数据量发生变化时，我们现在可以仅通过 SQL 修改分区布局或更改表模式，即使是嵌套结构。

```py
traffic_source                STRUCT<name STRING, medium STRING, source STRING>,
```

这就是 Iceberg 所谓的分区演进。

> 当你修改分区规范时，使用早期规范写入的现有数据不会受到影响。

每个分区版本的元数据被单独保存。Iceberg 不需要昂贵的操作，例如重写表数据或迁移到新表。

## 改进的分区

在 Iceberg 中，这被称为“隐藏分区”。传统的数据湖分区方式称为“Hive 分区布局”。

让我们考虑一个在 BigQuery 中具有 Hive 分区布局的外部表：

```py
CREATE OR REPLACE EXTERNAL TABLE production.user_transactions (
  payment_date date,
  timestamp timestamp,
  user_id int,
  total_cost_usd float,
  registration_date string

)
WITH PARTITION COLUMNS (
dt STRING, -- column order must match the external path
category STRING)
OPTIONS (
uris = ['gs://user-transactions/*'],
format = 'AVRO',
hive_partition_uri_prefix = 'gs://user-transactions'
);
```

在 Hive 中，分区必须是明确的，并且显示为列，因此 `**user_transactions**` 表将包含一个 `**dt**` 日期列。这意味着对我们表进行操作的所有 SQL 查询都必须除了 `**timestamp**` 过滤器外，还要有 `**dt**` 过滤器。

> Hive 需要分区值。它不了解事务时间戳和我们表中的 `dt` 之间的联系。

**相反**，Iceberg 通过取列值并在必要时进行修改来生成分区值。Iceberg 负责将事务**`timestamp`** 转换为 `**dt**` 并维护连接。Iceberg 可能会隐藏分区，因为它不需要用户维护分区列。

分区值总是被适当地创建并在可能的情况下用于加速查询。

> `dt` 对生产者和消费者都是不可见的，查询不再依赖于我们表的实际物理布局。

它有助于避免在转换数据时出现分区错误。例如，使用错误的日期格式，导致不正确的分区，而不是失败：`**20230101**` 代替 `**2023–01–01**`。***这是 Hive 分区布局中一个众所周知且最常见的问题。*** 另一个 Iceberg 有助于解决的问题是，在 Hive 分区布局中，所有工作的查询都与表的分区方案相关，因此更改分区设置将会破坏查询。

## 结论

关系型数据库和数据仓库都支持原子事务和时间旅行，但它们以其专有的方式实现。Iceberg 作为一个 Apache 项目，是完全开源的，不依赖于任何特定的工具或数据湖引擎。

Iceberg 支持行业标准文件格式如 Parquet、ORC 和 Avro，并且与 Dremio、Spark、Hive 和 Presto 等关键数据湖引擎兼容。其广泛的社区合作生成新想法，并提供长期的帮助。它拥有各种活跃的社区，例如公共 Slack 频道，任何人都可以自由参与。它经过设计并证明在全球最大工作负载和环境中的数据湖平台上表现出色。

组织现在可以通过利用 Iceberg 充分发挥数据湖架构的潜力和优势。体验基于云存储的数据平台的成本效益，同时无需牺牲传统数据库和数据仓库解决方案的功能和能力。

> 它使我们的数据湖平台真正灵活且可靠

**总结一下，Iceberg 表格格式的优点如下：**

+   多个独立程序可以同时一致地处理相同的数据集。

+   改进的数据处理和数据可靠性（针对非常大的数据湖规模表格的更新更加高效）。

+   随着时间的推移，改进的模式处理。

+   ETL 管道大大简化（通过对数据湖中的数据进行操作，而不是在多个独立系统之间传输数据）。

## 推荐阅读：

1\. [`cloud.google.com/bigquery/docs/time-travel`](https://cloud.google.com/bigquery/docs/time-travel)

2\. [`www.apache.org/`](https://www.apache.org/)

3\. [`hive.apache.org/`](https://hive.apache.org/)

4\. [`iceberg.apache.org/community/`](https://iceberg.apache.org/community/)

5\. [`cloud.google.com/bigquery/docs/iceberg-tables`](https://cloud.google.com/bigquery/docs/iceberg-tables)

6\. [`medium.com/towards-data-science/data-platform-architecture-types-f255ac6e0b7`](https://medium.com/towards-data-science/data-platform-architecture-types-f255ac6e0b7)

7\. [`medium.com/towards-data-science/big-data-file-formats-explained-275876dc1fc9`](https://medium.com/towards-data-science/big-data-file-formats-explained-275876dc1fc9)

8\. [`iceberg.apache.org/docs/latest/evolution/`](https://iceberg.apache.org/docs/latest/evolution/)

9\. [`iceberg.apache.org/docs/latest/partitioning/#icebergs-hidden-partitioning`](https://iceberg.apache.org/docs/latest/partitioning/#icebergs-hidden-partitioning)

10\. [`cloud.google.com/bigquery/docs/external-data-cloud-storage#sql`](https://cloud.google.com/bigquery/docs/external-data-cloud-storage#sql)
