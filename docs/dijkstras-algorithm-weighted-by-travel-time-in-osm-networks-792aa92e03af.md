# Dijkstra 算法在 OSM 网络中按旅行时间加权

> 原文：[`towardsdatascience.com/dijkstras-algorithm-weighted-by-travel-time-in-osm-networks-792aa92e03af`](https://towardsdatascience.com/dijkstras-algorithm-weighted-by-travel-time-in-osm-networks-792aa92e03af)

## 使用 OSMNX 1.6 寻找最快和最短路径

[](https://bryanvallejo16.medium.com/?source=post_page-----792aa92e03af--------------------------------)![Bryan R. Vallejo](https://bryanvallejo16.medium.com/?source=post_page-----792aa92e03af--------------------------------)[](https://towardsdatascience.com/?source=post_page-----792aa92e03af--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----792aa92e03af--------------------------------) [Bryan R. Vallejo](https://bryanvallejo16.medium.com/?source=post_page-----792aa92e03af--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----792aa92e03af--------------------------------) ·7 min 阅读·2023 年 10 月 10 日

--

![](img/692de08c2ac0d0d9fef8304a5bad06dc.png)

图片由作者提供。摩洛哥示例中的最快路线（红色）和最短路线（橙色）

最短路径（Dijkstra）算法可以应用于 OSM 网络中，如驾驶、骑行和步行，以找到起点和终点之间的最优路线。但该算法在网络中计算基于距离的最短路线，这并不意味着最优路线。在道路网络中，距离可以是相对的，当我们考虑道路的速度时。显然，在所有道路速度相等的情况下，两点之间的最优路线可能是最短的。如果我们比较高速公路与城市街道的速度，我们会重新调整这个想法，理解最优路线是最快的。

> “在道路网络中，距离可能是相对的，当我们考虑道路的速度时”

借助 Python 库 OSMNX，可以在全球范围内为不同类型的道路添加速度，并计算 OSM 网络中节点之间的*旅行时间*。这使得 Python 库可以处理以*旅行时间*加权的最短路径算法。

这一实践是之前一个教程的延续，那个教程使用了最短路径算法来计算摩洛哥两个位置之间的最短路线。

## 摩洛哥 OSM 网络中的最短路线

[](/shortest-path-dijkstras-algorithm-step-by-step-python-guide-896769522752?source=post_page-----792aa92e03af--------------------------------) ## 最短路径（Dijkstra）算法逐步 Python 指南

### 使用 OSMNX 1.6 的更新及长距离路径

towardsdatascience.com

# 访问编码教程

如果你还不是[Medium](https://medium.com/u/504c7870fdb6?source=post_page-----792aa92e03af--------------------------------)的会员，你需要订阅才能访问这些故事。你可以通过使用我的个人链接来跟随更多编码教程并支持我的工作。成为这段编码之旅的一部分。

> 这里加入 👉 [`bit.ly/3yjLsSL`](https://bit.ly/3yjLsSL)

# OSM 数据许可证

+   **Open Street Map 数据。** 许可于 [Open Data Commons Open Database License (ODbl)](https://opendatacommons.org/licenses/odbl/) 或署名许可。用户可以自由复制、分发、传输和调整数据，只要注明作者，如© [OpenStreetMap](https://www.openstreetmap.org/copyright) 贡献者。

# 介绍

接下来的步骤将指导如何使用旅行时间应用最短路径。我们将比较最快路线和最短路线，以了解旅行时间和长度的变化。

此外，我们还将使用 OSMNX 中的更多函数来改进结果，如`utils_graph.route_to_gdf()`，以及计算*旅行时间*的`add_edge_speeds()`和`add_edge_travel_times()`。

这是由[Hanae](https://medium.com/u/3b913af53dd0?source=post_page-----792aa92e03af--------------------------------)建议的起点和终点的快速视图。

![](img/dd1ded2472ba3b204998b6b2c29a83e0.png)

作者提供的图像。起点和终点位置。

# 编码实践

开始获取我们需要的库。

```py
import osmnx as ox
import geopandas as gpd
from shapely.geometry import Point
import pandas as pd
import matplotlib.pyplot as plt
```

## 1\. 定义起点和终点为 GeoDataFrames

开始添加坐标并使用 Geopandas 创建新的 GDF。

```py
# --- origin and destination geom

origin_geom = Point(-5.6613932957355715, 32.93210288339607)
destination_geom = Point(-3.3500597061072726, 34.23038027794419)

# --- create origin dataframe

origin =  gpd.GeoDataFrame(columns = ['name', 'geometry'], crs = 4326, geometry = 'geometry')
origin.at[0, 'name'] = 'origin'
origin.at[0, 'geometry'] =origin_geom

# --- create destination dataframe

destination =  gpd.GeoDataFrame(columns = ['name', 'geometry'], crs = 4326, geometry = 'geometry')
destination.at[0, 'name'] = 'destination'
destination.at[0, 'geometry'] = destination_geom
```

## 2\. 获取图网络

当我们有长途路线时，建议使用*envelope*函数来获取图。

使用之前定义的函数

```py
def get_graph_from_locations(origin, destination, network='drive'):
    '''
    network_type as drive, walk, bike
    origin gdf 4326
    destination gdf 4326
    '''
    # combine and area buffer
    combined = pd.concat([origin, destination])

    convex = combined.unary_union.envelope # using envelope instead of convex, otherwise it breaks the unary_union

    graph_extent = convex.buffer(0.02)

    graph = ox.graph_from_polygon(graph_extent, network_type= network)

    return graph
```

应用并可视化

```py
# --- Get Graph
graph = get_graph_from_locations(origin, destination)
```

```py
fig, ax = ox.plot_graph(graph, node_size=0, edge_linewidth=0.4, bgcolor='black', edge_alpha=0.2,  edge_color='yellow')
```

![](img/86ce92fc96ba9311953cbbad921a39d9.png)

作者提供的图像。包含起点和终点的道路网络图

## 3\. 向道路网络添加旅行时间

我们将使用函数`add_edge_speeds()`添加旅行时间，该函数输入速度（以 km/h 为单位）。插补将按高速公路类型添加道路的平均最大速度值。然后，我们使用函数`add_edge_travel_time()`计算*旅行时间*

我们将保留相同的变量*graph*

```py
# --- add edge speed
graph = ox.add_edge_speeds(graph)

# --- add travel time
graph = ox.add_edge_travel_times(graph)
```

如果你想修改高速公路类别，可以传递一个基于本地速度值的字典

```py
# --- add speeds define by local authorities (example)
hwy_speeds = {"residential": 35, "secondary": 60, "tertiary": 75}

graph = ox.add_edge_speeds(graph, hwy_speeds)
graph = ox.add_edge_travel_times(graph)
```

现在，通过获取边缘类别来快速查看按高速公路类型的旅行时间。

```py
# --- get the edges as GDF
edges = ox.graph_to_gdfs(graph, nodes=False)[['highway', 'speed_kph', 'length', 'travel_time', 'geometry']].reset_index(drop=True)

# --- see mean speed/time values by road type
edges["highway"] = edges["highway"].astype(str)
edges.groupby("highway")[["speed_kph", "travel_time"]].mean().round(0)
```

![](img/e6d02f38607c6f55db56f06ffa1277c3.png)

作者提供的图像。道路网络中的速度和旅行时间

## 4\. 查找起点和终点的最近节点

使用起点和终点坐标获取网络中最接近的节点。该函数在新版本 1.6 中为`nearest_nodes()`

```py
# ------------- get closest nodes

# origin
closest_origin_node = ox.nearest_nodes(G=graph, 
                                       X=origin_geom.x, 
                                       Y=origin_geom.y)

# destination
closest_destination_node = ox.nearest_nodes(G=graph, 
                                           X=destination_geom.x, 
                                           Y=destination_geom.y)
```

然后，我们在节点之间应用最短路径算法。

## 5\. 计算使用旅行时间的最短路径

我们将使用 `shortest_path()` 函数来计算我们的路线。我们将同时使用距离和时间来比较路线在 `weight` 参数下的不同之处。

```py
# --- calculate shortest path with length and travel time

# time
fastest_route = ox.shortest_path(graph, 
                                orig = closest_origin_node, 
                                dest = closest_destination_node, 
                                weight="travel_time")

# distance
shortest_route = ox.shortest_path(graph, 
                                orig = closest_origin_node, 
                                dest = closest_destination_node,  
                                weight="length")
```

这将返回一组属于该路线的节点代码。

![](img/93c60e89b3cfd82895cb495dd0e3f234.png)

作者提供的图像。路线的节点

## 6\. 从节点创建路线（单行代码）

osmnx 实现了一个函数 `utils_graph.route_to_gdf()`，可以在一行中将节点转换为 GeoDataFrame。很方便，我们可以动态获取感兴趣的列。

```py
# --- get gdf of routes

# fastest
fastest_route_gdf = ox.utils_graph.route_to_gdf(graph, fastest_route, weight='travel_time')[['highway', 'speed_kph', 'travel_time', 'geometry']]

# shortest
shortest_route_gdf = ox.utils_graph.route_to_gdf(graph, shortest_route, weight='length')[['highway', 'speed_kph', 'travel_time', 'geometry']]
```

## 7\. 快速比较（时间和距离）

我们将比较两条路线的旅行时间和长度。

```py
# --- comparison

d1 = fastest_route_gdf['length'].sum()
d2 = shortest_route_gdf['length'].sum()

t1 = fastest_route_gdf['travel_time'].sum()
t2 = shortest_route_gdf['travel_time'].sum()
```

打印值

```py
print(f'Fastest Route: Time {round(t1/3600, 2)} hours, Distance {round(d1/1000, 2)} km')
print(f'Shortest Route: Time {round(t2/3600, 2)} hours, Distance {round(d2/1000, 2)} km')
```

> *最快路线：时间 5.42 小时，距离 378.93 公里
> 
> 最短路线：时间 5.6 小时，距离 362.84 公里*

结果显示最快的路线更长。如预期的那样，涉及速度时，距离变得相对。

## 8\. 保存文件并可视化

保存网络和路线

```py
# --- save

edges.to_file('osm_drive_network.gpkg')

fastest_route_gdf.to_file('fastest_route.gpkg')

shortest_route_gdf.to_file('shortest_route.gpkg')
```

在 QGIS 中

![](img/1fe4f787061be2e19a0907d58ce2108d.png)

作者提供的图像。最快的路线（橙色）和最短的路线（黄色）

在 Matplotlib 中

```py
# --- plot network
ax = edges.plot(figsize=(12, 10), linewidth = 0.1, color='grey', zorder=0);

# --- origin and destination
origin.plot(ax=ax, markersize=100, alpha=0.8, color='blue', zorder=1)
destination.plot(ax=ax, markersize=100, alpha=0.8, color='green', zorder=2)

# --- route
fastest_route_gdf.plot(ax=ax, linewidth = 4, color='red', alpha=0.4, zorder=3)
shortest_route_gdf.plot(ax=ax, linewidth = 4, color='yellow', alpha=0.4, zorder=4)

plt.axis(False);
```

![](img/4c910cbf3a0c59ef031d78ac5e8bf926.png)

作者提供的图像。较快的路线（红色）和最短的路线（黄色）

# 已知改进

从 osmnx 使用的新功能 `utils_graph.route_gdf()` 创建了一个干净的路线。它使用了道路段，而不仅仅是节点之间的联接，新路径覆盖了 OSM 道路网络。

![](img/fcbca57f332951bddbf520786144db7d.png)

作者提供的图像。路线覆盖了道路网络。

# **结论**

最短路径算法可以使用 OSMNX 函数 `add_edge_speeds()` 和 `add_edge_travel_times()` 计算道路速度。这种不同的方法表明，如果在路线计算中实施速度（旅行时间），最短路径是相对的。正如预期的那样，最快的路线最终比最短路线更长，但它以最短的旅行时间到达了目的地。

生成覆盖道路网络的路线的改进使得在城市区域级别的可达性和邻近性研究中，距离和旅行时间的计算更加准确。

# 致谢

多亏了[Geoff Boeing](https://medium.com/u/c490d872f7a?source=post_page-----792aa92e03af--------------------------------)提供的资料，我得以探索这些功能并理解 OSMNX 的功能。

如果你想就问题或定制分析联系我：

> [*Bryan R. LinkedIn*](https://www.linkedin.com/in/bryanrvallejo/)
