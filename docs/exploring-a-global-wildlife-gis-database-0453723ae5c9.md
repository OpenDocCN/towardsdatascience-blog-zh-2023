# 探索全球野生动物 GIS 数据库

> 原文：[`towardsdatascience.com/exploring-a-global-wildlife-gis-database-0453723ae5c9`](https://towardsdatascience.com/exploring-a-global-wildlife-gis-database-0453723ae5c9)

![](img/e575367617d8605992d7e0d4a6e360a5.png)

所有哺乳动物栖息地的全球地图，每个栖息地随机着色。

## 使用 Python 来表征国际自然保护联盟（IUCN）的地理空间数据库。

[](https://medium.com/@janosovm?source=post_page-----0453723ae5c9--------------------------------)![Milan Janosov](https://medium.com/@janosovm?source=post_page-----0453723ae5c9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----0453723ae5c9--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----0453723ae5c9--------------------------------) [Milan Janosov](https://medium.com/@janosovm?source=post_page-----0453723ae5c9--------------------------------)

·发表于[数据科学前沿](https://towardsdatascience.com/?source=post_page-----0453723ae5c9--------------------------------) ·13 分钟阅读·2023 年 10 月 19 日

--

*国际自然保护联盟（IUCN）启动了多个保护野生动物的项目。其中一个努力促成了一个高质量的全球地理空间数据库，包含了超过 100,000 种物种的栖息地。在本文中，我将深入探讨其子集，重点关注陆生哺乳动物。*

IUCN 红色名录濒危物种数据库包含了超过 150,000 种物种，其中 80%以上具有栖息地的地理空间信息。这个数据库的庞大规模提出了几个挑战，我可能会在以后的文章中解决这些问题。现在，我专注于一个较小的子集——全球数据库中包含 12,436 条陆生哺乳动物记录，每条记录对应一个物种的栖息地。这个哺乳动物-栖息地数据库基于大约四百个不同的来源，包含了 5,626 种以双名法命名的物种，登记时间为 2008 年至 2022 年。此外，数据库包括详细的分类信息，如物种的目和科。此外，数据库的一个主要优势是，它以多边形文件的形式提供详细的栖息地地理空间信息，我将在后续部分详细探讨。

首先，我将介绍并探讨数据集的非几何特征，然后进行一些特定于不同物种地理空间分布的分析步骤。通过这些分析，我希望推广这一数据源，并鼓励未来对其进行潜在应用于野生动物保护政策的研究。

你可以在[IUCN 数据资源](https://www.iucnredlist.org/resources/spatial-data-download)找到所有 IUCN 数据源，我从中下载了陆生哺乳动物的多边形数据（搜索日期：2023 年 10 月 2 日 15:30:02）

*本文中的所有图片均由作者创作。*

# 1\. 统计探索

**1.1\. 解析数据集**

首先，让我们使用 GeoPandas 解析数据库，看看它包含了什么：

```py
import geopandas as gpd # version: 0.9.0

gdf_iucn = gpd.read_file('MAMMALS_TERRESTRIAL_ONLY')
print('Number of records: ', len(gdf_iucn))
print('Number of attributes: ', len(gdf_iucn.keys()))
gdf_iucn.head(3)
```

该单元格的输出：

![](img/530322fd47679f0a652a0280d4f52909.png)

IUCN MAMMALS_TERRESTRIAL_ONLY 数据集的预览。

地理空间数据文件似乎有 29 个属性，我们也可以在[元数据文档](https://nc.iucnredlist.org/redlist/content/attachment_files/METADATA_for_Digital_Distribution_Maps_of_The_IUCN_Red_List_of_Threatened_Species___v6_3.pdf)中了解更多。让我们在这里探索一下吧！

我们将处理的物种数量，每个物种都有一个唯一的 ID，如下所示：

```py
print(len(set(gdf_iucn.id_no)))
print(len(set(gdf_iucn.sci_name)))
```

哪个单元格返回值 5626 两次，确保每个物种确实有一个唯一的*id_no*和一个唯一的*sci_name*。

**1.2 分类学类别**

我们有几个列描述分类学类别：*kingdom*、*phylum*、*class*、*order_*、*family*和*genus*。让我们看看哪些类别最为频繁。在这里，我只保留物种名称和分类学类别作为唯一的配对，因此频率以每个分类级别的唯一物种数来表示。

```py
from collections import Counter

for a in ['kingdom', 'phylum', 'class', 'order_', 'family', 'genus']:    
    print(a, Counter(gdf_iucn[[a, 'sci_name']].drop_duplicates()[a].to_list()).most_common(3), len(set(gdf_iucn[a])))
```

该单元格的输出：

![](img/73d90831bb1e86c52ccc61aa59c481a5.png)

每个分类学变量的前三个值。

这些统计数据如下：ANIMALIA（动物界）、CHORDATA（脊索动物门）和 MAMMALIA（哺乳纲）覆盖了数据集中的所有物种，这意味着它们都是动物，所有物种都有脊髓，并且它们都是哺乳动物。

首个区分出现在生物学级别的分类顺序，其中前 3 名 — 26 个不同的分类顺序中 — 是“RODENTIA”（啮齿动物目）有 2275 个条目，“CHIROPTERA”（翼手目）有 1317 个条目，以及“PRIMATES”（灵长目）有 521 个条目。

数据集中共有 136 个科，其中排名前列的是“MURIDAE”（鼠科）有 763 个条目，“CRICETIDAE”（仓鼠科）有 659 个条目，以及“VESPERTILIONIDAE”（晚蝠科）有 461 个条目，这些是分类顺序级别下的更小的子类，汇集了类似的物种。

最终出现的是属，其中有 1171 个不同的类别，排名前列的是“Crocidura”（长尾鼠）有 196 个条目，“Myotis”（耳蝠）有 120 个条目，以及“Rhinolophus”（盔蝠）有 92 个条目。蝙蝠侠越来越真实了，是不是？

在拆解了顶级列表且未看到太多我们日常生活中熟悉的物种后，我们还可以对一些更为知名的物种进行反向搜索：

```py
gdf_iucn[gdf_iucn.genus=='Canis'].head(5)
```

![](img/914bce94f41f4d745bf39ea4ede81d0f.png)

属于 Canis 的前五个记录 — 与狗最亲近的亲属。

这个快速搜索查询显示了家犬属的结果，其属名为“Canis”。在科学分类中，家犬属于 Canis 属，该属包括各种犬科动物的物种和亚种。家犬的科学名称是 Canis lupus familiaris。Canis 属的其他成员包括灰狼（Canis lupus）、郊狼（Canis latrans）和金豺（Canis aureus）等。

此外，*subspecies* 和 *subpop* 列也属于此处；然而，在这个子集里，它们实际上是空字段。* 同样，*tax_comm* 列仅包含一些关于分类数据的评论，因此我忽略它。

**1.3 栖息地类型如何被表征？**

了解不同物种及其分类后，我们来看看它们的栖息地类型。*presence*、*origin*、*seasonal* 列以及它们在*legend*中的总结包含了这些其他属性的专家评估，前 3 项在 1 到 6 的范围内评分（从现存到灭绝/存在不确定），并给 legend 参数提供了文字描述。其网站上的一个示例展示了如何实现这一点：

+   presence = 1（现存）；origin = 2（再引入）；seasonal = 2（繁殖季节）对应于‘现存 & 再引入（繁殖）’

+   presence = 3（可能现存）；origin = 1（本地）；seasonal = 1（常驻）对应于‘可能现存（常驻）’

了解更多关于评分的内容，请访问[这里](https://www.iucnredlist.org/resources/spatial-data-legend)和[这里](https://nc.iucnredlist.org/redlist/content/attachment_files/Legend_combinations_Dec2018.pdf)。

```py
gdf_iucn[['presence', 'origin', 'seasonal', 'legend']].head(5)
```

![](img/669f186aa18320edb7f9374c66dc4e5c.png)

数据集的栖息地评分子集预览。

让我们看看这些栖息地属性的分布情况：

```py
import matplotlib.pyplot as plt
from collections import Counter

def get_distribution(x):
    return list(zip(*[(k, v) for k, v in Counter(x.to_list()).most_common()]))

f, ax = plt.subplots(1,3,figsize = (15,4))

for idx, feat in enumerate(['presence', 'origin', 'seasonal']):    
    values, frequencies = get_distribution(gdf_iucn[feat])
    ax[idx].bar(values, frequencies)
    ax[idx].set_yscale('log')
    ax[idx].set_title(feat) 
```

该单元格的输出：

![](img/b04d4b7b7cf703be392bd3517c01cf2c.png)

栖息地评分的直方图。

```py
f, ax = plt.subplots(1,1,figsize = (15,5))

values, frequencies = get_distribution(gdf_iucn['legend'])
ax.bar(values, frequencies)
ax.set_title('Frequency of habitat-characteristics', fontsize = 20)
ax.set_yscale('log')
ax.set_xticks(range(len(values)))
ax.set_xticklabels(values, rotation = 60, ha = 'right')
plt.show()
```

该单元格的输出：

![](img/94f5d36cbe0be9031e1d8f072061c40c.png)

栖息地图例值的直方图。

这个图表显示了最常见的标签是现存。然而，灭绝和可能灭绝的类别也在前 10 名之中，这已经是一个令人担忧的迹象。

描述栖息地特征的其他列有 *island* 和 *marine*；然而，由于这个子样本的性质，这些在这里不适用。

**1.4\. 元信息**

在深入数据表时，您还可能找到有关数据本身的来源和类型的信息，这些信息由 *compiler*、*yrcompiled*、*citation* 和 *source* 捕获。以下快速排名显示了哪些实体在编制此数据库时最为繁忙，哪些出版物是最常见的来源，以及这些记录引用最多的是谁：

```py
Counter(gdf_iucn.compiler).most_common(5)
```

![](img/363dc962f5e6e374419e220bf3d25cda.png)

前 5 个数据记录编制实体。

```py
Counter(gdf_iucn.source).most_common(5)
```

![](img/3056bdd4fb5a551a66c6d8be66652e10.png)

前 5 个数据来源。

```py
Counter(gdf_iucn.citation).most_common(5)
```

![](img/36c6ced649ca2dbc1a208b3f5160cf3c.png)

前 5 个被引用最多的来源。

此外，编制年份列可以提示数据集的更新程度，显示记录在 2008 年到 2022 年之间的值。2008 年的巨大峰值可能对应数据库的启动，之后，可能由于几年的资金有限和/或启动后对更新的需求减少。

```py
min(gdf_iucn.yrcompiled), max(gdf_iucn.yrcompiled)
```

```py
f, ax = plt.subplots(1,1,figsize = (15,5))

ax.set_title('The number of records per year', fontsize = 20)
values, frequencies = get_distribution(gdf_iucn['yrcompiled'])
ax.bar(values, frequencies)
```

![](img/d78da668ac77406cc94931324683a8cb.png)

随时间变化的数据记录数量。

**1.5\. 危害信息**

这个数据集中最有趣的类别变量可能包含有关物种状况的严重程度的信息。为了描述这一点，IUCN 红色名录引入了九个类别来描述不同物种的保护状态，这些类别记录在*类别*列中。在这九个类别中，有八个存在于此数据集中：

+   严重濒危（CR），

+   数据不足（DD），

+   濒危（EN），

+   野外灭绝（EW），

+   灭绝（EX），

+   最低关切（LC），

+   不受威胁（NT），

+   区域灭绝（RE），

+   脆弱（VU）

根据这一分类，严重濒危（CR）、濒危（EN）和脆弱（VU）物种被考虑在内。

让我们重新映射成可读格式，并计算各类别，确保每个物种只计算一次：

```py
category_d = {  'EX' : 'Extinct',
                'EW' : 'Extinct in The Wild',
                'RE' : 'Regionally Extinct',
                'CR' : 'Critically Endangered',
                'EN' : 'Endangered',
                'VU' : 'Vulnerable', 
                'DD' : 'Data Deficient',
                'LC' : 'Least Concern',
                'NT' : 'Not Threatened'
     }

gdf_iucn['category'] = gdf_iucn['category'].map(category_d)

Counter(gdf_iucn[['sci_name', 'category']].drop_duplicates().category).most_common()
```

该单元格的结果：

![](img/25b9a2d0decb222df7a5b13aaae8af86.png)

危害类别的频率分布。

这些统计数据显示，3205（56%）的物种属于最不关心类别；它们现在是安全的。然而，22%属于濒危（脆弱、濒危或严重濒危）。此外，我们缺少 14%的数据，还有大约 15 个物种已经灭绝。让我们在这里缅怀它们：

```py
sorted(set(gdf_iucn[gdf_iucn.category.isin(['Extinct', 'Extinct in The Wild'])].sci_name.to_list()))
```

已经灭绝的物种（感谢 ChatGPT 的英文翻译）：

- Dusicyon australis（福克兰岛狼）

- Dusicyon avus（达尔文狐）

- Juscelinomys candango（塞拉多鼠）

- Leporillus apicalis（小枝巢鼠）

- Melomys rubicola（荨麻岛鼠）

- Nesoryzomys darwini（达尔文稻鼠）

- Nyctophilus howensis（霍威岛长耳蝙蝠）

- Oryx dammah（弯角羚）

- Palaeopropithecus ingens（巨型眼镜猴）

- Pennatomys nivalis（山地侏儒负鼠）

- Pipistrellus murrayi（东方长翼蝠）

- Pteropus subniger（黑色飞狐）

- Pteropus tokudae（马里亚纳果蝠）

- Sus bucculentus（爪哇疣猪）

- Xenothrix mcgregori（麦格雷戈猿）

让我们也可视化频率分布：

```py
def get_color(x):
    if x in ['Critically Endangered', 'Endangered', 'Vulnerable']:
        return 'red'
    elif x in ['Extinct in The Wild', 'Regionally Extinct', 'Extinct']:
        return 'k'
    else:
        return 'green'

f, ax = plt.subplots(1,2,figsize = (12,6))

ax[0].set_title(70 * ' ' + 'The number of species category', fontsize = 20, pad = 30)
values, frequencies = get_distribution(gdf_iucn[['sci_name', 'category']].drop_duplicates().category)
colors = [get_color(v) for v in values]

for idx in range(2):
    ax[idx].bar(values, frequencies, color = colors)
    ax[idx].set_xticks(range(len(values)))
    ax[idx].set_xticklabels(values, rotation = 60, ha = 'right')
ax[1].set_yscale('log')

plt.tight_layout()
```

![](img/f9fbb09a86c1d0d0254f16794977db75.png)

每个危害类别中的物种数量。

# 2\. 地理空间探索

**2.1\. 栖息地大小分布**

我们可能首先关注的两个几何信息是 SHAPE_Leng 和 SHAPE_Area，分别对应栖息地边界的总长度和整个面积。准备这些数据是方便的，因为根据地图投影计算长度和面积是有挑战性的。关于这个主题的更多信息请见[这里](https://medium.com/@janosovm/the-world-map-with-many-faces-map-projections-f58a210ff2f7)。

现在，来看一下最显著和最小的区域——哪些物种随处可见，哪些物种需要近距离观察和探险才能追踪到？

```py
# lets sum up the area of each patch a species may have
gdf_iucn.groupby(by = 'sci_name').sum().sort_values(by = 'SHAPE_Area').head(10)
```

此单元的输出：

![](img/eda30459262fc046e21bee09fa2041e0.png)

对哺乳动物栖息地数据库的面积聚合版本进行预览。

现在获取基于栖息地面积的前十和后十物种：

```py
gdf_iucn.groupby(by = 'sci_name').sum().sort_values(by = 'SHAPE_Area').head(10).index.to_list()
```

此单元的输出，以及英文翻译如下：

1\. Melomys rubicola（灌木丛鼠）

2\. Eudiscoderma thongareeae（Thongaree 的 Discoderma）

3\. Murina balaensis（巴拉长尾鼠）

4\. Nyctophilus nebulosus（东部管鼻蝠）

5\. Cavia intermedia（圣卡塔里娜豚鼠）

6\. Fukomys livingstoni（利文斯顿地鼠）

7\. Rhinolophus kahuzi（卡胡兹马蹄蝠）

8\. Microtus breweri（布鲁尔田鼠）

9\. Myotis nimbaensis（尼姆蝙蝠）

10\. Hypsugo lophurus（草原鼠耳蝠）

现在来看另一端——最大的栖息地：

```py
gdf_iucn.groupby(by = 'sci_name').sum().sort_values(by = 'SHAPE_Area', ascending = False).head(10).index.to_list()
```

1\. Mus musculus（家鼠）

2\. Vulpes vulpes（红狐）

3\. Canis lupus（灰狼）

4\. Mustela erminea（白鼬）

5\. Mustela nivalis（最小鼬）

6\. Ursus arctos（棕熊）

7\. Gulo gulo（狼獾）

8\. Alces alces（驼鹿）

9\. Rangifer tarandus（驯鹿）

10\. Lepus timidus（山兔）

**2.2\. 全球可视化栖息地多边形**

还有一列我没有提到，那就是几何信息。然而，它可能包含所有信息中最丰富的内容。首先，绘制一个简单的 GeoPandas 地图，显示每个栖息地多边形的位置。在此可视化中，我用有色阴影区域和细边框标记每个栖息地。颜色基于*tab20* 色图随机分配。

此外，我将地理空间数据转换为 Mollweide 投影，以使我的地图更美观。有关全球地图投影的更多信息，请查看[这里](https://medium.com/@janosovm/the-world-map-with-many-faces-map-projections-f58a210ff2f)。

```py
# transform the coordinate reference system
gdf_iucn_t = gdf_iucn.copy()
gdf_iucn_t = gdf_iucn_t.to_crs('+proj=moll +lon_0=0 +x_0=0 +y_0=0 +datum=WGS84 +units=m +no_defs')
```

```py
f, ax = plt.subplots(1,1,figsize=(15,10))
gdf_iucn_t.sample(200).plot(ax=ax, edgecolor = 'k', linewidth = 0.5, alpha = 0.5, cmap = 'tab20')
```

此代码块的输出：

![](img/991f37ea14cce617b33867519f7ece7b.png)

可视化 200 种随机选择的物种的栖息地斑块，每个斑块颜色随机。

现在，完整的数据集！此外，为了进一步改进我的可视化，我添加了一个带有 library contextily 的底图。我添加了两个不同的版本，让我们根据个人口味来决定！

```py
import contextily as ctx
f, ax = plt.subplots(1,1,figsize=(15,10))
gdf_iucn_t.plot(ax=ax, edgecolor = 'k', linewidth = 0.5, alpha = 0.15, cmap = 'tab20')
ctx.add_basemap(ax, alpha = 0.8, crs = gdf_iucn_t.crs, url = ctx.providers.Esri.WorldPhysical)
ax.axis('off')
ax.set_ylim([-9.5*10**6, 9.5*10**6])
```

此代码块的输出：

![](img/e575367617d8605992d7e0d4a6e360a5.png)

所有哺乳动物栖息地的全球地图，每个斑块颜色随机。

```py
f, ax = plt.subplots(1,1,figsize=(15,10))
gdf_iucn_t.plot(ax=ax, edgecolor = 'k', linewidth = 0.5, alpha = 0.15, cmap = 'tab20')
ctx.add_basemap(ax, alpha = 0.8, crs = gdf_iucn_t.crs, url = ctx.providers.Esri.WorldGrayCanvas)
ax.axis('off')
ax.set_ylim([-9.5*10**6, 9.5*10**6])
#plt.savefig('worldmap_habitats_WorldGrayCanvas.png', dpi = 600, bbox_inches = 'tight')
```

![](img/4e83bfe8ae2ba7e028c7d13155087ac9.png)

所有哺乳动物栖息地的全球地图，每个斑块颜色随机。

**2.3\. 本地可视化栖息地多边形**

在同一张地图上绘制每个栖息地，数以千计，确实会产生一些令人兴奋的图形；然而，从中得出见解并不简单。在得出这些见解的过程中，让我们放大，可视化几个精选物种的栖息地。

例如，当我们搜索长颈鹿（Giraffa camelopardalis）时，会发现十个不同的栖息地斑块：

```py
gdf_iucn[gdf_iucn.genus.str.contains('Giraffa')].head(5)
```

此代码块的输出：

![](img/5740165948cf42e5ef043abb74c64dba.png)

长颈鹿栖息地斑块的预览。

现在，让我们使用下面的代码块生成长颈鹿、猩猩、狮子和非洲象的栖息地图！

```py
f, ax = plt.subplots(1,1,figsize=(15,10))
gdf_iucn[gdf_iucn.sci_name=='Giraffa camelopardalis'].plot(ax=ax, edgecolor = 'k', linewidth = 0.5, alpha = 0.9, color = '#DAA520')

ax.set_xlim([-5, 55])
ax.set_ylim([-38, 15])
ax.set_title('The habitat patches of Giraffa camelopardalis (Giraffes)', fontsize = 18, y = 1.03)

ctx.add_basemap(ax, alpha = 0.8, crs = gdf_iucn.crs, url = ctx.providers.Esri.WorldPhysical)
plt.savefig('1_giraffe.png', dpi = 200)
```

![](img/4d8436a553bc86af95c087ab6e803b02.png)![](img/f199ace88b4d5303663b1ebdc47a7ce9.png)

长颈鹿、猩猩、狮子和非洲象的栖息地图。

![](img/7e70aeb0c40f8c69da80e892b79808ee.png)![](img/a216aaa612c506ceb851f6991d1daccc.png)

**2.4. 映射到国家**

在仔细研究了一些选定的栖息地之后，接下来我们进行国家级的汇总。具体来说，我将每个栖息地多边形映射到国家的行政边界，然后计算物种总数、濒危物种总数及其比率。

我使用了[Natural Earth 数据库](https://www.naturalearthdata.com/downloads/10m-cultural-vectors/)的 Admin 0 — Countries 文件来获取国家级的行政边界。

```py
world = gpd.read_file('ne_10m_admin_0_countries')
print(len(set(world.ADMIN)))
world.plot()
```

这个代码块的结果：

![](img/5ea9daa223b7f0ba72f3945ddac40d3e.png)

基于 Natural Earth 的国家数据集的世界地图。

让我们将濒危类别进行分组：

```py
def is_endangered(x):
    if x in ['Critically Endangered', 'Endangered', 'Vulnerable']:
        return True
    else:
        return False

gdf_iucn['endangered_species'] = gdf_iucn.category.apply(is_endangered)
print(Counter(gdf_iucn['endangered_species']))
```

基于此，属于濒危物种的栖息地斑块数量为 2680，而其他为 9756。

现在建立测量每个国家物种数量和濒危物种数量的国家级字典：

```py
number_of_all_species = gpd.overlay(world, gdf_iucn).groupby(by = 'ADMIN').count().to_dict()['geometry']

number_of_end_species = gpd.overlay(world, gdf_iucn[gdf_iucn.endangered_species==True]).groupby(by = 'ADMIN').count().to_dict()['geometry']

world['number_of_all_species'] = world.ADMIN.map(number_of_all_species)
world['number_of_end_species'] = world.ADMIN.map(number_of_end_species)
world['number_of_all_species'] = world['number_of_all_species'].fillna(0)
world['number_of_end_species'] = world['number_of_end_species'].fillna(0)
world['ratio_of_end_species']  = world['number_of_end_species'] / world['number_of_all_species']
```

```py
Finally, use these updated to visualize the global distributions on the level of countries:from mpl_toolkits.axes_grid1 import make_axes_locatable
from matplotlib.colors import LogNorm

f, ax = plt.subplots(1,1,figsize=(15,7))

ax.set_title('Total number of species', fontsize = 20, pad = 30)
world.plot(ax=ax, color = 'grey', alpha = 0.5, linewidth = 0.5, edgecolor = 'grey')

divider = make_axes_locatable(ax)
cax = divider.append_axes("right", size="2%", pad=-0.01)
world[world.number_of_all_species>0].plot(column = 'number_of_all_species',ax=ax,legend_kwds={'label': "Total number of species"}, edgecolor ='k',  linewidth = 1.5, cax=cax, cmap = 'Greens', legend=True,  norm=LogNorm(vmin=1, vmax=world.number_of_all_species.max())) 
```

![](img/9836a3f748f2bc599ca8f4ecd671c8f8.png)

每个国家的物种总数。

```py
f, ax = plt.subplots(1,1,figsize=(15,7))

ax.set_title('Number of endangered species', fontsize = 20, pad = 30)
world.plot(ax=ax, color = 'grey', alpha = 0.5, linewidth = 0.5, edgecolor = 'grey')
divider = make_axes_locatable(ax)
cax = divider.append_axes("right", size="2%", pad=-0.01)
world[world.number_of_end_species>0].plot(column = 'number_of_end_species',ax=ax, legend_kwds={'label': "Number of endangered of species"}, edgecolor ='k', linewidth = 1.5, cax=cax, cmap = 'RdYlGn_r',  legend=True,  norm=LogNorm(vmin=1, vmax=world.number_of_end_species.max()))

plt.savefig('2_map.png', dpi = 200)
```

![](img/cf7ad1f46b5801faa62fcff02e19b8db.png)

每个国家的濒危物种数量。

```py
f, ax = plt.subplots(1,1,figsize=(15,7))

ax.set_title('Ratio of endangered species', fontsize = 20, pad = 30)
world.plot(ax=ax, color = 'grey', alpha = 0.5, linewidth = 0.5, edgecolor = 'grey')

divider = make_axes_locatable(ax)
cax = divider.append_axes("right", size="2%", pad=-0.01)
world.plot(column = 'ratio_of_end_species',ax=ax,  legend_kwds={'label': "Fraction of endangered of species"}, edgecolor ='k', linewidth = 1.5, cax=cax, cmap = 'RdYlGn_r', legend=True, vmin = 0, vmax = 0.22) 
```

![](img/28a460da72abac56147506a9b0b72998.png)

每个国家的濒危物种比率。

# 3. 总结

在这篇文章中，我介绍了 IUCN 的地理空间数据集，其中包含了成千上万种物种的记录栖息地。经过简短的统计概述后，我展示了如何访问、可视化和操作这些地理空间数据集，以激发未来的工作。

虽然最后我建立了一个简单的指数来量化不同国家野生动物的濒危程度，但 IUCN 提供了现成的地图，基于更复杂的方法。务必在[这里](https://www.iucnredlist.org/resources/other-spatial-downloads#InputPolygon)查看一下！

最后，IUCN 接受未来资助的捐款[这里](https://www.iucnredlist.org/support/donate)。

**参考文献**：IUCN. 2022. IUCN 红色名录。版本 2022–2。 [`www.iucnredlist.org.`](https://www.iucnredlist.org.) 访问时间：2023 年 10 月 02 日。
