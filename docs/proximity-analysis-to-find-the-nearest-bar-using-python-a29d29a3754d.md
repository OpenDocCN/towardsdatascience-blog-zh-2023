# 使用 Python 进行邻近分析以找到最近的酒吧

> 原文：[https://towardsdatascience.com/proximity-analysis-to-find-the-nearest-bar-using-python-a29d29a3754d?source=collection_archive---------2-----------------------#2023-10-26](https://towardsdatascience.com/proximity-analysis-to-find-the-nearest-bar-using-python-a29d29a3754d?source=collection_archive---------2-----------------------#2023-10-26)

## **关于空间数据处理的几点说明**

[](https://medium.com/@mik.sarafanov?source=post_page-----a29d29a3754d--------------------------------)[![Mikhail Sarafanov](../Images/88869a3c6f664785c90539dd7aab6d74.png)](https://medium.com/@mik.sarafanov?source=post_page-----a29d29a3754d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a29d29a3754d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a29d29a3754d--------------------------------) [Mikhail Sarafanov](https://medium.com/@mik.sarafanov?source=post_page-----a29d29a3754d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F209c78c40898&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fproximity-analysis-to-find-the-nearest-bar-using-python-a29d29a3754d&user=Mikhail+Sarafanov&userId=209c78c40898&source=post_page-209c78c40898----a29d29a3754d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a29d29a3754d--------------------------------) ·10分钟阅读·2023年10月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa29d29a3754d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fproximity-analysis-to-find-the-nearest-bar-using-python-a29d29a3754d&user=Mikhail+Sarafanov&userId=209c78c40898&source=-----a29d29a3754d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa29d29a3754d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fproximity-analysis-to-find-the-nearest-bar-using-python-a29d29a3754d&source=-----a29d29a3754d---------------------bookmark_footer-----------)![](../Images/41128cdb9b2221c5fd5299b99c687008.png)

预览图片（作者提供）

> 免责声明：在本文中，我们将使用开源库[estaty](https://github.com/wiredhut/estaty)演示所有方法。这个库的出现是因为我们希望将工作中使用的算法形式化为一个工具，供其他开发者使用。换句话说，本文由该库的维护者撰写。

今天我们将继续讨论使用开源Python库进行空间数据处理的话题。我们已经讨论过如何[结合Open Street Map和Landsat开放数据来验证房地产对象附近的绿地面积](/combining-open-street-map-and-landsat-open-data-to-verify-areas-of-green-zones-b1956e561321)。

现在让我们考虑另一种类型的分析：邻近性分析，或一些有用对象的可用性（或可达性），如公园、医院、幼儿园等（图1）。

![](../Images/ae719b69fbe6895a8c1cedfb97528dd7.png)

图1\. 说明邻近性分析方法的示意图（假设这里讨论的是理发店等） （图片作者提供）

## **关于邻近性分析的简要说明**

我们建议从简要的文献综述开始这个故事。我喜欢文章“[每个数据科学家都应该了解的地点分析用例](/location-analytics-use-cases-that-every-data-scientist-should-know-740b708a2504)”（注意——这是*仅限会员的故事*），它非常容易让读者了解地理信息分析是什么。

文章“[创建公园可达性成本距离表面](https://www.azavea.com/blog/2014/09/04/creating-a-cost-distance-surface-to-measure-park-access/)”和“[犹他州的增长状态：对犹他州居民与医疗设施邻近性分析](https://blog.lib.utah.edu/the-growing-state-of-utah-a-proximity-analysis-of-utah-residents-to-medical-care-facilities/)”也非常清晰地展示了工程师在城市中可以计算的特征，以进行此类分析。这个话题相当受欢迎，已经开发了大量的科学论文和实际应用——让我们也来参与吧。

## **我们使用的库**

现在让我们转到一个更具体的讨论：用于这种邻近性分析的工具（除了地理信息系统如QGIS或ArcGIS和作为最终用户的专有服务之外）。

作为开发人员，我们希望利用方便的库或服务（当然最好是开源的）分析原始数据而非汇总数据。我们没有找到适合我们的工具，因此开发并使用了我们自己的用Python编写的开源库。它叫做**estaty**，如果你愿意，可以在阅读本文后查看文档页面：

+   [**https://estaty.readthedocs.io/en/latest/?badge=latest**](https://estaty.readthedocs.io/en/latest/?badge=latest)

基于这个库，我们编写了使用案例。基于这些示例，我们开发了帮助房地产经理进行分析的微服务。除了现成的服务，这些脚本还组织成使用案例，任何想要使用该库的人都可以使用它们。

**estaty** 可以使用不同的数据源进行计算，但开放的示例（为了保持其真正开放）是基于 OpenStreetMap 数据的。在当前论文中，我们想展示如何进行相当快速的附带估算，并在需要时将该方法扩展到新的数据源。

## **分析（我们可以做的）**

首先，使用命令安装库

```py
pip install estaty==0.1.0
```

或者，如果您使用 poetry：

```py
poetry add estaty==0.1.0
```

我们现在准备使用版本 0.1.0

让我们开始实现。我们将对以下地址进行邻近分析：“Berlin, Neustädtische Kirchstraße 4–7” — 坐标 {latitude: 52.5171411, longitude: 13.3857187}。我们将在半径 2 公里的范围内分析该对象的邻近区域。

为了找出我们到达最近酒吧需要多长时间（根据 OSM — 值得一提），我们来编写并运行[以下代码](https://github.com/wiredhut/estaty_examples/blob/main/release_01/proximity_tds/path_to_parks.py)：

这里发生了以下情况——我们构建了一个用于分析属性的处理管道。这个管道将呈现为一个由数据上的连续变换组成的图形：

![](../Images/987db08a3aef0f05cfd11c82cf9d54da.png)

图 2\. 使用 OpenStreetMap 数据计算路线到酒吧的距离的空间数据处理管道（图片由作者提供）

让我们更仔细地看看“引擎盖下”的内容。我们从 estaty 架构的 5 个抽象层开始（在上面的管道中，仅使用了三种类型的节点）：

+   **数据源** — 从所需源加载数据，将数据标准化（矢量和栅格）。无论源是什么，这个节点的输出将只有两种可能的变体，从而使其进一步处理得以统一；

+   **预处理器** — 对矢量或栅格数据进行预处理，例如分配新的坐标参考系统（CRS）；

+   **合并** — 以所需方式组合数据。可能的组合方式：矢量数据与矢量数据，栅格数据与栅格数据，矢量数据与栅格数据；

+   **分析器** — 库的核心 — 使用对栅格和矢量对象的简单原子变换来执行某些分析，例如面积匹配或路径查找；

+   **报告** — 可选的最终分析模块，允许以用户友好的格式生成报告。

通过以特定方式组合这些节点，可以在不修改原始数据处理方式的情况下，为数据分析构建不同的管道——仅通过替换原始数据源或分析方式（参见动画 1）。

![](../Images/eceb41cd22d0a33ea3e80629adbcacdb.png)

动画 1\. 库在准备分析管道时的灵活性（动画由作者提供）

因此，图 2 显示在分析中仅使用了三个节点——这是一个相当简单的管道。其工作结果将得到一个 geopandas GeoDataFrame（一个包含对象几何的表格），其中包含线性对象——到我们感兴趣的对象的路线。在这种情况下——到酒吧。结果的可视化将如下所示：

![](../Images/a9c310bbd45eb46e2fe034aaa0436a79.png)

图 3\. 路线到酒吧的距离计算（作者提供的图像）

我们有权对获得的线性对象集进行任何操作——例如，找到到最近酒吧的距离，或从样本中请求平均值（在代码中完成）：

+   最小长度：308.93 米

+   平均长度：2306.49 米

作为大城市酒吧位置的真正专家，我可以说，在分析区域内，酒吧实际上可能更多。但我们在一开始就同意基于 OSM 数据进行分析，所以……

管道本身与进行分析的数据源无关。这是通过库模块之间的弱关联性来确保的。让我们从 DataSource 节点开始——它具有将数据转换为一种类型的内置机制（例如，这里的矢量数据将以 geopandas GeoDataFrame 格式存在，并且无论我们使用了什么数据源，所有数据都将转换为点、线或多边形）。接下来是一个预处理器，它将自动确定适合我们数据的度量投影。预处理器唯一关心的是数据必须来自节点的 DataSource，其余的应由之前的节点处理。我们的简单管道由一个分析块完成，它可以操作任何几何对象，无论是区域、线性还是点。

因此，我们可以对从 OSM 提取的其他类别的数据进行完全相同的计算（当然，这个列表不仅限于 OSM），例如，对学校、公园、水体、公共厕所、垃圾桶、警察局等进行邻近分析——任何（对于可以通过标签定义的对象类型，请参见 [地图特征](https://wiki.openstreetmap.org/wiki/Map_features) 或 [关于数据源的文档](https://estaty.readthedocs.io/en/latest/documentation/data_source/)）：

![](../Images/457410efe02b4595444aac98106f6f9d.png)

图 4\. 对学校、公园、水体、垃圾桶、警察局的邻近分析（作者提供的图像）

## **如何扩展这个方法**

我们提到过，任何性质的空间数据都可以进行分析：社会数据、自然对象或人工建筑。让我们更深入地查看源代码。从图中可以看出，距离计算是在考虑道路的路线中进行的，这是合理的，因为通常我们不是乘坐直升机去酒吧，而是步行。因此，“距离”分析包括在道路图上搜索路线，使用的是来自OSM的图。因此，算法在以下抽象上搜索路线（我们依赖于以下文章，强烈推荐——[OSMnx: Python for Street Networks](https://geoffboeing.com/2016/11/osmnx-python-street-networks/)）：

![](../Images/d87c87e46592715c3e95c1c8a5c2eed3.png)

图5\. 步行道路图，用于搜索最佳路线（图片作者提供）

图是地理参考的（具有纬度和经度属性）节点和边。为了找到从对象A到对象B的最短路径，我们需要执行三个步骤：

1.  找到图中离对象A最近的节点（我们称之为节点1）

1.  找到图中离对象B最近的节点（我们称之为节点2）

1.  使用图遍历算法，找到从节点1到节点2的最佳路线

如果我们想要获得距离，需要将从节点1到节点2的路径长度与从对象A到节点1以及从对象B到节点2的距离相加。最后两个分量可以通过直线计算，因为节点网络相当密集。但这里有一个难点——如果对象A和B是点，相对容易找到它们最近的节点（计算点之间的距离）。如果对象A的几何形状例如是一个区域，那么我们必须寻找与多边形“最近”的节点。线性对象的情况也是一个需要解决的问题。

那么，让我们回答这个问题：“如何统一计算图节点到不同类型矢量对象的距离：线、点和多边形”？为此，库在开始路线计算之前将对象转换为点类型（计算点之间的距离非常简单）。我们将这种转换称为表示（见图6）。

![](../Images/c606a11844f867e1cbaad26b27a6df49.png)

图6\. 表示矢量对象的几种方法（图片作者提供）

也就是说， instead of 编写和维护三个算法，我们编写一个（搜索到点的路径），然后将所有初始数据类型简化为一个。如何将它们简化为一个类型是一个模糊的问题。我们可以简单地通过计算质心来做到这一点。然而，在城市地区尤其是对于大型对象（如公园）的计算中，为质心寻找路线可能是不必要的——我们认为，当我们从铺砌的道路进入宜人的泥土公园黑暗中时，我们才真正进入公园，而不是当我们站在公园中心时。

因此，我们决定使用一种方法，在这种方法中，我们找到多边形或线性对象到分析对象（从中建立路径）的最近点。然后，这个点被视为最终的目标节点——到这个节点的进一步路径将在图中构建。

## **更高级的分析**

仅凭前往酒吧的路径是无法令人印象深刻的。因此，让我们把任务复杂化。假设我们想通过结合来自不同来源的数据来进行更复杂的分析。例如，我们想计算在去橡树公园的平均步行距离（我们喜欢在酒吧之后散步，欣赏公园里的树木）。为此，我们将结合来自两个来源的数据：

+   从 GBIF | 全球生物多样性信息设施下载矢量数据（查看 [https://www.gbif.org/](https://www.gbif.org/)，截至2023年10月20日）

+   OpenStreetMap — 库中的数据下载本地集成

GBIF 数据是一个点几何的矢量层，显示了发现某些植物和动物（以及其他东西）的位置。在这种情况下，我们将关注橡树（或拉丁文中的 Quercus robur）。当我们将橡树的点层叠加到公园多边形上时，我们的数据将如下所示：

![](../Images/c9b0324952621ab326e045f72c9bfe48.png)

图7\. 结合 OSM 公园数据和 GBIF 数据计算前往橡树林公园的路径（图像由作者提供）

用于分析的代码 [长这样](https://github.com/wiredhut/estaty_examples/blob/main/release_01/proximity_tds/path_to_parks_with_quercus.py)：

需要注意的一点是，点不必完全落在公园的多边形内。我们结合来自不同来源的数据，它们可能会略有不同。因此，我们设置了 10 米的缓冲区，并查看结果（图8）——如果公园多边形与缓冲区多边形有交集，则我们将认为公园中有橡树：

![](../Images/7d12aec560528017faa0882d63f18479.png)

图8\. 前往橡树出现的公园的路径，计算出的距离（图像由作者提供）

## **关于相关性的更多信息**

分析主题需求明显。例如，许多源代码解决方案中都提供了路由功能（在准备这篇文章时，我发现了一个有趣的笔记本“[探索用于计算一组起点-终点对路径的路由工具](https://github.com/rajesvariparasa/spatial-routing-libraries-and-services/blob/main/Routing_Libraries_Services.ipynb)”），以及像 Google Maps 这样的流行用户服务和隐含的地图相关服务。如果谈到邻近分析，这里也有很多实现的解决方案。例如，我们可以提到一个工具，如 [pandana](https://udst.github.io/pandana/introduction.html)。

所有这些工具无疑很有用，但在应用示例中使用它们需要编写一些辅助代码。我们正在尝试为自己的团队制作一个方便的工具，并希望与社区分享我们的发现。在本文介绍的方法中，我们可以提到数据源无关的方法和灵活的数据组合方式。我们欢迎评论和建议。

有用的链接：

+   [https://github.com/wiredhut/estaty](https://github.com/wiredhut/estaty) — 用于处理空间数据和准备物业分析原型算法的 Python 开源库。该库较新，但我们计划进行开发和改进；

+   [https://estaty.readthedocs.io/en/latest/](https://estaty.readthedocs.io/en/latest/) — 文档页面；

+   [https://github.com/wiredhut/estaty_examples](https://github.com/wiredhut/estaty_examples) — 本文示例启动的开源存储库

本文使用的数据集（及许可证）：

1.  地图数据版权归 OpenStreetMap 贡献者所有，可从 [https://www.openstreetmap.org](https://www.openstreetmap.org) 获取

    许可证 — [开放数据公共开放数据库许可证](https://opendatacommons.org/licenses/odbl/) 链接更新至2023年10月25日

1.  GBIF.org (2023年10月20日) GBIF 发生数据下载 [https://doi.org/10.15468/dl.f487j5](https://doi.org/10.15468/dl.f487j5)

    许可证 — [署名-非商业性使用 4.0 国际](https://creativecommons.org/licenses/by-nc/4.0/legalcode) 链接更新至2023年10月25日

使用 OpenStreetMap 数据及更多内容的邻近分析故事由 [Mikhail Sarafanov](https://github.com/Dreamlone) 和 [Wiredhut 团队](https://wiredhut.com/) 提供
