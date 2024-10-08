# 大学橄榄球会议重组 — 聚类

> 原文：[`towardsdatascience.com/college-football-conference-realignment-clustering-6ca16840ed3d`](https://towardsdatascience.com/college-football-conference-realignment-clustering-6ca16840ed3d)

[](https://medium.com/@gspmalloy?source=post_page-----6ca16840ed3d--------------------------------)![Giovanni Malloy](https://medium.com/@gspmalloy?source=post_page-----6ca16840ed3d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6ca16840ed3d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ca16840ed3d--------------------------------) [Giovanni Malloy](https://medium.com/@gspmalloy?source=post_page-----6ca16840ed3d--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ca16840ed3d--------------------------------) ·阅读时间 12 分钟·2023 年 8 月 7 日

--

欢迎来到本系列的第三部分，关于会议重组！这是我们将开始使用数据集来指导重组决策的博客文章。常有人抱怨会议重组破坏了传统的对抗关系和大学橄榄球的地区特色。确实，大学体育往往具有地区性，这甚至体现在会议名称中：太平洋 12，大西洋海岸，东南和大东会议等等。当我们包括 FCS 时，有些会议名称更为具体：俄亥俄谷会议。当然，FBS 中的地区会议时代早已过去。最近几天，Pac 12 似乎也可能成为过去的遗物。

本系列分为四个部分（完整的动机见第一部分）：

1.  大学橄榄球会议重组 — Python 中的探索性数据分析

1.  大学橄榄球会议重组 — 回归分析

1.  [大学橄榄球会议重组 — 聚类](https://medium.com/p/6ca16840ed3d#0733-ef9637a21b53)

1.  [大学橄榄球会议重组 — node2vec](https://medium.com/towards-data-science/college-football-conference-realignment-node2vec-ba2e931bb1c)

![](img/7d2b1f585bc37cced342233b8418b0fd.png)

图片由 [Gene Gallin](https://unsplash.com/@genefoto?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

希望系列中的每一部分都能为你提供对大学橄榄球这一心爱的游戏未来的新视角。如果你没有阅读第一部分或第二部分，简要概述是我创建了自己从网络来源汇总的数据集。这些数据包括[每个 FBS 项目的基本信息](https://en.wikipedia.org/wiki/List_of_NCAA_Division_I_FBS_football_programs)、所有[大学橄榄球竞争对手的非规范性近似](https://en.wikipedia.org/wiki/List_of_NCAA_college_football_rivalry_games)、[体育场规模](https://en.wikipedia.org/wiki/List_of_NCAA_Division_I_FBS_football_stadiums)、[历史表现](https://en.wikipedia.org/wiki/List_of_NCAA_Division_I_FBS_football_bowl_records)、[AP 前 25 名投票的出现频率](https://collegepollarchive.com/football/index.cfm)、学校是否为[AAU](https://www.aau.edu/)或[R1](https://en.wikipedia.org/wiki/List_of_research_universities_in_the_United_States)机构（在 Big Ten 和 Pac 12 中具有历史重要性）、[NFL 选秀选手数量](https://www.ncaa.com/news/football/article/2020-04-27/college-football-teams-most-nfl-draft-picks-2000)、2017–2019 年的[项目收入数据](https://graphics.wsj.com/table/NCAA_2019)，以及[关于大学橄榄球球迷基础规模的最新估计](https://drive.google.com/file/d/1MiUOwx8X3H2bSkUOz8a1YhceyJWLLCoJ/view)。在第一部分中，我们发现有几个特征与球迷基础规模有很强的相关性，因此在第二部分中，我们开发了一个线性回归和随机森林回归模型来预测球迷基础规模。

**聚类**

我撰写这篇文章的动机如下：今天的会议建立在传统的核心上。你可以将它们看作是一个新的计算机硬盘驱动器。在区域会议中有条不紊地组织。然而，多年来，就像我们在硬盘驱动器上创建、操作和删除文件一样，大学橄榄球界也出现了新会议（最近是 AAC），旧会议崩溃（Big East 橄榄球），以及曾经的中西部会议从纽约扩展到洛杉矶。最有趣的案例研究是西部运动会议。下面是一个来自[维基百科](https://en.wikipedia.org/wiki/Western_Athletic_Conference)的会员图表，WAC 已经多次自力更生。我会为另一天保留深入探讨的文章。

![](img/ae5ad5683b9d58348c7249a664b9f6f2.png)

[维基百科](https://en.wikipedia.org/wiki/Western_Athletic_Conference)有这些令人着迷的会员图表，而 WAC 是其中最疯狂的之一。

目前，我想知道如果我们重新安排大学橄榄球会议的磁盘驱动器会发生什么。如果我们从零开始构建会议呢？如果我们去掉传统和权利的授予，2022 年学校将如何分组？回答这个问题的最佳方法在于一种叫做聚类的无监督机器学习方法。无监督机器学习意味着我们将向模型提供未标记的数据，希望能够引发隐藏的模式。具体来说，聚类的目标是将相似的观察结果分组在一起。这些组可能在经过彻底的探索性数据分析后还是显而易见或不明显的。

**K 均值聚类**

最广泛实施的聚类算法之一是 k-means 聚类。k-means 的基本思想描述得很好 [这里](https://www.geeksforgeeks.org/k-means-clustering-introduction/)。本质上，用户定义所需的聚类数 k。算法随机分配 k 个质心，并使用欧几里得距离找到离每个项目最近的质心。然后，该项目被视为该质心簇的成员。接着，质心被移动到分配给该质心的项目的平均位置。在 [scikit-learn](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html) 包中，这个过程有一个用户定义的迭代次数。

**特征工程**

现在你已经了解了我们将如何处理这个问题，开始编码吧。首先，我们将导入数据集并删除不需要的列。记住，我们希望从模型中隐藏像当前和过去的会议这样的标签，以便重新开始。

```py
import numpy as np
import pandas as pd
cfb_info_df = pd.read_csv(r'.\FBS_Football_Team_Info.csv', encoding = 'unicode_escape')
clustering_data_df = cfb_info_df.drop(['Team','Nickname', 'City', 'Current_conference', 'Former_conferences', 'First_played', 'Joined_FBS'], axis = 1)
```

谁不喜欢州内竞争呢？从铁碗战到床垫战再到旧木桶战，这些比赛在整个年度的社区聚会上都是重要的吹嘘权利。所以，让我们保留这个特征，但使用 pandas 将其转换为独热编码，就像我们在本系列第二部分中做的那样。（我们的数据也包括现有对手的独热编码）。

```py
clustering_data_df = pd.get_dummies(clustering_data_df,prefix = 'is_state', columns = ['State'])
```

对于这次分析，我们不需要担心训练集和测试集的划分。因此，我们可以一次性对所有数值特征应用最小-最大缩放。

```py
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()
clustering_data_df['Latitude'] = scaler.fit_transform(clustering_data_df[['Latitude']])
clustering_data_df['Longitude'] = scaler.fit_transform(clustering_data_df[['Longitude']])
clustering_data_df['Enrollment'] = scaler.fit_transform(clustering_data_df[['Enrollment']])
clustering_data_df['years_playing'] = scaler.fit_transform(clustering_data_df[['years_playing']])
clustering_data_df['years_playing_FBS'] = scaler.fit_transform(clustering_data_df[['years_playing_FBS']])
clustering_data_df['Stadium_capacity'] = scaler.fit_transform(clustering_data_df[['Stadium_capacity']])
clustering_data_df['total_draft_picks_2000_to_2020'] = scaler.fit_transform(clustering_data_df[['total_draft_picks_2000_to_2020']])
clustering_data_df['first_rd_draft_picks_2000_to_2020'] = scaler.fit_transform(clustering_data_df[['first_rd_draft_picks_2000_to_2020']])
clustering_data_df['number_1_draft_picks_2000_to_2020'] = scaler.fit_transform(clustering_data_df[['number_1_draft_picks_2000_to_2020']])
clustering_data_df['wsj_college_football_revenue_2019'] = scaler.fit_transform(clustering_data_df[['wsj_college_football_revenue_2019']])
clustering_data_df['wsj_college_football_value_2018'] = scaler.fit_transform(clustering_data_df[['wsj_college_football_value_2018']])
clustering_data_df['wsj_college_football_value_2017'] = scaler.fit_transform(clustering_data_df[['wsj_college_football_value_2017']])
clustering_data_df['tj_altimore_fan_base_size_millions'] = scaler.fit_transform(clustering_data_df[['tj_altimore_fan_base_size_millions']])
clustering_data_df['bowl_games_played'] = scaler.fit_transform(clustering_data_df[['bowl_games_played']])
clustering_data_df['bowl_game_win_pct'] = scaler.fit_transform(clustering_data_df[['bowl_game_win_pct']])
clustering_data_df['historical_win_pct'] = scaler.fit_transform(clustering_data_df[['historical_win_pct']])
clustering_data_df['total_games_played'] = scaler.fit_transform(clustering_data_df[['total_games_played']])
```

**聚类实施——Power 5 对阵 Group of 5**

我们再次依赖 scikit-learn 来实现一个简单的 k-means 聚类函数。我们可以在大学橄榄球中做的最基本的划分是 Power 5 对阵 Group of 5 队伍，所以我们从这里开始。这意味着我们希望将数据分成 2 个簇。模型不会知道这两个簇是 Power 5 和 Group of 5 队伍。它只会尝试找到一种将 133 支 FBS 队伍分成两组的方法。鉴于 Power 5 和 Group of 5 队伍在收入和粉丝基础规模上的差异，我的初步假设是这应该相对容易。

```py
from sklearn.cluster import KMeans
# Implement K means clustering
kmeans_conf_p5_v_g5 = KMeans(n_clusters=2, random_state=0).fit(clustering_data_df)
```

我手动将模型的输出与 FBS 球队的真实分布进行了比较。结果显示，我们正确识别了 69 支 Power 5 球队中的 59 支和 64 支 Group of 5 球队中的 63 支。

```py
import plotly.express as px

#Manually compare to P5 v G5 conferences 2025
num_tp = len(list_p5) - 1
num_fp = 1 # Tulane
num_tn = len(list_g5) - 10
num_fn = 10 #Baylor, BYU, Cincinnati, Houston, Oregon State, TCU, Texas Tech, UCF, Wake Forest, Washington State
fig = px.imshow([[num_tn, num_fn],
                 [num_fp, num_tp]], text_auto=True,
               labels=dict(x="True P5 v G5", y="Clustering P5 v G5"),
                x=['Group of 5 Team', 'Power 5 Team'],
                y=['Group of 5 Team', 'Power 5 Team'])
fig.show()
```

![](img/e3d3f7a4c43f2696b7019cadf5602ff4.png)

2-means 聚类很好地区分了 Power 5 和 Group of 5 球队。

这 10 个错误标记的 Power 5 球队包括七支 Big 12 球队（贝勒、BYU、辛辛那提、休斯顿、TCU、德州科技和 UCF）、两支 Pac 12 球队（俄勒冈州立和华盛顿州立）以及一支 ACC 球队（维克森林）。鉴于数据基于 BYU、辛辛那提、休斯顿和 UCF 的首个 Big 12 赛季之前的收入，因此它们与当前的 Group of 5 球队聚集在一起也就不足为奇了。贝勒、TCU 和德州科技位于一个足球忠诚度竞争激烈的州，而孤星州的休闲球迷常常倾向于德州农工或德州。对于那些关注 Pac 12 未来变化的人来说，许多人预测俄勒冈州立和华盛顿州立可能会被 Big Ten 和/或 Big 12 在任何实际调整中排除在外。

唯一一个有 Power 5 信誉却被排除在外的球队是杜兰。这个赛季是杜兰本千年以来第一次被排名，因此他们没有被选中加入 Power 5 会议也不足为奇。然而，[30 年来](https://en.wikipedia.org/wiki/Tulane_Green_Wave_football)，他们曾是 SEC 会议的成员。

**创建 10 个新会议**

现在，是时候将一切整合在一起了。假设大学橄榄球现在保持 10 个会议。这意味着我们将实施 10-means 聚类，以建议基于数据驱动的调整所产生的会议。希望通过在形成新会议时加入竞争和地理信息，除了收入和球迷基础大小外，我们可以同时考虑金钱和传统。

```py
kmeans_10_conf = KMeans(n_clusters=10, random_state=0).fit(clustering_data_df)

labels_10_conf = kmeans_10_conf.labels_
```

标签将告诉我们哪个球队在哪个会议中。查看这些聚类时，你会注意到一些聚类非常大（20+），而其他聚类只有四支球队。根据[NCAA 章程](http://www.ncaapublications.com/productdownloads/D121.pdf)，会议必须至少有八支球队。尽管这篇博文的全部内容都是挑战现有会议结构，但我们仍希望将结果限制在这一规则内。

幸运的是，[微软和 RPI 的研究人员开发了一种约束 k-means 算法](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/tr-2000-65.pdf)，通过将其建模为最小成本流优化问题。有一个[包](https://pypi.org/project/k-means-constrained/)可以在 Python 中实现这种约束 k-means 算法。它的实现与 scikit-learn 包一样简单。然而，我确实需要通过 pip 安装 ortools 9.3.10497。

除了将我们最低的会议规模设置为八个，我们还将最大会议规模设置为二十个。这是一个随机模型，所以如果你更改随机状态，你会得到一组新的会议。

```py
import ortools
import ortools.graph.pywrapgraph
from k_means_constrained import KMeansConstrained #needs pip install --user ortools==9.3.10497
clf = KMeansConstrained(n_clusters=10,size_min=8,size_max=20,random_state=0)
kmeans_Constrained_10_conf = clf.fit_predict(clustering_data_df)
```

**数据驱动的 FBS 会议**

现在是大揭晓的时刻。我冒昧地为每个集群命名了它们的新会议名称。

+   *西南部*：阿肯色州大学、贝勒大学、路易斯安那州立大学、赖斯大学、TCU、德克萨斯大学、德克萨斯农工大学、德克萨斯科技大学。除了路易斯安那州立大学，这些球队曾经在 1990 年代前互相比赛，这个联盟被称为[西南会议](https://en.wikipedia.org/wiki/Southwest_Conference)。现在，它回来了！

+   *阳光美国*：阿克伦大学、阿巴拉契亚州立大学、阿肯色州立大学、博灵格林州立大学、夏洛特大学、海岸卡罗来纳大学、乔治亚南方大学、杰克逊维尔州立大学、詹姆斯·麦迪逊大学、肯特州立大学、自由大学、路易斯安那州立大学-蒙罗分校、中田纳西大学、北德克萨斯大学、老国王大学、萨姆休斯顿州立大学、南阿拉巴马大学、德克萨斯州立大学、特洛伊大学、西肯塔基大学。就像阳光联盟扩展成一个超级联盟，并说服了一些俄亥俄州球队加入。

+   *大 8*：爱荷华大学、密歇根大学、明尼苏达大学、内布拉斯加大学、俄亥俄州立大学、俄克拉荷马大学、佩恩州立大学、威斯康星大学。这是大十足球的核心，加上了来自另一个充满农场并喜爱足球的中西部州的强队：俄克拉荷马大学。

+   *国家运动*：亚利桑那州立大学、波士顿学院、辛辛那提大学、堪萨斯州立大学、肯塔基大学、路易斯安那州立大学、路易斯维尔大学、密西西比州立大学、北卡罗来纳州立大学、新墨西哥大学、俄亥俄大学、俄克拉荷马州立大学、俄勒冈州立大学、南卡罗来纳大学、南密西西比大学、锡拉丘兹大学、坦普尔大学、德克萨斯大学-厄尔帕索、华盛顿州立大学、西弗吉尼亚大学。这个会议在所有四个时区都有存在，这目前是像大 12 这样的会议所羡慕的。这个会议有来自当前十个会议中的九个的成员（没有大十学校），它包含了每个人和一切。

+   *SEC*：阿拉巴马大学、奥本大学、克莱姆森大学、佛罗里达大学、乔治亚大学、乔治亚理工学院、密西西比大学、田纳西大学。除了克莱姆森大学，这些学校都是今天 SEC 会议的创始成员，所以它们保留了这个名字。

+   *篮球天才*：亚利桑那大学、布法罗大学、科罗拉多大学、杜克大学、伊利诺伊大学、印第安纳大学、爱荷华州立大学、堪萨斯大学、密歇根州立大学、密苏里大学、北卡罗来纳大学、西北大学、匹兹堡大学、普渡大学、罗格斯大学、杜兰大学、犹他大学、范德比尔特大学、维吉尼亚大学、华盛顿大学。我称这个为篮球学校会议，因为它包括传统的强队亚利桑那大学、杜克大学、堪萨斯大学和 UNC，以及大十篮球学校普渡大学和印第安纳大学。这个会议与国家运动会议的真正区别在于所有这些学校都是 AAU 成员，因此有了"天才"这个副标题。

+   *MAC+*：陆军、博尔州立、BYU、中密歇根、东卡罗来纳、东密歇根、路易斯安那理工、马歇尔、孟菲斯、迈阿密（OH）、海军、新墨西哥州立、北伊利诺伊、SMU、托莱多、塔尔萨、UMass、犹他州立、维克森林、西密歇根。似乎每个人都在给他们的名字加上“+”，那么为什么不在以 MAC 队伍为核心的扩展会议中也加上呢？这是三个具有巨大地理多样性的会议中的最后一个。我认为可以说这是相对于国家体育会议的低收入、小球迷基础的等价物。

+   *Fun Belt*：FIU、佛罗里达大西洋、佛罗里达州立、乔治亚州立、休斯顿、迈阿密（FL）、内华达、南佛罗里达、UAB、UCF、UConn、UNLV、UTSA。这个会议的联系在于其相对接近赤道。我想 UConn 也因为雪鸟球迷的加入而参与其中。有人提到 SEC 或 Big Ten 可能会招揽佛罗里达州立和迈阿密。在这种情况下，我们的模型倾向于将它们排除在外。

+   *Mountain West*：空军、博伊西州立、科罗拉多州立、弗雷斯诺州立、夏威夷、圣地亚哥州立、圣荷西州立、怀俄明。这一会议包含了 Mountain West 会议中的八支现有队伍。这是当对手、地理位置和收入数据全部对齐时的一个极佳示例。Mountain West 始终在赛场上提供优质的橄榄球产品。自 1999 年成立以来，[九支不同的队伍赢得了会议冠军](https://en.wikipedia.org/wiki/List_of_Mountain_West_Conference_champions#Football)，这证明了赛场上的长期平衡。

+   *Paclantic 8*：加州、马里兰、诺特丹、俄勒冈、斯坦福、UCLA、USC、弗吉尼亚理工。这些队伍代表了西海岸最大的几个名字，结合了一个共同的对手诺特丹。除了诺特丹外，Paclantic 8 的所有学校都是 AAU 成员。

**主成分分析**

我们已经有了新的会议及其名称，但这些会议有多不同呢？一种方法是比较不同特征的会议级分布。我们在本博客系列的第一部分做了很多这种探索性数据分析，所以让我们改为使用主成分分析（PCA）来减少这些特征的维度。PCA 将减少数据的维度，这有助于可视化。

首先，我们将新的会议名称添加到数据集中。

```py
# Initialize new column to define our newly assigned conference
cfb_info_df['k_means_conf'] = 'Southwest'

#for loop to add the conference name for each team
for i in range(len(cfb_info_df['Team'])):
    if cfb_info_df['Team'].iloc[i] in cluster_1:
        cfb_info_df['k_means_conf'][i] = 'Sun USA'
    elif cfb_info_df['Team'].iloc[i] in cluster_2:
        cfb_info_df['k_means_conf'][i] = 'Big 8'
    elif cfb_info_df['Team'].iloc[i] in cluster_3:
        cfb_info_df['k_means_conf'][i] = 'National Athletic'
    elif cfb_info_df['Team'].iloc[i] in cluster_4:
        cfb_info_df['k_means_conf'][i] = 'SEC'
    elif cfb_info_df['Team'].iloc[i] in cluster_5:
        cfb_info_df['k_means_conf'][i] = 'Basketball Brainiacs'
    elif cfb_info_df['Team'].iloc[i] in cluster_6:
        cfb_info_df['k_means_conf'][i] = 'MAC+'
    elif cfb_info_df['Team'].iloc[i] in cluster_7:
        cfb_info_df['k_means_conf'][i] = 'Fun Belt'
    elif cfb_info_df['Team'].iloc[i] in cluster_8:
        cfb_info_df['k_means_conf'][i] = 'Mountain West'
    elif cfb_info_df['Team'].iloc[i] in cluster_9:
        cfb_info_df['k_means_conf'][i] = 'Paclantic 8'
```

现在，我们可以使用 scikit-learn 来无缝计算两个主成分。这将减少我们的维度，使我们能够生成散点图并可视化队伍的相似性。我们将结果添加到一个数据框中，并用新的会议名称标记，以便继续绘制。

```py
from sklearn.decomposition import PCA

# Set the n_components=2
principal = PCA(n_components=2)
principal.fit(clustering_data_df)
pca_clustering_data = principal.transform(clustering_data_df)

# Create data frame for plot
pca_clustering_data_df = pd.DataFrame(pca_clustering_data, columns = ['PCA_1', 'PCA_2'])
pca_clustering_data_df['k_means_conference'] = cfb_info_df['k_means_conf']
```

使用 plotly，我们将绘制所有队伍的散点图。

```py
import plotly.express as px

fig = px.scatter(pca_clustering_data_df, x="PCA_1", y="PCA_2", color="k_means_conference",
                labels=dict(PCA_1="PCA Dimension 1", 
                            PCA_2="PCA Dimension 2", 
                            k_means_conference="10-means Conference"))
fig.show()
```

结果如下：

![](img/5dbb36fb40a37857d63d01baeafd61e0.png)

主成分分析将聚类数据的维度减少到两个。这样我们可以直观地检查各个会议之间的差异。

通过视觉检查，很容易看出 Sun USA、Fun Belt 和 MAC+、Mountain West 紧密对齐且彼此密切相关。像 Big 8 和 SEC 这样高收入的会议则分布得更加分散。

我们从会议团队名称中感受到地理分布，但我们可以在地图上绘制这些会议，以查看我们在多大程度上保留了这项运动的区域历史：

```py
import plotly.graph_objects as go

fig = go.Figure(data=go.Scattergeo(
    lon = cfb_info_df['Longitude'],
    lat = cfb_info_df['Latitude'],
    text = cfb_info_df['Team'],
    mode = 'markers',
    marker = dict(color = cfb_info_df['map_color'])))

fig.update_layout(title = 'Conference Membership',
        geo_scope='usa')
fig.show()
```

![](img/8c004cde1d8209ec463a8ecdb564b877.png)

会议会员地图展示了许多地理上分散的会议。

结果是一个混合体。有些会议确实是区域性的：西南、SEC、Big 8 和 Mountain West，而其他则拥抱更具全国性的分布。也许这就是大学足球的命运。

感谢阅读！一如既往，请在下方评论你的想法。我知道这是一个思维实验，所以请告诉我你的反应。你的团队最终去了哪里？你会更倾向于这些联盟而不是今天的会议吗？

对我的内容感兴趣吗？请考虑[在 Medium 上关注我](https://medium.com/@gspmalloy)。

在 Twitter 上关注我：@malloy_giovanni
