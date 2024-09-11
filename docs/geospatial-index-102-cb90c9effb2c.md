# 地理空间索引 102

> 原文：[https://towardsdatascience.com/geospatial-index-102-cb90c9effb2c?source=collection_archive---------16-----------------------#2023-04-11](https://towardsdatascience.com/geospatial-index-102-cb90c9effb2c?source=collection_archive---------16-----------------------#2023-04-11)

## 一个实际示例，展示如何应用地理空间索引

[](https://medium.com/@thanakornpanyapiang?source=post_page-----cb90c9effb2c--------------------------------)[![Thanakorn Panyapiang](../Images/4e68a74a2039c8404c5873d7d364be43.png)](https://medium.com/@thanakornpanyapiang?source=post_page-----cb90c9effb2c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cb90c9effb2c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cb90c9effb2c--------------------------------) [Thanakorn Panyapiang](https://medium.com/@thanakornpanyapiang?source=post_page-----cb90c9effb2c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3946a0d40f8c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-index-102-cb90c9effb2c&user=Thanakorn+Panyapiang&userId=3946a0d40f8c&source=post_page-3946a0d40f8c----cb90c9effb2c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cb90c9effb2c--------------------------------) · 7分钟阅读 · 2023年4月11日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcb90c9effb2c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-index-102-cb90c9effb2c&source=-----cb90c9effb2c---------------------bookmark_footer-----------)

# 介绍

地理空间索引是一种索引技术，它提供了一种优雅的方式来管理基于位置的数据。它使得地理空间数据可以高效地搜索和检索，从而系统能够为用户提供最佳体验。本文将通过将地理空间索引应用于实际数据，并展示这样做所带来的性能提升，来演示其实际操作。让我们开始吧。*（注意：如果你从未听说过地理空间索引或希望了解更多信息，可以查看* [*这篇文章*](https://medium.com/towards-data-science/geospatial-index-101-df2c011da04b)*)*

# 数据

本文使用的数据是[芝加哥犯罪数据](https://console.cloud.google.com/marketplace/details/city-of-chicago-public-data/chicago-crime)，这是[Google Cloud 公共数据集计划](https://cloud.google.com/bigquery/public-data)的一部分。任何拥有 Google Cloud Platform 账户的人都可以免费访问此数据集。数据集包含约 800 万行数据（总量为 1.52 GB），记录了自 2001 年以来发生在芝加哥的犯罪事件，每条记录都有指示事件位置的地理数据。

# 平台

我们不仅会使用 Google Cloud 的数据，还会使用 Google Big Query 作为数据处理平台。Big Query 提供了每个执行查询的作业执行细节。这包括使用的数据量和处理的行数，这将非常有助于展示优化后的性能提升。

# 实现

我们将通过优化基于位置的查询来展示地理空间索引的强大功能。在这个例子中，我们将使用[*Geohash*](https://en.wikipedia.org/wiki/Geohash#:~:text=Geohash%20is%20a%20public%20domain,Morton%20in%201966.)作为索引，因为它的简洁性和 Google BigQuery 的原生支持。

我们将检索发生在距离*芝加哥联合车站*2公里范围内的所有犯罪记录。在优化之前，让我们看看在原始数据集上运行此查询时的性能表现：

```py
-- Chicago Union Station Coordinates = (-87.6402895591744 41.87887332682509)
SELECT 
  * 
FROM 
  `bigquery-public-data.chicago_crime.crime`
WHERE 
  ST_DISTANCE(ST_GEOGPOINT(longitude, latitude), ST_GEOGFROMTEXT("POINT(-87.6402895591744 41.87887332682509)")) <= 2000
```

以下是作业信息和执行细节的样子：

![](../Images/5a9071f2857845eb83a5a8cf8cee8403.png)

作业信息（作者提供的图片）

![](../Images/ea4e015ea548cd43aa2f2f79b8af4d8b.png)

执行细节（作者提供的图片）

从*处理的字节数*和*读取的记录数*中，你可以看到查询扫描了整个表并处理了每一行，以获取最终结果。这意味着数据越多，查询所需时间越长，处理成本也越高。这可以更高效吗？当然可以，这就是地理空间索引发挥作用的地方。

上述查询的问题在于，尽管许多记录距离关注点（芝加哥联合车站）较远，但仍然必须处理这些记录。如果我们能排除这些记录，那么查询将更高效。

Geohash 可以是解决这个问题的方案。除了将坐标编码为文本外，geohash 的另一个优点是哈希还包含地理空间属性。哈希之间的相似性可以推测它们所代表位置的地理相似性。例如，`wxcgh` 和 `wxcgd` 代表的两个区域相近，因为这两个哈希非常相似，而 `accgh` 和 `dydgh` 之间则相距较远，因为这两个哈希非常不同。

我们可以利用这一属性与 [聚簇表](https://cloud.google.com/bigquery/docs/clustered-tables) 进行配合，通过预先计算每一行的 geohash。然后，我们计算芝加哥联合车站的 geohash。这样，我们可以在预先筛选掉所有与芝加哥联合车站的 geohash 不够 *接近* 的记录。

实现方法如下：

1.  **创建一个新表，并添加一个新列来存储坐标的 geohash。**

```py
CREATE TABLE `<project_id>.<dataset>.crime_with_geohash_lv5` AS (
  SELECT *, ST_GEOHASH(ST_GEOGPOINT(longitude, latitude), 5) as geohash
  FROM `bigquery-public-data.chicago_crime.crime` 
)
```

**2\. 使用 geohash 列作为聚簇键创建聚簇表**

```py
CREATE TABLE `<project_id>.<dataset>.crime_with_geohash_lv5_clustered` 
CLUSTER BY geohash
AS (
  SELECT *
  FROM `<project_id>.<dataset>.crime_with_geohash_lv5`
)
```

通过使用 geohash 作为聚簇键，我们创建了一个表，其中共享相同哈希的行被物理地存储在一起。如果你考虑一下，实际发生的情况是数据集按地理位置进行分区，因为地理位置越接近的行更有可能具有相同的哈希。

**3\. 计算芝加哥联合车站的 geohash。**

在这篇文章中，我们使用了 [*这个网站*](https://www.movable-type.co.uk/scripts/geohash.html)，但有很多不同编程语言的库可以让你以编程方式实现这一功能。

![](../Images/2ec306ddc119f65cc73ea6ffdeb21e60.png)

芝加哥联合车站的 geohash（作者提供的图像）

**4\. 将 geohash 添加到查询条件中。**

```py
SELECT 
  * 
FROM 
  `<project_id>.<dataset>.crime_with_geohash_lv5_clustered`
WHERE 
  geohash = "dp3wj" AND 
  ST_DISTANCE(ST_GEOGPOINT(longitude, latitude), ST_GEOGFROMTEXT("POINT(-87.6402895591744 41.87887332682509)")) <= 2000
```

这次查询应该只扫描位于 `dp3wj` 的记录，因为 geohash 是表的聚簇键。这应该能节省很多处理时间。让我们看看会发生什么。

![](../Images/6da1e87a82d8e8f37c360c66615f8f34.png)

创建聚簇表后的作业信息（作者提供的图像）

![](../Images/cc1df2e52830f58cc0abaf6f6603b535.png)

创建聚簇表后的执行细节（作者提供的图像）

从作业信息和执行细节中，你可以看到处理的字节数和扫描的记录数显著减少（从 1.5 GB 减少到 55 MB，7M 减少到 260k）。通过引入 geohash 列并将其用作聚簇键，我们可以仅通过查看一列就提前排除所有明显不满足查询的记录。

但是，我们还没有完成。仔细查看输出的行数，你会发现它只有 100k 条记录，而正确的结果应该有 380k 条。我们得到的结果仍然不正确。

**5\. 计算邻近区域并将它们添加到查询中。**

在这个例子中，所有的邻近哈希是 `dp3wk`、`dp3wm`、`dp3wq`、`dp3wh`、`dp3wn`、`dp3wu`、`dp3wv` 和 `dp3wy`。我们使用在线的 [*geohash 探索器*](https://chrishewett.com/blog/geohash-explorer/) 来查看这些，但这也可以完全通过代码实现。

![](../Images/7b540290a4d435d840fa9c82c8223071.png)

dp3wj 的邻居（作者提供的图像）

为什么我们需要在查询中添加邻域区域？因为地理哈希只是位置的*一种近似*。虽然我们知道芝加哥联合车站在`dp3wj`区域内，但我们仍然不知道它在区域中的具体位置。是在顶部、底部、左侧还是右侧？我们无从得知。如果它在顶部，那么在`dp3wm`中的一些数据可能比2公里更接近。如果它在右侧，则`dp3wn`区域中的一些数据可能距离更近。依此类推。这就是为什么所有邻域哈希必须包含在查询中以获得正确结果的原因。

请注意，地理哈希级别5的精度为5公里。因此，除上图中的区域外，所有其他区域距离芝加哥联合车站都太远。这是另一个需要做出的重要设计选择，因为它有巨大的影响。如果精度过粗，我们得到的收益很少。另一方面，使用过细的精度级别会使查询变得复杂。

这是最终查询的样子：

```py
SELECT 
  * 
FROM 
  `<project_id>.<dataset>.crime_with_geohash_lv5_clustered`
WHERE 
  (
    geohash = "dp3wk" OR
    geohash = "dp3wm" OR
    geohash = "dp3wq" OR
    geohash = "dp3wh" OR
    geohash = "dp3wj" OR
    geohash = "dp3wn" OR
    geohash = "dp3tu" OR
    geohash = "dp3tv" OR
    geohash = "dp3ty"
  ) AND 
  ST_DISTANCE(ST_GEOGPOINT(longitude, latitude), ST_GEOGFROMTEXT("POINT(-87.6402895591744 41.87887332682509)")) <= 2000
```

这就是执行查询时发生的情况：

![](../Images/9b7f234072d4f08729a578b8d41ce331.png)

添加邻域哈希后的工作信息（图片由作者提供）

![](../Images/11ee2dfe07a9d4fc63b6d7838a205b63.png)

添加邻域哈希后的执行细节（图片由作者提供）

现在结果是正确的，查询处理了527 MB的数据，总共扫描了250万条记录。与原始查询相比，使用地理哈希和聚簇表节省了大约3倍的处理资源。然而，任何东西都不是免费的。应用地理哈希增加了数据预处理和检索的复杂性，例如需要提前选择的精度级别和额外的SQL查询逻辑。

# 结论

在本文中，我们已经看到地理空间索引如何帮助改善地理空间数据的处理。然而，它有一定的成本，需要提前仔细考虑。归根结底，这不是免费的午餐。要使其正常工作，需要对算法和系统要求有良好的理解。

*最初发布于* [*https://thanakornp.com*](https://thanakornp.com/geospatial-index-in-practice)*。*
