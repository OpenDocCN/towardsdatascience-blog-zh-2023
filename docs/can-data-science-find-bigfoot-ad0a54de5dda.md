# 数据科学能找到大脚怪吗？

> 原文：[https://towardsdatascience.com/can-data-science-find-bigfoot-ad0a54de5dda?source=collection_archive---------9-----------------------#2023-05-24](https://towardsdatascience.com/can-data-science-find-bigfoot-ad0a54de5dda?source=collection_archive---------9-----------------------#2023-05-24)

## 追踪世界上最难捉摸的神秘生物之一的分析方法

[](https://bradley-stephen-shaw.medium.com/?source=post_page-----ad0a54de5dda--------------------------------)[![Bradley Stephen Shaw](../Images/b3ef5e6e292083ff0f8523ec5ffe89f0.png)](https://bradley-stephen-shaw.medium.com/?source=post_page-----ad0a54de5dda--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ad0a54de5dda--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ad0a54de5dda--------------------------------) [Bradley Stephen Shaw](https://bradley-stephen-shaw.medium.com/?source=post_page-----ad0a54de5dda--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc5cd0a58b5ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-data-science-find-bigfoot-ad0a54de5dda&user=Bradley+Stephen+Shaw&userId=c5cd0a58b5ae&source=post_page-c5cd0a58b5ae----ad0a54de5dda---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ad0a54de5dda--------------------------------) ·14 min read·2023年5月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fad0a54de5dda&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-data-science-find-bigfoot-ad0a54de5dda&user=Bradley+Stephen+Shaw&userId=c5cd0a58b5ae&source=-----ad0a54de5dda---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fad0a54de5dda&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-data-science-find-bigfoot-ad0a54de5dda&source=-----ad0a54de5dda---------------------bookmark_footer-----------)![](../Images/6c49f35f14e455a92cfac74916260c94.png)

图片由 [Jon Sailer](https://unsplash.com/@jonmsailer?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

看着大脚怪研究人员在夜晚追逐——有时是字面意义上的——间接证据、声音和半隐约的阴影，通过森林寻找，确实让电视节目变得精彩纷呈。

我喜欢在闲暇时间看一些电视节目，而一档名为“探险大脚怪”的节目在许多午休时光中给我带来了乐趣。然而，这档节目并不是普通的神秘生物纪录片，因为它声称使用“数据算法”来首先确定一个目标搜索区域，然后派遣人类研究人员去追踪这个毛茸茸的对象。

显然，当我听到这种情况时，我的耳朵就会竖起来，除了打包过夜行李外，我开始思考我能做些什么来帮助搜索。所以开始吧，“帮助”！

虽然这将是一篇相当轻松的文章，我将查看一些数据，包括：

1.  使用Pandas Profiling进行探索性数据分析

1.  传统方式的探索性数据分析

1.  使用OPTICS进行聚类

# 数据

数据来自Timothy Renner¹，他从Bigfoot Field Researchers Organization²那里获得了这些观察结果。

数据快速预览：

![](../Images/e7b2268c436f44d36b851f02db1e9e73.png)

图片由作者提供

有很多有趣的字段，例如：

+   位置特征如目击地点的州、县以及经纬度。

+   包含温度、云量和降水量的天气特征，但包括温度范围、降水细节和风速。

+   像月相这样神秘的信息。

+   对导致目击事件的完整文本描述，以及目击事件本身的详细信息。

# 使用Pandas-Profiling进行探索性数据分析

Pandas-Profiling是探索数据的绝佳工具。只需几行代码，你就能获得一个非常有用的交互式报告，概述数据的重要特征。让我们看看它的实际效果。

```py
# create the report
from pandas_profiling import ProfileReport
profile = ProfileReport(df)
profile.to_file(os.path.join(folder,'bfro_reports_eda.html'))
```

我们可以更深入地查看`classification`特征：

![](../Images/735deaa183abd765456d5c4dd362a159.png)

图片由作者提供

对一个重要特征的详细拆解——稍后会有更多内容。

我们还可以可视化互动——这个视觉图解释了风速和气压之间的关系：

![](../Images/fc116ed0d37dd466fe5f0cb57101f22b.png)

图片由作者提供

Pandas-Profiling还包括帮助我们通过各种相关性测量来理解数据的功能：

![](../Images/d539c6e393e2a5b298a44be87c0590cb.png)

图片由作者提供

当然，没有对缺失情况的总结，EDA是不完整的。这里我们有几个很棒的展示——首先是按列的缺失情况图表。

![](../Images/6bfa4ed781b453f675c477078f2373ab.png)

图片由作者提供

… 以及缺失值之间的相关性：

![](../Images/87abb16238772c245670ee2810701120.png)

图片由作者提供

这么多有用的信息，付出如此少的努力。

# 传统方式的探索性数据分析

有时我们需要动手来更好地理解数据。让我们这样做，并在过程中讨论。

## 一个变化趋势

我对大脚怪目击事件随时间变化的趋势感兴趣。在我们继续之前，让我们快速看一下数据库中包含的报告类型。

总结BFRO网站³，**A类**报告涉及在可以更有信心排除其他动物误解或误认的情况下的明确目击。

在任何情况下未能清晰观察到目标的目击被认为是**B类**目击。这包括远距离的目击和光线条件差的目击。

**C类**报告是那些高潜在不准确的报告。大多数间接报告或源头无法追溯的故事都属于这一类别。

重要的是，B类报告并不被认为比A类报告更不可信或不重要——BFRO认为这两类报告都足够可信，可以展示给公众。然而，C类报告被保存在BFRO档案中，但很少公开列出；1958年前的已发布或本地记录的事件和在非小报报纸或杂志中提到的目击事件是例外。

让我们来看看随时间变化的年度目击，并将A类和B类目击分开。

![](../Images/f3f88b0ce1cc63e98f8bea4373db15fc.png)

作者提供的图像

随着时间的推移，整体呈现出强烈的下降趋势，同时，A类和B类目击的比例保持相对一致。总体下降趋势的驱动因素可能难以确定：

1.  改变的社会规范可能会阻止目击者报告潜在的目击——在今天，多少人相信“大脚怪”确实存在？

1.  类似地，有多少人知道“大脚怪实地研究办公室”和它的数据库？在这些人中，有多少人愿意花时间完成提交？我敢打赌，不会太多，并且这个数字可能会随着时间的推移而减少。

1.  数据库中报告越来越少可能有实际原因。一个主要原因是报告提交的便捷性：在一个优化为便利的数字化世界中，你会花多长时间填写详细的表单并提供准确的信息？

回到数据。值得记住的是，我们在这里讨论的是*绝对*数量。理想情况下，我们应该考虑一些曝光度的度量，以便获得更好的洞察：一种能指示看到“大脚怪”可能性的特征。例如，将年目击次数表示为进入森林区域的年均外出次数的一个比例，会是一个更有趣的度量。

目前，目击的绝对数量是人们外出的频率的直接函数，可能不可靠：看到更多的“大脚怪”可能是由于更多的人外出，而不是“大脚怪”数量的变化或人们发现神秘生物的能力的变化。

## 季节性组合

在这样的时间序列数据中，我们可以预期在规律的间隔内会有一些变化；这被称为季节性⁴。

任何**大脚怪的季节性行为**自然会在数据中产生季节性影响。例如，冬季的冬眠将导致冬季观察数量减少。像雪暴、降雨以及日光时数等季节性因素将**影响我们实际看到大脚怪的能力**。观察季节性的最大驱动因素可能是**季节对潜在观察者的影响**。目击事件可能会与夏季露营、季节性狩猎和浪漫的秋季森林散步有关。

现在我们可能没有足够的数据来可视化每月的季节性变化，以免噪声掩盖任何效果，所以我们将查看整体的目击分布情况。

![](../Images/7bd6cd9efb5fd30d049cba554488f63d.png)

作者提供的图片

我们看到大多数目击事件发生在夏季。我们应该预期这一点，因为良好的天气和充足的日光使人们能够更远、更广泛地进入户外。作为一个喜欢温暖毯子和热茶的人，我并不感到惊讶于最少的目击事件发生在最寒冷和最黑暗的月份。*也许大脚怪对茶和毯子有相同的看法。*

我们看到这种季节性趋势在时间上是一致的：

![](../Images/bd78cc8588317402a5dce53794fc3b0e.png)

作者提供的图片

不幸的是，我们没有太多关于观察时间的数据，所以无法深入探讨。我想象一下，天黑后情况会变得有点疯狂——这在电视上总是会发生的。

到目前为止，我们的探索性数据分析（EDA）帮助我们了解了报告给BFRO的大脚怪目击数量随着时间的推移而下降，并且绝大多数目击事件发生在夏季和秋季。这些见解主要与每个报告的时间元素相关——接下来我们将关注位置。

# 在北美的哪里呢？

![](../Images/af6e01064b9fa6f22601caabb108f72e.png)

照片由 [Tabea Schimpf](https://unsplash.com/@tabeaschimpf?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

让我们使用纬度和经度坐标对来了解大脚怪活动的区域。Folium⁵ 是一个用于创建地图可视化的优秀 Python 包。

## 使用 Folium 进行互动可视化

首先是热力图，高活动区域以红色和黄色突出显示：

```py
import folium
from folium.plugins import HeatMap,MiniMap

# data for plotting
filter_map = (df['latitude'].notna()) & (df['longitude'].notna())
df_map = df.loc[filter_map,['latitude','longitude','observed']].copy()

# create the map
heat_map = folium.Map(
    location = [42,-97.37],
    tiles = 'OpenStreetMap',
    zoom_start = 4,
    control_scale = True
)

# add the heat to the map
HeatMap(
    data = df_map[['latitude','longitude','observed']],
    min_opacity = 0.1
).add_to(heat_map)

# add mini map
heat_map.add_child(MiniMap(position = 'bottomleft',toggle_display = True)) 
```

![](../Images/7c84aeaa244455be6decccdedc0684c4.png)

作者提供的图片

像所有优秀的科幻电影一样，活动似乎仅限于美国（主要是下48州）。

华盛顿州和俄勒冈州似乎是活动的绝对重灾区；而俄亥俄州和佛罗里达州代表了最活跃的东部区域。中部地区似乎活动非常少。

我的地理知识不是很好，但我知道华盛顿州和部分俄勒冈州林木繁茂，而佛罗里达州的大部分地区野性十足（既是大沼泽地⁶ 的家园，也是佛罗里达人⁷ 的家乡）。

如果我猜的话，我会说大脚怪可能更喜欢森林或沼泽栖息地，因此华盛顿和佛罗里达似乎是合理的观察结果。到目前为止，都是合情合理的。接下来我们来探讨一些更科学的内容——通过聚类数据来识别大脚怪的热点区域。

## 使用OPTICS进行聚类

聚类是一项旨在将数据集划分为子集的工作，每个子集中的观察结果在某种程度上是相似的。

在这种情况下，我们将使用纬度和经度来识别簇——或者说大脚怪活动的热点。我们将根据各种期望的特性选择我们的聚类算法：

+   该算法需要能够反映坐标对所隐含的物理距离——即，它必须能够使用haversine度量。

+   我们需要该算法来过滤掉“噪声”：我们预期一些目击事件是随机的，因此*应该*落在簇之外。算法需要能够筛选出这些点并将其排除在簇之外。

+   我不知道有多少个大脚怪热点；做出先验假设可能会使分析有偏差。因此，算法应该能够自行确定“最佳”簇数——即它应该是无监督的。

+   理想情况下，我们还可以指定最小簇大小——即热点中的最小目击次数——以便将该簇视为高活动区域。

**排序点以识别聚类结构（OPTICS）**是一种基于密度的聚类算法，能够提取具有不同密度和形状的簇⁸。它的性质与DBSCAN算法类似，但设计上旨在克服在不同密度数据中检测有意义簇的问题⁹。

scikit-learn文档¹⁰对这些概念做了很好的总结，假设你对DBSCAN已经有所了解：

> OPTICS算法与DBSCAN算法有许多相似之处，可以被认为是DBSCAN的推广¹⁰。
> 
> DBSCAN和OPTICS之间的关键区别在于，OPTICS算法构建了一个*可达性*图，这个图为每个样本分配了一个`可达性_`距离，以及一个簇内的`排序_`属性；这两个属性在模型拟合时分配，并用于确定簇的成员资格¹⁰。
> 
> OPTICS生成的*可达性*距离允许在单个数据集中提取具有可变密度的簇¹⁰。

如果你不熟悉DBSCAN，我在这里详细说明了它的工作原理（并解释了haversine距离）：

[](/lets-do-spatial-clustering-with-dbscan-c3dbfd9fc4d2?source=post_page-----ad0a54de5dda--------------------------------) [## 让我们来做：使用DBSCAN进行空间聚类

### 演示如何结合自定义度量和贝叶斯优化来调整基于密度的空间聚类…

towardsdatascience.com](/lets-do-spatial-clustering-with-dbscan-c3dbfd9fc4d2?source=post_page-----ad0a54de5dda--------------------------------)

我们将编写一个函数来帮助我们进行聚类。这里没有什么太复杂的：

1.  该函数将接受我们的（过滤后的）数据集、最小聚类大小的参数和季节的参数。

1.  该函数根据`season`进行一些过滤。

1.  我们初始化了OPTICS聚类，并进行聚类（记得转换为弧度，因为我们使用的是haversine距离度量）。在这种情况下，我们只改变了聚类中的最小样本数，即`min_cluster_size`。

1.  该函数返回一个包含结果聚类质心的数据集。我们通过计算聚类中所有点的平均纬度和平均经度来粗略计算质心，记得不要包括聚类`-1`，因为这是OPTICS分配给“噪声”观察的标签。

```py
# function to do clustering
def get_cluster(data,cluster_size = 20,season = None):

    # get data
    if season is not None:
        d = data.loc[data['season'] == season,:].copy()
    else:
        d = data.copy()

    # cluster
    cluster = OPTICS(
        min_cluster_size = cluster_size,
        metric = 'haversine',
        algorithm = 'ball_tree',
        n_jobs = -1
    )

    cluster.fit(
        np.radians([x for x in zip(d['latitude'],d['longitude'])])
    )

    # get cluster centroids
    d['cluster'] = pd.Series(cluster.labels_)
    fields = ['latitude','longitude']
    d_agg = d.loc[d['cluster'] >= 0,:].groupby('cluster')[fields].mean()

    return d_agg
```

我们差不多准备好运行聚类了。在开始之前，让我们做一些粗略的计算，以帮助设置最小聚类大小。

我将集中关注过去20年的数据，并考虑如果某个区域有持续的活动报告，则将其视为热点。如果不按季节进行分层，我认为每年一次报告是一个好的起点——即将`cluster_size`设置为20，`season = None`。如果我们要考虑季节性因素，则可以将其调整为反映我们上面调查的季节性分布。

这意味着：

+   在“总体”数据集上进行的任何聚类都将具有最小聚类大小为20。

+   夏季、秋季、冬季和春季的最小聚类大小分别为7、6、3和3。这反映了每个季节中观察到的37%、30%、16%和15%的数据（分别基于上面的EDA）。

最后，是时候进行聚类和可视化了！首先是“总体”聚类结果。

![](../Images/56c59813b4178e84ee7188df6fef8817.png)

图片来源：作者

在华盛顿州和波特兰地区，我们看到了一些聚类，正如热图所示。有趣的是，俄亥俄州并没有很多热点，佛罗里达州则没有。

让我们看看季节性聚类的样子：

![](../Images/6f4ce60df96d86674b90eb8b86b3338d.png)

图片来源：作者

有趣！

看起来在美国西部，大脚怪主要在夏季（红点）和秋季（橙点）出现。中央地区似乎有一些春季热点（绿色）。如果你住在美国东部，你甚至可以在冬季（蓝点）看到大脚怪。

如果我们稍微放大一点，可以集中在你最有可能全年遇到大脚怪的区域：

![](../Images/476c4ed1c385fac24ec224ef674cf05f.png)

图片来源：作者

现在我们将忽略城市化程度较高的地区似乎吸引大脚怪的事实，转而识别大脚怪经常出现的地标。

## 词云

我们的数据包含一个名为`location_details`的字段。*不用猜也知道这是什么*。

词云将允许我们利用这个字段来了解“大脚怪”目击中最常见的地点和地标，而无需绘制频率或比例——令人耳目一新！

词云只是一个包含一组单词的图像，每个单词的大小表示其重要性或频率。词云是非常简单的展示方式，可以用最少的努力提供大量的见解。让我们使用`wordcloud` Python包来生成一个词云。

```py
from wordcloud import WordCloud

# get text
text = df.loc[:,'location_details'].str.cat(sep = ' ').lower()

# cloud
wordcloud = WordCloud(scale = 5,background_color = 'white').generate(text)

# show
plt.figure(figsize = (20,7.5))
plt.imshow(wordcloud, interpolation = 'bilinear')
plt.axis('off')
plt.show()
```

我们需要做的唯一数据处理是将描述列转换为一个单一的文本字段，并将所有字母转换为小写。这可以通过`cat`和`lower`字符串方法轻松完成。`wordcloud`则处理了大部分其他工作——生成一个外观出色的词云：

![](../Images/089886a861b2c2e69e97fe7c17e53022.png)

图片来源：作者

看起来大多数目击事件涉及某种高速公路或湖泊。显然山脉、森林和林地并不多。

# 总结

在这篇文章中我们涵盖了相当多的内容。让我们总结一下。

## 数据探索

我们看到如何使用Pandas-Profiling来减少许多手动的探索性数据分析任务，但也看到有时如果我们想更详细地理解数据中的某些元素，动手实践是必要的。

下次我可能会专注于在运行分析之前进行一些轻量级的预处理，因为报告最终将日期字段视为字符串而不是日期时间。这是一个误解——主题上是一致的。

我确实想知道运行时间和报告的解释性如何随着更广泛、高维的数据框的扩展而变化：更多的总结、交互和相关性计算无疑会带来计算成本；更大的相关性可视化可能会使解释变得更加困难。

## 可视化

Folium使我们能够相当轻松地创建出色的交互式可视化，创建了热图并绘制了各种OPTICS聚类运行的结果。

虽然我在这里没有演示，但Folium也可以轻松创建和可视化聚类。如果我再做一次这种分析——或类似的分析——在进行更正式的聚类工作之前利用这个功能可能是有利的。

## 使用OPTICS进行聚类

尽管我没有明确展示，但我过滤掉了所有缺少纬度或经度的目击事件。在没有对坐标对的缺失进行任何分析的情况下，我不能确定这是否引入了任何偏差，如果我们在进行严肃分析时需要检查这一点。

这实际上引出了数据中的一个重要细节——坐标对的准确性。我想象一下，当你刚刚发现一个大脚怪时获取精确的GPS读数可能是具有挑战性的，因此很多经纬度读数可能只是参考地标的最佳估计。我们*可能*无法纠正这一点，因为任何形式的干预都可能引入偏差。*这种位置“误差”实际上比你想象的更常见——例如，交通事故的位置可能是从最近的安全地点获取的。*

现在很明显，聚类需要一些改进，尤其是因为一些聚类中心落在了相当城市化的区域。我们不仅要在OPTICS中调整`min_cluster_size`，还需要对聚类区域进行控制，以便得到一些“可搜索”的区域。我不知道OPTICS算法中是否有允许我们控制聚类区域的参数，因此我们需要引入某种形式的后处理——类似的内容在我上面链接的文章中有介绍。

如果你读到这里，谢谢你的阅读！希望你享受阅读的过程，就像我享受写作一样。

# 最后的一句话

*尽管这篇文章的重点是使用数据科学的方法来寻找一个神秘生物，但请以我写作时的心态来看待它——这是一个有趣的分析。我认为大脚怪存在并在某个地方徘徊吗？不。我认为如果科学家们真的发现了一个新的动物物种，那将会很惊人。科学万岁！*

*布拉德*

# 参考资料和资源

1.  [大脚怪目击数据集 — timothyrenner | data.world](https://data.world/timothyrenner/bfro-sightings-data)，根据公共领域许可证提供。

1.  [大脚怪领域研究组织（bfro.net）](https://www.bfro.net/)

1.  [BFRO数据库历史及报告分类系统](https://www.bfro.net/GDB/classify.asp)

1.  [季节性 — 维基百科](https://en.wikipedia.org/wiki/Seasonality)

1.  [GitHub — python-visualization/folium: Python 数据. Leaflet.js 地图。](https://github.com/python-visualization/folium)

1.  [大沼泽地 — 维基百科](https://en.wikipedia.org/wiki/Everglades)

1.  [佛罗里达人 — 维基百科](https://en.wikipedia.org/wiki/Florida_Man)

1.  [ML | OPTICS 聚类解释 — GeeksforGeeks](https://www.geeksforgeeks.org/ml-optics-clustering-explanation/)

1.  [OPTICS算法 — 维基百科](https://en.wikipedia.org/wiki/OPTICS_algorithm)

1.  [2.3\. 聚类 — scikit-learn 1.2.2 文档](https://scikit-learn.org/stable/modules/clustering.html#optics)
