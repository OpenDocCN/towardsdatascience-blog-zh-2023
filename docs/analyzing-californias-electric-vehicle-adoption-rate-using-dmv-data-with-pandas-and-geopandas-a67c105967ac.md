# 分析加州电动汽车的采纳率

> 原文：[https://towardsdatascience.com/analyzing-californias-electric-vehicle-adoption-rate-using-dmv-data-with-pandas-and-geopandas-a67c105967ac?source=collection_archive---------14-----------------------#2023-04-26](https://towardsdatascience.com/analyzing-californias-electric-vehicle-adoption-rate-using-dmv-data-with-pandas-and-geopandas-a67c105967ac?source=collection_archive---------14-----------------------#2023-04-26)

## 使用 DMV 数据与 Pandas 和 GeoPandas

[](https://medium.com/@danwilentz94?source=post_page-----a67c105967ac--------------------------------)[![Dan Wilentz](../Images/c40fb1588d08c9a36f302583af059d67.png)](https://medium.com/@danwilentz94?source=post_page-----a67c105967ac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a67c105967ac--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a67c105967ac--------------------------------) [Dan Wilentz](https://medium.com/@danwilentz94?source=post_page-----a67c105967ac--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F116ff4c1a13c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyzing-californias-electric-vehicle-adoption-rate-using-dmv-data-with-pandas-and-geopandas-a67c105967ac&user=Dan+Wilentz&userId=116ff4c1a13c&source=post_page-116ff4c1a13c----a67c105967ac---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a67c105967ac--------------------------------) ·12 min read·2023年4月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa67c105967ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyzing-californias-electric-vehicle-adoption-rate-using-dmv-data-with-pandas-and-geopandas-a67c105967ac&user=Dan+Wilentz&userId=116ff4c1a13c&source=-----a67c105967ac---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa67c105967ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyzing-californias-electric-vehicle-adoption-rate-using-dmv-data-with-pandas-and-geopandas-a67c105967ac&source=-----a67c105967ac---------------------bookmark_footer-----------)![](../Images/f1544e62404ec3986a22141acf28c49d.png)

特斯拉（图片由 Matt Weissinger 提供，来源于 [pexels.com](https://www.pexels.com/photo/a-white-tesla-car-on-a-road-11099564/))

加州正在推动积极的社会变革，朝着净零排放的未来迈进，而其中一个重要部分就是其公民用于日常生活的车辆。结合 [通货膨胀减少法案](https://electrek.co/2023/04/17/which-electric-vehicles-still-qualify-for-us-federal-tax-credit/)（为新购电动车提供最高 $7,500 的税收抵免，并为二手电动车提供最高 $4,000 的税收抵免——具体取决于车辆组装地点和电池材料来源），加州还实施了 [先进清洁汽车 II（ACC II）法规](https://ww2.arb.ca.gov/our-work/programs/advanced-clean-cars-program/advanced-clean-cars-ii)，要求汽车制造商到2026年销售中至少35%必须是电动车。2026年之后，该要求每年线性增加，直到2035年所有销售必须是电动车。

本分析专注于加州人在这些新激励措施下的电动车（EV）采纳率。我将 EV 采纳率定义为：

> EV 采纳率 = （总电动车购买量）/（总车辆购买量）

在本分析中，我们将探讨加州是否在轨道上达到2026年35% EV采纳率的目标，并进一步分析地理层面和汽车制造商层面的进展。

**重要说明：** 由于35%的要求最终是基于车辆销售，而DMV提供的是车辆注册数量（而非车辆销售数量），因此本分析通过注册数量来近似销售情况。具体而言，对于每年的DMV数据，我仅使用了注册日期前三年的车辆来近似为新车购买。

# 数据

数据公开在 [加州开放数据门户](http://data.ca.gov/) 和 [加州州立地理门户](https://gis.data.ca.gov/) 上。

+   [DMV车辆计数数据](https://data.ca.gov/dataset/vehicle-fuel-type-count-by-zip-code)（包括按车型年份、汽车制造商、重量类别、燃料类型和邮政编码划分的车辆注册数量），涵盖2018年、2020年、2021年和2022年

+   [加州2021年按邮政编码划分的人口](https://www.california-demographics.com/zip_codes_by_population)

地理数据：

+   [县界](https://data.ca.gov/dataset/california-county-boundaries)

+   [邮政编码边界](https://gis.data.ca.gov/datasets/CDEGIS::california-zip-codes/about)

+   [城市边界](https://data.ca.gov/dataset/city-boundaries)

# 过程

1.  获取公开的数据并使用 [Pandas](https://pandas.pydata.org/) 清洗数据。

1.  使用 Pandas 分析数据。将其叠加到地图上，并使用 [GeoPandas](https://geopandas.org/en/stable/) 绘制。

1.  定期将代码推送到 [github](https://github.com/danwilentz/california_ev_adoption_rate)。

1.  不断迭代！

# 技术方法（GeoPandas 教程）

如果你只对结果感兴趣，可以跳过这一部分。

这个项目让我有机会学习如何使用 GeoPandas，一个用于数据分析项目的 Python 库，特别是有空间组件的数据。

使用 GeoPandas 的一般工作流程是将你想要绘制的数据（例如邮政编码中的车辆和电动车数量）与一个相关的几何（例如邮政编码的几何边界）连接在一个名为 [GeoDataFrame](https://geopandas.org/en/stable/docs/reference/api/geopandas.GeoDataFrame.html) 的结构中。GeoDataFrame 是 GeoPandas 的核心，是 Pandas DataFrame 对象的子类，并包含一个几何列。

对我而言，我有邮政编码级别的车辆计数，但我想在县级别绘制车辆计数。我从所需的库导入开始，并读取了邮政编码和县边界的 geojson 文件。

> import geopandas as gpd
> 
> import matplotlib.pyplot as plt
> 
> zip_codes = gpd.read_file(zip_code_geojson_path)
> 
> counties = gpd.read_file(county_geojson_path)

GeoDataFrames 只能有一个“活动”几何列。无论哪个列是活动的，都将用于连接、绘图或其他应用。你可以使用 GeoDataFrame.set_geometry() 方法将几何设置为不同的列。此外，当两个 GeoDataFrames 连接时，一个活动几何列将被丢弃（因为 GeoDataFrame 只能有一个活动几何列）

由于我想将邮政编码和县的 GeoDataFrames 结合起来，但又保留两者的几何信息，我重命名了邮政编码几何列。我还制作了县几何列的副本。

> # 重命名 zip_code 几何列
> # 
> zip_codes.rename_geometry(‘zip_code_geometry’, inplace=True)
> 
> # 创建副本县几何列
> # 
> counties[‘county_geometry’] = counties.geometry

由于一些邮政编码的边界与多个县的边界重叠，并且我只想为一个县分配一个邮政编码，我取了每个邮政编码边界的质心（即对象的几何中心），然后查看该邮政编码的质心是否位于一个县的边界内。实际上，我将每个邮政编码的整体形状简化为其中心点，然后确定给定邮政编码的中心点位于哪个县的范围内。

为此，我首先将每个 GeoDataFrame 的 CRS（坐标参考系统）从 4326（默认值）设置为 3857。这实际上将我们的坐标系统从地球模型转换为地图：

> zip_codes.to_crs(3857, inplace=True)
> 
> counties.to_crs(3857, inplace=True)

然后，我计算了邮政编码的质心，并将这些质心设置为活动几何：

> # 计算邮政编码质心
> # 
> zip_codes[‘zip_code_centroid’] = zip_codes.centroid
> 
> # 将邮政编码的活动几何设置为质心列
> # 
> zip_codes.set_geometry(‘zip_code_centroid’, inplace=True)

最后，我将两个 GeoDataFrames 连接起来：

> zip_codes_with_county = gpd.sjoin(zip_codes, counties, how=‘inner’, predicate=‘intersects’)

一旦我有了一个包含邮政编码名称、县名称、邮政编码几何形状和县几何形状的 GeoDataFrame，我将按邮政编码将车辆计数和电动车计数合并到我的 GeoDataFrame 中，并将计数汇总到县级。这使我得到了一个包含58行（即加利福尼亚58个县）的 GeoDataFrame，其中包括县名称、县地理信息、车辆计数和电动车计数。非常适合绘图！

下面是绘图代码的一个示例。在其中，我还包含了一个额外的 GeoDataFrame，展示了加利福尼亚的一些城市，以作为我的图中的地标：

> # 电动车采纳率 2022
> # 
> fig, ax = plt.subplots(figsize = (10, 10))
> 
> county_gdf.plot(ax=ax,
> 
> column=’ev_rate_2022',
> 
> legend=True,
> 
> legend_kwds={‘shrink’:0.5},
> 
> cmap = ‘Greens’)
> 
> city_gdf.plot(ax=ax,
> 
> color = ‘orange’,
> 
> markersize = 10)
> 
> for idx, row in city_gdf.iterrows():
> 
> plt.annotate(text=row[‘city’], xy=row[‘coords’], horizontalalignment=’center’, color=’Black’)
> 
> ax.get_xaxis().set_visible(False)
> 
> ax.get_yaxis().set_visible(False)
> 
> for edge in [‘right’, ‘bottom’, ‘top’,’left’]:
> 
> ax.spines[edge].set_visible(False)
> 
> ax.set_title(‘CA EV Adoption Rate (2022) by County’, size=18, weight=’bold’)

这段代码生成了下一节（结果）中的加利福尼亚首张地图。

# 结果

**电动车采纳率（整体）：**

![](../Images/02e01b57a317c874e7ee795994d7090b.png)

上面是一个图表，展示了2018年至2022年整个加利福尼亚州的电动车采纳率。电动车被定义为电池电动汽车、氢燃料电池汽车或插电式混合动力车。然而，电动车不包括混合动力汽油车（因为这些不符合税收优惠要求或汽车制造商规定）。例如，2019年的丰田普锐斯将不算在内。

我们可以注意到，从2021年到2022年的销售出现了上升。这一增长从约6.6%上升到9.5%。这一增长似乎主要来自ZEV购买的增加。

如果我们假设2021年到2022年的线性外推会继续，那么到2026年我们可能达不到35%的电动车采纳率。然而，DMV 数据反映的是每年年初的车辆计数，因此没有计算新激励结构推出后的时间段（2022年8月）。上图中的蓝色圆圈对此进行了补偿。它展示了2022年全州的电动车采纳率，数据来自 [energy.ca.gov](https://www.energy.ca.gov/data-reports/energy-almanac/zero-emission-vehicle-and-infrastructure-statistics/new-zev-sales)。如果我们将蓝色圆圈包含在趋势中并进行线性外推，那么到2026年实现35%的目标看起来是有可能的。

尽管如此，假设线性外推是一种过于简单化的处理方式，甚至可能不是趋势的正确形状。总体而言，很难预测未来4年将会怎样，但从2021年到2022年的增长是一个令人鼓舞的迹象，另外来自[energy.ca.gov](https://www.energy.ca.gov/data-reports/energy-almanac/zero-emission-vehicle-and-infrastructure-statistics/new-zev-sales)的数据点也是一个积极的信号。

**电动汽车采纳率（县级）：**

我们还可以查看县级的电动汽车采纳率，以了解州内各区域购买电动汽车的趋势：

![](../Images/d2d1a105963ec0a0402ef65d3bcf3874.png)

在上图中，我们可以看到湾区的电动汽车采纳率远远高于全州的其他地区。在州的其他部分，电动汽车采纳率通常在沿海县和湾区-太浩走廊沿线较高。具体来说，以下5个县的电动汽车采纳率最高：

有一种假设认为，湾区的电动汽车采纳率较高是因为它反映了居住在那里公民的财富和政治倾向（虽然这超出了本次分析的范围，我们可以利用[最新人口普查数据](https://data.census.gov/)和[加州人在2022年对30号提案的投票情况](https://www.sfchronicle.com/projects/2022/california-election-results/)进一步探讨这个问题）。

加利福尼亚州电动汽车采纳率最低的地区通常集中在州的东北部。这5个电动汽车采纳率最低的县是：

加利福尼亚州的东北地区人口稀少。这些地区可能有居民对电动汽车的采纳持保留态度，因为这些地区往往面临严酷的天气（或许居民认为电动汽车在这些条件下会比他们习惯的车辆功能下降）。此外，该州这部分地区的充电基础设施可能也很少。这里的大异类是帝国县，这是加利福尼亚州最东南的县。它的居民数量比其他县多，并且是沙漠地区（与红木覆盖的山区相对）。它也可能面临基础设施短缺的问题。虽然这超出了本次分析的范围，但我们可以通过查看[电动汽车充电器位置数据](https://afdc.energy.gov/fuels/electricity_locations.html#/find/nearest?fuel=ELEC)来确定基础设施的缺乏是否与电动汽车采纳率相关。

如果我们对每个县2021年和2022年的电动汽车采纳率进行线性外推，我们可以估算出哪些县会在2026年达到目标，哪些县不会。

![](../Images/f5655ab169744b9ae23251674df88c07.png)

线性外推 — 基于2021年到2022年的全县增长数据

然而，这种推断不包括新激励结构后的电动汽车采用率。如果我们假设年增长率等于2021年到2022年全县平均增长率和2022年到2023年州级增长率的平均值，我们可以得出以下预测：

![](../Images/b92ea60a48ea63b6c3fb59feab7e8749.png)

线性推断 —— 基于2021年到2022年全县平均增长和2022年到2023年州级增长的平均值

类似于我们之前看到的电动汽车采用率图表，电动汽车采用率较高的县通常是我预计将在2026年达到目标的县。如果我们考虑所有未预计达到目标的县，并计算它们预计的未达到目标的程度，按其各自的人口比重加权，我们可以确定哪些县最“重要”，需要专注于。这些县可以被认为是改进空间最大、对整个加州推向2026年目标最有帮助的地区。以下是最有机会的前5个县：

这些县大多位于州东南角和南中央谷地区。它们人口众多，电动汽车使用率较低。如果加州在2026年电动汽车采用率落后35%，这个地区可能是一个重要原因。洛杉矶县特别显著，我预测它在2026年的电动汽车采用率为33.7%（几乎达到35%的目标，但还未完全达到），但由于人口众多，它出现在最重要县的前列。

值得注意的是，上述模型用于估计2026年电动汽车采用率非常简单，只是我们可以考虑预测未来电动汽车采用的一种方式。在今后的工作迭代中，我们可以增加更多复杂性以获得更准确的结果。

**电动汽车采用率（汽车制造商水平）：

我们还可以按汽车制造商的水平查看数据，评估哪些汽车制造商正在朝着达到2026年目标的方向前进，哪些滞后。

**重要提示：** 我注意到DMV的数据中有大约28%的电动汽车标记为“其他/未知”，其制造商不明确。我不确定这些电动汽车属于哪些组，但如果它们被正确分配，这些结果可能会有所不同。

如果我们纯粹看注册最多的电动汽车，我们可以看到以下情况：

2022 DMV数据

我们可以看到特斯拉占据主导地位，丰田排名第二。此后，其他电动汽车销售商的长尾排列。大多数汽车制造商的电动汽车销售率在低个位数，少数豪华品牌略高。从这些数据可以清楚地看出，汽车制造商需要做大量工作才能在2026年实现35%的电动汽车销售目标。

我与几位行业专业人士讨论了这个列表，他们指出缺少一些特定的汽车制造商，特别是日产、起亚和现代。我做了一些调查，发现日产特别在2022年注册了许多*较旧*的电动车（例如2018年或2019年生产的电动车），因此被我的规则过滤掉了，该规则只考虑了注册年份过去3年内生产的汽车。如果我多考虑一年，日产就被包含在了第8位的列表中。在这种情况下，本田也进入了列表。这项调整对现代和起亚的结果影响不大。这两家汽车制造商可能是上述“其他/未知”组的重要组成部分。

如果我们按照总体销售最多的车辆来排序数据，我们可以看到以下信息：

2022年 DMV 数据

从这张图表可以看出，除了特斯拉之外，所有大型汽车制造商都有很多工作要做，以实现35%的电动车销售目标。

[IRS 更新](https://www.consumerreports.org/cars/hybrids-evs/electric-cars-plug-in-hybrids-that-qualify-for-tax-credits-a7820795671/) 关于哪些车辆将符合 IRA 税收优惠的最新信息已于2023年4月17日公开。如果在可预见的未来此列表没有变化，税收优惠将主要惠及雪佛兰、福特、特斯拉、吉普和林肯，而对日产、沃尔沃、奥迪、现代、起亚、斯巴鲁、丰田和大众产生不利影响（尽管我看到关于大众 ID.4 是否符合条件的信息存在冲突）。这并不意外，因为 IRA 旨在刺激美国及其盟国的汽车制造业（从而促进汽车制造商的就业），并减少对中国等“外国关注实体”（FEC）的依赖。

我们还可以从县级别考察加利福尼亚州，看看每个县的主要电动车销售商：

![](../Images/239c1b668c28411db9397e72a1022997.png)

特斯拉远远领先于其他品牌，丰田排在第二位。之后，是否有任何电动车销售商在特定地区具有明显优势尚不明确。

**人均车辆数（县级别）：**

虽然这与电动车直接无关，我还查看了加利福尼亚州每个县的人均车辆数，并考察了该数据从2018年到2022年的趋势。随着我们向更可持续的社会过渡，我们不仅要向电动车转型，还需要在总体上减少个人车辆的使用，依赖公共交通、自行车和拼车。这部分分析研究了加利福尼亚州每个人的车辆数量，以及该数据在地区上的趋势是上升还是下降。

![](../Images/1e620d295fdb1ba5dbf24aec6cf4e2df.png)

2022年每人车辆数

![](../Images/66712068b4663114c7a0d2a5e52805f4.png)

2018年到2022年每人车辆数的百分比变化

正如您在第一幅图中看到的那样，Sierra山脉地区是拥有最高人均车辆的县，而旧金山县（虽然可能难以看清）和湾区则是最低的。一般来说，我们在中央谷地区看到的人均车辆较少，但这可能更多是因为较低的财富而不是湾区，而湾区则可能是因为较高的交通便利性。中央谷地区人均车辆较少也可能与车辆注册较少有关。

这幅图展示了过去5年各县每人拥有的车辆数量发生了多大变化。在大多数县，这个数量要么保持相对稳定，要么有所增加。在湾区，这个数量显著下降。我对此的一个假设是远程办公的出现。我猜想，由于许多科技行业的人（特别是在湾区集中的人群）因疫情开始远程办公，他们不需要通勤，因此对汽车的需求减少，因此他们卖掉了自己的车（尽管超出了本次分析的范围，二手车销售数据可能用于进一步探讨这一点）。

**结论：**

加利福尼亚州的电动车市场格局复杂而广泛，我们在这里只是初步探索了一部分。总体而言，看起来加州人在2026年达到35%电动车普及率的目标方面走在了正确的方向上，但各个汽车制造商还有很多工作要做才能实现这一目标。未来几年的发展将会非常有趣！

希望您喜欢这篇分析！您可以在[我的 GitHub 页面](https://github.com/danwilentz/california_ev_adoption_rate)上查看所有代码。如果您喜欢这篇分析，请关注我以获取我的未来工作通知。

**注意：** 所有图片（除非另有说明）均为作者所拍摄。
