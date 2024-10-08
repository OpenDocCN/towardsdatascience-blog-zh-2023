# 所有道路都通向罗马吗？

> 原文：[`towardsdatascience.com/do-all-the-roads-lead-to-rome-5b6756ce7d52`](https://towardsdatascience.com/do-all-the-roads-lead-to-rome-5b6756ce7d52)

![](img/ddb463a3b8ecd8ace22b540dd4a9f1a9.png)

## 使用 Python、网络科学和地理空间数据量化古老的问题

[](https://medium.com/@janosovm?source=post_page-----5b6756ce7d52--------------------------------)![Milan Janosov](https://medium.com/@janosovm?source=post_page-----5b6756ce7d52--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5b6756ce7d52--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5b6756ce7d52--------------------------------) [Milan Janosov](https://medium.com/@janosovm?source=post_page-----5b6756ce7d52--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5b6756ce7d52--------------------------------) ·7 分钟阅读·2023 年 10 月 8 日

--

我最近遇到了一份令人兴奋的数据集，名为[*Roman Road Network (version 2008)*](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi%3A10.7910%2FDVN%2FTI0KAU)，这是哈佛 Dataverse 上的一份数据集：罗马帝国历史道路网络的完美 GIS 格式！此外，我还在进行一个关于公共交通网络的项目，研究如何识别网络科学中的热点和瓶颈。然后我迅速意识到，通过将这些信息结合起来，我可以迅速回答这个古老的问题，并查看罗马地区在当时到底有多么中心。

*在本文中，所有图像均由作者创建。*

# 1\. 阅读和可视化数据

首先，让我们使用 GeoPandas 和 Matplotlib 快速加载和探索罗马道路网络数据。

```py
import geopandas as gpd # version: 0.9.0
import matplotlib.pyplot as plt # version: 3.7.1

gdf = gpd.read_file('dataverse_files-2')
gdf = gdf.to_crs(4326)
print(len(gdf))
gdf.head(3)
```

本单元的输出：

![](img/236c8aec20f72cad735368de3904be4b.png)

[*Roman Road Network (version 2008)*](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi%3A10.7910%2FDVN%2FTI0KAU) *数据集的预览。*

现在可视化它：

```py
f, ax = plt.subplots(1,1,figsize=(15,10))
gdf.plot(column = 'CERTAINTY', ax=ax)
```

![](img/afae17af9df7767171eae21ae9b0eb98.png)

[*Roman Road Network (version 2008)*](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi%3A10.7910%2FDVN%2FTI0KAU) *数据集的可视化。*

# 2\. 将道路网络转换为图形对象

上一张图显示了道路网络是一堆线状多边形。然而，为了能够量化例如罗马的重要性，我计划进行一些图计算。这意味着我需要将这些线字符串转换为图形。

[OSMNx](https://osmnx.readthedocs.io/en/stable/user-reference.html) 包正好适合这个需求——它处于地理空间数据工具与著名的图分析库 NetworkX 的交汇点。特别是，我跟随了 [这个帖子](https://stackoverflow.com/questions/65496981/osmnx-graph-from-gdfs-keyerror-x-when-converting-a-geopackage-to-a-graph) 从原始数据集中导出了节点和边表。

```py
# create an edge table
edges = gdf.copy()
edges['u'] = [str(g.coords[0][0]) + '_' + str(g.coords[0][1]) for g in edges.geometry.to_list()]
edges['v'] = [str(g.coords[-1][0]) + '_' + str(g.coords[-1][1]) for g in edges.geometry.to_list()]
edges_copy = edges.copy()
edges['key'] = range(len(edges))
edges = edges.set_index(['u', 'v', 'key'])
edges.head(3)
```

这个单元格的结果：

![](img/421a9fd392412bc14151312b42e3dd97.png)

边表的预览。

```py
import pandas as pd # version: 1.4.2
from shapely.geometry import Point # version: 1.7.1

# create a node table
nodes = pd.DataFrame(edges_copy['u'].append(edges_copy['v']), columns = ['osmid'])
nodes['geometry'] = [Point(float(n.split('_')[0]), float(n.split('_')[1])) for n in nodes.osmid.to_list()]
nodes['x'] = [float(n.split('_')[0]) for n in nodes.osmid.to_list()]
nodes['y'] = [float(n.split('_')[1]) for n in nodes.osmid.to_list()]
nodes = gpd.GeoDataFrame(nodes)
nodes = nodes.set_index('osmid')
nodes.head(3)
```

这个单元格的结果：

![](img/e4a19e068051b341b2e140c79a0049b4.png)

节点表的预览。

创建图形：

```py
import osmnx as ox # version: 1.0.1

# Now build the graph
graph_attrs = {'crs': 'epsg:4326', 'simplified': True}
G = ox.graph_from_gdfs(nodes, edges[[ 'geometry']], graph_attrs)
print(type(G))
print(G.number_of_nodes()), print(G.number_of_edges())
```

在这里，我成功地将 GIS 数据文件转换为一个包含 5122 个节点和 7154 条边的网络对象。现在，我想查看一下。也可以使用 NetworkX 可视化网络。然而，我更倾向于使用开源软件 [Gephi](https://gephi.org)。它提供了更多的灵活性和更好的视觉微调选项。让我们将 G 转换为 Gephi 兼容的文件并导出——在这个版本中，我将处理一个无权、无向的图。

```py
# Transform and export the graph
import networkx as nx # version: 2.5
G_clean = nx.Graph()
for u, v, data in G.edges(data=True):
    G_clean.add_edge(u, v)

G_clean2 = nx.Graph()
G_clean2.add_edges_from(G_clean.edges(data=True))

nx.write_gexf(G_clean2, 'roman_empire_network.gexf')
```

此外，我创建了一个名为 coordinates.csv 的数据表，在其中保存了道路网络中每个节点（交叉点）的坐标。

```py
nodes2 = nodes[nodes.index.isin(set(G.nodes))].drop(columns = ['geometry'])
nodes2.index.name = 'Id'
nodes2.to_csv('coordinates.csv')
```

# 3\. 在 Gephi 中可视化网络

在 Gephi 中可视化网络的具体操作值得单独讲解，因此在这里，我将展示结果。

在这个可视化中，每个节点对应一个交叉点，颜色编码了所谓的网络社区（密集互联的子图），而节点的大小则根据它们的中介中心性进行调整。中介中心性是一个网络中心性度量，量化了节点的桥接作用。因此，节点越大，它越中心。

在可视化中，还很有趣地观察地理如何驱动簇的形成，以及意大利如何意外地独立出来，可能是因为其内部道路网络更为密集。

![](img/ec0863f4c4ecc54761e71e3d75f8a184.png)

罗马帝国的道路网络。每个节点对应一个标记的交叉点，节点颜色编码网络社区，节点大小与它们的中介中心性成正比。

# 4\. 网络中心性

在欣赏完这些视觉效果后，让我们回到图本身并进行量化。在这里，我将计算每个节点的总度数，即它的连接数量，以及每个节点的未归一化的中介中心性，即计算经过每个节点的最短路径的总数。

```py
node_degrees = dict(G_clean2.degree)
node_betweenness = dict(nx.betweenness_centrality(G_clean2, normalized = False))
```

现在，我有了每个交叉点的重要性评分。此外，在*节点*表中，我们还有它们的位置——现在是时候回答主要问题了。为此，我量化了每个节点在罗马行政边界内的相对重要性。为此，我需要罗马的行政边界，这在 OSMnx 中相对容易获取（注意：今天的罗马可能与过去的罗马有所不同，但大致上应该没问题）。

```py
admin = ox.geocode_to_gdf('Rome, Italy')
admin.plot()
```

这个单元格的输出：

![](img/840a0c4be205eb0304eed02f03817e39.png)

罗马的行政边界。

此外，从视觉上看，罗马并不是作为道路网络中的一个单独节点存在；相反，许多节点在附近。因此，我们需要某种类型的分箱，空间索引，这帮助我们将所有属于罗马的道路网络节点和交叉口进行分组。此外，这种聚合也希望能够与帝国的其他区域进行比较。这就是为什么，我选择了 Uber 的[H3](https://www.uber.com/en-HU/blog/h3/) 六边形分箱，而不是仅仅将节点映射到罗马的行政区域，并创建六边形网格。然后，将每个节点映射到包围它的六边形中，并根据封闭网络节点的中心性得分计算该六边形的汇总重要性。最后，我将讨论最中心的六边形如何与罗马重叠。

首先，以近似的方式获取罗马帝国的行政区域：

```py
import alphashape # version:  1.1.0
from descartes import PolygonPatch

# take a random sample of the node points
sample = nodes.sample(1000)
sample.plot()

# create its concave hull
points = [(point.x, point.y) for point in sample.geometry]
alpha = 0.95 * alphashape.optimizealpha(points)
hull = alphashape.alphashape(points, alpha)
hull_pts = hull.exterior.coords.xy

fig, ax = plt.subplots()
ax.scatter(hull_pts[0], hull_pts[1], color='red')
ax.add_patch(PolygonPatch(hull, fill=False, color='green'))
```

该单元格的输出：

![](img/90850b325495b1544320452cd02ed8cb.png)

网络节点的子集和包围的凹形外壳。

让我们将帝国的多边形分割成六边形网格：

```py
import h3 # version: 3.7.3
from shapely.geometry import Polygon # version: 1.7.1
import numpy as np # version: 1.22.4

def split_admin_boundary_to_hexagons(polygon, resolution):
    coords = list(polygon.exterior.coords)
    admin_geojson = {"type": "Polygon",  "coordinates": [coords]}
    hexagons = h3.polyfill(admin_geojson, resolution, geo_json_conformant=True)
    hexagon_geometries = {hex_id : Polygon(h3.h3_to_geo_boundary(hex_id, geo_json=True)) for hex_id in hexagons}
    return gpd.GeoDataFrame(hexagon_geometries.items(), columns = ['hex_id', 'geometry'])

roman_empire = split_admin_boundary_to_hexagons(hull, 3)
roman_empire.plot()
```

结果：

![](img/5a82f64bb78bd82dbfb69501b4e38c5a.png)

罗马帝国的六边形网格。

现在，将道路网络节点映射到六边形中，并将中心性得分附加到每个六边形上。然后，我通过汇总每个六边形内节点的连接数和经过它们的最短路径数量来聚合每个节点的重要性：

```py
gdf_merged = gpd.sjoin(roman_empire, nodes[['geometry']])
gdf_merged['degree'] = gdf_merged.index_right.map(node_degrees)
gdf_merged['betweenness'] = gdf_merged.index_right.map(node_betweenness)
gdf_merged = gdf_merged.groupby(by = 'hex_id')[['degree', 'betweenness']].sum()
gdf_merged.head(3)
```

![](img/bdff5f3c67ff5eae83c3c8c561654656.png)

汇总六边形网格表的预览。

最终，将汇总的中心性得分与帝国的六边形地图结合起来：

```py
roman_empire = roman_empire.merge(gdf_merged, left_on = 'hex_id', right_index = True, how = 'outer')
roman_empire = roman_empire.fillna(0)
```

然后进行可视化。在这个视觉中，我还添加了空白网格作为基础地图，并根据道路网络节点的总重要性为每个网格单元着色。这样，着色将突出显示最关键的单元格为绿色。此外，我还添加了白色的罗马多边形。首先，用度着色：

```py
f, ax = plt.subplots(1,1,figsize=(15,15))

gpd.GeoDataFrame([hull], columns = ['geometry']).plot(ax=ax, color = 'grey', edgecolor = 'k', linewidth = 3, alpha = 0.1)
roman_empire.plot(column = 'degree', cmap = 'RdYlGn', ax = ax)
gdf.plot(ax=ax, color = 'k', linewidth = 0.5, alpha = 0.5)
admin.plot(ax=ax, color = 'w', linewidth = 3, edgecolor = 'w')
ax.axis('off')
plt.savefig('degree.png', dpi = 200)
```

结果：

![](img/7ef4612630cbd4fefa1253404df2f259.png)

罗马帝国的六边形地图，每个六边形根据封闭道路网络节点的总度数着色。

现在，用介于度着色：

```py
f, ax = plt.subplots(1,1,figsize=(15,15))

gpd.GeoDataFrame([hull], columns = ['geometry']).plot(ax=ax, color = 'grey', edgecolor = 'k', linewidth = 3, alpha = 0.1)
roman_empire.plot(column = 'betweenness', cmap = 'RdYlGn', ax = ax)
gdf.plot(ax=ax, color = 'k', linewidth = 0.5, alpha = 0.5)
admin.plot(ax=ax, color = 'w', linewidth = 3, edgecolor = 'w')
ax.axis('off')
plt.savefig('betweenness.png', dpi = 200, bbox_inches = 'tight')
```

![](img/91816155619376b4cf17186dd1e87c51.png)

罗马帝国的六边形地图，每个六边形根据封闭道路网络节点的总最短路径（介于度）着色。

最终，我们得出一个令人安心的结论。如果根据累计度数着色六边形单元格，罗马的区域遥遥领先。如果根据介于度着色六边形，图像类似——罗马再次占据主导地位。这里的一个附加点是，连接罗马与中东的高速公路也作为一个关键的高介于度段显现出来。

**tl;dr 网络科学也表明所有道路都通向罗马！**
