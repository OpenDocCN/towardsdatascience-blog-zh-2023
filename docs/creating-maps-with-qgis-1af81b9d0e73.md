# 使用 QGIS 创建地图

> 原文：[https://towardsdatascience.com/creating-maps-with-qgis-1af81b9d0e73?source=collection_archive---------3-----------------------#2023-12-24](https://towardsdatascience.com/creating-maps-with-qgis-1af81b9d0e73?source=collection_archive---------3-----------------------#2023-12-24)

## 一份关于最佳开源 GIS 软件的全面指南

[](https://medium.com/@teosiyang?source=post_page-----1af81b9d0e73--------------------------------)[![Jake Teo](../Images/9687f43822fab69befb750a8ec58516d.png)](https://medium.com/@teosiyang?source=post_page-----1af81b9d0e73--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1af81b9d0e73--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1af81b9d0e73--------------------------------) [Jake Teo](https://medium.com/@teosiyang?source=post_page-----1af81b9d0e73--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F52b0d82d5bf5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-maps-with-qgis-1af81b9d0e73&user=Jake+Teo&userId=52b0d82d5bf5&source=post_page-52b0d82d5bf5----1af81b9d0e73---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1af81b9d0e73--------------------------------) · 11分钟阅读 · 2023年12月24日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1af81b9d0e73&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-maps-with-qgis-1af81b9d0e73&user=Jake+Teo&userId=52b0d82d5bf5&source=-----1af81b9d0e73---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1af81b9d0e73&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-maps-with-qgis-1af81b9d0e73&source=-----1af81b9d0e73---------------------bookmark_footer-----------)![](../Images/f4f9377b8d9df9d532572dadd1e4967e.png)

图片来源：[Louis Hansel](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral)于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

十多年前，当我开始从事数据职业，担任 GIS（地理信息系统）分析师时，有两款全能型 GIS 软件非常突出。十年过去了，仍然是这两款软件。[**ArcGIS**](https://www.esri.com/en-us/arcgis/about-arcgis/overview)由**ESRI**公司提供，是目前的主导软件，易于使用，功能丰富，并且附带了出色的 Python 库。然而，它是收费软件，仅能在 Windows 系统上使用。

[**QGIS**](https://qgis.org/en/site/)（量子 GIS）是最佳的开源替代方案。它支持大多数操作系统，并且有一个非常活跃的开源社区。选择任一软件都不会错，但随着我深入编程职业，使用 Windows 变得越来越困难，我通常会尽量避免企业软件以避开采购行政工作。因此，我现在更倾向于使用 QGIS。

![](../Images/ce26be7474a8974d9b82e4b671cc8fc3.png)

QGIS 标志。[(CC BY-SA)](https://www.qgis.org/en/site/getinvolved/styleguide.html)

*下面的教程展示的是 QGIS 3.34 Prizren 版本，可能在之前或未来的版本中略有不同。*

# 目录

[键盘快捷键](#65b2) [添加底图](#1c57)

[创建新图层](#a1e9)

[添加要素](#590b)

[编辑要素](#dd95)…
