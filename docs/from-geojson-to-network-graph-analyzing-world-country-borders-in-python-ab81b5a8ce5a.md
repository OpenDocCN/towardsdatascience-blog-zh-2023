# 从 GeoJSON 到网络图：在 Python 中分析世界国家边界

> 原文：[`towardsdatascience.com/from-geojson-to-network-graph-analyzing-world-country-borders-in-python-ab81b5a8ce5a`](https://towardsdatascience.com/from-geojson-to-network-graph-analyzing-world-country-borders-in-python-ab81b5a8ce5a)

## 利用 NetworkX 进行基于图的国家边界分析

[](https://amandaiglesiasmoreno.medium.com/?source=post_page-----ab81b5a8ce5a--------------------------------)![Amanda Iglesias Moreno](https://amandaiglesiasmoreno.medium.com/?source=post_page-----ab81b5a8ce5a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ab81b5a8ce5a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab81b5a8ce5a--------------------------------) [Amanda Iglesias Moreno](https://amandaiglesiasmoreno.medium.com/?source=post_page-----ab81b5a8ce5a--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab81b5a8ce5a--------------------------------) ·阅读时间 7 分钟·2023 年 10 月 15 日

--

![](img/77bc3f84ad2518f2a0543607dcb80e36.png)

[Maksim Shutov](https://unsplash.com/es/@maksimshutov) 在 Unsplash

Python 提供了广泛的库，使我们能够轻松快捷地解决各种研究领域的问题。**地理空间数据分析和图论是 Python 提供强大实用库的两个研究领域**。在这篇文章中，**我们将对世界边界进行简单的分析**，特别是探索哪些国家彼此接壤。我们将首先利用包含所有国家多边形的 GeoJSON 文件的信息。最终目标是创建一个使用 NetworkX 表示各种边界的图，并利用该图进行多种分析。

# GeoJSON 数据导入：读取和加载全球国家数据

**GeoJSON 文件能够表示各种地理区域，并广泛应用于地理分析和可视化**。我们分析的初始阶段涉及读取 `countries.geojson` 文件，并使用 `GeoPandas` 将其转换为 `GeoDataFrame`。该文件来源于以下 GitHub 仓库，包含了代表世界各国的多边形。

[](https://github.com/datasets/geo-countries/tree/master?source=post_page-----ab81b5a8ce5a--------------------------------) [## GitHub - datasets/geo-countries: 作为 GeoJSON 的国家多边形数据包

### 作为 GeoJSON 的国家多边形数据包。通过创建帐户来为 datasets/geo-countries 的开发做出贡献…

github.com](https://github.com/datasets/geo-countries/tree/master?source=post_page-----ab81b5a8ce5a--------------------------------) ![](img/ead5062faa333fd24721c725d9bd8f0d.png)

包含全面国家信息的 GeoDataFrame（图片由作者创建）

如上所示，`GeoDataFrame`包含以下列：

1.  `ADMIN`：表示地理区域的行政名称，如国家或地区名称。

1.  `ISO_A3`：代表 ISO 3166–1 alpha-3 国家代码，一个唯一识别国家的三字母代码。

1.  `ISO_A2`：表示 ISO 3166–1 alpha-2 国家代码，一个用于国家识别的两字母代码。

1.  `geometry`：此列包含定义地理区域形状的几何信息，表示为`MULTIPOLYGON`数据。

你可以使用`plot`方法可视化构成`GeoDataFrame`的所有多边形，如下所示。

![](img/f0564c8fa7797be724b2d76f811b1bed.png)

GeoDataFrame 的可视化表示（图片由作者创建）

# 计算多边形坐标：纬度和经度

`geometry`列中的多边形属于`shapely.geometry.multipolygon.MultiPolygon`类。这些对象包含各种属性，其中之一是`centroid`属性。`centroid`属性提供`MULTIPOLYGON`的几何中心，并返回一个表示该中心的`POINT`。

随后，我们可以使用此`POINT`提取每个`MULTIPOLYGON`的纬度和经度，并将结果存储在`GeoDataFrame`中的两列中。我们进行此计算，因为稍后我们将使用这些纬度和经度值根据其实际地理位置在图中可视化节点。

# 创建国家边界网络图

现在是时候继续**构建一个表示全球不同国家之间边界的图**。在这个图中，**节点将代表国家**，而**边将表示这些国家之间边界的存在**。如果两个节点之间存在边界，图中将有一条连接它们的边；否则，将没有边。

函数`create_country_network`处理`GeoDataFrame`中的信息，并构建一个表示国家边界的`Graph`。

最初，函数遍历`GeoDataFrame`的每一行，每行对应一个国家。然后，为国家创建一个节点，并将纬度和经度作为属性添加到节点中。

如果几何体无效，它将使用`buffer(0)`方法进行修正。此方法通过应用一个距离为零的小缓冲区操作来修复无效几何体。此操作解决了多边形表示中的自交或其他几何不规则性问题。

在创建节点之后，下一步是为网络添加相关的边。为此，我们遍历不同的国家，如果表示两个国家的多边形之间有交集，则意味着它们共享一个共同的边界，因此在它们的节点之间创建一条边。

# **可视化构建的国家边界网络**

下一步是可视化创建的网络，其中节点代表全球的国家，而边表示它们之间存在边界。

函数 `plot_country_network_on_map` 负责处理图 `G` 的节点和边，并在地图上显示它们。

![](img/29fb71e836616248341f13088b856b92.png)

国家边界网络（图像由作者创建）

**图中节点的位置是由国家的经纬度坐标确定的**。此外，背景中放置了一张地图，以提供更清晰的创建网络的背景。这张地图是使用 `GeoDataFrame` 的 `boundary` 属性生成的。这个属性提供了表示国家几何边界的信息，帮助创建背景地图。

需要注意一个细节：在使用的 GeoJSON 文件中，有些岛屿被视为独立国家，即使它们在行政上属于某个国家。这就是为什么你可能会看到海域中有很多点。请记住，创建的图依赖于生成它的 GeoJSON 文件中的信息。如果我们使用不同的文件，生成的图将会不同。

# **探索见解：利用国家边界网络回答问题**

**我们创建的国家边界网络可以迅速帮助我们解决多个问题**。下面，我们将概述通过处理网络提供的信息可以轻松得出的三种见解。然而，这个网络可以帮助我们解答的其他问题还有很多。

## 见解 1：检查选定国家的边界

在这一部分，**我们将直观评估特定国家的邻国**。

`plot_country_borders` 函数可以快速可视化特定国家的边界。这个函数生成一个包含所提供输入国家及其邻国的子图。接着，它会可视化这些国家，使得观察特定国家的邻国变得非常容易。在这个实例中，选择的国家是墨西哥，但我们可以很容易地调整输入以可视化其他国家。

![](img/976bdf9eeb114631b0d7d9a510e55eca.png)

墨西哥的国家边界网络（图像由作者创建）

从生成的图像中可以看出，墨西哥与三个国家接壤：美国、伯利兹和危地马拉。

## 见解 2：边界最多的前 10 个国家

在本节中，**我们将分析哪些国家拥有最多的邻国**并将结果显示在屏幕上。为此，我们实现了 `calculate_top_border_countries` 函数。该函数评估网络中每个节点的邻国数量，并仅显示邻国最多的节点（前 10 名）。

![](img/1571df8e76e853cc4ae48816c722a927.png)

拥有最多边界的前 10 个国家（图像由作者创建）

我们必须重申，获得的结果依赖于初始的 GeoJSON 文件。在这种情况下，Siachen 冰川被编码为一个独立的国家，这就是为什么它被显示为与中国共享边界。

## 见解 3：探索最短的国家间路线

我们通过路线评估结束我们的分析。在这种情况下，**我们将评估从起始国到目的国旅行时必须穿越的最小边界数量**。

`find_shortest_path_between_countries` 函数计算从起始国到目的国的最短路径。然而，需要注意的是，这个函数仅提供可能的最短路径之一。这一限制源于它使用了 `NetworkX` 的 `shortest_path` 函数，由于算法的特性，它固有地找到单一的最短路径。

要访问两个点之间的所有可能路径，包括多个最短路径，可以选择其他选项。在 `find_shortest_path_between_countries` 函数的上下文中，可以探索诸如 `all_shortest_paths` 或 `all_simple_paths` 的选项。这些替代方案能够返回多个最短路径，而不仅仅是一个，具体取决于分析的要求。

我们使用该函数来查找西班牙和波兰之间的最短路径，分析结果显示，从西班牙到波兰旅行所需的最小边界穿越数量为 3。

![](img/3da8b3bf6a901f58dc330c475fbc0835.png)

从西班牙到波兰的最佳路线（图像由作者创建）

# **总结**

Python 提供了大量跨领域的库，这些库可以无缝集成到任何数据科学项目中。在这个实例中，我们利用了专门用于几何数据分析和图形分析的库来创建一个表示世界边界的图形。随后，我们展示了如何利用这个图形快速回答问题，使得地理分析变得轻松自如。

感谢阅读。

Amanda Iglesias
