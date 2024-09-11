# 使用 GeoPandas 进行地理空间数据分析

> 原文：[`towardsdatascience.com/geospatial-data-analysis-with-geopandas-876cb72721cb?source=collection_archive---------4-----------------------#2023-05-06`](https://towardsdatascience.com/geospatial-data-analysis-with-geopandas-876cb72721cb?source=collection_archive---------4-----------------------#2023-05-06)

## 学习如何使用 Python 的 GeoPandas 操作和可视化矢量数据

[](https://eugenia-anello.medium.com/?source=post_page-----876cb72721cb--------------------------------)![Eugenia Anello](https://eugenia-anello.medium.com/?source=post_page-----876cb72721cb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----876cb72721cb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----876cb72721cb--------------------------------) [Eugenia Anello](https://eugenia-anello.medium.com/?source=post_page-----876cb72721cb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F86fdc517c278&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-analysis-with-geopandas-876cb72721cb&user=Eugenia+Anello&userId=86fdc517c278&source=post_page-86fdc517c278----876cb72721cb---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----876cb72721cb--------------------------------) ·6 分钟阅读·2023 年 5 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F876cb72721cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-analysis-with-geopandas-876cb72721cb&user=Eugenia+Anello&userId=86fdc517c278&source=-----876cb72721cb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F876cb72721cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-analysis-with-geopandas-876cb72721cb&source=-----876cb72721cb---------------------bookmark_footer-----------)![](img/3c457c4199b362e1f2907d84b08b6323.png)

图片来源于 [Artem Beliaikin](https://unsplash.com/@belart84) 在 [Unsplash](https://unsplash.com/photos/v6kii3H5CcU)

*这是关于地理空间数据分析系列的第三篇文章：*

1.  *使用 QGIS 进行地理空间数据分析*

1.  *OpenStreetMap 入门指南*

1.  *使用 GeoPandas 进行地理空间数据分析（本文）*

1.  *使用 OSMnx 的地理空间数据分析*

1.  [*数据科学家的地理编码*](https://www.datacamp.com/tutorial/geocoding-for-data-scientists)

1.  [*使用 Geemap 进行地理空间数据分析*](https://www.kdnuggets.com/geospatial-data-analysis-with-geemap)

本文是《使用 QGIS 的地理空间数据分析实用入门》和《OpenStreetMap 入门全面指南》的续篇。在之前的教程中，我概述了地理空间数据分析，这是一个无处不在的子领域，可以应用于许多领域，如物流、交通和保险。

这一学科专注于分析一种特殊类型的数据，即地理空间数据，其特点是具有位置，由一个或多个坐标对描述。例如，餐馆、道路和国家之间的边界。要展示一个连续的表面，比如卫星图像，地理表格已经不够用了，你需要一个包含一个或多个的数组……
