# Apache Spark 优化技术

> 原文：[`towardsdatascience.com/apache-spark-optimization-techniques-fa7f20a9a2cf`](https://towardsdatascience.com/apache-spark-optimization-techniques-fa7f20a9a2cf)

## 对一些最常见的 Spark 性能问题及其解决方案的回顾

[](https://pierpaoloippolito28.medium.com/?source=post_page-----fa7f20a9a2cf--------------------------------)![Pier Paolo Ippolito](https://pierpaoloippolito28.medium.com/?source=post_page-----fa7f20a9a2cf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fa7f20a9a2cf--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fa7f20a9a2cf--------------------------------) [Pier Paolo Ippolito](https://pierpaoloippolito28.medium.com/?source=post_page-----fa7f20a9a2cf--------------------------------)

·发表于[数据科学之路](https://towardsdatascience.com/?source=post_page-----fa7f20a9a2cf--------------------------------) ·阅读时间 5 分钟·2023 年 1 月 11 日

--

![](img/26df9dea1fbdfec07dfe0a6878c48e93.png)

图片由[Manuel Nägeli](https://unsplash.com/@gwundrig?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

Apache Spark 目前是业内最受欢迎的大数据技术之一，由 Databricks 和 Palantir 等公司支持。

数据工程师在使用 Spark 时的一个关键职责是编写高度优化的代码，以充分利用 Spark 的分布式计算能力（图 1）。

![](img/5aeb1c52bb6752bdcec1ead3e338ef96.png)

图 1：Apache Spark 架构（图片由作者提供）。

在本文中，你将了解在使用 Spark 时一些常见的性能问题（例如 5 Ss）以及如何解决这些问题。如果你对 Apache Spark 完全陌生，可以在我的上一篇文章中找到更多信息。

# 5 Ss

5 Ss（Spill、Skew、Shuffle、Storage、Serialization）是 Spark 中 5 个最常见的性能问题。提高 Spark 性能的两个关键通用方法是：

+   减少摄取的数据量。

+   减少 Spark 读取数据的时间（例如，使用磁盘分区/ Z Order 聚类的谓词下推）。

我们将深入探讨与 5 Ss 相关的每一个问题。

## Spill

Spill 是由于内存不足时将临时文件写入磁盘造成的（一个分区过大无法容纳在 RAM 中）。在这种情况下，RDD 首先从 RAM 移动到磁盘，然后再移回 RAM，以避免内存溢出（OOM）错误。磁盘读写虽然可能很昂贵，因此应尽可能避免。

通过检查 Spark UI 中的 **溢出（内存）** 和 **溢出（磁盘）** 值，可以更好地理解溢出问题。

+   溢出（内存）：溢出分区在内存中的数据大小。

+   溢出（磁盘）：溢出分区在磁盘上的数据大小。

为了缓解溢出，有两种可能的方法：一是为每个工作节点实例化更多内存的集群，二是增加分区数量（从而使现有分区变小）。

## 偏斜

使用 Spark 时，数据通常以 128 MB 的均匀分布分区进行读取。对数据应用不同的变换可能导致一些分区变得比平均值大得多或小得多。

偏斜是不同分区之间大小不均衡的结果。少量偏斜可能是可以接受的，但在某些情况下，偏斜可能导致溢出和内存溢出错误。

减少偏斜的两种可能方法是（见图 2）：

+   用随机数对偏斜的列进行盐化，以重新分配分区大小。

+   使用自适应查询执行（Spark 3）。

![](img/5edc2b3878a127c518c20edb49c082ef.png)

图 2：偏斜前后分区大小分布（作者提供的图像）。

## 洗牌

洗牌是指在执行宽变换（例如连接、分组等）或某些动作（如计数）时在执行器之间移动数据（见图 3）。处理不当的洗牌问题可能导致偏斜。

![](img/7f34a9526e0c58489f4cece8de613e3d.png)

图 3：洗牌过程（作者提供的图像）。

为了减少洗牌量，可以使用以下方法：

+   实例化更少且更大的工作节点（从而减少网络 IO 开销）。

+   在洗牌前对数据进行预筛选，以减少其大小。

+   对涉及的数据集进行反规范化。

+   优先使用固态硬盘而非硬盘驱动器，以实现更快的执行速度。

+   在处理小表时，对较小的表进行广播哈希连接。对于大表，则使用排序合并连接（广播哈希连接在大表中可能导致内存溢出问题）。

## 存储

存储问题出现在数据以非最佳方式存储在磁盘上。与存储相关的问题可能会导致过度洗牌。与存储相关的三个主要问题是：小文件、扫描和模式。

+   **小文件：** 处理小于 128 MB 的分区文件。

+   **扫描：** 在扫描目录时，我们可能会遇到单个目录中有大量文件，或者在高度分区的数据集中有多个层级的文件夹。为了减少扫描量，我们可以将其注册为表。

+   **模式：** 根据所使用的文件格式，可能会有不同的模式问题。例如，使用 JSON 和 CSV 时，需要读取全部数据以推断数据类型。对于 Parquet，只需读取一个文件，但如果需要处理可能的模式变化，必须读取所有 Parquet 文件。为了提高性能，提前提供模式定义可能会有所帮助。

## 序列化

序列化包括所有与代码在集群中分发相关的问题（代码被序列化，发送到执行器，然后反序列化）。

在 Python 的情况下，这个过程可能会更复杂，因为代码必须被 pickle 化，并且需要为每个执行器分配一个 Python 解释器实例。

在与遗留系统（例如 Hadoop）、第三方库和自定义框架集成时，可能会出现序列化问题。我们可以采取的一个关键方法是避免使用 UDF 或 Vectorized UDF（它们对于 Catalyst Optimizer 来说像一个黑匣子）。

# 结论

即使在最新的 Apache Spark 3 发布版中，Spark 优化仍然是从业人员的专业知识和领域知识至关重要的核心领域之一，以便成功地最大限度地利用 Spark 的能力。作为本文的一部分，涵盖了一些在 Spark 项目中可能遇到的关键问题，尽管这些问题在某些情况下可能高度相关，从而使得追踪主要根本原因变得困难。

# 联系方式

如果你想随时了解我的最新文章和项目，请 [关注我在 Medium 上](https://pierpaoloippolito28.medium.com/subscribe) 并订阅我的 [邮件列表](http://eepurl.com/gwO-Dr)。以下是我的一些联系方式：

+   [Linkedin](https://uk.linkedin.com/in/pier-paolo-ippolito-202917146)

+   [个人网站](https://pierpaolo28.github.io/)

+   [Medium 个人资料](https://towardsdatascience.com/@pierpaoloippolito28)

+   [GitHub](https://github.com/pierpaolo28)

+   [Kaggle](https://www.kaggle.com/pierpaolo28)
