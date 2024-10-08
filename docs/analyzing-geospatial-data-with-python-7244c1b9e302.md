# 使用 Python 分析地理空间数据

> 原文：[`towardsdatascience.com/analyzing-geospatial-data-with-python-7244c1b9e302`](https://towardsdatascience.com/analyzing-geospatial-data-with-python-7244c1b9e302)

## 一篇实用的数据分析文章，包含 Python 代码。

[](https://gustavorsantos.medium.com/?source=post_page-----7244c1b9e302--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----7244c1b9e302--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7244c1b9e302--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7244c1b9e302--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----7244c1b9e302--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7244c1b9e302--------------------------------) ·阅读时间 8 分钟·2023 年 8 月 19 日

--

# 简介

地理空间数据科学是我感兴趣的领域之一。我觉得将数据可视化在地图上的方式非常迷人，而且——很多时候——数据点之间的关系能够迅速提供深刻的洞见。

我相信这一数据科学子领域的应用对任何业务都非常有用，例如杂货店、汽车租赁、物流、房地产*等等*。在这篇文章中，我们将探讨来自*AirBnb*的一个数据集，地点是美国北卡罗来纳州的阿什维尔市。

附注：在那个城市里有一处美国最惊人的房地产之一——*我敢说是世界上最惊人的*。这处房产属于范德比尔特家族，曾经是国家上最大的私人财产。嗯，值得一游览，但这不是本文的核心主题。

![](img/bd00be96cce8bdcd4578ea74975fb8ad.png)

位于北卡罗来纳州阿什维尔的比尔特莫尔庄园。照片由[Stephanie Klepacki](https://unsplash.com/@sklepacki?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，[Unsplash](https://unsplash.com/photos/60IeQ4lmDGs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供。

本练习使用的数据集为阿什维尔市的 AirBnb 出租数据。可以直接从[`insideairbnb.com/get-the-data`](http://insideairbnb.com/get-the-data)网站下载，符合[知识共享署名 4.0 国际许可协议](http://creativecommons.org/licenses/by/4.0/)。

让我们开始工作吧。

# 地理空间数据科学

本文的知识大多来源于下述书籍（《使用 Python 进行应用地理空间数据科学》，作者：David S. JORDAN）。所以，让我们开始导入一些模块到我们的会话中吧。

```py
import pandas as pd
import geopandas as gpd
import matplotlib.pyplot as plt
import pysal
import splot
import re
import seaborn as sns
import folium

# For points map
import geoplot.crs as gcrs
import geoplot as gplt
```

现在注意，有些数据点对你来说可能是新的，就像对我一样。如果需要，使用 `pip install module_name` 安装所需的任何软件包。在我的情况下，`pysal` 和 `geoplot` 对我来说是新的，所以它们必须被安装。

接下来，我们将从 AirBnb 读取数据。

```py
# Open listings file
listings = pd.read_csv('/content/listings.csv',
                       usecols=['id', 'property_type', 'neighbourhood_cleansed',
                                'bedrooms', 'beds', 'bathrooms_text', 'price',
                                'latitude','longitude'])
#listings.columns
listings.sample(4)
```

![](img/30da60e4caf9a9a65a89dd66d58eaed7.png)

阿什维尔，NC 的 AirBnb 列表。图像由作者提供。

如果我们执行 `listings.info()`，我们会看到 `price` 变量被识别为对象类型。这是因为数字前面有美元符号 `$`。这在 Python 中很容易纠正。

```py
# Correct Price to Float (Replace $ and , with nothing)
listings['price'] = (listings['price']
                     .replace("[$,]", "", regex=True)
                     .astype(float)
                     )
```

## 探索性分析

现在我们可以检查该变量的统计信息。

```py
#price stats
listings.price.describe()

count    3239.000000
mean      179.771843
std       156.068212
min        14.000000
25%        95.000000
50%       135.000000
75%       212.500000
max      2059.000000
Name: price, dtype: float64
```

有趣。平均价格接近 $180，但中位数为 $135。 这表明我们可能有一些高值扭曲了分布，使其向右偏斜。为了确认这一点，我们可以检查价格范围。

```py
# Check price range
sns.displot(listings['price'], kde=True);
```

![](img/d81e6bb10d643ab098122644ba002317.png)

阿什维尔，NC 列表的价格。图像由作者提供。

正如预期的那样，我们的数据大多数集中在 $500 左右。其余的大部分是异常值，数量很少。我们可以通过运行代码 `np.quantile(listings[‘price’], 0.97)` 并检查第 97 百分位数是 $538 美元来确认这一点。

非常好。

下一步是开始将数据绘制到地图上。让我们可视化数据点并开始绘制一些洞察。为此，我们首先需要将 Pandas 数据框转换为 Geopandas。

```py
# Convert Pandas df to Geopandas df
listings_gpd = gpd.GeoDataFrame(listings,
                                     geometry=gpd.points_from_xy(listings.longitude, listings.latitude, crs=4326))

# Look at the geometry variable created
listings_gpd.head(2)
```

查看创建的数据集，可以观察到它现在带有一个 `geometry` 变量。

![](img/7a1bcad732d941d73fadf485973439c4.png)

由 geopandas 创建的‘geometry’变量。图像由作者提供。

完成后，使用 geoplot 绘制点是很简单的。我们只使用了 $538 以下的值，这样颜色看起来分布更均匀，否则数据的偏斜会使点变成一大块紫色混合。

```py
# Points map using geoplot
ax = gplt.webmap(listings_gpd.query('price < 538'), projection=gcrs.WebMercator())
gplt.pointplot(listings_gpd.query('price < 538'), ax=ax, hue= 'price', legend=True);
```

![](img/9d289ad81cada770004c0db11c9668ec.png)

数据点：来自北卡罗来纳州阿什维尔的 AirBnb 列表。图像由作者提供。

好的。从这里开始，我们可以看到一些不错的东西：

+   大多数列表（如预期的那样）都在平均值附近浮动。

+   I-40 高速公路沿线有租赁物业的集中区域。

+   深蓝色和浅蓝色点混合得很好，显示价格在 100 到 200 美元之间。

+   红点的出现非常稀疏，所以在阿什维尔找不到非常贵的租赁房源应该不是很常见。

## 热图

接下来，我认为我们可以开始创建热图。

JORDAN 的书中展示的一个选项是使用 geoplot。我们可以从 *insideairbnb.com* 下载 geojson 文件，并用它来创建热图。

在这里，我们将使用 geopandas 读取文件，并将其转换为 4326 坐标系统，这是 GPS 的标准。

```py
# Reading the Asheville polygon shapefile (geojson)
geofile = '/content/neighbourhoods.geojson'
asheville = gpd.read_file(geofile)
asheville = asheville.to_crs(4326)
```

然后，使用这些几行代码创建热图非常简单。首先，我们创建一个密度图（kde）以便在地图上投影，然后使用 polyplot 显示这两个图层。

```py
# Heatmap
ax = gplt.kdeplot(listings_gpd,
          fill=True, cmap='Reds',
          clip=asheville.geometry,
          projection=gcrs.WebMercator())

# Plotting the heatmap on top of the geometry
gplt.polyplot(asheville, ax=ax, zorder=1);
```

这是漂亮的结果。列表在城市中心区域非常集中。

![](img/64c2401869ec8d25204f8dffe827dd07.png)

**阿什维尔**列表的热图。图像由作者提供。

继续，我们将使用`Folium`模块创建按区域划分的价格热图和一个**分层地图**。

首先是热图。考虑到我们希望看到不同颜色的数据点，按价格范围分组，这里有一个代码来创建这些组。首先我们使用`pandas.cut`为每 100 美元创建区间，然后使用`map()`将标签转换为颜色。这将在我们的下一步中使用。

```py
from numpy import Inf
# Create clip levels for prices
listings['price_bins']= pd.cut(listings.price,
       bins= [-Inf, 100, 200, 300, 400, 500, Inf],
       labels= ['0-100', '100-200', '200-300', '300-400', '400-500', '500+'])

# Create bin colors
listings['colors'] = listings.price_bins.map({'0-100': 'lightblue', '100-200':'blue', '200-300':'gold', '300-400':'orange', '400-500':'red', '500+':'black'})
```

我们来创建一个基础地图，起点为北卡罗来纳州**阿什维尔**市中心。

```py
# Creating a base map
m = folium.Map(location= [35.5951, -82.5515], zoom_start=10)
```

然后我们可以添加点。

```py
# Adding the points
for lat, lon, ptcolor in zip(listings.latitude, listings.longitude, listings.colors):
  folium.CircleMarker(
     location=[lat, lon],
     radius=2,
     opacity=0.5,
     color=ptcolor,
     fill=True,
     fill_color=ptcolor,
  ).add_to(m)
```

现在我们将创建热图。我们必须将数据转换为值的列表。仅包括纬度、经度和价格。热图将按价格显示。然后，我们导入`HeatMap from folium.plugins`并将其添加到基础地图中。

```py
from folium import plugins

# Preparing data for plot
data = listings[['latitude','longitude', 'price']].values
data =data.tolist()

# Create Heat Map with Folium
hm = plugins.HeatMap(data,gradient={0.1: 'blue', 0.2: 'lime', 0.4: 'yellow', 0.6: 'orange', 0.9: 'red'}, 
                min_opacity=0.1, 
                max_opacity=0.9, 
                radius=20,
                use_local_extrema=False)

# Add to base map
hm.add_to(m);

# Display
m
```

这是结果。一个漂亮的热图，展示了有价值的见解。

![](img/b5e417a0d95a75f59db68bff44a5c57b.png)

阿什维尔 AirBnb 列表的热图。图像由作者提供。

看看市中心附近的价格更高。而在比尔特莫景点附近，列表并不那么密集。那里有一些，价格相对较低，可能是因为离市中心较远。

![](img/990fb2951787da3dc81426fee9b85c9b.png)

市中心与比尔特莫周边。图像由作者提供。

## **分层地图**

最后，如果我们想从这些数据创建一个快速的**分层地图**，以下是代码片段。我们可以使用之前创建的`asheville`文件作为我们的地理数据，`listings`文件是价格来源，`neighbourhood`变量是 geojson 与数据框之间的链接。

```py
# Add a choropleth layer
folium.Choropleth(
    geo_data=asheville,
    name="choropleth",
    data=listings,
    columns=["neighbourhood", "price"],
    key_on="feature.properties.neighbourhood",
    fill_color="RdBu_r",
    fill_opacity=0.5,
    line_opacity=0.5,
    legend_name="Prices",
).add_to(m)
m
```

![](img/e119b49728226f8ab4556e6eb02c27f3.png)

阿什维尔 AirBnb 列表的**分层地图**。图像由作者提供。

从地图上看，我们看到最高的价格在右侧。如果你不熟悉这个地区，阿什维尔是一个位于**蓝岭山脉**旁边的城市，是北卡罗来纳州一个著名而美丽的地方，特别是在秋季。*蓝岭公路*是美国最著名的公路之一，经常被提及为最美的驾车路线之一。所以让我们绘制另一个**分层地图**，这次启用地形模式，然后我们可以看到山脉的位置。

```py
# Base map with Terrain mode
m = folium.Map(location= [35.5951, -82.5515], zoom_start=10, tiles="Stamen Terrain")

# Add a choropleth layer to a terrain map
folium.Choropleth(
    geo_data=asheville,
    name="choropleth",
    data=listings,
    columns=["neighbourhood", "price"],
    key_on="feature.properties.neighbourhood",
    fill_color="RdBu_r",
    fill_opacity=0.5,
    line_opacity=0.5,
    legend_name="Prices",
).add_to(m)
m
```

红线是蓝岭公路的位置。

![](img/d95a5ce09860c9ecbad03976e7df2311.png)

**分层地图**叠加在地形图上。图像由作者提供。

这是**蓝岭公路**的一种视角。我想你可以理解为什么那些出租物业更贵了吧？真是美丽的景色！

![](img/40aace42ecc045526260dd4c2145d9a0.png)

蓝岭公路的视图。图片来自作者的个人收藏。

# 在你离开之前

我希望你喜欢这个小项目。地理空间数据非常强大，可以带来许多见解。要使用它，像 Geopandas、Geoplot 和 Folium 这样的包是必不可少的。

这是本练习的完整代码：

[github.com](https://github.com/gurezende/Studying/tree/master/Python/Geospatial?source=post_page-----7244c1b9e302--------------------------------) [## Studying/Python/Geospatial 在 master · gurezende/Studying

### 这是一个包含我的测试和新包学习的代码库 - Studying/Python/Geospatial 在 master ·…

[github.com](https://github.com/gurezende/Studying/tree/master/Python/Geospatial?source=post_page-----7244c1b9e302--------------------------------)

如果你喜欢这些内容，请关注我的博客获取更多更新。此外，你也可以在[LinkedIn](https://www.linkedin.com/in/gurezende/)找到我。

[gustavorsantos.medium.com](https://gustavorsantos.medium.com/?source=post_page-----7244c1b9e302--------------------------------) [## Gustavo Santos - Medium

### 阅读 Gustavo Santos 在 Medium 上的文章。数据科学家。我从数据中提取见解，以帮助人们和公司……

[gustavorsantos.medium.com](https://gustavorsantos.medium.com/?source=post_page-----7244c1b9e302--------------------------------)

# 参考资料

[JORDAN, David S. 2023.*应用地理空间数据科学与 Python*. 第 1 版。Pactk Publishing.](https://www.amazon.com/Applied-Geospatial-Data-Science-Python/dp/1803238127/ref=asc_df_1803238127/?tag=hyprod-20&linkCode=df0&hvadid=598352683676&hvpos=&hvnetw=g&hvrand=1787360199645971497&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=1020988&hvtargid=pla-1875036424962&psc=1)

[medium.com](https://medium.com/data-hackers/criando-mapas-interativos-e-choropleth-maps-com-folium-em-python-abffae63bbd6?source=post_page-----7244c1b9e302--------------------------------) [## 使用 Folium 在 Python 中创建交互式地图和 choropleth 地图

### 学习如何创建简单的地图并使用 Folium 包添加基于颜色的值编码（choropleth）……

[medium.com](https://medium.com/data-hackers/criando-mapas-interativos-e-choropleth-maps-com-folium-em-python-abffae63bbd6?source=post_page-----7244c1b9e302--------------------------------) [## 快速入门 - Folium 0.14.0 文档

### 要创建基础地图，只需将起始坐标传递给 Folium：要在 Jupyter notebook 中显示它，只需请求……

[python-visualization.github.io](https://python-visualization.github.io/folium/quickstart.html?source=post_page-----7244c1b9e302--------------------------------)
