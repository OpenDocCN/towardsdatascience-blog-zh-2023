# 从簇到洞察；下一步

> 原文：[`towardsdatascience.com/from-clusters-to-insights-the-next-step-1c166814e0c6`](https://towardsdatascience.com/from-clusters-to-insights-the-next-step-1c166814e0c6)

## 了解如何定量检测哪些特征驱动了簇的形成

[](https://erdogant.medium.com/?source=post_page-----1c166814e0c6--------------------------------)![Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----1c166814e0c6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1c166814e0c6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c166814e0c6--------------------------------) [Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----1c166814e0c6--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c166814e0c6--------------------------------) ·阅读时间 9 分钟·2023 年 5 月 10 日

--

![](img/f07bdc7ebd537a94cb31545f18cd623b.png)

图片由作者提供。

聚类分析是一种识别具有相似模式的组的优秀技术。然而，一旦簇形成后，确定簇背后的驱动特征可能依然具有挑战性。但这一步骤对于揭示之前可能遗漏的宝贵见解至关重要，这些见解可以用于决策制定和更深入地理解数据集。确定驱动特征的一种方法是通过特征值对样本进行着色。尽管这种方法很有洞察力，但当特征数量达到数百时，这种方法就显得劳动密集。此外，随着簇的大小和密度的不同，判断特定特征集的确切贡献可能会很困难。***我将演示如何定量检测簇背后的驱动特征。*** *在这篇博客中，* ***clusteval*** *库被用来进行聚类评估，并确定驱动簇形成的特征。*

# 背景

无监督聚类是一种在数据中识别自然或数据驱动的组的技术，而无需使用预定义的标签或类别。聚类方法的挑战在于，不同的方法可能会由于对数据施加的隐含结构而导致不同的分组。要确定什么构成了“*好*”的聚类，我们可以使用定量指标。更多详细信息可以在博客“*从数据到聚类；你的聚类什么时候足够好？*”中阅读 **[*1**]*。

[](/from-data-to-clusters-when-is-your-clustering-good-enough-5895440a978a?source=post_page-----1c166814e0c6--------------------------------) ## 从数据到簇；你的聚类什么时候足够好？

### 使用聚类方法可以找到合理的簇和隐藏的宝石，但你需要正确的聚类评估方法！

[towardsdatascience.com

# `clusteval`库测试特征是否与簇标签显著关联。

***Clusteval***是一个 Python 包，用于评估聚类的倾向、质量、簇的数量，并确定簇与特征之间的统计关联。*Clusteval*返回最佳簇标签的簇标签，以产生最佳的样本划分。实现了以下评估策略：*轮廓系数、Davies-Bouldin 指数和导数（或肘部）方法*，这些可以与*K 均值、凝聚聚类、DBSCAN 和 HDBSCAN* *[*1**]*结合使用。

```py
pip install clusteval
```

为了检测簇标签背后的驱动特征，*HNET*库 [*2*] 在*clusteval*中被利用，执行对分类特征的*超几何检验*和对连续值的*曼-惠特尼 U 检验*，以评估特征是否与簇标签显著关联。更多详细信息可以在这里阅读：

[](/explore-and-understand-your-data-with-a-network-of-significant-associations-9a03cf79d254?source=post_page-----1c166814e0c6--------------------------------) ## 探索并理解你的数据，利用显著关联网络。

### 探索以了解你的数据可以决定一个项目是否成功！

[towardsdatascience.com

# 确保聚类结果是可靠的。

在我们能够检测簇背后的驱动特征之前，我们首先需要对数据进行聚类，并确信我们的聚类是有效的。与监督方法相对，聚类算法处理的是同质数据，其中所有变量具有相似的类型或测量单位。这非常重要，因为聚类算法基于数据点的相似性对其进行分组，因此在混合数据类型或使用非同质数据时会产生不可靠的结果。确保以下几点：

1.  数据根据研究目标和数据的统计特性进行标准化。

1.  使用适当的距离度量。

1.  评估簇的倾向和质量。

有了簇标签，我们可以调查特征的贡献。*让我们在下一节中做一个小的用例。*

# 玩具示例揭示簇标签背后的驱动特征。

对于这个用例，我们将加载*在线购物者意图*数据集，并进行预处理、聚类、评估，然后确定与簇标签显著相关的特征。此数据集包含 12330 个样本和 18 个特征。此混合数据集需要更多的预处理步骤，以确保所有变量具有相似的类型或测量单位。因此，第一步是创建具有可比单位的同质数据集。常见的方法是离散化并创建一个一次性矩阵。我将使用`df2onehot`库，按照以下预处理步骤进行离散化：

+   分类值`0`、`None`、`?`和`False`被移除。

+   少于 50 个正值的一次性特征被移除。

+   对于具有 2 个类别的特征，仅保留一个。

+   特征值独特性达到 80%或以上的被认为是数值型的。

预处理步骤将数据集转换为一个包含相同 12330 个样本但现在具有 121 个一次性特征的矩阵。值得注意的是，上述标准并不是绝对标准，而是应根据每个用例进行探索。对于聚类，我们将使用`agglomerative`聚类方法，并以`hamming`距离和`complete`连通性进行聚类。请参见下面的代码部分。

```py
# Intall libraries
pip install df2onehot

# Import libraries
from clusteval import clusteval
from df2onehot import df2onehot

# Load data from UCI
url = 'https://archive.ics.uci.edu/ml/machine-learning-databases/00468/online_shoppers_intention.csv'

# Initialize clusteval
ce = clusteval()
# Import data from url
df = ce.import_example(url=url)

# Preprocessing
cols_as_float = ['ProductRelated', 'Administrative']
df[cols_as_float]=df[cols_as_float].astype(float)
dfhot = df2onehot(df, excl_background=['0.0', 'None', '?', 'False'], y_min=50, perc_min_num=0.8, remove_mutual_exclusive=True, verbose=4)['onehot']

# Initialize using the specific parameters
ce = clusteval(evaluate='silhouette',
               cluster='agglomerative',
               metric='hamming',
               linkage='complete',
               min_clust=2,
               verbose='info')

# Clustering and evaluation
results = ce.fit(dfhot)

# [clusteval] >INFO> Saving data in memory.
# [clusteval] >INFO> Fit with method=[agglomerative], metric=[hamming], linkage=[complete]
# [clusteval] >INFO> Evaluate using silhouette.
# [clusteval] >INFO: 100%|██████████| 23/23 [00:28<00:00,  1.23s/it]
# [clusteval] >INFO> Compute dendrogram threshold.
# [clusteval] >INFO> Optimal number clusters detected: [9].
# [clusteval] >INFO> Fin.
```

**在数据集上运行*clusteval*后，返回了 9 个簇。** 由于数据包含 121 个维度（特征），我们无法直接在散点图中可视化检查簇。然而，我们可以进行嵌入，然后使用散点图进行视觉检查，如下面代码部分所示。指定`embedding='tsne'`时，嵌入会自动执行。

```py
# Plot the Silhouette and show the scatterplot using tSNE
ce.plot_silhouette(embedding='tsne')
```

![](img/2a4c6675381dc0fa13ea3af791d6f998.png)

图 1\. 左面板：显示了带有检测到的簇和标签的轮廓评分图。右面板：散点图中样本按簇标签上色。两面板中的颜色和簇标签是匹配的。图片由作者提供。

图 1（右面板）的结果展示了 t-SNE 嵌入后的散点图，样本按簇标签上色。左面板展示了轮廓图，我们可以在其中视觉评估聚类结果的质量，如聚类的同质性、簇的分离情况以及通过聚类算法检测到的最佳簇数量。

此外，轮廓评分范围为-1 到 1（x 轴），其中接近 1 的评分表示簇内的数据点彼此非常相似，与其他簇的数据点不相似。簇 0、2、3 和 5 表示为良好分离的簇。接近 0 的轮廓评分表示簇之间的重叠或数据点与自身簇和邻近簇同样相似。接近-1 的评分则表明数据点与邻近簇的数据点更相似，而不是与自身簇的数据点。

条形的宽度代表了每个簇的密度或大小。较宽的条形表示簇较大且数据点更多，而较窄的条形表示簇较小且数据点较少。虚线红线（在我们的情况下接近 0）表示所有数据点的平均轮廓得分。它作为评估整体聚类质量的参考。平均轮廓得分高于虚线的簇被认为是分离良好的，而得分低于虚线的簇可能表示聚类效果较差。一般来说，好的聚类应该具有接近 1 的轮廓得分，表示簇的分离度良好。*然而，请注意，我们现在已经在高维空间中对数据进行了聚类，并在低维 2D 空间中的 t-SNE 嵌入后评估了聚类结果。投影可能会给现实带来不同的视角。*

另外，我们也可以先进行嵌入，然后在低维空间中对数据进行聚类（见下方代码部分）。现在我们将使用`Euclidean`距离度量，因为我们的输入数据不再是独热编码，而是来自 t-SNE 映射的坐标。经过拟合后，我们检测到 27 个簇的最佳数量，这比我们之前的结果多得多。我们可以看到簇评估得分（图 2）似乎有些不稳定。这与数据的结构以及是否能够形成最佳聚类有关。

```py
# Initialize library
from sklearn.manifold import TSNE
xycoord = TSNE(n_components=2, init='random', perplexity=30).fit_transform(dfhot.values)

# Initialize clusteval
ce = clusteval(cluster='agglomerative', metric='euclidean', linkage='complete', min_clust=5, max_clust=30)

# Clustering and evaluation
results = ce.fit(xycoord)

# Make plots
ce.plot()
ce.plot_silhouette()
```

![](img/694087a09265dfde2639fdeef35dcc6f.png)

图 2\. 簇评估得分（得分越高越好）。

![](img/05bf391346846286fd2d3f7d2c9cc5bf.png)

图 3\. 左侧面板：显示检测到的簇及标签的轮廓得分图。右侧面板：样本在簇标签上的散点图。两面板之间的颜色和簇标签是匹配的。图像由作者提供。

轮廓图现在显示出比以前更好的结果，表明簇的分离度更好。*在下一部分，我们将检测哪些特征与簇标签显著相关。*

> 确定最佳簇数后，接下来是具有挑战性的一步；理解哪些特征驱动了簇的形成。

# 识别簇标签背后的驱动特征。

此时，我们检测到了每个样本被分配的最佳聚类数。为了检测聚类标签背后的驱动特征，我们可以计算特征与检测到的聚类标签之间的统计关联。这将确定某些变量的特定值是否倾向于与一个或多个聚类标签同时出现。各种统计关联测量，如*卡方检验、费舍尔精确检验和超几何检验*，通常用于处理有序或名义变量。我将使用*超几何检验*来测试类别变量与聚类标签之间的关联，使用*Mann-Whitney U 检验*来测试连续变量与聚类标签之间的关联。这些检验在*HNET*中可以方便地实现，而该工具也被用于*clusteval*库中。通过`enrichment`功能，我们现在可以测试统计显著的关联。在这一步之后，我们可以使用散点图功能将丰富的特征绘制到聚类图上。

```py
# Enrichment between the detected cluster labels and the input dataframe
enrichment_results = ce.enrichment(df)

# [df2onehot] >Auto detecting dtypes.
# 100%|██████████| 18/18 [00:00<00:00, 53.55it/s]
# [df2onehot] >Set dtypes in dataframe..
# [hnet] >Analyzing [cat] Administrative...........................
# [hnet] >Analyzing [num] Administrative_Duration...........................
# [hnet] >Analyzing [cat] Informational...........................
# [hnet] >Analyzing [num] Informational_Duration...........................
# [hnet] >Analyzing [cat] ProductRelated...........................
# [hnet] >Analyzing [num] ProductRelated_Duration...........................
# [hnet] >Analyzing [num] BounceRates...........................
# [hnet] >Analyzing [num] ExitRates...........................
# [hnet] >Analyzing [num] PageValues...........................
# [hnet] >Analyzing [num] SpecialDay...........................
# [hnet] >Analyzing [cat] Month...........................
# [hnet] >Analyzing [cat] OperatingSystems...........................
# [hnet] >Analyzing [cat] Browser...........................
# [hnet] >Analyzing [cat] Region...........................
# [hnet] >Analyzing [cat] TrafficType...........................
# [hnet] >Analyzing [cat] VisitorType...........................
# [hnet] >Analyzing [cat] Weekend...........................
# [hnet] >Analyzing [cat] Revenue...........................
# [hnet] >Multiple test correction using holm
# [hnet] >Fin

# Make scatterplot and show the top n_feat enriched features per cluster.
ce.scatter(n_feat=2)
```

![](img/5583899ef181b39446c00e8b6760ad3a.png)

图 4. 带有统计显著特征的聚类标签散点图。图片由作者提供。

# 最后的话。

了解哪些特征驱动了聚类的形成对于从复杂数据集中提取有价值的见解至关重要。通过特征值为聚类上色的可视检查在处理具有大量特征、不同大小和密度的大型数据集时可能劳动密集且具有挑战性。*clusteval*库提供了一种定量方法，通过使用超几何检验和 Mann-Whitney U 检验分别对类别变量和连续变量与聚类标签之间的关联进行统计测试，从而评估聚类背后的驱动特征。

一个重要但具有挑战性的步骤是通过适当的数据标准化、距离度量选择和聚类评估来确保聚类的可信度。只有这样，聚类背后的驱动特征才能提供合理的信息。*在线购物者意图*的示例数据集展示了*clusteval*在识别聚类背后驱动特征方面的实际应用。总体而言，在聚类分析中结合定量方法来确定驱动特征可以极大地增强复杂数据集的可解释性和价值。

*保持安全，保持冷静。*

***干杯 E.***

*如果你喜欢这篇关于聚类的博客，欢迎* [*关注我*](http://erdogant.medium.com/) *以便随时了解我的最新内容，因为我写了更多类似的博客。如果你使用我的推荐链接，你可以支持我的工作，并且可以无限制地访问所有 Medium 博客。*

## 软件

+   [clusteval Colab 笔记本](https://erdogant.github.io/clusteval/pages/html/Documentation.html)

+   [clusteval Github/文档](https://erdogant.github.io/clusteval)

## 让我们联系吧！

+   [在 LinkedIn 上联系我](https://www.linkedin.com/in/erdogant/)

+   [在 Github 上关注我](https://github.com/erdogant)

+   [在 Medium 上关注我](https://erdogant.medium.com/)

# 参考资料

1.  E. Taskesen，*从数据到聚类：你的聚类足够好吗？*，2023 年 7 月 Medium。

1.  E. Taskesen，*通过重要关联网络探索和理解你的数据*，2021 年 8 月 Medium
