# 如何用 Python 绘制 Landsat 卫星图像上的坐标

> 原文：[`towardsdatascience.com/how-to-plot-coordinates-on-landsat-satellite-images-with-python-5671613887aa?source=collection_archive---------3-----------------------#2023-05-16`](https://towardsdatascience.com/how-to-plot-coordinates-on-landsat-satellite-images-with-python-5671613887aa?source=collection_archive---------3-----------------------#2023-05-16)

## 使用 Landsat 元数据和 Rasterio 将像素位置映射到地理坐标

[](https://conorosullyds.medium.com/?source=post_page-----5671613887aa--------------------------------)![Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----5671613887aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5671613887aa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5671613887aa--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----5671613887aa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ae48256fb37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-plot-coordinates-on-landsat-satellite-images-with-python-5671613887aa&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=post_page-4ae48256fb37----5671613887aa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5671613887aa--------------------------------) ·8 分钟阅读·2023 年 5 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5671613887aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-plot-coordinates-on-landsat-satellite-images-with-python-5671613887aa&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=-----5671613887aa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5671613887aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-plot-coordinates-on-landsat-satellite-images-with-python-5671613887aa&source=-----5671613887aa---------------------bookmark_footer-----------)![](img/45e764239f69e9589c7b61ffd50583f1.png)

图片由 [GeoJango Maps](https://unsplash.com/@geojango_maps?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

位置、位置、位置！这不仅仅是房地产市场的陈词滥调，而且对遥感非常重要。无论是监测环境变化、分析城市发展还是追踪作物健康，精准的地理位置至关重要。我们需要知道卫星图像中物体的确切坐标，以确保准确的分析和解释。

因此，我们将探讨如何使用两种方法将坐标直接绘制到 Landsat 场景上：

+   Landsat 元数据文件（MLT）

+   Rasterio — 一个用于访问地理空间栅格数据的包

我们还将使用 Rasterio 来**重投影**卫星图像的坐标。特别是，我们将从原始坐标参考系统**(UTM)**转换为谷歌地图使用的坐标系统**(EPSG:4326)**。在此过程中，我们将讨论代码，你可以在[GitHub](https://github.com/conorosully/medium-articles/blob/master/src/remote%20sensing/landsat_GPS.ipynb)上找到完整的项目。

# 下载 Landsat 场景

我们需要从下载一个 Landsat 场景开始。你可以使用[EarthExplorer](http://earthexplorer.usgs.gov)门户来完成这一操作。或者，如果你想使用 Python，下面的文章将带你完成这个过程：
