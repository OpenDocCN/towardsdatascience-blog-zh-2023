# 使用 OSMnx 进行地理空间数据分析

> 原文：[`towardsdatascience.com/geospatial-data-analysis-with-osmnx-8a300d77b592?source=collection_archive---------5-----------------------#2023-06-16`](https://towardsdatascience.com/geospatial-data-analysis-with-osmnx-8a300d77b592?source=collection_archive---------5-----------------------#2023-06-16)

## 学习如何使用 Python 下载、分析和可视化 OpenStreetMap 数据

[](https://eugenia-anello.medium.com/?source=post_page-----8a300d77b592--------------------------------)![Eugenia Anello](https://eugenia-anello.medium.com/?source=post_page-----8a300d77b592--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8a300d77b592--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8a300d77b592--------------------------------) [Eugenia Anello](https://eugenia-anello.medium.com/?source=post_page-----8a300d77b592--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F86fdc517c278&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-analysis-with-osmnx-8a300d77b592&user=Eugenia+Anello&userId=86fdc517c278&source=post_page-86fdc517c278----8a300d77b592---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----8a300d77b592--------------------------------) ·6 分钟阅读·2023 年 6 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8a300d77b592&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-analysis-with-osmnx-8a300d77b592&user=Eugenia+Anello&userId=86fdc517c278&source=-----8a300d77b592---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8a300d77b592&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-analysis-with-osmnx-8a300d77b592&source=-----8a300d77b592---------------------bookmark_footer-----------)![](img/7dad29578270e9b12624d8eb686a6fd3.png)

照片由[Denys Nevozhai](https://unsplash.com/@dnevozhai)拍摄，发布在[Unsplash](https://unsplash.com/photos/7nrsVjvALnA)上

*这是关于地理空间数据分析系列的第四篇文章：*

1.  *使用 QGIS 进行地理空间数据分析*

1.  *OpenStreetMap 入门指南*

1.  *使用 GeoPandas 进行地理空间数据分析*

1.  *使用 OSMnx 进行地理空间数据分析（这篇文章）*

1.  [*Geocoding for Data Scientists*](https://www.datacamp.com/tutorial/geocoding-for-data-scientists)

1.  [*Geospatial Data Analysis with Geemap*](https://www.kdnuggets.com/geospatial-data-analysis-with-geemap)

在之前的教程中，我介绍了地理空间数据分析的各个方面。我开始时展示了不使用任何代码的地理空间数据的实际例子，以便让你深入理解这些概念。地理空间数据分析是一个普遍的领域，旨在处理一种特殊类型的数据，即地理空间数据。

这包括将位置添加到非地理数据中。内容充满了例子。你可以考虑咖啡馆、医院、道路、河流、卫星图像等。即使你用 Google Maps 搜索一个地方，你也在与地理空间数据互动。

这次我将重点关注从 OpenStreetMap 下载、可视化和分析数据，OpenStreetMap 是最大的免费和可编辑的……
