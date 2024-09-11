# PyrOSM：处理开放街图数据

> 原文：[https://towardsdatascience.com/pyrosm-working-with-open-street-map-data-e3ac80922044?source=collection_archive---------4-----------------------#2023-10-21](https://towardsdatascience.com/pyrosm-working-with-open-street-map-data-e3ac80922044?source=collection_archive---------4-----------------------#2023-10-21)

## 高效的地理空间操作用于 OSM 地图数据

[](https://deabardhoshi.medium.com/?source=post_page-----e3ac80922044--------------------------------)[![Dea Bardhoshi](../Images/14ce0986fc2a4a192797a52ed9908d1e.png)](https://deabardhoshi.medium.com/?source=post_page-----e3ac80922044--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e3ac80922044--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e3ac80922044--------------------------------) [Dea Bardhoshi](https://deabardhoshi.medium.com/?source=post_page-----e3ac80922044--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd61c58ba988e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpyrosm-working-with-open-street-map-data-e3ac80922044&user=Dea+Bardhoshi&userId=d61c58ba988e&source=post_page-d61c58ba988e----e3ac80922044---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e3ac80922044--------------------------------) · 4分钟阅读 · 2023年10月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe3ac80922044&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpyrosm-working-with-open-street-map-data-e3ac80922044&user=Dea+Bardhoshi&userId=d61c58ba988e&source=-----e3ac80922044---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe3ac80922044&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpyrosm-working-with-open-street-map-data-e3ac80922044&source=-----e3ac80922044---------------------bookmark_footer-----------)![](../Images/8a3a66110df17612b904c2434650013f.png)

图片由 [Tabea Schimpf](https://unsplash.com/@tabeaschimpf?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果你以前处理过 OSM 数据，你知道提取起来并不容易。OSM 数据可能非常庞大，找到有效的解决方案来分析你想要的内容往往是一项挑战。PyrOSM 是一个使读取和处理 OSM 数据的过程更加高效的包。怎么做到的？PyrOSM 基于 Cython（C Python）构建，并使用更快的库来反序列化 OSM 数据，同时还有像 numpy 数组这样的较小优化，使其能够快速处理数据。特别是如果你之前使用过 OSMnx（用于非常类似的用例），你会知道大型数据集加载到内存中需要很长时间，这就是 PyrOSM 可以帮助你处理它们的地方。让我们了解一下这个库能做什么吧！

## 🌎 PBF 数据

让我们谈谈 OSM 数据的具体文件格式。PBF 代表“Protocolbuffer Binary Format”，它在处理存储 OSM 数据时非常高效。OSM 数据组织成**fileblocks**，这些是可以独立编码或解码的数据组。Fileblocks 包含**PrimitiveGroups**，它们又包含成千上万的 OSM 实体，如节点、道路和关系。

数据可以根据用户期望的粒度级别进行缩放。例如，当前 OSM 数据库的分辨率约为 ~1 cm。实际上，如果你愿意，你可以将整个 Open Street Maps 数据下载到一个文件中，这个文件被称为 Planet（约 1000 Gb 的数据）！

## 👩‍💻 PyrOSM 基础：读取数据集

PyrOSM 是一个读取 Open Street Map 的 PBF 数据的包，基于两个主要的数据分发商：Geofabrik（全球和国家级数据）和 BBBike（城市级数据）。该包允许用户访问许多类型的功能：

+   建筑物、兴趣点（POIs）、土地使用

+   街道网络

+   自定义过滤器

+   作为网络导出

+   还有更多！

目前 BBBike 支持全球 235 个城市，你可以通过调用**“sources.cities.available”**方法轻松获取完整列表。入门非常简单，你只需初始化一个 OSM 读取器对象并加载所需的数据：

从这一点开始，你需要使用 OSM 对象来与 Berkeley 数据交互。现在让我们获取 Berkeley 的驾驶街道网络：

![](../Images/080e775dbd2e52d1353ef015180ad933.png)

Berkeley 的 OSM 街道网络数据框

打印出实际的 street_network 对象显示它存储在一个 GeoPandas GeoDataFrame 中，包含所有 OSM 属性，如长度、高速公路、最高速度等，这对于进一步分析非常有用。

**附注：** BBBike（数据来源提供商）有许多其他大小的数据格式，包括 Organic Maps OSM、Garmin OSM 或 SVG Mapnik，这取决于你的使用案例。

## 🔍 更好的过滤

上述数据加载的结果包括了整个 Berkeley 的数据，实际上甚至包括了邻近城市的数据，这并不是理想的。那如果你想要一个更小或更具体的区域呢？这就是使用边界框的用武之地。你可以通过以下方式创建边界框：

+   手动指定一个格式为 **[minx, miny, maxx, maxy]** 的 4 个坐标列表

+   传入 Shapely 几何对象（例如 LineString 或 Multipolygon）

要查找边界框坐标，我通常使用这个 [bbox finder](http://bboxfinder.com/#0.000000,0.000000,0.000000,0.000000) 网站，它允许你绘制矩形然后复制坐标。以下是如何界定 UC Berkeley 校园周围的区域并获取其步行网络：

![](../Images/94c7482901919addf2468d153ea4af40.png)

使用边界框的街道网络

## 🎯 导出和处理图形

PyrOSM 的另一个优点是它允许网络处理并与其他网络分析库连接。除了将街道网络保存为地理数据框外，PyrOSM 还允许你通过将节点和边存储在两个单独的数据框中来提取它们。这里是节点数据框：

![](../Images/818743f62ad37e355a502bc406ae2009.png)

从街道网络中提取的节点数据框

如果你有这些图形表示，保存为各种格式是非常简单的：OSMnx、igraph 和 Pandana 并在这些工具中使用它们。

## 💭 结束思考

这是 pyrosm 在你的地理空间工作中能为你提供的简短总结！我提到了几种非常有用的方法，比如从某个区域下载特定的数据集，或通过界定感兴趣区域以及它与其他库的关系。我认为 pyrosm 最棒的地方就是它能够弥合庞大的 OSM 数据集与工程或分析问题之间的差距。

感谢阅读！
