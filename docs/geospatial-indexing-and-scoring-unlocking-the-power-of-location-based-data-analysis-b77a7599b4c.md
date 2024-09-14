# 地理空间索引和评分：释放基于位置的数据分析的力量

> 原文：[`towardsdatascience.com/geospatial-indexing-and-scoring-unlocking-the-power-of-location-based-data-analysis-b77a7599b4c`](https://towardsdatascience.com/geospatial-indexing-and-scoring-unlocking-the-power-of-location-based-data-analysis-b77a7599b4c)

## 使用 Python 和 H3 进行地理空间索引的实用指南

[](https://ransakaravihara.medium.com/?source=post_page-----b77a7599b4c--------------------------------)![Ransaka Ravihara](https://ransakaravihara.medium.com/?source=post_page-----b77a7599b4c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b77a7599b4c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b77a7599b4c--------------------------------) [Ransaka Ravihara](https://ransakaravihara.medium.com/?source=post_page-----b77a7599b4c--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----b77a7599b4c--------------------------------) ·阅读时长 7 分钟·2023 年 1 月 25 日

--

![](img/6b75f0648ad78b2c59482c7d2ccc6497.png)

图片由[Antoine Merour](https://unsplash.com/@amerour?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在本文中，我们将讨论地理空间索引和评分，以及如何使其对开发人员变得简单。地理空间索引是创建一个与地理位置（如纬度和经度）相关的数据集的索引的过程。评分则是根据特定标准对数据进行排名或排序的过程。地理空间索引和评分结合在一起，可以创建强大且高效的应用程序，能够快速检索和显示地理数据。

地理空间索引和评分可能复杂且耗时，但借助合适的工具和技术，可以使其变得简单。本文将探讨如何使用 Uber 的 H3 库进行索引和评分任务。

为了更好地理解，让我们定义一下假设的问题陈述。

> 作为零售公司的一名数据科学家，你被分配了一个重要的项目。公司需要组织和了解其运营的不同区域。
> 
> 这个项目的最终目标是将区域划分为不同的部分，并识别每个区域中的关键热点。这些信息将帮助公司了解哪些区域对他们的业务最为重要，以及他们应集中精力的地方。
> 
> 但你的工作并没有到此为止。公司还需要你为每个网点分配一个“重要性评分”。这个评分将基于之前提到的热点机制。那些位于潜在热点附近的网点将获得更高的重要性评分。

好的，让我们开始构建这个解决方案。

## 需要遵循的步骤

– 了解你的数据（Understand the data）

– 将区域拆分成小六边形

– 为区域内的每个六边形推导一个分数

– 定义出口重要性分数

## 1️⃣ 了解你的数据

根据问题陈述，我们有两个数据集。

1.  兴趣点数据集 — 用于推导重要性分数

1.  出口位置数据集

*注意：提供的数据是随机生成的，不反映实际情况。你可以从* [*这里*](https://github.com/Ransaka/Spatial-indexing-with-h3/tree/master/data)*.* 访问这些数据。

```py
import pandas as pd

pois = pd.read_csv("pois.csv")
outlets = pd.read_csv("outlets.csv")
```

![](img/af295a3610a20c4565e786ece872ea4e.png)![](img/6d06804d9764462f2e02871a8682dc72.png)

让我们绘制兴趣点类别分布。

![](img/d65f8600e9bf1e3d7d62b9ba887e7cfd.png)

此外，我们还需要该区域的地理信息。我已经通过*geojson.io*网站创建了区域的 JSON 文件。

```py
{'type': 'FeatureCollection',
 'features': [{'type': 'Feature',
   'properties': {},
   'geometry': {'coordinates': [[[80.19371259270753, 6.307037132469091],
      [80.19371259270753, 6.072771744744614],
      [80.4434199500783, 6.072771744744614],
      [80.4434199500783, 6.307037132469091],
      [80.19371259270753, 6.307037132469091]]],
    'type': 'Polygon'}}]}
```

让我们把这些整合在一起。

![](img/61431557b42db7e6369dbee391342ec1.png)

目前我们所拥有的 | 作者提供的图片

在上图中，你可以看到两个图层。

1.  矩形 geojson 图层 — 代表我们正在分析的区域

1.  兴趣点图层 — 提供该区域的兴趣点数据集

## 2️⃣ 将区域拆分成小六边形

为了在特定区域内确定热点，有必要将区域划分为较小的部分。一个方法是使用六边形。六边形在这个背景下特别有用，因为它们具有一些优点，如能够均匀地划分区域和准确表示地理形状。如果你对为何使用六边形或它们提供的好处有进一步问题，可以参考 H3 的官方文档获取更多信息。

简而言之，通过选择正确的分辨率，可以通过一个六边形单元表示多个位置。

![](img/0e5f63ee0b807359544640b2e06a78bf.png)

将多个点映射到一个六边形 | 作者提供的图片

[## H3 | H3

### 六边形层级地理空间索引系统 H3 提供了一个易于使用的 API，用于将坐标索引到六边形网格中……

h3geo.org.](https://h3geo.org/?source=post_page-----b77a7599b4c--------------------------------)

我们可以使用 H3 的`*polyfill*`函数将给定区域拆分成六边形。但在此之前，我们必须将区域 geojson 文件转换为 shapely 多边形格式。

```py
from h3 import h3
from shapely.geometry import Polygon,mapping

def dict_to_shapely(d):
    coords = d['features'][0]['geometry']['coordinates'][0]
    return Polygon(coords)

def polygon_to_h3(polygon,resolution):
    polygon = mapping(polygon)
    hexas = h3.polyfill(polygon,res=resolution,geo_json_conformant=True)
    return list(set(hexas))

#read geojson file
with open("sample-bbx.geojson",'r') as f:
    polygon_dict = json.loads(f.read())
    shapely_polygon = dict_to_shapely(polygon_dict)
    f.close()

h3_idx = polygon_to_h3(shapely_polygon,resolution=8)
h3_idx = pd.DataFrame(pd.Series(h3_idx,name='h3'))
```

在上述代码片段中，`polygon_to_h3`函数将输入的多边形转换为六边形列表。你还需要提供分辨率参数。分辨率参数定义了六边形的大小。根据提供的分辨率参数，我们可以将给定区域拆分成六边形，如下所示。

![](img/c5a2536918b77cd158aacb2a23c440f4.png)

将 geojson 瓦片转换为六边形列表 | 作者提供的图片

在这种情况下，使用了 8 的分辨率。根据文档，分辨率为 8 的六边形面积为 0.737327598 平方公里。在这个特定的情况下，区域被划分为 912 个六边形。

## 3️⃣ 为区域内每个六边形推导得分

由于我们已经将区域划分为六边形，我们现在可以继续为每个六边形推导得分。

该过程的方法包括以下内容：

+   为数据集中每个兴趣点分配唯一的六边形 ID。

+   建立评分标准，对每个类别给予加权。

+   将带分数的数据集与区域数据集合并。

```py
scores = 1- (pois['category'].value_counts(normalize=True)) * 3
scores_dict = dict(zip(scores.index,scores.values * 100))

pois['weight'] = pois['category'].map(scores_dict)

grouped_df = pois.groupby("h3").agg(
    counts=('category','count'),
    score = ('weight','sum')
).sort_values(by='counts',ascending=False)

grouped_df.head()
```

![](img/db4a45b58bf38644b788325d9baf9f83.png)

让我们将评分层叠加在基础区域六边形上。右侧的图示展示了如何将得分分配给每个六边形。条形的高度反映了重要性得分。

![](img/bd8668bb067fbc57340156a879f766b2.png)![](img/14344f12fb07b9860a6fbb7f629062e2.png)

图片来源于作者

让我们检查推导出的得分如何分布。

```py
grouped_df.reset_index(inplace=True)
full_scored_idx = h3_idx.merge(grouped_df,on='h3',how='left').fillna(0)

full_scored_idx['score'].plot.hist(color='skyblue',edgecolor='black',bins=5,figsize=(10,8))
plt.title("Score Distibutin")
plt.xlabel("Score")
plt.ylabel("Frequency")
plt.grid(visible=True, linestyle='-.')
plt.show()
```

![](img/87dd1c77a6aed36b8b098d0febfd0583.png)

得分分布 | 图片来源于作者

现在区域内的所有六边形都已评分，是时候创建一个新的数据层进行可视化了。

![](img/ca2ef40844387c6b697cbff5a8eb0799.png)![](img/35bfe710f91dc9b52b438a79453634b2.png)

区域的热点 | 图片来源于作者

请注意上面的颜色编码，灰蓝色的六边形表示重要性低的六边形，而粉色和米色的六边形表示该区域内相对重要的六边形。

## 4️⃣ 定义出口重要性分数

我们现在有一个包含所选区域所有六边形得分的数据集。这使我们能够识别该区域最关键（热点）六边形。这解决了问题的一半。接下来，我们需要为出口定义重要性得分。

将遵循的步骤包括以下内容：

1.  为出口数据集分配唯一的六边形 ID。

1.  对于每个出口，识别最近的 *K* 个六边形。

1.  根据 *K* 个六边形的得分定义出口的重要性。

```py
h3_to_score = dict(zip(full_scored_idx['h3'],full_scored_idx['score']))
get_krings = lambda h:h3.k_ring(h,k=3)
outlets_h3['rings'] = outlets_h3['h3'].apply(get_krings)

print(outlets_h3[['category','h3','rings']])
```

![](img/35edafbe67d7d0efa9d98131ec4ad016.png)

在上面的代码片段中，`k_ring` 函数用于检索每个出口最近的 K 个六边形。你还会注意到在这个例子中，K 被设置为 3。

让我们可视化数据，以更好地理解我们在这一步中所完成的工作。

![](img/e9fb9d242b1839679569ae7cf3803d04.png)

图片来源于作者

现在我们差不多完成了。由于我们已经定义了该区域内每个六边形单元的得分，我们需要做的就是汇总每个出口附近的六边形得分。

```py
outlets_exploded = outlets_h3.explode(column='rings')
outlets_exploded['weight'] = outlets_exploded['rings'].map(h3_to_score)

#can contains hexagons not relavent to current region
outlets_exploded.dropna(inplace=True)

#final scoring for outlets
outlets_exploded.groupby("category").agg(outlet_importance=("weight",'sum'))
```

![](img/6fe3c37ba18b537b6e87cb8e3ec242ca.png)

最终评分 | 图片来源于作者

很好，我们现在已经成功建立了每个出口的重要性分数。这样，两个目标都已经实现了。做得好！ :)

你可以在 GitHub 仓库下访问所有代码。

[](https://github.com/Ransaka/Spatial-indexing-with-h3.git?source=post_page-----b77a7599b4c--------------------------------) [## GitHub - Ransaka/Spatial-indexing-with-h3

### 目前无法执行该操作。你在另一个标签页或窗口中登录了。在其他标签页或…

github.com](https://github.com/Ransaka/Spatial-indexing-with-h3.git?source=post_page-----b77a7599b4c--------------------------------)

## **结论**

这个简化的例子突出了 H3 库的一些宝贵功能。从测量两个地理位置之间的距离到进行最近邻分析，H3 库提供了广泛的选项。通过在适当的时候纳入空间索引，不仅可以节省资源，还能降低成本。原因在于，对点数据进行数学运算可能会消耗大量内存，但通过索引可以大大减少内存使用。

感谢阅读！可以在[LinkedIn](https://www.linkedin.com/in/ransaka/)与我联系。
