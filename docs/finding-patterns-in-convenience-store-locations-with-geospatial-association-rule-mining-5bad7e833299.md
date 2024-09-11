# 使用地理空间关联规则挖掘发现便利店位置中的模式

> 原文：[https://towardsdatascience.com/finding-patterns-in-convenience-store-locations-with-geospatial-association-rule-mining-5bad7e833299?source=collection_archive---------2-----------------------#2023-04-01](https://towardsdatascience.com/finding-patterns-in-convenience-store-locations-with-geospatial-association-rule-mining-5bad7e833299?source=collection_archive---------2-----------------------#2023-04-01)

## 理解东京便利店位置的空间趋势

[](https://medium.com/@elz1582?source=post_page-----5bad7e833299--------------------------------)[![Elliot Humphrey](../Images/62f398bd178bd4eae0fb5c4062972e23.png)](https://medium.com/@elz1582?source=post_page-----5bad7e833299--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5bad7e833299--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5bad7e833299--------------------------------) [Elliot Humphrey](https://medium.com/@elz1582?source=post_page-----5bad7e833299--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F13e1322246bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-patterns-in-convenience-store-locations-with-geospatial-association-rule-mining-5bad7e833299&user=Elliot+Humphrey&userId=13e1322246bb&source=post_page-13e1322246bb----5bad7e833299---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----5bad7e833299--------------------------------) ·7分钟阅读·2023年4月1日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5bad7e833299&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-patterns-in-convenience-store-locations-with-geospatial-association-rule-mining-5bad7e833299&source=-----5bad7e833299---------------------bookmark_footer-----------)![](../Images/f95c12ffb6708caef8d6d65c6ee57709.png)

摄影：由[Matt Liu](https://unsplash.com/@shams_of_tabiriz?utm_source=medium&utm_medium=referral)拍摄，刊登于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

当你在东京四处逛逛时，你经常会经过很多便利店，本地人称之为“konbinis”，这是有道理的，因为在日本有[**超过56,000**](https://www.statista.com/statistics/810901/japan-convenience-store-numbers/)家便利店。通常会有不同的便利店连锁店彼此靠近，很常见的是在街角或者马路对面看到两家店。考虑到东京的人口密度，竞争激烈的企业被迫靠近对方是可以理解的，但是**这两家便利店连锁店之间可能存在某种关系吗**？

# 定义任务

目标是从东京一个街区的许多便利店连锁店中收集位置数据，了解彼此之间是否存在任何关系。为了做到这一点，我们需要：

+   查询不同便利店的位置，检索每个店铺的名称和位置

+   找出在预定义半径内与彼此共同定位的便利店连锁店

+   使用共同位于的店铺数据来推导关联规则

+   在检查中绘制和可视化结果

让我们开始吧！

# 第一步：使用 OpenStreetMap 提取数据

对于我们的用例，我们想找到东京的便利店，所以首先我们需要做一些功课，了解常见的店铺连锁。一个快速的谷歌搜索告诉我，主要的店铺是**FamilyMart、Lawson、7-Eleven、Ministop、Daily Yamazaki 和 NewDays**。

现在我们知道我们要搜索什么了，让我们去[OSMNX](https://osmnx.readthedocs.io/en/stable/)吧；这是一个在 OpenStreetMap（OSM）中搜索数据的很不错的 Python 包。根据 OSM 的架构，我们应该能在 ***'brand:en'*** 或 ***'brand'*** 字段中找到店铺名称。

我们可以首先导入一些有用的库来获取我们的数据，并定义一个函数，以返回指定区域内给定便利店连锁店的位置表：

```py
import geopandas as gpd
from shapely.geometry import Point, Polygon
import osmnx
import shapely
import pandas as pd
import numpy as np
import networkx as nx

def point_finder(place, tags):
    '''
    Returns a dataframe of coordinates of an entity from OSM.

            Parameters:
                    place (str): a location (i.e., 'Tokyo, Japan')
                    tags (dict): key value of entity attribute in OSM (i.e., 'Name') and value (i.e., amenity name)
            Returns:
                    results (DataFrame): table of latitude and longitude with entity value 
    '''

    gdf = osmnx.geocode_to_gdf(place)
    #Getting the bounding box of the gdf
    bounding = gdf.bounds
    north, south, east, west = bounding.iloc[0,3], bounding.iloc[0,1], bounding.iloc[0,2], bounding.iloc[0,0]
    location = gdf.geometry.unary_union
    #Finding the points within the area polygon
    point = osmnx.geometries_from_bbox(north,
                                       south,
                                       east,
                                       west,
                                       tags=tags)
    point.set_crs(crs=4326)
    point = point[point.geometry.within(location)]
    #Making sure we are dealing with points
    point['geometry'] = point['geometry'].apply(lambda x : x.centroid if type(x) == Polygon else x)
    point = point[point.geom_type != 'MultiPolygon']
    point = point[point.geom_type != 'Polygon']

    results = pd.DataFrame({'name' : list(point['name']),
                            'longitude' : list(point['geometry'].x),
                            'latitude' : list(point['geometry'].y)}
                            )

    results['name'] = list(tags.values())[0]
    return results

convenience_stores = place_finder(place = 'Shinjuku, Tokyo',
                                  tags={"brand:en" : " "})
```

我们可以传递每个便利店的名称并将结果合并为一个店铺名称、经度和纬度的表。对于我们的用例，我们可以关注东京的**新宿区**，看看每个便利店的丰富度如何：

![](../Images/acf50fec1bce19be4c37bc6c9b14dd1d.png)

便利店的频次统计。图片作者自拍。

显然，FamilyMart 和 7-Eleven 在店铺频次方面占主导地位，但空间上是什么样子的呢？当使用 [**Kepler.gl**](https://kepler.gl/) 绘制地理空间数据时，非常简单直观，它包括一个不错的界面，可用于创建可视化效果，可保存为 HTML 对象或直接在 Jupyter notebooks 中可视化：

![](../Images/d847364d9f9ed3288317cf3be7dcaaa9.png)

新宿便利店的位置地图，店铺名称按颜色编码。图片作者自拍。

![](../Images/63828fe9dac26fd66176363b3228fbd9.png)

新宿便利店位置图，颜色编码的两分钟步行半径密度（168米）。图片由作者提供。

# 第二步：寻找最近邻

现在我们有了数据，下一步是为每个便利店找到最近邻。为此，我们将使用 Scikit Learn 的 [‘BallTree’ 类](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.BallTree.html) 来寻找两分钟步行半径内的最近便利店名称。我们不关心多少家店被认为是最近邻，所以我们只关注在定义的半径内的便利店连锁。

```py
# Convert location to radians
locations = convenience_stores[["latitude", "longitude"]].values
locations_radians =  np.radians(locations)

# Create a balltree to search locations
tree = BallTree(locations_radians, leaf_size=15, metric='haversine')

# Find nearest neighbours in a 2 minute walking radius
is_within, distances = tree.query_radius(locations_radians, r=168/6371000, count_only=False, return_distance=True)

# Replace the neighbour indices with store names
df = pd.DataFrame(is_within)
df.columns = ['indices']
df['indices'] = [[val for val in row if val != idx] for idx, row in enumerate(df['indices'])]

# create temporary index column
convenience_stores = convenience_stores.reset_index()
# set temporary index column as index
convenience_stores = convenience_stores.set_index('index')
# create index-name mapping
index_name_mapping = convenience_stores['name'].to_dict()

# replace index values with names and remove duplicates
df['indices'] = df['indices'].apply(lambda lst: list(set(map(index_name_mapping.get, set(lst)))))
# Append back to original df
convenience_stores['neighbours'] = df['indices']

# Identify when a store has no neighbours
convenience_stores['neighbours'] = [lst if lst else ['no-neighbours'] for lst in convenience_stores['neighbours']]

# Unique store names
unique_elements = set([item for sublist in convenience_stores['neighbours'] for item in sublist])
# Count each stores frequency in the set of neighbours per location
counts = [dict(Counter(row)) for row in convenience_stores['neighbours']]

# Create a new dataframe with the counts
output_df = pd.DataFrame(counts).fillna(0)[sorted(unique_elements)]
```

如果我们想提高工作的准确性，我们可以用更准确的度量方式来替换哈弗辛距离（例如，通过使用 [networkx](https://networkx.org/) 计算的步行时间），但我们会保持简单。

这将给我们一个数据框，其中每行对应一个位置，以及该位置附近哪些便利店连锁的二进制计数：

![](../Images/f50dbf61e63e86f931a1d11164981431.png)

每个位置的便利店最近邻样本数据框。图片由作者提供。

# 第三步：关联规则挖掘

我们现在有一个数据集，准备进行关联规则挖掘。通过使用 [mlxtend](https://github.com/rasbt/mlxtend) 库，我们可以使用 [Apriori 算法](https://www.geeksforgeeks.org/apriori-algorithm/) 推导关联规则。最低**支持度**为 5%，这样我们只需检查数据集中与频繁出现相关的规则（例如，共同出现的便利店连锁）。我们在推导规则时使用‘lift’度量；**lift** 是同时包含前提和结论的地点比例相对于在独立假设下预期支持度的比率。

```py
from mlxtend.frequent_patterns import association_rules, apriori

# Calculate apriori
frequent_set = apriori(output_df, min_support = 0.05, use_colnames = True)
# Create rules
rules = association_rules(frequent_set, metric = 'lift')
# Sort rules by the support value
rules.sort_values(['support'], ascending=False)
```

这将给我们以下结果表：

![](../Images/b88372e80f7e9e3833bedd411770f65a.png)

便利店数据的关联规则。图片由作者提供。

我们现在将解释这些关联规则，以获得一些高级的学习成果。要解释此表，最好阅读更多关于关联规则的内容，使用以下链接：

+   ***关联规则完整指南*** — [https://towardsdatascience.com/association-rules-2-aa9a77241654](/association-rules-2-aa9a77241654)

+   ***使用 Apriori 算法的关联规则 —*** [https://towardsdatascience.com/association-rules-with-apriori-algorithm-574593e35223](/association-rules-with-apriori-algorithm-574593e35223)

+   ***理解关联规则 —*** [https://towardsdatascience.com/a-simple-way-to-understand-association-rule-from-the-customer-basket-analysis-use-case-c7bcd75bdec1](/a-simple-way-to-understand-association-rule-from-the-customer-basket-analysis-use-case-c7bcd75bdec1)

好的，回到表格。

支持度告诉我们不同便利店连锁实际一起出现的频率。因此，我们可以说7-Eleven和FamilyMart在数据中约31%的情况下同时出现。提升值超过1表明前提的存在增加了结论发生的可能性，这表明这两个连锁店的位置在某种程度上是相互依赖的。另一方面，7-Eleven和Lawson之间的关联显示出更高的提升值，但置信度较低。

Daily Yamazaki在我们的截止点附近支持度较低，并且与FamilyMart的位置显示出略高于1的弱关系。

其他规则是参考便利店的组合。例如，当7-Eleven和FamilyMart已经共同存在时，1.42的高提升值表明它们与Lawson有较强的关联。

# 那么这为什么有用？

如果我们仅仅停留在找到每个商店位置的最近邻，我们就无法确定这些商店之间的关系。

地理空间关联规则为何对企业有洞察力的一个例子是确定新店位置。如果一个便利店连锁正在开设新位置，关联规则可以帮助识别哪些商店可能会共同出现。

当量身定制营销活动和定价策略时，这一点变得清晰，因为它提供了关于哪些商店可能会竞争的定量关系。由于我们知道FamilyMart和7-Eleven经常共同出现（我们通过关联规则证明了这一点），因此这两个连锁店都应更多关注它们的产品如何相对于其他连锁店（如Lawson和Daily Yamazaki）进行竞争。

# 结论

在这篇文章中，我们为东京一个社区的便利店连锁创建了地理空间关联规则。这是通过从OpenStreetMap中提取数据、寻找最近邻便利店连锁、在地图上可视化数据以及使用Apriori算法创建关联规则来完成的。

感谢阅读！
