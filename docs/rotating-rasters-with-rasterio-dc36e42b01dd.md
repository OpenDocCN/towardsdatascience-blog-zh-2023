# 使用 Rasterio 旋转栅格数据

> 原文：[https://towardsdatascience.com/rotating-rasters-with-rasterio-dc36e42b01dd?source=collection_archive---------8-----------------------#2023-08-07](https://towardsdatascience.com/rotating-rasters-with-rasterio-dc36e42b01dd?source=collection_archive---------8-----------------------#2023-08-07)

## 使用 Python 旋转卫星图像，同时保持地理位置的准确性

[](https://conorosullyds.medium.com/?source=post_page-----dc36e42b01dd--------------------------------)[![Conor O'Sullivan](../Images/2dc50a24edb12e843651d01ed48a3c3f.png)](https://conorosullyds.medium.com/?source=post_page-----dc36e42b01dd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dc36e42b01dd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dc36e42b01dd--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----dc36e42b01dd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ae48256fb37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frotating-rasters-with-rasterio-dc36e42b01dd&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=post_page-4ae48256fb37----dc36e42b01dd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dc36e42b01dd--------------------------------) · 6分钟阅读 · 2023年8月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdc36e42b01dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frotating-rasters-with-rasterio-dc36e42b01dd&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=-----dc36e42b01dd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdc36e42b01dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frotating-rasters-with-rasterio-dc36e42b01dd&source=-----dc36e42b01dd---------------------bookmark_footer-----------)![](../Images/d2f08f5cf0abfedc0fe7d88da8e9039b.png)

(来源：作者)

栅格数据类似于普通图像数据。不同的是，每个像素都与地球表面上的一个位置相关联。这使得情况变得复杂。如果我们想旋转数据，还必须考虑底层的坐标参考系统（CRS）。在没有调整地理位置的情况下扭曲栅格数据会导致不准确的空间分析。

调整地理位置并不是一件简单的事。幸运的是，[Rasterio](https://rasterio.readthedocs.io/en/stable/) 可以提供帮助。它是一个流行的 Python 库，用于地理空间数据分析。我们将使用这个包来：

+   **旋转**栅格数据

+   并且**重新投影**图像到正确的 CRS。

在这个过程中，我们将讨论 Python 代码，你可以在 [GitHub](https://github.com/conorosully/medium-articles/blob/master/src/remote%20sensing/rotating_rasters.ipynb) 上找到完整的项目。

这篇文章假设读者对栅格数据及其 CRS 有基本了解。如果你想复习一下，可以查看下面的文章。它详细讲解了栅格数据的重投影。

[](/how-to-plot-coordinates-on-landsat-satellite-images-with-python-5671613887aa?source=post_page-----dc36e42b01dd--------------------------------) [## 如何使用 Python 在 Landsat 卫星图像上绘制坐标

### 使用 Landsat 元数据和 Rasterio 将像素位置映射到地理坐标

[如何使用 Python 在 Landsat 卫星图像上绘制坐标](/how-to-plot-coordinates-on-landsat-satellite-images-with-python-5671613887aa?source=post_page-----dc36e42b01dd--------------------------------)

# 下载 Landsat…
