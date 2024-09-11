# 生成地理区域的3D网格

> 原文：[https://towardsdatascience.com/generate-a-3d-mesh-of-a-geographic-area-with-qgis-3844e3f7806a?source=collection_archive---------10-----------------------#2023-03-14](https://towardsdatascience.com/generate-a-3d-mesh-of-a-geographic-area-with-qgis-3844e3f7806a?source=collection_archive---------10-----------------------#2023-03-14)

## 从数字高程模型到3D网格

[](https://mattiagatti.medium.com/?source=post_page-----3844e3f7806a--------------------------------)[![Mattia Gatti](../Images/9d5aeb356ff01ed9e4ead66c18994595.png)](https://mattiagatti.medium.com/?source=post_page-----3844e3f7806a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3844e3f7806a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3844e3f7806a--------------------------------) [Mattia Gatti](https://mattiagatti.medium.com/?source=post_page-----3844e3f7806a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F19bc376db93c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerate-a-3d-mesh-of-a-geographic-area-with-qgis-3844e3f7806a&user=Mattia+Gatti&userId=19bc376db93c&source=post_page-19bc376db93c----3844e3f7806a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3844e3f7806a--------------------------------) ·5分钟阅读·2023年3月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3844e3f7806a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerate-a-3d-mesh-of-a-geographic-area-with-qgis-3844e3f7806a&user=Mattia+Gatti&userId=19bc376db93c&source=-----3844e3f7806a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3844e3f7806a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerate-a-3d-mesh-of-a-geographic-area-with-qgis-3844e3f7806a&source=-----3844e3f7806a---------------------bookmark_footer-----------)![](../Images/bf84705a91075e835b6f5e4ac3984c06.png)

图片由 [Planet Volumes](https://unsplash.com/@planetvolumes?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

3D网格可以用于表示地理数据，如地形、建筑物和其他结构。这种网格可以用于多种目的，例如城市规划、环境分析或虚拟现实模拟。然而，创建一个地理区域的3D网格的过程并不总是简单的，但本指南涵盖了所有必要的步骤。

# 介绍

要生成给定区域的 3D 网格，需要该区域的高程数据。这些数据存储在数字高程模型（DEM）中：

> 数字高程模型（DEM）是一种地理栅格文件，用于表示地表起伏。

DEM 基本上是一个高程值的网格。如果你想了解更多信息，我在之前的[文章](/the-ultimate-beginners-guide-to-geospatial-raster-data-feb7673f6db0)中讨论了 DEM。

![](../Images/41059f8b3822f57ceff36a4448ebd9af.png)

DEM 的图形可视化。[NSIDC](https://commons.wikimedia.org/wiki/File:Digital_elevation_model_(DEM)_of_the_Mt._Everest_region_-_50090548573.png)，[CC BY 2.0](https://creativecommons.org/licenses/by/2.0)，通过维基共享资源

这些模型是使用以下一种或多种技术创建的：

+   LiDAR — 这种方法使用激光扫描仪来测量地形的高度……
