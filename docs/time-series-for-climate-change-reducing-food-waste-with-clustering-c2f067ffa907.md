# 气候变化的时间序列: 通过聚类减少食物浪费

> 原文：[`towardsdatascience.com/time-series-for-climate-change-reducing-food-waste-with-clustering-c2f067ffa907`](https://towardsdatascience.com/time-series-for-climate-change-reducing-food-waste-with-clustering-c2f067ffa907)

## 使用时间序列聚类进行更好的需求预测

[](https://vcerq.medium.com/?source=post_page-----c2f067ffa907--------------------------------)![维托·塞尔奎拉](https://vcerq.medium.com/?source=post_page-----c2f067ffa907--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c2f067ffa907--------------------------------)![数据科学的道路](https://towardsdatascience.com/?source=post_page-----c2f067ffa907--------------------------------) [维托·塞尔奎拉](https://vcerq.medium.com/?source=post_page-----c2f067ffa907--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c2f067ffa907--------------------------------) ·6 分钟阅读·2023 年 6 月 7 日

--

![](img/ca1d60ca239e200dd831049dbabe61e5.png)

图片由 [卢克·迈克尔](https://unsplash.com/@lukemichael?utm_source=medium&utm_medium=referral) 提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是系列*气候变化的时间序列*的第七部分。文章列表：

+   第一部分: 预测风力发电

+   第二部分: 太阳辐射预测

+   第三部分: [预测大洋波浪](https://medium.com/towards-data-science/time-series-for-climate-change-forecasting-large-ocean-waves-78484536be36)

+   第四部分: [预测能源需求](https://medium.com/towards-data-science/time-series-for-climate-change-forecasting-energy-demand-79f39c24c85e)

+   第五部分: 预测极端天气事件

+   第六部分: 使用深度学习进行精准农业

# 减少食物浪费

改善供应链是减少我们生态足迹的另一个关键步骤。在发达国家，通常会有大量的消费品，如食品。这些过剩的物品需要大量的能源和资源，通常会被浪费。

减少过度生产是减少温室气体排放的重要里程碑。我们可以通过更好地了解需求来解决这个问题。

以食品为例。每年我们损失约 13 亿公吨食品[1]。当然，这并非所有都是剩余食品或与供应链有关的。一部分食品在生产或运输过程中丧失，例如由于冷藏条件差。然而，更好的需求预测模型可以对减少过度生产产生显著影响。

# 食品需求时间序列的聚类

![](img/212fcb8a46f93037605dd8b612afd5a0.png)

图片由[Diego Marín](https://unsplash.com/@diegosmarines?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我们可以使用聚类分析来改进需求预测。

聚类涉及根据相似性对观察值进行分组。在这种情况下，每个观察值是代表某个产品销售的时间序列。一般而言，您可以使用时间序列聚类来：

+   识别具有相似模式的时间序列，例如趋势或季节性；

+   将时间序列划分为不同的组。这在时间序列数量较大时特别有用。

对于需求时间序列，聚类可以用来识别销售模式相似的产品。然后可以根据每个簇的特征量身定制预测模型。最终，这将带来更好的预测。

聚类需求时间序列对企业也很有价值。识别相似的产品有助于制定更好的营销或促销策略。

# 实践

在本文的其余部分，我们将对食品需求时间序列进行聚类分析。您将学习如何：

+   使用特征提取总结一组时间序列；

+   使用 K-Means 和层次方法进行时间序列聚类。

完整代码可在 Github 上获取：

+   [`github.com/vcerqueira/tsa4climate`](https://github.com/vcerqueira/tsa4climate)

## 数据集

我们将使用美国农业部收集的每周食品销售时间序列数据集。该数据集包含按产品类别和子类别划分的食品销售信息。时间序列按州划分，但我们将使用每个时期的全国总销售额。

下面是数据集的一个示例：

![](img/258cb5f027fc6686f078a18e066110cf.png)

美国不同产品子类别的销售额（以百万美元计）

整体数据如下所示：

![](img/1cb60e01ca78ff7b85151b6355d08361.png)

不同食品子类别的销售金额（百万美元）。图像由作者提供。

## 基于特征的时间序列聚类

我们将使用基于特征的时间序列聚类方法。这个过程包括两个主要步骤：

1.  将每个时间序列总结为一组特征，例如平均值；

1.  对特征集应用传统的聚类算法，例如 K-means。

我们逐步进行每个步骤。

## 使用*tsfel*进行特征提取

我们从提取一组统计数据开始，以总结每个时间序列。目标是将每个系列转换为一小组特征。

有几种时间序列特征提取工具。我们将使用*tsfel*，它相对于其他方法提供了竞争力的性能[3]。

这是如何使用*tsfel*的方法：

```py
import pandas as pd
import tsfel

# get configuration
cfg = tsfel.get_features_by_domain()

# extract features for each food subcategory
features = {col: tsfel.time_series_features_extractor(cfg, data[col])
            for col in data}

features_df = pd.concat(features, axis=0)
```

这个过程会生成大量特征。其中一些可能是冗余的，因此我们进行特征选择过程。

下面，我们对特征集进行三项操作：

+   归一化：将变量转换为 0–1 的值范围；

+   方差选择：去除方差为 0 的变量；

+   相关性选择：去除与其他现有变量有高相关性的变量。

```py
from sklearn.preprocessing import MinMaxScaler
from sklearn.feature_selection import VarianceThreshold
from src.correlation_filter import correlation_filter

# normalizing the features
features_norm_df = pd.DataFrame(MinMaxScaler().fit_transform(features_df),
                                columns=features_df.columns)

# removing features with 0 variance
min_var = VarianceThreshold(threshold=0)
min_var.fit(features_norm_df)
features_norm_df = pd.DataFrame(min_var.transform(features_norm_df),
                                columns=min_var.get_feature_names_out())

# removing correlated features
features_norm_df = correlation_filter(features_norm_df, 0.9)
features_norm_df.index = data.columns
```

## K 均值聚类

在预处理数据集后，我们准备对时间序列进行聚类。我们将每个序列总结为一小组无序特征。因此，我们可以使用任何传统的聚类算法。一个流行的选择是 K 均值。

使用 K 均值时，我们需要选择所需的聚类数量。除非我们有一些领域知识，否则这个参数没有明显的先验值。但是，我们可以采用数据驱动的方法来选择聚类数量。我们测试不同的值，并选择最佳值。

下面，我们测试最多 24 个聚类的 K 均值算法。然后，我们选择最大化轮廓系数的聚类数量。这个指标量化了获得的聚类的凝聚度。

```py
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score

kmeans_parameters = {
    'init': 'k-means++',
    'n_init': 100,
    'max_iter': 50,
}

n_clusters = range(2, 25)
silhouette_coef = []
for k in n_clusters:
    kmeans = KMeans(n_clusters=k, **kmeans_parameters)
    kmeans.fit(features_norm_df)

    score = silhouette_score(features_norm_df, kmeans.labels_)

    silhouette_coef.append(score)
```

如下图所示，轮廓系数在 5 个聚类时最大。

![](img/a08953139af91fc919b920609e9c609d.png)

最多 24 个聚类的轮廓系数。图片由作者提供。

我们可以绘制平行坐标图来理解每个聚类的特征。以下是一个包含三个特征的样本示例：

![](img/072e14b920b211aaa16c613f2ec692e5.png)

带有特征样本的平行坐标图。图片由作者提供。

我们还可以利用聚类的信息来改进需求预测模型。例如，通过为每个聚类构建一个模型。参考文献[5]中的论文是这种方法的一个很好的例子。

## 层次聚类

层次聚类是 K 均值的替代方法。它通过迭代地合并聚类对，形成类似树的结构。*scipy*库提供了这种方法的实现。

```py
import scipy.cluster.hierarchy as shc

# hierarchical clustering using the ward method
clustering = shc.linkage(features_norm_df, method='ward')

# plotting the dendrogram
dend = shc.dendrogram(clustering,
                      labels=categories.values,
                      orientation='right',
                      leaf_font_size=7)
```

层次聚类模型的结果最好通过树状图进行可视化：

![](img/f97bf5369f96da4d44bde4f684e40aa0.png)

使用树状图可视化层次聚类的结果。图片由作者提供。

我们可以使用树状图来理解聚类的特征。例如，我们可以看到大多数罐装食品被分组（橙色）。橙子也与煎饼/蛋糕混合物聚类。这两者经常在早餐中一起出现。

# 关键要点

+   发达国家有大量的食品资源盈余。这些盈余通常会被浪费。

+   减少食物浪费可以通过减少温室气体排放对气候变化产生强烈影响；

+   我们可以通过改进食品需求预测模型来实现这一点；

+   对需求时间序列进行聚类可以改善涉及多个时间序列的预测模型。一种方法是为每个聚类训练一个模型。

+   聚类也可以帮助理解数据集中不同群体的特征。

感谢阅读，下次故事见！

## 参考文献

[1] Jenny Gustavsson, Christel Cederberg, Ulf Sonesson, Robert Van Otterdijk, 和 Alexandre Meybeck. 2011. 全球食品损失和食品浪费. 联合国粮食及农业组织，罗马。

[2] [每周零售食品销售](https://www.ers.usda.gov/data-products/weekly-retail-food-sales/) 由经济研究服务提供（许可证：公有领域）

[3] Henderson, Trent, 和 Ben D. Fulcher. “使用 theft 包的基于特征的时间序列分析。” *arXiv 预印本 arXiv:2208.06146* (2022).

[4] Rolnick, David, 等. “用机器学习应对气候变化。” *ACM Computing Surveys (CSUR)* 55.2 (2022): 1–96.

[5] Kasun Bandara, Christoph Bergmeir, 和 Slawek Smyl. 利用递归神经网络对相似系列的时间序列数据库进行预测：一种聚类方法. 专家系统应用, 140:112896, 2020.
