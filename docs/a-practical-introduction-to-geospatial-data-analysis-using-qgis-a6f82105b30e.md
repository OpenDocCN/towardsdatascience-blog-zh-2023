# 使用 QGIS 进行地理空间数据分析的实用介绍

> 原文：[`towardsdatascience.com/a-practical-introduction-to-geospatial-data-analysis-using-qgis-a6f82105b30e`](https://towardsdatascience.com/a-practical-introduction-to-geospatial-data-analysis-using-qgis-a6f82105b30e)

## 这是一个互动教程，可以在使用 QGIS 时学习 GIS 关键概念。

[](https://eugenia-anello.medium.com/?source=post_page-----a6f82105b30e--------------------------------)![Eugenia Anello](https://eugenia-anello.medium.com/?source=post_page-----a6f82105b30e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a6f82105b30e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a6f82105b30e--------------------------------) [Eugenia Anello](https://eugenia-anello.medium.com/?source=post_page-----a6f82105b30e--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a6f82105b30e--------------------------------) ·阅读时长 6 分钟·2023 年 2 月 27 日

--

![](img/7e80a72add9dcb55d0fadf62b861dee1.png)

图片由 [Chris Lawton](https://unsplash.com/@chrislawton) 在 [Unsplash](https://unsplash.com/photos/duQ1ulzTJbM) 提供

*这是关于地理空间数据分析系列的第一篇文章：*

1.  *使用 QGIS 进行地理空间数据分析（本篇文章）*

1.  *OpenStreetMap 入门指南*

1.  *使用 GeoPandas 进行地理空间数据分析*

1.  *使用 OSMnx 进行地理空间数据分析*

1.  [*为数据科学家准备的地理编码*](https://www.datacamp.com/tutorial/geocoding-for-data-scientists)

1.  [*使用 Geemap 进行地理空间数据分析*](https://www.kdnuggets.com/geospatial-data-analysis-with-geemap)

你想学习地理空间数据分析但不知道从何开始吗？那么，这个教程适合你。在你开始这段旅程时，有很多被视为理所当然的概念，这些概念将帮助你处理数据集中的地理信息。

地理空间数据分析是数据科学的一个子领域，专注于一种特殊类型的数据，即地理空间数据。与普通数据不同，地理空间数据的每一条记录对应于一个特定位置，并可以绘制在地图上。

一个具体的数据点可以通过纬度和经度来描述。但当你有一个包含更复杂项目的数据集时，比如道路、河流、国界或带有山脉、沙漠和森林的物理地图，一对坐标就不再足够了。我激起你的兴趣了吗？让我们开始吧！

**目录：**

+   **地理空间数据的类型**

+   **矢量数据的格式**

+   **栅格数据的格式**

+   **QGIS 的实际示例**

## 地理空间数据类型

地理空间数据主要有两种类型：**矢量数据**和**栅格数据**。处理矢量数据时，你依然会有一个表格数据集，而栅格数据更类似于具有红色、绿色和蓝色三个通道的彩色图像。仅关注矢量数据，我们可以区分三种不同的情况：点数据、线数据和多边形数据。

![](img/5cf3b3b5b00e758ea7866c0d71898255.png)

作者截屏。点数据示例来自 QGIS。

**点数据**是最简单的数据类型，由一对坐标（纬度和经度）描述。点数据的例子包括城市、餐馆和购物中心。

上面，你可以看到一个点数据的示例，显示了世界上所有机场的位置，这些数据来自于[自然地球数据](https://www.naturalearthdata.com/downloads/10m-cultural-vectors/)，这是许多免费数据源之一。

现在，介绍**线数据**，它由一个起点和一个终点的线段组成。经典的例子包括街道、火车路线和河流，你可以在下面看到。

![](img/26389b7826e0890073f9fba670cd9c34.png)

作者截屏。线数据示例来自 QGIS。

最后第三种情况是**多边形数据**，它由不同的点组成，这些点连接起来并形成闭合。最简单的例子是国家边界。下面，我提供了我们的冰川和最近退冰区域的概述。

![](img/de506ad66f629231142ea92a45b6d0e8.png)

作者截屏。点数据示例来自 QGIS。

在解释了矢量数据后，接下来是**栅格数据**，这是我最感兴趣的。正如我之前所说，它可能与图像混淆，因为它们都是像素矩阵。但与常见图像不同，每个像素对应一个不同的地理区域，每个像素的值描述了该区域的特定特征。

![](img/824fa76b0bfe2db24eea2e46570ea0ef.png)

作者截屏。栅格数据示例来自 QGIS。

从这个可视化中你可以推断出，栅格数据在实际地表信息方面提供的信息比矢量数据更多。栅格数据的示例包括卫星图像和航拍照片。

这些数据对于监测灾害和加快救援速度至关重要。因此，它不仅为企业提供了可操作的见解，还可以拯救生命。这是通过训练深度学习模型来描绘卫星图像中的特定对象实现的。

## **矢量数据的格式**

在处理地理空间数据时，了解文件的格式也很重要。对于矢量数据，最常见的地理空间文件是 **Shapefile**。你可以从许多免费的开源数据集中找到它。当你下载矢量数据时，你会得到一个压缩文件，其中包含三个必需的文件：

+   `.shp` 是最重要的文件，提供几何信息，它包含用于绘制地图上点、线和多边形的几何形状。

+   `.dbf` 是标准数据库文件，包含属性数据，由非地理空间字段组成，这些字段有助于理解地理空间数据的背景，例如城市、河流、街道和国家的名称。

+   `.shx` 提供特征几何体的位置索引。将属性与几何体链接是很重要的。

另一种常见的格式是 **GeoJSON**，即地理 JavaScript 对象表示法，用于基于 web 的地图。它由两个文件组成：`.geojson` 和 `.json`。

## 光栅数据的格式

光栅数据也有一种常见的格式，称为 **GeoTIFF**。类似于 Shapefile，它由三个文件组成：`.tif`、`.tiff`、`.ovr`。Shapefile 和 GeoTIFF 可能还会有其他文件，但幸运的是这些文件不是必须的。

其他格式的替代方案有 **ERDAS Imagine** (`.img`) 和 **IDRISI Raster** (`.rst`、`.rdc`)。就是这样！

## QGIS 的实际示例

QGIS 是我们将使用的开源软件，用于可视化地理空间数据。如果你还没有 QGIS，可以从[这里](https://www.qgis.org/en/site/forusers/download.html)下载。安装完成后，你可以打开它，应该会看到如下的窗口：

![](img/a7c876dc4084f5402e33fc38fab636e7.png)

作者在 QGIS 上的截图

第一步是将背景地图添加到地图窗口。最常用的方法是使用 [**OpenStreetMap**](https://www.openstreetmap.org/#map=5/42.088/12.564)，它提供了最大量的免费且可编辑的地理数据库，并由志愿者团队不断更新。添加它的过程非常简单：

+   点击面板上“XYZ Tiles”选项前面的箭头。

+   双击 OpenStreetMap

![](img/1b1efb3fc23fd822097729cf5d651e30.png)

作者制作的 GIF。添加 OpenStreetMap 图层。

然后，瞧，我们已经将 OSM 数据导入到我们的 QGIS 项目中。接下来，我们可以将你选择的地理数据拖到图层面板中。例如，让我们导入由 [Natural Earth Data](https://www.naturalearthdata.com/downloads/10m-cultural-vectors/) 提供的机场数据，如前面章节所示。

![](img/7c092fa2c1a96a453fef50d36e2c6819.png)

作者制作的 GIF。添加包含机场的数据。

我们也可以检查数据的信息并更改点的颜色：

![](img/2c323298a45b73295b61934a5d1830b6.png)

作者制作的 GIF。检查数据信息并更改点的颜色。

信息概述了数据类型，即点数据，以及**坐标参考系统**，这是地理空间数据的另一个特征。最后这一点对于将地球上不规则的椭球形状的位置转换为二维地图至关重要。你可以注意到它与 QGIS 的 CRS 不匹配，需要进行更改。

![](img/4d127382600e73e8a090346be3a39529.png)

GIF 作者提供。更改 CRS。

现在，错误已经修正，我们可以松一口气了。

## 最终想法：

就这些！这是一个简短快速的教程，旨在将你引入地理空间数据分析的神奇世界。我决定在本教程中使用 QGIS，以提供直观的地理空间数据示例。这仅仅是个开始。在接下来的文章中，我将涵盖更多与 Python 库相关的应用。如果你有兴趣深入了解并寻找免费的 GIS 数据源，请查看[这里](https://gisgeography.com/best-free-gis-data-sources-raster-vector/)。

## 有用的资源：

+   Dhrumil Patel 的地理空间工作入门

+   [GIS 文档](https://docs.qgis.org/2.18/en/docs/gentle_gis_introduction/i)

+   [在 Python 中分析地理空间数据：GeoPandas 和 Shapely](https://www.learndatasci.com/tutorials/geospatial-data-python-geopandas-shapely/)

+   [地理空间概念介绍](https://datacarpentry.org/organization-geospatial/)
