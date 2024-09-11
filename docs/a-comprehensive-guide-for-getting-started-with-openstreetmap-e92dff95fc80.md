# OpenStreetMap 入门的全面指南

> 原文：[`towardsdatascience.com/a-comprehensive-guide-for-getting-started-with-openstreetmap-e92dff95fc80?source=collection_archive---------4-----------------------#2023-04-24`](https://towardsdatascience.com/a-comprehensive-guide-for-getting-started-with-openstreetmap-e92dff95fc80?source=collection_archive---------4-----------------------#2023-04-24)

## 在使用网站的同时学习 OpenStreetMap 的基础概念

[](https://eugenia-anello.medium.com/?source=post_page-----e92dff95fc80--------------------------------)![Eugenia Anello](https://eugenia-anello.medium.com/?source=post_page-----e92dff95fc80--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e92dff95fc80--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e92dff95fc80--------------------------------) [Eugenia Anello](https://eugenia-anello.medium.com/?source=post_page-----e92dff95fc80--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F86fdc517c278&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-guide-for-getting-started-with-openstreetmap-e92dff95fc80&user=Eugenia+Anello&userId=86fdc517c278&source=post_page-86fdc517c278----e92dff95fc80---------------------post_header-----------) 在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e92dff95fc80--------------------------------) 上发布 ·6 分钟阅读·2023 年 4 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe92dff95fc80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-guide-for-getting-started-with-openstreetmap-e92dff95fc80&user=Eugenia+Anello&userId=86fdc517c278&source=-----e92dff95fc80---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe92dff95fc80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-guide-for-getting-started-with-openstreetmap-e92dff95fc80&source=-----e92dff95fc80---------------------bookmark_footer-----------)![](img/0f1e2b8234602d8f73a50b3568c5c633.png)

照片由 [Unsplash+](https://unsplash.com/plus?referrer=%2Fphotos%2Fw98knetr8EA) 提供，来源于 [Unsplash](https://unsplash.com/photos/w98knetr8EA)

*这是关于地理空间数据分析系列的第二篇文章：*

1.  *使用 QGIS 进行地理空间数据分析*

1.  *OpenStreetMap 入门指南（本文）*

1.  *使用 GeoPandas 进行地理空间数据分析*

1.  *使用 OSMnx 进行地理空间数据分析*

1.  [*数据科学家的地理编码*](https://www.datacamp.com/tutorial/geocoding-for-data-scientists)

1.  [*使用 Geemap 进行地理空间数据分析*](https://www.kdnuggets.com/geospatial-data-analysis-with-geemap)

本文与故事 地理空间数据分析的实用介绍 相关联。在上一篇文章中，我介绍了地理空间数据分析的神奇世界，这是数据科学的一个子领域，涉及对一种特殊类型的数据——地理空间数据的操作和推断。

与普通数据不同，每行地理空间数据对应一个特定位置，并且可以绘制在地图上。最简单的情况是位置仅通过纬度和经度描述的具体数据点，但也可能有更复杂的特征，如道路、河流、国家边界和地形，其中一对坐标不足以…
