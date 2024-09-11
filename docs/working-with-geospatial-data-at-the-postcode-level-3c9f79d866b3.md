# 在邮政编码级别处理地理空间数据

> 原文：[https://towardsdatascience.com/working-with-geospatial-data-at-the-postcode-level-3c9f79d866b3?source=collection_archive---------9-----------------------#2023-07-06](https://towardsdatascience.com/working-with-geospatial-data-at-the-postcode-level-3c9f79d866b3?source=collection_archive---------9-----------------------#2023-07-06)

## 如何将“点”邮政编码与区域普查数据关联起来

[](https://lakshmanok.medium.com/?source=post_page-----3c9f79d866b3--------------------------------)[![Lak Lakshmanan](../Images/9faaaf72d600f592cbaf3e9089cbb913.png)](https://lakshmanok.medium.com/?source=post_page-----3c9f79d866b3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3c9f79d866b3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3c9f79d866b3--------------------------------) [Lak Lakshmanan](https://lakshmanok.medium.com/?source=post_page-----3c9f79d866b3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F247b0630b5d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fworking-with-geospatial-data-at-the-postcode-level-3c9f79d866b3&user=Lak+Lakshmanan&userId=247b0630b5d6&source=post_page-247b0630b5d6----3c9f79d866b3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3c9f79d866b3--------------------------------) · 9分钟阅读 · 2023年7月6日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3c9f79d866b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fworking-with-geospatial-data-at-the-postcode-level-3c9f79d866b3&user=Lak+Lakshmanan&userId=247b0630b5d6&source=-----3c9f79d866b3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3c9f79d866b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fworking-with-geospatial-data-at-the-postcode-level-3c9f79d866b3&source=-----3c9f79d866b3---------------------bookmark_footer-----------)

在一些国家，邮政编码是点或路线，而非区域。例如，加拿大邮政编码的最后三位数字对应于本地投递单元，可能对应于街道一侧或乡村路线上的房屋。类似地，英国的邮政编码形式为“YO8 9UR”。这可以小到伦敦的一座建筑。在5+4的美国邮政编码中，最后四位数字决定了一个邮政投递路线（即一组地址），而不是一个区域。与普遍认为的相反，美国5位数的邮政编码也不是区域——它们只是5+4邮政路线的集合，通常由一个邮局提供服务。

法国作为公制系统的发源地，非常逻辑化。在法国，邮政编码对应于一个区域——最后两位数字对应于区，因此 75008 对应于巴黎的第八区，确实是一个区域。然而，邮件配送路线可能并不最佳。

由于人们和商店有地址，而这些地址有相关的邮政编码，大多数消费者数据都是以邮政编码级别报告的。为了进行区域覆盖、市场份额等计算，有必要确定邮政编码的区域范围。这在法国很容易，但在那些邮政编码是邮政路线而不是区域的国家中则会很困难。

![](../Images/55b0a9e4ec5886def068a563d03fa27b.png)

英国邮政编码是皇家邮政的配送地址，而不是区域。加拿大和美国也是如此。照片由 [Monty Allen](https://unsplash.com/@monty_a?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，发布在 [Unsplash](https://unsplash.com/photos/vOPhBFg4Ldw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

由于他们的邮政编码是邮件配送地址，因此可以绘制无限多个多边形来将英国/加拿大/美国划分为有效的邮政编码“区域”。这就是为什么英国的人口统计数据由其国家统计局（ONS）按照行政区域（如县）发布，而不是邮政编码。美国人口普查以“邮政编码统计区”（ZCTA）级别发布数据，而美国投票数据则按县级发布。在处理英国/加拿大/美国的数据时，你通常会遇到地址（即点）和在区域内收集的空间数据的混合。你如何将这些数据关联起来？

为了说明这一点，我将在本文中将英国邮政编码数据和人口普查数据结合起来。

## 下载链接

如果你很急，可以从 [https://github.com/lakshmanok/lakblogs/tree/main/uk_postcode](https://github.com/lakshmanok/lakblogs/tree/main/uk_postcode) 下载我分析的结果——那里有几个 CSV 文件，包含你可能需要的数据。

[ukpopulation.csv.gz](https://github.com/lakshmanok/lakblogs/blob/main/uk_postcode/ukpopulation.csv.gz) 具有以下列：

```py
postcode,latitude,longitude,area_code,area_name,all_persons,females,males
```

[ukpostcodes.csv.gz](https://github.com/lakshmanok/lakblogs/blob/main/uk_postcode/ukpostcodes.csv.gz) 有一列额外的内容——每个邮政编码的 WKT 格式的多边形：

```py
postcode,latitude,longitude,area_code,area_name,all_persons,females,males,geometry_wkt 
```

请注意，数据或代码的使用风险由你自行承担——它是以“按原样”基础分发的，没有任何形式的明示或暗示的担保或条件。

在本文中，我将详细介绍我如何在那个 GitHub 仓库中创建数据集。你可以通过使用笔记本 [uk_postcodes.ipynb](https://github.com/lakshmanok/lakblogs/blob/main/uk_postcode/uk_postcodes.ipynb) 跟随我一起操作。

## 原始数据

我们从三个在 [英国开放政府许可证](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/) 下发布的原始数据源开始：

1.  Free Map Tools 提供了一个[可下载文件](https://www.freemaptools.com/download-uk-postcode-lat-lng.htm)，其中包含每个邮政编码的中心点纬度和经度。这对于空间分析还不够，因为你不能仅用一个点做诸如 ST_INTERSECTS 的操作。但这是一个好的开始。

1.  ONS 已发布[普查数据](https://www.ons.gov.uk/peoplepopulationandcommunity/populationandmigration/populationestimates/datasets/populationandhouseholdestimatesenglandandwalescensus2021)，覆盖如“达勒姆郡”这样的区域。这些不是邮政编码，通常是区、县或地区级别的数据。

1.  英国统计局已经帮助识别了[每个邮政编码相关的区域](https://geoportal.statistics.gov.uk/datasets/ons-postcode-directory-february-2023-version-2/about)。每个邮政编码存在于不同目的和分辨率定义的区域中。这包括但不限于教区、区、县和所在地区。为完整起见，这里是所有其他可用的关联（根据你的空间数据集，你可能需要其他列）：

```py
pcd,pcd2,pcds,dointr,doterm,oscty,ced,oslaua,osward,parish,usertype,oseast1m,osnrth1m,osgrdind,oshlthau,nhser,ctry,rgn,streg,pcon,eer,teclec,ttwa,pct,itl,statsward,oa01,casward,park,lsoa01,msoa01,ur01ind,oac01,oa11,lsoa11,msoa11,wz11,sicbl,bua11,buasd11,ru11ind,oac11,lat,long,lep1,lep2,pfa,imd,calncv,icb,oa21,lsoa21,msoa21
```

我的笔记本使用 wget 下载数据文件：

```py
mkdir -p indata
cd indata
if [ ! -f census2021.xlsx ]; then
    wget -O census2021.xlsx https://www.ons.gov.uk/file?uri=/peoplepopulationandcommunity/populationandmigration/populationestimates/datasets/populationandhouseholdestimatesenglandandwalescensus2021/census2021/census2021firstresultsenglandwales1.xlsx
fi
```

## 读取数据

直接将 CSV 读取到 Pandas 中很简单：

```py
import pandas as pd
pd.read_csv(POSTCODES_CSV)
```

这为我提供了每个邮政编码的中心点纬度和经度：

![](../Images/d556ab2c8fdffdc20bea403a145851e6.png)

每个邮政编码的中心点纬度和经度

有许多包允许你将 Excel 文件读取到 Pandas 中，但我决定使用 DuckDB，因为我会在笔记本中用 SQL 连接这三个数据集：

```py
import duckdb
conn = duckdb.connect()

ukpop = conn.execute(f"""
install spatial;
load spatial;

SELECT 
  * 
FROM
st_read('{POPULATION_XLS}', layer='P01')
""").df()
```

这个 Excel 文件有 7 行标题信息，我可以删除这些信息。我还将列重命名为有意义的变量：

```py
ukpop = ukpop.drop(range(0,7), axis=0).rename(columns={
    'Field1': 'area_code', 
    'Field2': 'area_name', 
    'Field3': 'all_persons', 
    'Field4': 'females', 
    'Field5': 'males'
})
```

那是名为 P01 的表格。请注意，P04 表格有一个人口密度信息，但它没有用，因为人口在区域编码上并不均匀分布。我们将推导出每个邮政编码的人口。

我将这些数据写入 CSV 文件，以便我可以从 DuckDB 中轻松读取它。

```py
ukpop.to_csv("temp/ukpop.csv", index=False)
```

同样，我从英国统计局文件中提取所需的列并将其写入 CSV 文件：

```py
onspd = pd.read_csv(ONSPD_CSV)
onspd = onspd[['pcd', 'oslaua', 'osward', 'parish']]
onspd.to_csv("temp/onspd.csv", index=False)
```

## 关联数据

现在，我们可以使用 DuckDB 将这三个准备好的数据集连接起来，以获取每个邮政编码的密度。为什么使用 DuckDB？即使我可以在 Pandas 中进行连接，但我发现 SQL 更加易读。此外，这也给了我一个使用新热门工具的理由。

我通过首先使用 read_csv_auto 将数据集读取到 DuckB 中来连接这些数据集。然后，我查找邮政编码所在的区、教区、县，并找到人口密度数据报告的区域（教区、区或县）：

```py
 /* pcd,oslaua,osward,parish */
WITH onspd AS (
    SELECT 
      * 
    FROM
    read_csv_auto('temp/onspd.csv', header=True)
),

/* area_code,area_name,all_persons,females,males */
ukpop AS (
    SELECT 
      * 
    FROM
    read_csv_auto('temp/ukpop.csv', header=True)
),

/* id,postcode,latitude,longitude */
postcodes AS (
    SELECT 
      * 
    FROM
    read_csv_auto('indata/ukpostcodes.csv', header=True)
),

/* postcode, area_code */
postcode_to_areacode AS (
  SELECT 
    pcd AS postcode,
    ANY_VALUE(area_code) as area_code
  FROM onspd
  JOIN ukpop 
  ON (area_code = oslaua OR area_code = osward OR area_code = parish)
  GROUP BY pcd
)

SELECT
  postcode, latitude, longitude, /* from postcodes */
  area_code, area_name, /* from ukpop */
  all_persons,females,males /* from ukpop, but has to be spatially corrected */
FROM postcode_to_areacode
JOIN postcodes USING (postcode)
JOIN ukpop USING (area_code) 
```

请注意，空间数量是对应于整个区域的标量，而不是邮政编码。它们必须在邮政编码之间进行分配。

## 在邮政编码之间分配区域数量

all_persons、females、males 对应于整个区域，而不是特定的邮政编码。我们可以根据邮政编码的面积按比例进行，但有无数种多边形可以适应邮政编码，正如我们稍后将看到的，靠近公园和湖泊的邮政编码的区域范围有点不确定。所以我们将做一些简单的处理，给我们一个唯一的答案——我们将把标量值均匀分配到该区域内的所有邮政编码中！这听起来并不奇怪——在高密度社区中，邮政编码更多，因此在邮政编码之间均等分配类似于按人口密度分配标量量。

```py
npostcodes = ukpop.groupby('area_code')['postcode'].count()
for col in ['females', 'males', 'all_persons']:
    ukpop[col] = ukpop.apply(lambda row:  row[col]/npostcodes[row['area_code']], axis=1)
```

目前，我们拥有每个邮政编码的数量——这是我们需要的关联：

![](../Images/5d6d8463c1d7ede6394a5582f05452b5.png)

所以，写出来：

```py
ukpop.to_csv("ukpopulation.csv", index=False)
```

## 邮政编码的区域范围

对于许多分析，我们希望邮政编码不是点，而是区域。尽管有无数种多边形可以将英国划分为每个多边形中仅有一个邮政编码质心，但确实存在一个“最佳”多边形的概念。那就是[沃罗诺伊划分](https://en.wikipedia.org/wiki/Voronoi_diagram)，它将区域划分为每个点都属于距离它最近的邮政编码：

![](../Images/5b96a1b8d8dcf021a26844d2c2bc70ce.png)

20 个点的沃罗诺伊分析，来自 Wikipedia

为了计算这一点，我们可以使用 scipy：

```py
import numpy as np
from scipy.spatial import Voronoi, voronoi_plot_2d

points = df[['latitude', 'longitude']].to_numpy()
vor = Voronoi(points)
```

我在这里假设区域足够小，以至于从纬度和经度计算的地理距离与欧几里得距离之间的差异不大。英国的邮政编码足够小，符合这一情况。

结果被组织成这样的结构：每个点都有一个由一组顶点组成的区域。我们可以为每个点创建一个 WKT 多边形字符串，方法如下：

```py
def make_polygon(point_no):
    region_no = vor.point_region[point_no]
    region = vor.regions[region_no]
    if len(region) >= 3:
        # close the ring
        closed_region = region.copy()
        closed_region.append(closed_region[0])
        # create a WKT of the points
        polygon = "POLYGON ((" + ','.join([ f"{vor.vertices[v][1]} {vor.vertices[v][0]}" for v in closed_region]) + "))"
        return polygon
    else:
        return None
```

这是一个示例结果：

```py
POLYGON ((-0.32491691953979235 51.7393550489536,-0.32527234008402217 51.73948967705648,-0.32515738641624575 51.73987124225542,-0.3241646650618929 51.74087626616231,-0.3215663358407994 51.742660660928614,-0.32145633473723817 51.742228570262824,-0.32491691953979235 51.7393550489536))
```

我们可以创建一个 GeoDataFrame 并绘制邮政编码的子集：

```py
import geopandas as gpd
from shapely import wkt
df['geometry'] = gpd.GeoSeries.from_wkt(df['geometry_wkt'])
gdf = gpd.GeoDataFrame(df, geometry='geometry')

gdf[gdf['area_name'] == 'St Albans'].plot()
```

![](../Images/868480570d8fce494971efd3c3def988.png)

圣奥尔本斯的邮政编码空间范围。由作者生成的图像。

这是伯明翰：

```py
gdf[gdf['area_name'] == 'Birmingham'].plot()
```

![](../Images/f4d207c37815f65293336b580442b1ca.png)

伯明翰的邮政编码空间范围。由作者生成的图像。

## 未开发区域

注意顶部的喇叭和中间的大块蓝色区域。发生了什么？让我们在 Google Maps 中查看伯明翰：

![](../Images/df427ee197d0fa38368486ed2960472d.png)

伯明翰，作者的 Google Maps 截图。

注意到公园区域了吗？皇家邮政不需要向那里任何人投递邮件。所以那里没有邮政编码。因此，附近的邮政编码被“扩展”到这些区域。这会导致空间计算中的问题，因为这些邮政编码看起来会比实际要大得多。

为了解决这个问题，我将采取一种相当启发式的方法。我将把英国划分为 0.01x0.01（大约 1 平方公里）分辨率的网格单元，并找到没有邮政编码的网格单元：

```py
GRIDRES = 0.01
min_lat, max_lat = np.round(min(df['latitude']), 2) - GRIDRES, max(df['latitude']) + GRIDRES
min_lon, max_lon = np.round(min(df['longitude']), 2) - GRIDRES, max(df['longitude']) + GRIDRES
print(min_lat, max_lat, min_lon, max_lon)

npostcodes = np.zeros([ int(1+(max_lat-min_lat)/GRIDRES), int(1+(max_lon-min_lon)/GRIDRES) ])
for point in points:
    latno = int((point[0] - min_lat)/GRIDRES)
    lonno = int((point[1] - min_lon)/GRIDRES)
    npostcodes[latno, lonno] += 1

unpop = []
for latno in range(len(npostcodes)):
    for lonno in range(len(npostcodes[latno])):
        if npostcodes[latno][lonno] == 0:
            # no one lives here.
            # make up a postcode for this location
            # postcode latitude longitude area_code area_name persons_per_sqkm
            unpop.append({
                'postcode': f'UNPOP {latno}x{lonno}',
                'latitude': min_lat + latno * 0.01,
                'longitude': min_lon + lonno * 0.01,
                'all_persons': 0
            }) 
```

我们将在这些无人居住的网格单元的中心创建虚拟邮政编码，并将零人口密度分配给这些编码。将这些虚拟邮政编码添加到实际的邮政编码中，然后重复 Voronoi 分析：

```py
df2 = pd.concat([df, pd.DataFrame.from_records(unpop)])
points = df2[['latitude', 'longitude']].to_numpy()
vor = Voronoi(points)
df2['geometry_wkt'] = [make_polygon(x) for x in range(len(vor.point_region))]
df2['geometry'] = gpd.GeoSeries.from_wkt(df2['geometry_wkt'])
gdf = gpd.GeoDataFrame(df2, geometry='geometry')
```

现在，当我们绘制伯明翰时，我们得到的效果要好得多：

![](../Images/7d356fbe0032c8050dd8de72e477ae85.png)

伯明翰的邮政编码多边形。图片由作者生成

就是这个数据框，我将其保存为第二个 CSV 文件：

```py
gdf.to_csv("ukpostcodes.csv", index=False)
```

## [可选] 加载到 BigQuery

我们可以将 CSV 文件加载到 BigQuery 中并进行一些空间分析，但最好是让 BigQuery 首先将最后一个字符串列解析为几何图形，并根据邮政编码对数据进行聚类：

```py
CREATE OR REPLACE TABLE uk_public_data.postcode_popgeo2
CLUSTER BY postcode
AS

SELECT 
  * EXCEPT(geometry_wkt),
  SAFE.ST_GEOGFROMTEXT(geometry_wkt, make_valid=>TRUE) AS geometry,
FROM uk_public_data.postcode_popgeo
```

现在，我们可以轻松地查询它。例如，我们可以对邮政编码使用 ST_AREA：

```py
SELECT 
  COUNT(*) AS num_postcodes,
  SUM(ST_AREA(geometry))/1e6 AS total_area,
  SUM(all_persons) AS population,
  area_name
 FROM uk_public_data.postcode_popgeo2
 GROUP BY area_name
 ORDER BY population DESC
```

## 总结

空间分析通常需要面积扩展，而不仅仅是点位置。在邮政编码为点/路线的国家中，有无数种方法可以生成邮政编码的多边形空间扩展。一种合理的方法是使用 Voronoi 区域来创建包含这些邮政编码的多边形。然而，如果这样做，你会在湖泊或公园附近得到不自然的大多边形，因为邮局不在那里投递邮件。为了解决这个问题，还需要对国家进行网格化，并在无人居住的网格单元中创建虚拟邮政编码。在这篇文章中，我演示了如何在英国进行此操作。相关的笔记本可以适应其他地方。

## 下一步

1.  查看完整的代码 [https://github.com/lakshmanok/lakblogs/blob/main/uk_postcode/uk_postcodes.ipynb](https://github.com/lakshmanok/lakblogs/blob/main/uk_postcode/uk_postcodes.ipynb)

1.  从 [https://github.com/lakshmanok/lakblogs/tree/main/uk_postcode](https://github.com/lakshmanok/lakblogs/tree/main/uk_postcode) 下载数据
