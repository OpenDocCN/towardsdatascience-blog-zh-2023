# 使用 QGIS 进行地理空间数据分析的实用入门

> 原文：[https://towardsdatascience.com/a-practical-introduction-to-geospatial-data-analysis-using-qgis-a6f82105b30e?source=collection_archive---------5-----------------------#2023-02-27](https://towardsdatascience.com/a-practical-introduction-to-geospatial-data-analysis-using-qgis-a6f82105b30e?source=collection_archive---------5-----------------------#2023-02-27)

## 这是一个互动教程，可以在使用 QGIS 的过程中学习 GIS 关键概念

[](https://eugenia-anello.medium.com/?source=post_page-----a6f82105b30e--------------------------------)[![尤金尼亚·安奈洛](../Images/537f444252cdc60709e7a19e37734c7b.png)](https://eugenia-anello.medium.com/?source=post_page-----a6f82105b30e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a6f82105b30e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a6f82105b30e--------------------------------) [尤金尼亚·安奈洛](https://eugenia-anello.medium.com/?source=post_page-----a6f82105b30e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F86fdc517c278&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-practical-introduction-to-geospatial-data-analysis-using-qgis-a6f82105b30e&user=Eugenia+Anello&userId=86fdc517c278&source=post_page-86fdc517c278----a6f82105b30e---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a6f82105b30e--------------------------------) ·6 min read·2023年2月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa6f82105b30e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-practical-introduction-to-geospatial-data-analysis-using-qgis-a6f82105b30e&user=Eugenia+Anello&userId=86fdc517c278&source=-----a6f82105b30e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa6f82105b30e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-practical-introduction-to-geospatial-data-analysis-using-qgis-a6f82105b30e&source=-----a6f82105b30e---------------------bookmark_footer-----------)![](../Images/7e80a72add9dcb55d0fadf62b861dee1.png)

[克里斯·劳顿](https://unsplash.com/@chrislawton) 拍摄于 [Unsplash](https://unsplash.com/photos/duQ1ulzTJbM)

*这是关于地理空间数据分析系列的第一篇文章：*

1.  *使用 QGIS 进行地理空间数据分析（本文）*

1.  [*OpenStreetMap 入门指南*](/a-comprehensive-guide-for-getting-started-with-openstreetmap-e92dff95fc80?sk=e0981a4fed7f4cfefa9a58477a863ea6)

1.  [*使用 GeoPandas 进行地理空间数据分析*](/geospatial-data-analysis-with-geopandas-876cb72721cb?sk=042a0f2fb834cb08ffd0f74eb856e7e1)

1.  [*使用 OSMnx 进行地理空间数据分析*](/geospatial-data-analysis-with-osmnx-8a300d77b592?sk=7afb9be17e024167937a615d7ea4b267)

1.  [*为数据科学家提供的地理编码*](https://www.datacamp.com/tutorial/geocoding-for-data-scientists)

1.  [*使用 Geemap 进行地理空间数据分析*](https://www.kdnuggets.com/geospatial-data-analysis-with-geemap)

想学习地理空间数据分析但不知道从哪里开始？那么，这个教程适合你。在开始这段旅程时，有很多概念是被视为理所当然的，它们将帮助你操作数据集中的地理信息。

地理空间数据分析是数据科学的一个子领域，专注于一种特殊类型的数据——地理空间数据。与普通数据不同，地理空间数据的每条记录都对应一个特定的位置，并可以在地图上绘制出来。

一个特定的数据点可以通过经度和纬度来描述。但你的数据集可能包含更复杂的项，如道路、河流、边界等…
