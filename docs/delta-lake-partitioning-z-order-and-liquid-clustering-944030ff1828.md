# Delta Lake — 分区、Z-Order 和 Liquid Clustering

> 原文：[`towardsdatascience.com/delta-lake-partitioning-z-order-and-liquid-clustering-944030ff1828`](https://towardsdatascience.com/delta-lake-partitioning-z-order-and-liquid-clustering-944030ff1828)

## Delta 中的不同分区/聚类方法是如何实现的？它们在实际中是如何工作的？

[](https://medium.com/@vitorf24?source=post_page-----944030ff1828--------------------------------)![Vitor Teixeira](https://medium.com/@vitorf24?source=post_page-----944030ff1828--------------------------------)[](https://towardsdatascience.com/?source=post_page-----944030ff1828--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----944030ff1828--------------------------------) [Vitor Teixeira](https://medium.com/@vitorf24?source=post_page-----944030ff1828--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----944030ff1828--------------------------------) ·10 分钟阅读·2023 年 11 月 8 日

--

![](img/19486b33b135625836f8ae64791213b1.png)

照片由 [frame harirak](https://unsplash.com/@framemily?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

使大数据变得困难的问题之一一直在于它的名字，它就是“大”。分区，特别是当做得好时，一直是一种通过将需要读取的数据减少到一个子集来提高对大量数据的查询执行时间的方法。然而，分区数据是复杂的，需要仔细考虑和一些前期规划，因为今天适用的要求可能不适合未来的要求。例如，在 Hive 风格的分区中，列可能需要更改或增加其基数，从而使数据过度分区（小文件问题），这会导致数据的完全重组，这并不理想。

Z-Order 聚类是另一种用于数据跳过的技术，同样避免了全面的数据扫描。然而，这种技术有一些局限性。其中之一是新摄取的数据默认情况下未排序，用户需要重新聚类，这意味着已经聚类的数据将被重新聚类和重写，增加了操作所花费的时间。Z-Order 用户还需要每次运行命令时定义聚类列，因为它们不是表属性的一部分。

这就是 Liquid Clustering 进入游戏的地方。前提是它可以无缝地融入当前的数据布局，并且能够适应未来的需求，而无需重写任何已经聚类的数据。

在这篇文章中，我们将解释 Delta 中不同数据修剪策略的细节及其应用方式。

# 分区修剪 — Hive 风格的分区

![](img/b0a77d05e7bf03d305d32e080f7a0bf0.png)

Hive 样式分区 — 作者提供的图片

Hive 样式分区是一种将表组织成小块的方式。这些数据块被组织成几个子文件夹，包含分区值的数据。

```py
dbfs://people10m/gender=M/data_0.json
dbfs://people10m/gender=M/data_1.json
dbfs://people10m/gender=F/data_0.json
dbfs://people10m/gender=F/data_1.json
```

这种方法不是 Delta 的原生方法，即不属于 Delta 协议的一部分。然而，由于 Delta 建立在 Apache Spark 之上，旧的 Hive 样式分区在某些场景下也可以很好地工作。

几种机制处理这种类型的分区，使得它对最终用户完全不可见。在 Apache Spark 中，当用户读取数据集时，*gender* 列会自动添加到模式中，并带有相应的值，可以像常规列一样进行查询。这种技术称为 *分区发现*，由 [DataSource’s resolveRelation](https://github.com/apache/spark/blob/master/sql/core/src/main/scala/org/apache/spark/sql/execution/datasources/DataSource.scala#L341C7-L341C22) 处理，它从给定的基本路径推断分区列。另一方面，当用户使用 [*partitionBy*](https://github.com/apache/spark/blob/master/sql/core/src/main/scala/org/apache/spark/sql/DataFrameWriter.scala#L194) 保存 DataFrame 时，会执行 [InsertIntoHadoopFsRelationCommand](https://github.com/apache/spark/blob/master/sql/core/src/main/scala/org/apache/spark/sql/execution/datasources/InsertIntoHadoopFsRelationCommand.scala#L178C9-L178C25) 作为执行计划的一部分，这会调用 [FileFormatWriter](https://github.com/apache/spark/blob/master/sql/core/src/main/scala/org/apache/spark/sql/execution/datasources/FileFormatWriter.scala#L111)，为每个底层 RDD 的分区生成一个写入作业（从最终模式中排除分区列并为其创建桶）。

在上述示例中，由于查询仅选择性别为 *F* 的数据，它将只需要实际扫描该文件夹，从而有效跳过数据，因为它只读取数据集中的一半文件。这称为 *分区剪枝*。

这种方法有一些缺点，特别是当选择具有非常高基数的分区列或多个分区级别时，这会导致许多小文件，从而导致更差的读取性能。此外，一旦定义了这种分区策略，就不能在不重写所有数据的情况下更改，因为它是在物理层面上定义的。

# I/O 剪枝 — Z-Order

另一种有效跳过数据的技术是对文件级统计数据进行过滤。在这种技术中，每个文件都有可用的统计数据，可以作为是否值得读取文件的指标。默认情况下，Delta 存储前 32 列的最小值、最大值和空值计数的统计数据。

以[*people10m*](https://learn.microsoft.com/en-us/azure/databricks/dbfs/databricks-datasets#create-a-table-based-on-a-databricks-dataset)公共数据集中的单列*id*为例。如果我们使用[*repartitionByRange*](https://github.com/apache/spark/blob/master/sql/core/src/main/scala/org/apache/spark/sql/Dataset.scala#L3713C24-L3713C24)在该列上将数据排序为 5 个不同的文件，最小/最大统计分布可能类似于以下内容：

![](img/14328ffa6c45225cca3e6f30bc100091.png)

按列 ID 范围分区后的文件 — 图片作者

![](img/c3766938e2950352d739854579abf2f3.png)

选择公司的前 20,000 名员工 — 图片作者

运行上述查询将产生一个良好的计划，因为我们的查询仅筛选该列且所有文件包含不重叠的 ID 集合。这样，数据库引擎更容易选择正确的文件进行扫描，而不会产生假阳性。

**如果我们想在查询中添加另一列怎么办？**

假设我们还想按员工的*薪资*进行筛选。

![](img/6da12186e5133712dc6e4fcdd761ddfa.png)

选择薪资大于 40,000 的公司前 20,000 名员工 — 图片作者

在我们按两列范围分区文件之后，我们最终得到如下结果：

![](img/e33bcb7cf94902d70e11fcb7477f6a6b.png)

按列 ID 和薪资范围分区后的文件 — 图片作者

薪资与 ID 没有直接关系，使用之前的线性方法将文件组织成有效的数据跳过方式将导致数据仅按第一列排序。通过简单地筛选薪资大于 40,000，我们最终会读取所有五个文件，而不仅仅是一个。

**我们如何解决这个问题？是否有办法在保持位置性的同时将多个统计信息分组到单一维度，从而使我们的范围分区正常工作？**

如果你猜测了 Z-Ordering，你猜对了。如果你猜测了填充空间曲线，你就更对了！

什么是空间填充曲线，我需要关注它吗？空间填充曲线是一种遍历嵌入空间中所有点的曲线。一些曲线能够将这些高维点映射到一个维度，同时保持在原始空间中的邻近性。听起来复杂？其实并不复杂。下面我们将详细介绍这些曲线的工作原理。

## **Z-Order 曲线**

Z-Order 曲线是 Delta 中空间填充曲线聚类的第一次实现，因此得名。

![](img/fa275e1adb7012551482c131ee7edb6d.png)

级别 1 Z-Order 曲线 — 图片作者

Z-Order 值，即形成 Z 形状的曲线的点，是使用称为位交错的技术计算得出的。位交错是一种使用位表示 N 维坐标的方法。例如，如果我们使用 4 位表示（0000 到 1111），我们能够通过逐位分配给每个轴来编码 4x4 网格坐标。接下来，我们将通过一个更直观的示例来展示这种技术。

在 Delta 中，Z-Ordering 用于以使数据跳过操作有效的方式对数据进行分组。所有 Z-Order 列都被“标记”为使用[RangePartitionId](https://github.com/delta-io/delta/blob/master/spark/src/main/scala/org/apache/spark/sql/delta/expressions/RangePartitionId.scala)表达式进行范围分区。该表达式只是一个占位符，将由一个[优化器](https://github.com/delta-io/delta/blob/master/spark/src/main/scala/org/apache/spark/sql/delta/optimizer/RangePartitionIdRewrite.scala)处理，该优化器将对 RDD 进行采样，以找到列的范围边界。（如果你曾经尝试对一个相当大的数据集进行多次 Z-Order，你可能会注意到其文件统计数据是不确定的。这是因为 Delta 使用[水库抽样](https://en.wikipedia.org/wiki/Reservoir_sampling)来避免在计算范围 ID 时读取整个数据集）。然后，所有计算出的范围被转换为字节并交错，这样就得出了行的 Z-Order 值。

以下我们将以简化的方式说明 Z-Order 在 Delta 中的工作原理，以一个 6 条记录和 3 个分区的组为例。

![](img/86c0579fc76b1c0e1309de6a3c372ded.png)

针对 6 条记录到 3 个不同范围 ID 的 Z-Order 优化—作者提供的图像

## **希尔伯特曲线**

曲线在保持局部性的能力越强，我们由于误报需要读取的文件就越少。这就是为什么希尔伯特曲线在保持局部性至关重要的场景中更常使用的原因。

在撰写时，[希尔伯特曲线](https://en.wikipedia.org/wiki/Hilbert_curve)尚未在 Delta 的开源版本中实现。然而，它们是 Databricks Z-Order 实现中使用的默认曲线，因为它们相比 Z-Order 曲线在处理高维数据时提供了更好的数据局部性。

![](img/d9cfa094a98eaae6c55367e2a4b80e26.png)

希尔伯特曲线—作者提供的图像

希尔伯特曲线可以以四种不同的方式出现，每种方式都是在上述基础上旋转 90º得到的。

**但为什么希尔伯特曲线在保持局部性方面比默认的 Z-Order 曲线更好？**

希尔伯特曲线的相邻点之间的距离始终为 1。与 Z-Order 不同，这意味着这些跳跃可能会生成具有较大最小/最大差异的 Z-Order 文件，从而使其无用。

![](img/cb1e1a96e06d0273fb050b44c844dc2f.png)

Z-Order 曲线上的相邻点之间的距离—作者提供的图像

该算法有几个实现，但在这篇文章中，我将介绍 John Skilling 在“[编程希尔伯特曲线](https://pubs.aip.org/aip/acp/article-abstract/707/1/381/719611/Programming-the-Hilbert-curve?redirectedFrom=PDF)”中的一个整洁的迭代方法。这个算法可能会让人困惑，因为它包含了一些位操作。如果你不需要了解细节，可以直接跳到下一节。

**请注意，由于 Databricks 代码是专有的，以下示例可能不代表当前实现。**

J. Skilling 编码方法将位交错并使用[格雷码](https://en.wikipedia.org/wiki/Gray_code)进行编码。这样，每次只改变一个位，因此遍历网格时只会在垂直或水平方向进行。然后，它遍历编码后的位，并应用一系列位交换和反转，最终返回坐标的位表示，可以通过解交错来恢复。

![](img/31ae9ad47977944b7913fe0f255c3c36.png)

将笛卡尔点转换为希尔伯特索引的 Skilling 变换 — 来源于 [编程希尔伯特曲线](https://pubs.aip.org/aip/acp/article-abstract/707/1/381/719611/Programming-the-Hilbert-curve?redirectedFrom=PDF)

类似于 Z-Order，我们需要一种将任意维度的坐标组编码为单一点的方法。为了实现这一点，我们将运行之前的算法，但要反向运行，以便可以检索希尔伯特曲线中的点。然后有两个循环，一个循环将遍历编码位，从最重要到最不重要，直到 *p-2*，其中 *p* 是每个轴上的位数，另一个内循环将从最不重要的位迭代到 *n-1*，其中 *n* 是维度数。根据当前的位，我们将交换位或反转它们。最后，我们需要对位进行格雷解码，就能得到我们的点。

接下来，我们将介绍如何对坐标 (2, 0) 进行编码，它表示希尔伯特曲线中的点号 14。

![](img/38876c735cce829a8cde5369afcf7bee.png)

将笛卡尔坐标转换为希尔伯特曲线点的算法 — 作者提供的图像

![](img/d80f071d67415f6d4e68a37c66bbc8a7.png)

4x4 希尔伯特曲线 — 作者提供的图像

从这里开始，我们假设过程与 Z-Order 实现相同，其中数据被划分范围，并且相近的记录被写入同一文件。

## **液体聚类**

那么，液体聚类究竟是什么？它不过是希尔伯特曲线加上一个名为 ZCube 的新特性，使得增量聚类成为可能！

*OPTIMIZE ZORDER BY* 命令要求完全重写数据，这对大型表非常昂贵。此外，当 *OPTIMIZE ZORDER* 命令中出现问题时，一切需要从头开始，这有时会非常麻烦。

**什么是 ZCubes？**

ZCubes 是由相同 *OPTIMIZE* 任务生成的一组文件。这样，一个大型表的 *OPTIMIZE* 任务可以分成几个不同的任务，这些任务会生成新的 ZCube，并在增量日志中生成新的条目，以实现增量聚类。每个新优化的文件将包含 AddFile 元数据中的 *ZCUBE_ID* 属性，这将使其可能区分优化和未优化的文件（即没有关联 ZCube 的文件）。

有两个新的可配置 ZCube 属性：

+   ***MIN_ZCUBE_SIZE*** 设置 ZCUBE 的最小尺寸。低于此尺寸的 ZCUBE 将被视为 *OPTIMIZE* 任务的一部分，新文件可以被合并，直到尺寸达到此阈值（默认为 100GB）。这些立方体被称为 **部分 ZCubes**。

+   ***TARGET_CUBE_SIZE*** 设置完成的立方体的目标尺寸，包含超过目标尺寸的文件。这些立方体被称为 **稳定 ZCubes**。

如果 *Delete* 命令使大量文件无效，从而使其小于 *MIN_ZCUBE_SIZE*，稳定 ZCubes 可能会重新变为部分 ZCubes。

**它如何无缝适应新的分区列？**

当用户更改聚类列时，只有包含相同聚类列的 ZCubes 会被考虑进行优化。其他立方体保持不变，新立方体会被创建。

**这在实践中是如何工作的？**

当发出 *OPTIMIZE table* 命令时，Delta 会选择有效的文件用于 ZCube 生成，这些文件是部分 ZCube 的一部分（可以进一步优化），以及新文件。然后，进行规划步骤，将文件打包到多个 ZCubes 下，这些 ZCubes 是相互独立运行的 *OPTIMIZE* 任务。

![](img/d5446f72461d392e0bec909f5ab936b0.png)

启用液态聚类的 OPTIMIZE 流程 — 作者图示

**如何启用/禁用液态聚类？**

```py
--New tables
CREATE TABLE <table>
USING delta
CLUSTER BY (<col1>, <col2>, …)

--Existing tables
ALTER TABLE <table>
CLUSTER BY (<col1>, <col2>, …)

--Remove liquid clustering
ALTER TABLE <table>
CLUSTER BY NONE
```

由于聚类列是在表级别定义的，*OPTIMIZE* 命令不需要定义任何参数。

**注意：这仍在** [**提议**](https://docs.google.com/document/d/e/2PACX-1vREkVPDxqlKrwnaQ7Et1EnaiCF-VhFXCwit7bGSomWKtGEfkxbuGhX4GP3cJ20LgllYfjzsjr2lyY5y/pub#kix.301alpimymwh) **中，可能会有所变化。**

# 结论

在这篇博客文章中，我们详细讨论了 Delta Lake 中可用的不同分区和聚类选项。我们讨论了 Hive 风格分区、Z-Order 及其当前问题，展示了液态聚类如何解决这些问题。

液态聚类非常有前途，因为它使用起来更简单，具有增量和更好的聚类性能，并且支持在没有任何开销的情况下更改分区列。如果你对性能感兴趣，这里有几个性能比较，你也可以尝试使用 Databricks Runtime 13.3+。Databricks 推荐将所有当前的分区列和 ZOrder 列更改为聚类列，以获得更好的性能。

如果你在使用开源 Delta，尽管液态聚类功能不可用，请确保查看我之前的帖子，了解如何保持你的表格快速而干净：

[](/delta-lake-keeping-it-fast-and-clean-3c9d4f9e2f5e?source=post_page-----944030ff1828--------------------------------) ## Delta Lake— Keeping it fast and clean

### 是否曾想过如何提升 Delta 表的性能？手把手教你如何保持 Delta 表快速而干净。

[towardsdatascience.com

# 参考文献

[`docs.databricks.com/en/delta/clustering.html`](https://docs.databricks.com/en/delta/clustering.html)

[`docs.google.com/document/d/e/2PACX-1vREkVPDxqlKrwnaQ7Et1EnaiCF-VhFXCwit7bGSomWKtGEfkxbuGhX4GP3cJ20LgllYfjzsjr2lyY5y/pub#kix.301alpimymwh`](https://docs.google.com/document/d/e/2PACX-1vREkVPDxqlKrwnaQ7Et1EnaiCF-VhFXCwit7bGSomWKtGEfkxbuGhX4GP3cJ20LgllYfjzsjr2lyY5y/pub#kix.301alpimymwh)

[`pubs.aip.org/aip/acp/article-abstract/707/1/381/719611/Programming-the-Hilbert-curve`](https://pubs.aip.org/aip/acp/article-abstract/707/1/381/719611/Programming-the-Hilbert-curve)

[`en.wikipedia.org/wiki/Z-order_curve`](https://en.wikipedia.org/wiki/Z-order_curve)

[`en.wikipedia.org/wiki/Hilbert_curve`](https://en.wikipedia.org/wiki/Hilbert_curve)
