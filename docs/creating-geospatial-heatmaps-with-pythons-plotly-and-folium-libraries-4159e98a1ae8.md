# 使用 Python 的 Plotly 和 Folium 库创建地理空间热图

> 原文：[`towardsdatascience.com/creating-geospatial-heatmaps-with-pythons-plotly-and-folium-libraries-4159e98a1ae8`](https://towardsdatascience.com/creating-geospatial-heatmaps-with-pythons-plotly-and-folium-libraries-4159e98a1ae8)

## 用于可视化地理空间变化的两个优秀 Python 选项

[](https://andymcdonaldgeo.medium.com/?source=post_page-----4159e98a1ae8--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----4159e98a1ae8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4159e98a1ae8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4159e98a1ae8--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----4159e98a1ae8--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4159e98a1ae8--------------------------------) ·阅读时间 6 分钟·2023 年 3 月 17 日

--

![](img/07f55e060065099f6e715799e103d191.png)

图片由 [KOBU Agency](https://unsplash.com/@kobuagency?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**热图**，也称为 **密度图**，是展示变量在地理区域内空间分布的数据可视化工具。它们是可视化和识别趋势、支持决策、检测异常值以及为演示文稿创建引人注目的可视化的绝佳工具。

有几种 Python 地图绘制库可供选择，然而，两个非常流行且易于使用的库是 Folium 和 Plotly Express。

[**Folium**](https://python-visualization.github.io/folium/) 是一个很棒的库，它使可视化地理空间数据变得简单。它由 [**Leaflet.js**](https://leafletjs.com/) 提供支持，Leaflet.js 是一个领先的 JavaScript 地图绘制库，且平台无关。 [**Plotly**](https://plotly.com/graphing-libraries/) 是一个流行的库，可以用极少的代码创建强大的交互式数据可视化，并可以与 MapBox 一起创建交互式地图。

在本文中，我们将展示如何使用这两个库在挪威大陆架上可视化声学压缩慢度数据。

使用 Plotly Express 的视频版本教程可以在我的 YouTube 频道找到。请查看下面的链接。

# 导入库和加载数据

在本教程中，我们将从导入 [pandas](https://pandas.pydata.org/) 开始，这将用于从 CSV 文件中加载数据。

```py
import pandas as pd
```

我们在本教程中使用的数据集是以下内容的组合：

+   Xeek Force 2020 机器学习岩石学竞赛数据集被用来推导 Shetland Group 的平均声波压缩慢度（DTC）值

+   来自挪威石油局网站的数据，用于提供位置数据

```py
df = pd.read_csv('xeek_force_2020_dtc_mapping.csv')
df
```

当数据加载完成后，我们可以使用以下方法调用数据框：

![](img/d861bdb997641ab938e82be90fb62a16.png)

挪威大陆架井的数据框视图。图片由作者提供。

我们可以看到我们有 7 列：

+   **井名** — 井的名称

+   **DTC** — Shetland Group 的平均声波压缩慢度

+   **温度** — 井底温度

+   **水深** — 从海平面到海底的深度

+   **完成年份** — 井的完成年份

+   **纬度** — 井的纬度位置，单位为十进制度

+   **经度** — 井的经度位置，单位为十进制度

# 使用 Plotly Express 创建热图

要开始使用 [Plotly Express](https://plotly.com/) 创建热图，我们需要将该库导入到我们的笔记本中，如下所示：

```py
import plotly_express as px
```

然后我们需要创建一个新的图形。这是通过调用 plotly express 的 `density_mapbox` 来完成的。

要创建我们的地图，我们需要传入几个参数：

+   `df` — 包含数据的数据框名称

+   `lat` — 纬度列的名称

+   `lon` — 经度列的名称

+   `z` — 包含我们希望在热图上可视化的数据的列名称

+   `center` — 地图的中心位置。我们可以调用纬度和经度列的均值

+   `zoom` — 地图的初始缩放级别

最后两个参数 `mapbox_style` 和 `height` 控制背景映射层和图的高度。

```py
fig = px.density_mapbox(df, lat='Latitude', lon='Longitude',
                        z='DTC', radius=20,
                        center=dict(lat=df.Latitude.mean(), 
                                    lon=df.Longitude.mean()), 
                        zoom=4,
                        mapbox_style="open-street-map", 
                        height=900)
fig.show()
```

当我们调用 `fig.show()` 时，我们会得到以下地图。

![](img/45f2ede798e774a19ebbcd09697a2c18.png)

使用 plotly express 的 density_mapbox 生成的热图。图片由作者提供。

从这张地图上，我们可以看到该区域北端的 DTC 值较高，这可能归因于几种因素，包括较低的压实度或较高的页岩度。

Plotly express 生成的地图的一个优点是我们可以悬停在数据上并获得绘制变量（DTC）的值。这是自动完成的，无需额外代码来创建弹出框。

![](img/895dc623da49ab17586f3dbbcf38a118.png)

使用 plotly express 的 density_mapbox 生成的热图增加了交互性。图片由作者提供。

# 使用 Folium 创建热图

要开始使用 Folium，我们需要导入它；然而，为了生成热图，我们还需要从`folium.plugins`导入 HeatMap 插件。

```py
import folium
from folium.plugins import HeatMap
```

一旦导入了 Folium，我们需要通过调用 `folium.map()` 来创建一个基础地图。在该类方法中，我们可以传入几个参数。对于这个例子，我们将使用以下参数：

+   `location` — 地图的中心位置

+   `zoom_start` — 地图的初始缩放级别

+   `control_scale` — 是否在地图上显示比例控制

如果你想了解有关地图函数的参数，可以查看[**folium.map**](https://python-visualization.github.io/folium/modules.html)类的帮助文档。

```py
m = folium.Map(location=[df_combined.Latitude.mean(), 
                         df_combined.Longitude.mean()], 
               zoom_start=6, control_scale=True)
```

接下来，我们需要创建热力图图层。

要实现这一点，我们首先需要将纬度、经度和值转换为列表，然后可以传递给`HeatMap`调用。

我们可以设置一些参数来美化热力图的外观，例如最小和最大不透明度、数据点的颜色半径等。

```py
map_values = df_combined[['Latitude','Longitude','DTC']]

data = map_values.values.tolist()

hm = HeatMap(data,
              min_opacity=0.05, 
              max_opacity=0.9, 
              radius=25).add_to(m)
```

当我们调用我们的地图对象时，我们可以看到与 plotly express 生成的图案类似。

![](img/c7e91b84a5461321f6da5b92bf4ef531.png)

使用 Folium 生成的热力图。图片由作者提供。

然而，这张地图有几个缺点：

没有类似于我们在 Plotly Express 中看到的悬停功能。我们可以添加标记，但这需要几行额外的代码。

如果你想了解如何实现这一点，可以查看我下面的文章。

[](/folium-mapping-displaying-markers-on-a-map-6bd56f3e3420?source=post_page-----4159e98a1ae8--------------------------------) ## Folium 映射：在地图上显示标记

### 在 Python 中向 Folium 地图添加标记的简短指南

towardsdatascience.com

另一个问题是没有颜色渐变条来帮助读者理解颜色范围。但可以通过创建颜色映射字典来解决这一问题，具体可以参见以下 StackOverflow 帖子。

[](https://stackoverflow.com/questions/47163728/how-to-add-legend-gradient-in-folium-heat-map?source=post_page-----4159e98a1ae8--------------------------------) [## 如何在 Folium 热力图中添加图例/渐变？

### 我正在使用 Folium 创建热力图。我的数据包含 3 列：类别、纬度和经度。纬度-经度点是…

stackoverflow.com](https://stackoverflow.com/questions/47163728/how-to-add-legend-gradient-in-folium-heat-map?source=post_page-----4159e98a1ae8--------------------------------)

# 结论

热力图提供了一种出色的方式来可视化和识别地理区域的趋势，并且可以使用两个流行的 Python 库：Folium 和 Plotly Express 轻松创建。这两个库使用简单，可以用于在大范围内绘制岩石物理和井日志属性。从这些地图中，我们可以获得有关领域或区域的趋势和变化的信息。

*感谢阅读。在你离开之前，你一定要订阅我的内容，将我的文章发送到你的收件箱。* [***你可以在这里做到这一点！***](https://andymcdonaldgeo.medium.com/subscribe)*或者，你可以* [***注册我的新闻通讯***](https://fabulous-founder-2965.ck.page/2ca286e572) *，以免费将额外内容直接发送到你的收件箱。*

*其次，你可以通过注册会员获得完整的 Medium 体验，支持我和其他成千上万的作者。每月仅需 $5，你即可全面访问所有精彩的 Medium 文章，还有机会通过写作赚钱。如果你通过* [***我的链接***](https://andymcdonaldgeo.medium.com/membership)***,* *你将直接用部分费用支持我，而且不会增加额外费用。如果你这样做了，非常感谢你的支持！*

# 本教程中使用的数据集

本教程使用的数据集由两个其他数据集组成：

1.  用于 Xeek 和 FORCE 2020 举办的机器学习竞赛中的训练数据集的一个子集 *(Bormann 等人, 2020)*。

    可以通过以下链接访问完整数据集：[`doi.org/10.5281/zenodo.4351155`](https://doi.org/10.5281/zenodo.4351155)。

1.  来自挪威石油局网站的井位数据

    数据可以在这里下载：[`factpages.npd.no/en/wellbore/tableview/exploration/all`](https://factpages.npd.no/en/wellbore/tableview/exploration/all)

这两个数据集均根据挪威政府的 NOLD 2.0 许可证授权，详细信息可以在这里找到：[挪威开放政府数据许可证 (NLOD) 2.0](https://data.norge.no/nlod/en/2.0/)。
