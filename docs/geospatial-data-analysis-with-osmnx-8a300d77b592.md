# 使用 OSMnx 进行地理空间数据分析

> 原文：[https://towardsdatascience.com/geospatial-data-analysis-with-osmnx-8a300d77b592?source=collection_archive---------5-----------------------#2023-06-16](https://towardsdatascience.com/geospatial-data-analysis-with-osmnx-8a300d77b592?source=collection_archive---------5-----------------------#2023-06-16)

## 学习如何使用 Python 下载、分析和可视化 OpenStreetMap 数据

[](https://eugenia-anello.medium.com/?source=post_page-----8a300d77b592--------------------------------)[![Eugenia Anello](../Images/537f444252cdc60709e7a19e37734c7b.png)](https://eugenia-anello.medium.com/?source=post_page-----8a300d77b592--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8a300d77b592--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8a300d77b592--------------------------------) [Eugenia Anello](https://eugenia-anello.medium.com/?source=post_page-----8a300d77b592--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F86fdc517c278&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-analysis-with-osmnx-8a300d77b592&user=Eugenia+Anello&userId=86fdc517c278&source=post_page-86fdc517c278----8a300d77b592---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----8a300d77b592--------------------------------) ·6 分钟阅读·2023年6月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8a300d77b592&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-analysis-with-osmnx-8a300d77b592&user=Eugenia+Anello&userId=86fdc517c278&source=-----8a300d77b592---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8a300d77b592&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-analysis-with-osmnx-8a300d77b592&source=-----8a300d77b592---------------------bookmark_footer-----------)![](../Images/7dad29578270e9b12624d8eb686a6fd3.png)

照片由[Denys Nevozhai](https://unsplash.com/@dnevozhai)拍摄，发布在[Unsplash](https://unsplash.com/photos/7nrsVjvALnA)上

*这是关于地理空间数据分析系列的第四篇文章：*

1.  [*使用 QGIS 进行地理空间数据分析*](/a-practical-introduction-to-geospatial-data-analysis-using-qgis-a6f82105b30e?sk=e6251697a54bc62fa33bc6a9a81258a7)

1.  [*OpenStreetMap 入门指南*](/a-comprehensive-guide-for-getting-started-with-openstreetmap-e92dff95fc80?sk=e0981a4fed7f4cfefa9a58477a863ea6)

1.  [*使用 GeoPandas 进行地理空间数据分析*](/geospatial-data-analysis-with-geopandas-876cb72721cb?sk=042a0f2fb834cb08ffd0f74eb856e7e1)

1.  *使用 OSMnx 进行地理空间数据分析（这篇文章）*

1.  [*Geocoding for Data Scientists*](https://www.datacamp.com/tutorial/geocoding-for-data-scientists)

1.  [*Geospatial Data Analysis with Geemap*](https://www.kdnuggets.com/geospatial-data-analysis-with-geemap)

在之前的教程中，我介绍了地理空间数据分析的各个方面。我开始时展示了不使用任何代码的地理空间数据的实际例子，以便让你深入理解这些概念。地理空间数据分析是一个普遍的领域，旨在处理一种特殊类型的数据，即地理空间数据。

这包括将位置添加到非地理数据中。内容充满了例子。你可以考虑咖啡馆、医院、道路、河流、卫星图像等。即使你用 Google Maps 搜索一个地方，你也在与地理空间数据互动。

这次我将重点关注从 OpenStreetMap 下载、可视化和分析数据，OpenStreetMap 是最大的免费和可编辑的……
