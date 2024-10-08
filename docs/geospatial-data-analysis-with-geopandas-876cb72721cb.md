# 使用 GeoPandas 进行地理空间数据分析

> 原文：[`towardsdatascience.com/geospatial-data-analysis-with-geopandas-876cb72721cb`](https://towardsdatascience.com/geospatial-data-analysis-with-geopandas-876cb72721cb)

## 学习如何使用 Python 的 GeoPandas 操作和可视化矢量数据

[](https://eugenia-anello.medium.com/?source=post_page-----876cb72721cb--------------------------------)![Eugenia Anello](https://eugenia-anello.medium.com/?source=post_page-----876cb72721cb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----876cb72721cb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----876cb72721cb--------------------------------) [Eugenia Anello](https://eugenia-anello.medium.com/?source=post_page-----876cb72721cb--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----876cb72721cb--------------------------------) ·6 分钟阅读·2023 年 5 月 6 日

--

![](img/3c457c4199b362e1f2907d84b08b6323.png)

图片由[Artem Beliaikin](https://unsplash.com/@belart84)拍摄，发布于[Unsplash](https://unsplash.com/photos/v6kii3H5CcU)

*这是关于地理空间数据分析系列的第三篇文章：*

1.  *使用 QGIS 进行地理空间数据分析*

1.  *OpenStreetMap 入门指南*

1.  *使用 GeoPandas 进行地理空间数据分析（本文）*

1.  *使用 OSMnx 进行地理空间数据分析*

1.  [*数据科学家的地理编码*](https://www.datacamp.com/tutorial/geocoding-for-data-scientists)

1.  [*使用 Geemap 进行地理空间数据分析*](https://www.kdnuggets.com/geospatial-data-analysis-with-geemap)

本文是使用 QGIS 进行地理空间数据分析的实用介绍和 OpenStreetMap 入门指南的延续。在之前的教程中，我提供了地理空间数据分析的概述，这是一门无处不在且可以应用于许多领域的子领域，如物流、运输和保险。

这个学科专注于分析一种特殊类型的数据，即地理空间数据，其特点是具有位置，由一对或多对坐标描述。例子包括餐馆、道路和国家之间的边界。为了展示连续的表面，比如卫星图像，地理表格已不再足够，你需要一个包含一个或多个通道的数组。

在这篇文章中，我将重点讲解最简单的情况，即地理表格，也称为矢量数据。对于这个任务，[GeoPandas](https://geopandas.org/en/stable/) 是将用于处理和可视化这种类型地理空间数据的 Python 库。正如你可能猜到的，它是 Pandas 的扩展，一个流行的 Python 包，允许你轻松且快速地处理地理空间数据。让我们开始吧！

**目录：**

+   **导入普查数据**

+   **向普查数据添加几何信息**

+   **使用 GeoPandas 创建地图**

+   **从几何信息中提取质心**

+   **创建更复杂的地图**

## **导入普查数据**

开始地理空间数据分析之旅的最佳方法是通过处理普查数据，这些数据提供了世界各国所有人和家庭的详细信息。

在本教程中，我们将使用一个提供英国汽车或货车数量的数据集，该数据集来自于[英国数据服务](https://ukdataservice.ac.uk/)。数据集的链接是[这里](https://statistics.ukdataservice.ac.uk/dataset/car-or-van-availability-2011/resource/7544ad18-8577-4200-bd9f-53f8963dbc9d)。

我将从一个不包含地理信息的数据集开始：

数据集的每一行对应于一个特定的输出区域，这是英国普查提供的最低地理级别。有三个特征：地理编码、国家以及一个或多个家庭成员拥有的汽车或货车数量。

如果我们现在想要可视化地图，我们将无法实现，因为我们没有必要的地理信息。在展示 GeoPandas 的潜力之前，我们需要进一步的步骤。

## **向普查数据添加几何信息**

为了可视化我们的普查数据，我们需要添加一个存储地理信息的列。添加地理信息的过程，例如为每个城市添加纬度和经度，称为**地理编码**。

在这种情况下，不只是一个坐标对，而是有不同的坐标对，它们连接并闭合，形成输出区域的边界。我们需要从这个[链接](https://statistics.ukdataservice.ac.uk/dataset/2011-census-geography-boundaries-uk/resource/a7ccd59d-2dff-4bd0-95b9-b39b607452cc)导出 Shapefile。它提供了每个输出区域的边界。

一旦数据集被导入，我们可以使用它们的公共字段 geo_code 合并这两个表：

在评估了数据框的维度在左连接后没有变化后，我们需要检查新列中是否存在空值：

```py
df.geometry.isnull().sum()
# 0 
```

幸运的是，没有空值，我们可以使用 GeoDataFrame 类将数据框转换为地理数据框，并将几何列设置为我们地理数据框的几何体：

现在，地理信息和非地理信息被合并到一个独特的表中。所有的地理信息都包含在一个字段中，称为 geometry。就像在普通的数据框中一样，我们可以打印出这个地理数据框的信息：

从输出中，我们可以看到我们的地理数据框是`geopandas.GeoDataFrame`对象的一个实例，几何体使用几何类型进行编码。为了更好地理解，我们还可以显示第一行中几何列的类型：

```py
type(gdf.geometry[0])

# shapely.geometry.polygon.Polygon
```

了解几何对象中有三种常见的类是很重要的：点、线和多边形。在我们的例子中，我们处理的是多边形，这很有意义，因为它们是输出区域的边界。然后，数据集已经准备好了，我们可以从现在开始构建漂亮的可视化图。

## 使用 GeoPandas 创建地图

现在，我们有了使用 GeoPandas 可视化地图的所有必要要素。由于 GeoPandas 的一个缺点是它处理大量数据时存在困难，而我们有超过 20 万行数据，因此我们将专注于北爱尔兰的人口普查数据：

```py
gdf_ni = gdf.query(‘Country==”Northen Ireland”’)
```

要创建地图，只需在地理数据框上调用`plot()`方法：

我们还希望通过根据每个输出区域的频率进行着色，查看汽车/面包车在北爱尔兰的分布情况：

从这个图中，我们可以观察到大多数区域的车辆数量在 200 辆左右，除了标记为绿色的小区域。

## **从几何体中提取质心**

假设我们想要更改几何形状，并将坐标放在输出区域的中心，而不是多边形。可以通过使用`gdf.geomtry.centroid`属性来计算每个输出区域的质心来实现这一点：

```py
gdf_ni[‘centroid’] = gdf.geometry.centroid
gdf_ni.sample(3)
```

![](img/6844f82e81b6c8188565f38413960620.png)

如果我们再次显示数据框的信息，我们可以注意到几何体和质心都被编码为几何类型。

更好地理解我们真正获得了什么的是在一个唯一的地图中可视化几何体和质心列。要绘制质心，需要使用`set_geometry()`方法来切换几何体。

## 创建更复杂的地图

有一些高级功能可以在地图中可视化更多细节，而无需创建其他信息列。之前我们显示了每个输出区域的汽车或面包车数量，但这比信息量更混乱。基于我们的数值列创建一个类别特征会更好。使用 GeoPandas，我们可以跳过那一阶段，直接进行绘图。通过指定参数`scheme=’intervals’`，我们能够根据等间隔创建汽车/面包车的类别。

地图没有发生太大变化，但你可以看到图例相比之前的版本更清晰。更好的可视化地图的方法是基于分位数构建的级别来着色：

现在，在地图上可以发现更多的变化，因为每个级别包含了更多分布的区域。值得注意的是，大多数区域属于最后两个级别，对应于车辆数量最多的情况。在第一次可视化中，200 辆车似乎数量不多，但实际上存在许多高频率的离群值，这扭曲了我们的解释。

此时，我们还希望有一个背景地图来更好地使我们的结果具有上下文。最流行的方法是使用 contextily 库，它可以获取背景地图。该库需要 Web Mercator 坐标参考系统（EPSG:3857）。因此，我们需要将数据转换为该坐标参考系统。绘制地图的代码保持不变，只需增加一行代码来添加来自 Contextily 库的基础地图：

太好了！现在，我们有了一张更专业、更详细的地图！

## 最后思考：

这是一个入门教程，用于开始用 Python 处理地理空间数据。GeoPandas 是一个专门处理矢量数据的 Python 库。它使用起来非常简单直观，因为它具有类似于 Pandas 的属性和方法，但一旦数据量增加，特别是在绘制数据时，它会变得非常缓慢。

除了这个缺点外，还有一个事实是它依赖于 Fiona 库来读取和写入矢量数据格式。如果 Fiona 不支持某些格式，即使是 GeoPandas 也无法支持它们。一个解决方案是结合使用 GeoPandas 来处理数据和 QGIS 来可视化地图。或者尝试其他 Python 库来可视化数据，如 Folium。你知道其他的替代方案吗？如果有其他想法，请在评论中提出。

代码可以在 [这里](https://jovian.com/eugeniaring/geopandas-example) 找到。希望你觉得这篇文章有用。祝你有美好的一天！

*免责声明：数据集的许可为英国开放政府许可证（OGL）*

**有用的资源：**

+   [Geopandas 文档](https://geopandas.org/en/stable/docs.html)

+   [Contextily 文档](https://contextily.readthedocs.io/en/latest/#)

+   [《地理数据科学书籍》](https://geographicdata.science/book/intro.html)

+   [地理空间数据分析课程（特伦托大学）](https://napo.github.io/geospatial_course_unitn/)
