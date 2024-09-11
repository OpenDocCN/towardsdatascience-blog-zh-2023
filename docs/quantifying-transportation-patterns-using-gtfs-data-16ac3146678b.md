# 使用 GTFS 数据量化交通模式

> 原文：[https://towardsdatascience.com/quantifying-transportation-patterns-using-gtfs-data-16ac3146678b?source=collection_archive---------3-----------------------#2023-12-04](https://towardsdatascience.com/quantifying-transportation-patterns-using-gtfs-data-16ac3146678b?source=collection_archive---------3-----------------------#2023-12-04)

![](../Images/3e31e03dfd6c7f5378c6c47506ddbf85.png)

## 在这篇文章中，我探讨了依托于通用交通信息规范（General Transit Feed Specification）和各种空间数据科学工具的四个精选城市的公共交通系统。

[](https://medium.com/@janosovm?source=post_page-----16ac3146678b--------------------------------)[![Milan Janosov](../Images/77b62460041f66ec4585a81baef81a03.png)](https://medium.com/@janosovm?source=post_page-----16ac3146678b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----16ac3146678b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----16ac3146678b--------------------------------) [Milan Janosov](https://medium.com/@janosovm?source=post_page-----16ac3146678b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F838408aa2ad4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquantifying-transportation-patterns-using-gtfs-data-16ac3146678b&user=Milan+Janosov&userId=838408aa2ad4&source=post_page-838408aa2ad4----16ac3146678b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----16ac3146678b--------------------------------) ·12分钟阅读·2023年12月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F16ac3146678b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquantifying-transportation-patterns-using-gtfs-data-16ac3146678b&user=Milan+Janosov&userId=838408aa2ad4&source=-----16ac3146678b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F16ac3146678b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquantifying-transportation-patterns-using-gtfs-data-16ac3146678b&source=-----16ac3146678b---------------------bookmark_footer-----------)

我在这个笔记本中选择了四个城市：布达佩斯、柏林、斯德哥尔摩和多伦多，使用公开可用的GTFS（通用交通信息规范）数据概述它们的公共交通系统。这个笔记本旨在作为一个入门教程，介绍如何使用Pandas、GeoPandas和其他标准数据科学工具访问、操作、聚合和可视化公共交通数据，以获取有关公共交通的见解。后来，这种理解在各种应用场景中都可能有帮助，如交通、城市规划和位置智能。

此外，尽管GTFS格式应具有通用性和普遍性，我也会指出在接下来的分析步骤中仍需逐一进行城市级别的见解和人工验证的情况。

*在这篇文章中，所有图像均由作者创建。*

# 1\. 收集和解析GTFS数据

对于这篇文章，我从Transitfeeds.com下载了公共交通数据，这是一个公共交通数据的在线汇总网站。特别是，我下载了以下城市的最新更新时间的数据：

+   [布达佩斯](https://transitfeeds.com/p/bkk/42)（2023年9月）

+   [柏林](https://transitfeeds.com/p/verkehrsverbund-berlin-brandenburg/213)（2021年5月）

+   [斯德哥尔摩](https://transitfeeds.com/p/storstockholms-lokaltrafik/1086)（2020年2月）

+   [多伦多](https://transitfeeds.com/p/ttc/33)（2020年10月）

在接下来的代码块中，我将多次探索这些城市，创建比较图，并强调GTFS格式的通用性。此外，为了确保我的分析可以轻松更新到最新的数据转储，我将每个城市的GTFS数据存储在一个对应于更新日期的文件夹中：

```py
import os

root = 'data'
cities = ['Budapest', 'Toronto', 'Berlin', 'Stockholm']
updated = {city : [f for f in os.listdir(root + '/' + city) if '20' in f][0] for city in cities}

updated
```

此单元格的输出：

![](../Images/85025766848359c06e5112cf1da76e92.png)

我的GTFS数据转储的确切记录时间。

现在，让我们更详细地查看这些文件夹中存储的不同文件：

```py
for city in cities:
    print(len(os.listdir(root + '/' + city + '/' + updated[city])), sorted(os.listdir(root + '/' + city + '/' + updated[city])), '\n')
```

此单元格的输出：

![](../Images/06449b79f2383f9ca78850a749ad3c4b.png)

不同城市GTFS数据转储中的文件列表。

看起来所有这些文件中都有八个文件，类似于GTFS结构的核心部分。

虽然你可以在[这里](/where-is-the-bus-gtfs-will-tell-us-f8adc18a2f8e)阅读更多关于GTFS结构的内容，但现在我将继续讨论可能最基本的地理空间特征——公共交通站点的位置。

# 2\. 公共交通站点位置

首先，我创建了一个示例可视化，解析了布达佩斯的stops.txt文件。然后，我使用[Shapely](https://shapely.readthedocs.io)和GeoPandas，通过使用每个站点位置的经纬度坐标创建Point几何体，创建一个几何数据结构，即GeoDataFrame。接着，使用[GeoPandas](http://geopandas.org)内置的基于Matplotlib的绘图工具，我创建了一个简单的地图。此外，我还使用[OSMNx](http://osmnx.readthedocs.io)将城市边界添加为一个封闭的多边形。

```py
import geopandas as gpd
from shapely.geometry import Point
import matplotlib.pyplot as plt
import pandas as pd
import osmnx as ox

city = 'Budapest'

df_stops = pd.read_csv(root + '/' + city + '/' + updated[city] + '/stops.txt')
geometry = [Point(xy) for xy in zip(df_stops['stop_lon'], df_stops['stop_lat'])]
gdf_stops = gpd.GeoDataFrame(df_stops, geometry=geometry)
df_stops.crs = 'EPSG:4326'

admin = ox.geocode_to_gdf(city)

f, ax = plt.subplots(1,1,figsize=(10,10))

admin.plot(ax=ax, color = 'none', edgecolor = 'k')
gdf_stops.plot(ax=ax, alpha = 0.2)
```

结果图：

![](../Images/2de2f7a180101354a1ae3af0f7ff9aa0.png)

布达佩斯公共交通停靠点的位置。

起初看起来不错！绝大多数停靠点位置都在城市内；只有少数通勤线路位于城市之外。此外，在图的中心，我们可以清楚地看到多瑙河流经的地方没有停靠点。现在，让我们看看所有城市的这张图！

```py
stops = {}
admin = {}

f, ax = plt.subplots(2,2,figsize=(15,15))
indicies = [(i,j) for i in range(2) for j in range(2)]

for idx, city in enumerate(cities):

    bx = ax[indicies[idx]]

    df_stops = pd.read_csv(root + '/' + city + '/' + updated[city] + '/stops.txt')
    geometry = [Point(xy) for xy in zip(df_stops['stop_lon'], df_stops['stop_lat'])]
    gdf_stops = gpd.GeoDataFrame(df_stops, geometry=geometry)
    gdf_stops.crs = 'EPSG:4326'
    admin_ = ox.geocode_to_gdf(city)

    gdf_stops.plot(ax=bx, markersize = 3, alpha = 0.5)
    admin_.plot(ax=bx, color = 'none', edgecolor = 'k', linewidth = 4)
    bx.set_title(city, fontsize = 15)
    bx.axis('off')

    stops[city] = gpd.overlay(gdf_stops, admin_)
    admin[city] = admin_

plt.tight_layout()
```

![](../Images/747570e008067e45a9da15d10f803037.png)

四个目标城市的公共交通停靠点位置——未过滤。

既然我们已经研究了五个城市，我们可以对GTFS和公共交通数据的一般性得出更多的结论。一方面，从技术角度来看，这些数据在城市和地方交通机构之间确实非常相似。另一方面，当将数据添加到管道中时，必须单独检查每个城市，因为如斯德哥尔摩和多伦多的案例所示，与城市相关的数据实际上属于更大的，例如市政级别的区域。好消息是，例如，添加来自OpenStreetMap的行政边界，使得将数据过滤到实际城市变得容易！

实际上，我在前一个代码块的倒数第二行进行了最后的过滤步骤，并将城市级别的停靠点位置存储在*stops*字典中。

```py
f, ax = plt.subplots(2,2,figsize=(15,15))
indicies = [(i,j) for i in range(2) for j in range(2)]

for idx, (city, admin_) in enumerate(admin.items()):

    bx = ax[indicies[idx]]    
    gdf_stops = stops[city]
    gdf_stops.plot(ax=bx, markersize = 3, alpha = 0.5)
    admin_.plot(ax=bx, color = 'none', edgecolor = 'k', linewidth = 4)
    bx.axis('off')

plt.tight_layout() 
```

![](../Images/2b4e48221c33319cc3884f945a974aef.png)

四个目标城市的公共交通停靠点位置——过滤后。

# 3\. 出发时间

首先，我们探讨了停靠点的位置；现在，让我们看看何时应该接近这些点。另外，假设公共交通服务对需求和居民的时间表及习惯有显著见解，我们也可以从这些时间表的时间模式中获取一些见解。

```py
from datetime import datetime

def parse_time_string(time_string):
    hour_value = int(time_string.split(':')[0])
    if hour_value > 23: hour_value = hour_value-24
    time_string = str(hour_value) + ':' + time_string.split(':', 1)[1]    
    parsed_time = datetime.strptime(time_string, "%H:%M:%S")
    return parsed_time

stop_times = {}
for city in cities:
    print(city)
    df_stop_times = pd.read_csv(root + '/' + city + '/' + updated[city] + '/stop_times.txt')
    df_stop_times['departure_time'] = df_stop_times['departure_time'].apply(parse_time_string)
    stop_times[city] = df_stop_times

for city, df_stop_times in stop_times.items():
    print(city, len(df_stop_times))

df_stop_times.head(3)
```

这个代码块的输出：

![](../Images/3c0ee44bb53b1bae98a031b8dc131c4a.png)

每个城市的停靠点数量及其时间表的样本。

在读取、解析和转换出发时间信息后，让我们创建一系列关于每个城市城市级别出发时间分布的视觉图：

```py
f, ax = plt.subplots(4,1,figsize=(15,20))

for idx, (city, df_stop_times) in enumerate(stop_times.items()):

    bx = ax[idx]

    df_stop_times = df_stop_times

    df_stop_times['hour_minute'] = df_stop_times['departure_time'].dt.strftime('%H:%M')

    departure_counts = df_stop_times['hour_minute'].value_counts().sort_index()

    departure_counts.plot(kind='bar', color='steelblue', alpha = 0.8, width=0.8, ax = bx)
    bx.set_xlabel('Hour-Minute of Departure', fontsize = 18)
    bx.set_ylabel('Number of Departures', fontsize = 18)
    bx.set_title('Histogram of Departure Times by Hour-Minute in ' + city, fontsize = 26)

    bx.set_xticks([ijk for ijk, i in enumerate(departure_counts.index) if ':00' in i])
    bx.set_xticklabels([i for i in departure_counts.index if ':00' in i])

plt.tight_layout()
```

这个代码块的输出图像：

![](../Images/58b4c40d879891704e5c36e6a22a8d33.png)

每个目标城市的每日出发频率。

虽然对这些图表进行适当、详细的解释可能需要一些严肃的地方城市规划知识，但也可以根据个人经验和常识进行一些推测。

首先，考虑到这些城市气候、文化背景和昼夜节律的差异，我对它们在早上8点的对齐程度感到惊讶。布达佩斯、柏林和斯德哥尔摩都有几乎相同的起床模式，而有趣的是，多伦多的起床时间稍长一些。

其次，下午结束的时间似乎有一个明确的顺序——柏林最早结束，其次是布达佩斯、多伦多和斯德哥尔摩。一个有趣的问题是这些时间是否标志着工作班次的结束或人们回家的时间。

第三，布达佩斯和斯德哥尔摩的早晚高峰明显可区分，而柏林和多伦多则不那么明显，这可能与这些城市的中心化程度有关。

# 4\. 空间分布

对于表格数据、浮点数/整数变量和空间信息，计算一般趋势是可能的。这个过程被称为空间索引。[空间索引](https://postgis.net/workshops/postgis-intro/indexing.html)在实践中意味着我们将一个区域，例如城市的行政边界，划分成规则的网格。我的个人最爱是Uber的[H3六边形](https://www.uber.com/en-HU/blog/h3/)网格——你可以在这里了解更多。使用这个网格，我们可以通过计算每个网格单元格中的停靠点数量来高效地进行空间聚合！让我们尝试不同的六边形分辨率。

```py
import geopandas as gpd # version: 0.13.1
import h3 # version: 3.7.3
from shapely.geometry import Polygon # version: 1.8.5.post1
import numpy as np # version: 1.22.4

# use this function to split an admin polygon into a hexagon grid
def split_admin_boundary_to_hexagons(admin_gdf, resolution):
    coords = list(admin_gdf.geometry.to_list()[0].exterior.coords)
    admin_geojson = {"type": "Polygon",  "coordinates": [coords]}
    hexagons = h3.polyfill(admin_geojson, resolution, geo_json_conformant=True)
    hexagon_geometries = {hex_id : Polygon(h3.h3_to_geo_boundary(hex_id, geo_json=True)) for hex_id in hexagons}
    return gpd.GeoDataFrame(hexagon_geometries.items(), columns = ['hex_id', 'geometry'])

# let's test two resolutions for Budapest
hexagons_gdf_h8 = split_admin_boundary_to_hexagons(admin['Budapest'], 8)
hexagons_gdf_h9 = split_admin_boundary_to_hexagons(admin['Budapest'], 9)
hexagons_gdf_h8.plot()
hexagons_gdf_h9.plot() 
```

![](../Images/a6412debbedd0822a3bda735ff15fba2.png)

布达佩斯——六边形网格。

*技术说明：* 当我在处理下一个单元时，我遇到了一个错误——即，柏林的行政边界是一个多边形，而所有其他城市的行政区域都是简单的多边形，这也是我的 *split_admin_boundary_to_hexagons* 函数期望的输入。因此，我检查了一下，结果发现由于某种原因，存在一个额外的小多边形，面积几乎为零，所以我不得不通过运行以下命令来清理它：

```py
admin['Berlin'] = gpd.GeoDataFrame(admin['Berlin'].geometry.explode(), columns = ['geometry']).head(1)
```

现在我们已经有了H3网格构建的工作原型，让我们计算四个示例城市中每个六边形的停靠点数量和停靠次数。为了计算停靠点位置的数量，我们只需进行空间连接；但要计算停靠次数，我们还需要结合出发时间。这样，我们也使空间和时间维度得以结合！

```py
from matplotlib.colors import LogNorm

resolution = 7

for city in cities:

    # create the hexagon grid
    hexagons_gdf = split_admin_boundary_to_hexagons(admin[city], resolution)

    # merge stops and stopppings
    gdf_stop_times = stops[city].merge(stop_times[city], left_on = 'stop_id',right_on = 'stop_id')

    # compute the number of unique stops and stoppings in each hexagon
    gdf_stops = gpd.sjoin(gdf_stop_times, hexagons_gdf)
    nunique = gdf_stops.groupby(by = 'hex_id').nunique().to_dict()['stop_id']
    total = gdf_stops.groupby(by = 'hex_id').count().to_dict()['stop_id']
    hexagons_gdf['nunique'] = hexagons_gdf.hex_id.map(nunique).fillna(0)
    hexagons_gdf['total'] = hexagons_gdf.hex_id.map(total).fillna(0)

    # visualize the number of stops and stoppings
    f, ax = plt.subplots(1,2,figsize=(15,5))
    plt.suptitle(city + ', resolution = ' + str(resolution), fontsize=25)
    hexagons_gdf.plot(column = 'nunique', cmap = 'RdYlGn', legend = True, ax = ax[0])
    hexagons_gdf.plot(column = 'total', cmap = 'RdYlGn', legend = True, ax = ax[1])
    # for log-saled coloring:
    # hexagons_gdf.plot(column = 'total', cmap = 'RdYlGn', legend = True, ax = ax[1], norm=LogNorm(vmin=1, vmax = hexagons_gdf.total.max()))
    ax[0].set_title('Number of unique stops', fontsize = 17)
    ax[1].set_title('Number stoppings', fontsize = 17)
    for aax in ax: aax.axis('off') 
```

让我们看看这是什么样的：

![](../Images/274d57daa309d65807db12bcfc34a4f0.png)![](../Images/d1c325125551bf7d5f64c552479301a0.png)![](../Images/0414bdca9f90d60f2e4ad55f10a31b51.png)

# 5\. 不同的交通方式

现在，我们已经看到时间和空间中的聚合趋势。接下来，让我们放大并提取与数据记录相对应的不同交通方式。这些信息通常存储在 *routes.txt* 中，在 *route_type* 列下。[这个](https://developers.google.com/transit/gtfs/reference/extended-route-types)、[这个](https://developers.google.com/transit/gtfs/reference) 和 [这个](https://www.transit.land/routes/r-u0wj-717) 映射可以将交通方式代码编码为其英文名称。

基于这些，我创建了官方地图和一个简化版本，稍后我会使用。在简化版本中，我将例如‘电车、街车、轻轨’（代码0）和‘电车服务’（代码900）两个类别都重命名为‘电车’。

```py
map_complete = { 0   : 'Tram, Streetcar, Light rail', 
                 1   : 'Subway, Metro', 
                 2   : 'Rail',
                 3   : 'Bus', 
                 4   : 'Ferry', 
                 11  : 'Trolleybus', 
                 100 : 'Railway Service',
                 109 : 'Suburban Railway',
                 400 : 'Urban Railway Service', 
                 401 : 'Metro Service', 
                 700 : 'Bus Service', 
                 717 : 'Regional Bus',
                 900 : 'Tram Service', 
                 1000: 'Water Transport Service'}

map_simple = { 0    : 'Tram', 
               1    : 'Subway', 
               2    : 'Railway',
               3    : 'Bus', 
               4    : 'Ferry', 
               11   : 'Trolleybus', 
               100  : 'Railway',
               109  : 'Railway',
               400  : 'Railway', 
               401  : 'Subway', 
               700  : 'Bus',
               717  : 'Bus',
               900  : 'Tram', 
               1000 : 'Ferry', } 
```

现在，直观地展示每种交通方式的频率，测量方式是通过可能的路线数量：

```py
from collections import Counter
import matplotlib.pyplot as plt

# this function does some nice formatting on the axis and labels
def format_axis(ax):   
    for pos in ['right', 'top']:   ax.spines[pos].set_edgecolor('w')    
    for pos in ['bottom', 'left']: ax.spines[pos].set_edgecolor('k')         
    ax.tick_params(axis='x', length=6, width=2, colors='k')
    ax.tick_params(axis='y', length=6, width=2, colors='k') 
    for tick in ax.xaxis.get_major_ticks():  tick.label.set_fontsize(12) 
    for tick in ax.yaxis.get_major_ticks():  tick.label.set_fontsize(12)

f, ax = plt.subplots(1,4,figsize = (15,4))
routes = {}

for idx, city in enumerate(cities):

    # get the data, map the english names
    df_route = pd.read_csv(root + '/' + city + '/' + updated[city] + '/routes.txt')
    df_route['route_type'] = df_route['route_type'].astype(int)
    df_route['route_type_en'] = df_route['route_type'].map(map_simple)
    D = dict(Counter(df_route.route_type_en))
    routes[city] = df_route

    # define my color palette
    transport_colors = {'Railway': '#A65C47',  
                        'Bus': '#0BB3D9',      
                        'Tram': '#F2B705',     
                        'Ferry': '#997CA6'   ,  
                        'Trolleybus' : '#D91818',
                        'Subway' : '#0869A6'}

    # create the bar charts
    labels = D.keys()
    values = D.values()
    colors = [transport_colors[l] for l in labels]

    ax[idx].bar(labels, values, color = colors)
    ax[idx].set_xticklabels(labels, fontsize = 10, rotation = 60, ha = 'right')
    format_axis(ax[idx])
    ax[idx].set_title(city, fontsize = 18)
    ax[idx].set_ylabel('Number of routes', fontsize = 15)
    ax[idx].set_yscale('log')

plt.tight_layout()
```

这个代码块输出了以下图形：

![](../Images/cc520c787039984f4f7e3b03e199a53a.png)

研究城市间交通模式的分布。

# 6. 路线形状

在进行时空聚合和交通工具缩放后，最后但同样重要的是，我想强调我最喜欢的部分——记录每条路线形状的GPS点序列。

以这种方式访问：

```py
city = 'Budapest'
df_routes = pd.read_csv(root + '/' + city + '/' + updated[city] + '/shapes.txt')
df_routes
```

本单元的输出：

![](../Images/d6f099dff9979c770c822ef0d59c404d.png)

布达佩斯的路线形状表。

现在，将布达佩斯的路线表转换为由LineStrings组成的GeoDataFrame，添加上一节的路线类型映射，并使用连接路线（和交通工具）到形状的tripst.txt GTFS文件。

结果将是一个公共交通道路网络的地理地图，我们可以根据前一部分的颜色编码来上色，例如，按照交通工具的不同颜色进行标记。

```py
import contextily as ctx

for city in cities:

    df_shapes = pd.read_csv(root + '/' + city + '/' + updated[city] + '/shapes.txt')

    # transform the shape a GeoDataFrame
    df_shapes['shape_pt_lat'] = df_shapes['shape_pt_lat'].astype(float)
    df_shapes['shape_pt_lon'] = df_shapes['shape_pt_lon'].astype(float)

    df_shapes['geometry'] = df_shapes.apply(lambda row: Point(row['shape_pt_lon'], row['shape_pt_lat']), axis=1)
    df_shapes = gpd.GeoDataFrame(df_shapes[['shape_id', 'geometry']])
    df_shapes.crs = 4326

    gdf_shapes = gpd.GeoDataFrame(df_shapes[['shape_id', 'geometry']].groupby(by = 'shape_id').agg(list))
    gdf_shapes = gdf_shapes[[len(g)>1 for g in gdf_shapes['geometry'].to_list()]]
    gdf_shapes['geometry'] = gdf_shapes['geometry'].apply(lambda x: LineString(x))
    gdf_shapes = gpd.GeoDataFrame(gdf_shapes)

    # let's only keep those routes which have at least one segment that falls into the city
    gdf_shapes['shape_id'] = gdf_shapes.index
    shapes_in_city = set(gpd.overlay(gdf_shapes, admin[city]).shape_id.to_list())
    gdf_shapes = gdf_shapes[gdf_shapes.shape_id.isin(shapes_in_city)]

    # map the means of transport
    df_route = routes[city][['route_id', 'route_type_en']].drop_duplicates()
    df_trips = pd.read_csv(root + '/' + city + '/' + updated[city] + '/trips.txt')[['route_id', 'shape_id']].drop_duplicates()
    df_trips = df_trips.merge(df_route, left_on = 'route_id', right_on = 'route_id')

    # merge these
    gdf_shapes = gdf_shapes.merge(df_trips, left_index = True, right_on = 'shape_id')

    # create the visuals in matplotlib
    f, ax = plt.subplots(1,1,figsize=(15,15))

    cax = admin[city].plot(ax=ax, edgecolor = 'k', color = 'none')
    cax = admin[city].plot(ax=ax, edgecolor = 'k', alpha = 0.52)
    for transport, color in transport_colors.items():
        gdf_shapes[gdf_shapes.route_type_en==transport].plot(ax=ax, color = color, alpha = 0.9, linewidth = 1)    
    ax.axis('off')

    ctx.add_basemap(ax, alpha = 0.8, crs = 4326, url = ctx.providers.CartoDB.DarkMatterNoLabels)

    plt.tight_layout()
    plt.savefig('figure7_'+city+ '.png', dpi = 150, bbox_inches = 'tight')
    plt.close()
```

每个城市的结果图：

![](../Images/859a53b44d159e6af702bc4be5595ce1.png)

布达佩斯——仅有少数线路延伸至城市聚集区之外。

![](../Images/0fed29400890a94ec8af880e4c494f0f.png)

多伦多——拥有高度集中的公共交通网络。

![](../Images/e688045f1afc5924e863bc587b71493c.png)

柏林——展示了其铁路线路如何从城市开始并交织于乡村。

![](../Images/3e31e03dfd6c7f5378c6c47506ddbf85.png)

斯德哥尔摩——其公共交通网络从城市出发，但扩展到乡村。

# 结论

在这篇文章中，我简要概述了探索GTFS数据所需的技术细节，并以流线型的方式探索不同城市的公共交通系统。虽然所有这些城市使用的通用格式使得从一个城市到另一个城市的分析任务变得非常方便和易于采用，但我的发现和结果也突出了由于不同城市定义和术语的细微差别，逐个城市级别的验证和合理性检查的重要性。

最后，如果你想回到过去，查看类似的交通相关分析回溯到古罗马，请查看我之前的TDS文章，“[所有道路都通往罗马吗？](/do-all-the-roads-lead-to-rome-5b6756ce7d52)”。
