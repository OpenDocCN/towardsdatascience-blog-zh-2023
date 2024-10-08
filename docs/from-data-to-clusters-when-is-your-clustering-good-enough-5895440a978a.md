# 从数据到聚类：你的聚类何时足够好？

> 原文：[`towardsdatascience.com/from-data-to-clusters-when-is-your-clustering-good-enough-5895440a978a`](https://towardsdatascience.com/from-data-to-clusters-when-is-your-clustering-good-enough-5895440a978a)

## 通过聚类方法可以发现隐藏的宝石，但你需要正确的聚类方法和评估方法来创建合理的聚类。学习如何在四个步骤中找到它们。

[](https://erdogant.medium.com/?source=post_page-----5895440a978a--------------------------------)![Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----5895440a978a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5895440a978a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5895440a978a--------------------------------) [Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----5895440a978a--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----5895440a978a--------------------------------) ·阅读时间 17 分钟·2023 年 4 月 26 日

--

![](img/4760f872473a60131b462ef1392c5bbe.png)

图片来源：[Shubham Dhage](https://unsplash.com/@illustratiions?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/photos/3nwYFtexa4o?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

通过无监督聚类分析，我们可以对具有相似模式的观察数据进行分组，揭示数据中的（隐藏）趋势。使用聚类评估方法有助于确定聚类倾向、质量和最佳聚类数。**在本博客中，我们将深入探讨聚类评估方法，学习如何解释这些方法，并选择适合你用例的聚类方法。** 我们将从探讨聚类和评估方法的基础知识开始，这些方法用于评估聚类的质量，包括流行的技术，如*轮廓系数*、*Davies-Bouldin 指数*和*导数法*。通过使用玩具示例数据集，我们将研究每种评估方法的优缺点，提供关于如何解释其结果的实际见解。所有分析都使用[*clusteval 库*](https://erdogant.github.io/clusteval/)。

# 无监督聚类。

使用无监督聚类，我们的目标是确定数据中的“*自然*”或“*数据驱动*”的组，而不使用有关标签或类别的先验知识。使用不同的无监督聚类方法的挑战在于，它会导致样本的不同划分，因此会有不同的分组，因为每种方法隐式地对数据施加了结构。因此，问题出现了；*什么是“好的”聚类？* 图 1A 展示了一组在二维空间中的样本。*你看到多少个簇？* 我会说有两个簇而不使用任何标签信息。*为什么？* 因为点之间的距离很小，而杂乱点之间的“*间隙*”相对较大。

![](img/d02888b3fc2b82b9d581e873046985bd.png)

图 1。图像由作者提供

考虑到这一点，我们可以将对“*簇*”的直观理解转化为数学陈述；所谓簇内的样本方差应该很小（***簇内方差*** σW***，红色和蓝色***），而同时簇之间的方差应该很大（***簇间方差***，σB），如图 1B 所示。样本之间的距离（或内在关系）可以用***距离度量***（例如，*欧几里得*距离）来测量，并存储在所谓的*不相似矩阵*中。然后，可以使用***连接类型***计算样本组之间的距离（用于层次聚类）。

## 距离度量

最著名的距离度量是*欧几里得距离*。尽管它在许多方法中被设置为默认度量，但它并不总是最佳选择。各种距离度量的示意图见图 2。

> *理解距离度量的数学性质，以便它适合数据并与研究问题对齐。*

![](img/d6aeb16a4aa3edb061f15e0af6484c68.png)

图 2：最受欢迎的距离度量的示意图。图像由作者提供。

## 连接类型。

*层次*聚类的过程涉及将样本分组到一个更大的簇中的方法。在这个过程中，需要计算两个子簇之间的距离，不同类型的连接描述了簇之间的连接方式（图 3）。

![](img/61da71da6c25175de6519d67fa336a96.png)

图 3：连接类型。图像由作者提供。

简而言之，两个簇之间的***单链接***是它们两个最接近样本之间的距离。它产生一个长链，因此适用于异常检测或*蛇形簇*的聚类。两个簇之间的***完全链接***是它们两个最远样本之间的距离。直观地说，两个最远的样本之间的相似度不可能比其他相似度较低的样本对更大。这迫使簇变得球形，并且通常在边界处有“*紧凑*”的轮廓，但内部不一定紧凑。两个簇之间的***平均链接***是一个簇中所有对象与另一个簇中所有对象之间距离的算术平均值。***质心链接***是簇的几何质心之间的距离。换句话说，质心链接基于数据点的质心连接簇，而平均链接则考虑了两个簇中所有对象之间的距离。选择*度量标准*和*链接类型*时要小心，因为它直接影响最终的聚类结果。

# 从数据到簇的 4 个步骤。

为了确定数据驱动的簇，这些簇可能包含新的或未知的信息，我们需要仔细地执行四个构成步骤，从输入数据集到合理的簇。像***distfit***和***clusteval***这样的库可以帮助完成这个过程。

> 提出合理的样本分组需要的不仅仅是盲目运行聚类算法。

## 第一步：调查数据的基础分布。

调查数据的基础分布是一个重要的步骤，因为聚类算法依赖于数据的统计特性来识别模式并将相似的数据点分组在一起。通过理解数据的分布，如其均值、方差、偏度和峰度，我们可以做出明智的决定，选择合适的聚类算法以及如何设置其参数以实现最佳结果。此外，调查数据分布可以提供有关在聚类之前应用适当的归一化或缩放技术的见解。***虽然监督方法，如树模型，能够处理混合数据集，但聚类算法则设计用于处理同质数据***。***这意味着所有变量应该具有类似的类型或测量单位。*** 归一化或缩放是一个重要的步骤，因为聚类算法基于相似性使用度量标准对数据点进行分组。有关调查基础数据分布的更多细节可以在这里阅读 [[1]](/how-to-find-the-best-theoretical-distribution-for-your-data-a26e5673b4bd)：

[](/how-to-find-the-best-theoretical-distribution-for-your-data-a26e5673b4bd?source=post_page-----5895440a978a--------------------------------) ## 如何找到适合数据的最佳理论分布

### 了解潜在的数据分布是数据建模的关键步骤，并且有许多应用，例如异常检测……

[towardsdatascience.com

## 第 2 步：对簇的密度和期望簇大小做出有根据的猜测。

设定对簇密度、形状和簇数量的期望将有助于选择合适的聚类算法和参数设置，以实现期望的结果。此外，设定期望可以在解释和向利益相关者传达聚类结果时提供更多的信心和有效性。例如，如果目标是识别数据集中稀有的异常，并且聚类结果产生了一个非常低密度的小簇，这可能表明存在这样的异常。然而，并非总是可以设定关于簇数量或密度的期望。此时，我们需要根据与数据的统计属性和研究目标相匹配的数学属性选择聚类方法。

## 第 3 步：选择聚类方法。

选择聚类方法取决于第 1 到第 4 步，但我们还应该考虑可扩展性、鲁棒性和易用性等因素。例如，在生产环境中，我们可能需要与实验用例不同的属性。有几种流行的聚类方法，如 *K-means、层次聚类和基于密度的聚类算法*，每种方法都有其自身的假设、优点和局限性（见下文总结）。选择聚类方法后，我们可以开始对数据进行聚类并评估其性能。

> **K-means** 假设簇是球形的、大小相等的，并且具有相似的密度。它需要事先指定簇的数量（k）。注意，最佳簇数量的检测在 clusteval 库中会自动进行。
> 
> **层次聚类**通过基于距离或相似性度量递归地合并簇来构建树状结构的簇。它是聚合性的（自底向上）且不需要事先指定簇的数量。
> 
> **基于密度的聚类算法**，例如 DBSCAN（基于密度的空间聚类应用于噪声），将密集的点组在一起，并且在簇之间具有较低的密度。它们不假设簇的任何特定形状或大小，并且可以识别任意形状和大小的簇。基于密度的聚类特别适用于识别不同密度的簇和将异常点检测为噪声。然而，它们需要调整超参数，例如密度阈值，并且对参数选择敏感。
> 
> 不同的聚类方法可能会导致样本的不同划分，从而产生不同的分组，因为每种方法都隐含地对数据施加了一种结构。

## 第 4 步：簇评估。

簇评估是为了评估*聚类倾向、质量和最佳簇数*。有各种聚类评估方法，其中最流行的是***clusteval***库中包含的，即*Silhouette 分数、Davies-Bouldin 指数和导数（或肘部）方法*。使用这些技术的难点在于，聚类步骤及其评估通常是交织在一起的，*clusteval 库*内部处理了这一点。在下一节中，我将深入探讨各种方法并测试它们的表现。

> 在聚类过程中应进行簇评估，以为每个聚类分配分数，并实现结果之间的有意义比较。

## 下一步：从簇到洞察。

找到最佳簇数后的步骤是确定簇背后的驱动特征。一个很好的方法是使用**HNET**[2]进行富集分析，我们可以使用超几何检验和 Mann-Whitney U 检验来测试簇标签与特征之间的显著关联。更多细节可以在这里找到：

[](/explore-and-understand-your-data-with-a-network-of-significant-associations-9a03cf79d254?source=post_page-----5895440a978a--------------------------------) ## 通过显著关联网络探索和理解数据。

### 探索以理解您的数据可以决定一个项目的成功与否！

towardsdatascience.com

# Clusteval 库。

关于[*clusteval*](https://erdogant.github.io/clusteval/)库的几句说明，该库用于所有分析。[*clusteval 库*](https://erdogant.github.io/clusteval/)旨在应对评估簇和创建有意义图表的挑战。在[*之前的博客*](https://medium.com/towards-data-science/a-step-by-step-guide-for-clustering-images-4b45f9906128)[3]中，我展示了它在图像识别中的强大功能，用于图像的聚类。*clusteval*库包含了最受欢迎的评估方法，即*Silhouette 分数、Davies-Bouldin 指数和导数（或肘部）方法*，用于评估*层次聚类、K-means、DBSCAN 和 HDBSCAN*的聚类。它可以处理所有数据类型，例如连续数据、分类数据和离散数据集。只需确保 1. 数据正确归一化。2. 所有变量具有相似的类型或测量单位。3. 选择适当的距离度量和评估方法。

# ***Clusteval 功能***

+   *它包含了针对轮廓系数、Davies-Bouldin 指数以及用于评估层次聚类、K 均值、DBSCAN 和 HDBSCAN 的导数方法的最受欢迎的集群评估管道。*

+   *它包含创建有意义图形的功能；集群的最佳数量、带系数的轮廓图、散点图和树状图。*

+   *它包含了确定集群背后驱动特征的功能。*

+   *它可以处理所有数据类型，如连续型、类别型和离散数据集。只需确保所有变量具有相似的类型或测量单位。*

+   [*开源*](https://erdogant.github.io/clusteval/)

+   [*文档页面*](https://erdogant.github.io/clusteval/)

# 聚类方法的选择很重要。

不同的集群技术可以导致不同的分组，因为它们对不同的密度和组大小的响应各异。因此，确定正确的集群数量可能具有挑战性，尤其是在高维空间中，当视觉检查不可行时。通过***集群评估***方法，我们可以进行调查并进行一些回测，以确定最佳的集群数量并描述集群趋势。为了展示如何确定集群数量，我创建了七个玩具示例数据集（灵感来源于[scikit-learn](https://scikit-learn.org/stable/auto_examples/cluster/plot_cluster_comparison.html)），这些数据集具有不同的密度、形状和样本大小，即*蛇形集群*、*不同密度和大小的集群*以及*均匀分布的样本*（图 4）。每个玩具示例包含 1000 个样本和两个特征。因此，它可以在低维空间中工作时提供直观感受，例如在特征提取步骤之后，例如*PCA、t-SNE 或 UMAP*步骤。

![](img/cadecdd0a883e84d146c5f547ebd4bfd.png)

图 4\. 七个玩具示例数据集，具有不同的密度、形状和大小。样本根据其实际标签进行着色。图片由作者提供。

对于这七个示例数据，我们将使用在*clusteval*中可用的评估方法进行集群分析；*轮廓系数、Davies-Bouldin 指数和导数（或肘部）方法*。集群本身使用*K 均值、层次聚类（单一、完全和 Ward 距离）和基于密度的聚类算法（DBSCAN）*进行。

*如果你觉得这篇文章对你有帮助，请使用我的* [*推荐链接*](https://medium.com/@erdogant/membership) *继续无缝学习并注册 Medium 会员。此外，* [*关注我*](http://erdogant.medium.com/) *以保持更新我的最新内容！*

# 使用轮廓系数进行集群评估。

***轮廓系数法***是一种衡量样本与自身簇（内聚性）相比于其他簇（分离性）的相似度的指标。得分范围在[-1, 1]之间，高值表示样本或观察值与自身簇匹配良好，而与相邻簇匹配较差。这是一种样本级的方法，这意味着对于每个样本，都会计算一个*轮廓系数*，如果大多数样本的值很高，则认为聚类配置是合适的。如果许多点的值较低或为负，则可能是聚类配置的簇数过多或过少。*轮廓系数法与距离度量无关，这使得它成为一种有吸引力且通用的方法。*

让我们看看它如何在一个简单的数据集上对具有相似密度和大小的样本进行分组，即***blob 数据集***。在下面的代码部分，我们将初始化*DBSCAN*方法与*轮廓系数*评估方法。请注意，我们可以使用任何其他聚类方法，如下一节所示。对于*DBSCAN*，在 epsilon 参数上进行搜索，并检索具有最高*轮廓系数*的簇数量。具有*轮廓系数*的*DBSCAN*正确检测出 4 个簇，并且*样本级轮廓系数*（图 5 中间）看起来稳定（簇的高分且形状均匀）。这是预期的，因为簇中的样本远离其他簇，并且接近各自簇的中心。

```py
# Import library
from clusteval import clusteval

# Initialize
cl = clusteval(cluster='dbscan', evaluate='silhouette', max_clust=10)
# Import example dataset
X, y = cl.import_example(data='blobs', params={'random_state':1})

# find optimal number of clusters
results = cl.fit(X)

# Make plot
cl.plot()
# Show scatterplot with silhouette scores
cl.scatter()

# [clusteval] >INFO> Fit with method=[dbscan], metric=[euclidean], linkage=[ward]
# [clusteval] >INFO> Gridsearch across epsilon..
# [clusteval] >INFO> Evaluate using silhouette..
# [clusteval] >INFO: 100%|██████████| 245/245 [00:07<00:00, 33.88it/s]
# [clusteval] >INFO> Compute dendrogram threshold.
# [clusteval] >INFO> Optimal number clusters detected: [4].
# [clusteval] >INFO> Fin.
# [clusteval] >INFO> Estimated number of n_clusters: 4, average silhouette_score=0.812 
```

![](img/9ce1b14279bf38b55c39d8b15f314d49.png)

图 5\. 左侧：优化 Epsilon 参数以检测最佳簇数量。中间：每个样本的轮廓系数。右侧：确定的簇标签。图片由作者提供。

这只是一个数据集和一种聚类方法（*DBSCAN*）的评估。让我们在七个玩具示例数据集和五种聚类方法中进行迭代，使用*轮廓系数*方法确定最佳簇的数量。结果如图 6 所示，其中上半部分显示了按最佳簇数量着色的散点图，下半部分显示了不同簇数量的*轮廓系数*图。

> 图 6 的结论是**如果**我们选择适合数据集的聚类方法，**那么**我们可以很好地检测出正确的簇的数量。这可能看起来是一个微不足道的评论，但选择合适的聚类方法可能具有挑战性，因为你需要仔细执行前面章节中描述的第 1 到第 4 步。

***如果我们由于高维度无法直观地检查结果，我们仍然可以获取样本分布和簇形成的直觉。*** 如果各组明显分离，如*蛇形簇*和*斑点簇*，我们可以期待在特定簇数上出现*轮廓系数*的强峰值，并且使用正确的聚类方法。但当数据变得更嘈杂时，如*球形*数据集，多重峰值可能会出现，且轮廓系数的斜率可能会逐渐增加而没有明显的峰值。这通常暗示簇的形成不够强烈。通过*clusteval*中的`min_clust`和/或`max_clust`参数，我们可以限定搜索空间并确定相对较高的峰值。这个检查和限定的手动优化步骤被称为回测。图 6 中的一个有趣观察是，当数据变得嘈杂时，*DBSCAN*聚类方法的得分显示出涡流增加（见图 6，第二列）。原因是检测到许多离群点并开始形成更小的簇，后来这些离群点可能再次成为较大簇的一部分。

![](img/c17caf4d8610e553ab68511dc86d6085.png)

图 6\. 使用*轮廓系数*方法检测五种聚类方法和七个玩具示例数据集的最佳簇数。图片由作者提供。

# 使用***戴维斯-鲍尔丁指数***进行聚类评估。

***戴维斯-鲍尔丁指数***（*DBindex*）可以直观地描述为簇内距离与簇间距离的比率度量。得分在[0, 1]之间，其中较低的值表示簇之间更紧密和更好的分离。*较低的得分因此被认为更好。* 与*轮廓系数方法*不同，*戴维斯-鲍尔丁指数****不是***逐样本度量，因此使用速度更快。*DBindex*的一个缺点是它常常过度预估，因为随着簇数增加，较低的值更为常见。这可以通过得分逐渐下降的斜率没有明显低得分来清晰地看到。

让我们看看如何对具有相似密度和大小的样本进行聚类，以获取*戴维斯-鲍尔丁指数*性能的一些直觉。在下面的代码部分，我们将用*戴维斯-鲍尔丁指数*评估方法初始化*凝聚*聚类方法。如果我们在***clusteval***中使用`max_clust=10`限制搜索空间，它能正确检测到 4 个簇（图 7 左侧面板）。然而，如果我们不限制搜索空间，得分的斜率将逐渐下降，从而导致检测到错误的簇数（图 7 右侧面板）。

```py
# Import
from clusteval import clusteval

# Initialize
cl = clusteval(cluster='agglomerative', evaluate='dbindex', max_clust=10)
# Import example dataset
X, y = cl.import_example(data='blobs', params={'random_state':1})

# find optimal number of clusters
results = cl.fit(X)

# Make plot
cl.plot(figsize=(12, 7))
# Show scatterplot with silhouette scores
cl.scatter()

# [clusteval] >INFO> Fit with method=[agglomerative], metric=[euclidean], linkage=[ward]
# [clusteval] >INFO> Evaluate using dbindex.
# [clusteval] >INFO: 100%|██████████| 18/18 [00:00<00:00, 120.32it/s]
# [clusteval] >INFO> Compute dendrogram threshold.
# [clusteval] >INFO> Optimal number clusters detected: [4].
# [clusteval] >INFO> Fin.
```

![](img/a5cff6602068b80746a6bcbee38c2edd.png)

图 7\. *Davies-Bouldin 指数* 对不同数量簇的评分。左侧面板：最多 10 个簇。右侧面板：最多 20 个簇。图片由作者提供。

对于*Davies-Bouldin 指数*方法，我们也将遍历这七个示例数据集，但我将仅包括*Agglomeration*簇方法的单一、完整和 Ward 连接。请注意，*DBSCAN* 与*Davies-Bouldin 指数*不兼容。在所有示例中，搜索空间有一个限制（`max_clust=10`），以防止过度估计（图 8）。尽管我们限制了搜索空间，但仍然存在一个逐渐下降的斜率，在许多示例数据集中导致不正确的簇数量。这在*蛇形*和*各向异性*簇（前三行）中尤为明显。一种解决方案是进一步降低最大簇数量（`max_clust`）。对于*Blobs, Globular, 和 Density*示例数据集，可以从散点图中看出正确的簇趋势，但最佳簇数量经常过度估计。在这里，如果我们能够进一步将最大簇数量限制为 6 或 7，结果会有所改善。

![](img/c4505c12d525f7f7ffe8f6237ddf789b.png)

图 8\. 使用*Davies-Bouldin 指数方法*检测七个示例数据集的最佳簇数量。图片由作者提供。

# 使用***导数方法***进行簇评估。

***导数方法*** 比较每个簇合并的高度与平均值，并通过计算前几个层级深度的标准差进行归一化。最后，导数方法返回基于所选层次聚类方法（红色垂直线）的最佳截断的簇标签。换句话说，当蓝色线中的“*肘部*”很明显时，可以在橙色线中看到一个强峰值。让我们看看*导数方法*在聚类具有类似密度和大小的样本方面的效果。在下面的代码部分，我们将初始化*Agglomerative*聚类方法，并使用*导数*评估方法。与*Davies-Bouldin 指数*相比，我们不需要限制搜索空间（图 9），可以轻松检测到 4 个簇。

![](img/b1ad873d3d73519694841b9ec53311aa.png)

图 9\. *导数方法* 对不同数量簇的评分。图片由作者提供。

```py
# Import library
from clusteval import clusteval

# Initialize
cl = clusteval(cluster='agglomerative', evaluate='derivative', max_clust=20)
# Import example dataset
X, y = cl.import_example(data='blobs', params={'random_state':1})

# find optimal number of clusters
results = cl.fit(X)

# Make plot
cl.plot(figsize=(12, 8))
# Show scatterplot with silhouette scores
cl.scatter()
```

我们将遍历这七个示例数据集，并使用*导数*方法（图 8）确定最佳簇数量。可以看出，*导数*方法经常导致不正确的簇数量。如果我们仔细观察*导数*线（橙色），它在许多数据集中显示出一个小峰值，指向一个较弱的肘部，从而可能表示数据噪声。这表明估计的簇数量可能不可靠。

![](img/155446ac31004d51ca3260a4c0058da8.png)

图 9\. 使用*导数法*检测七个玩具示例数据集的最佳聚类数量。图片由作者提供。

> *请注意，聚类评估方法很容易被欺骗，因为分数可能会随着聚类数量的增加而逐渐提高。*

# 结束语。

我展示了不同的聚类方法如何响应不同的数据集，以及确定最佳聚类数量需要多个步骤。此外，不同的聚类方法将产生不同的分组，因为它们隐式地对数据施加了结构，从而影响样本的划分。***clusteval 包*** 可以帮助进行聚类调查和回测，其结果可以通过各种绘图功能进行探索。

一般来说，*K-means* 聚类效果很好，特别是对于*球形*、平衡的和*球状*聚类。然而，它不适合检测具有不同*密度*或*蛇形*聚类的情况。另一方面，*DBSCAN* 是一个很好的全能选手，可以在大多数情况下检测到正确的聚类数量。只是要注意，聚类倾向可能是正确的，但检测到的确切聚类数量可能不对。这在*球状*聚类中尤其如此，因为许多（组）样本可能被标记为离群点。*DBSCAN* 的一个缺点是计算量大，特别是在优化 epsilon 参数时。相反，对于*蛇形*聚类或离群点检测，使用单链聚类；对于噪声*球状*聚类，使用 ward 或完全链路，*K-means* 适用于*球形*、平衡的和*球状*聚类。

对于聚类评估方法，*轮廓系数*有很多优势。它是一种逐样本的度量，可以与任何距离度量一起使用，使其成为一种有吸引力且多用途的方法。另一方面，*Davies-Bouldin 指数*和*导数法*需要更多的迭代过程进行回测和调查，因为它们往往超出正确的聚类数量。*Davies-Bouldin 指数*在处理具有重叠聚类的数据集时被描述为更具鲁棒性，并且计算效率更高，因为它不需要计算所有数据点之间的成对距离。

总结来说，聚类评估方法的选择取决于数据集的特定特征、分析目标和应用要求。你可以使用多种方法进行回测，并比较其结果，以便根据问题的具体背景做出明智的决定。

*保持安全，保持警觉。*

***干杯 E.***

*如果你觉得这篇文章对你有帮助，请使用我的* [*推荐链接*](https://medium.com/@erdogant/membership) *继续无限制地学习，并注册 Medium 会员。此外，* [*关注我*](http://erdogant.medium.com/) *以保持最新内容！*

## 软件

+   [clusteval Colab 笔记本](https://erdogant.github.io/clusteval/pages/html/Documentation.html)

+   [clusteval Github/文档](https://erdogant.github.io/clusteval)

## 让我们联系！

+   [在 LinkedIn 上与我联系](https://www.linkedin.com/in/erdogant/)

+   [在 Github 上关注我](https://github.com/erdogant)

+   [在 Medium 上关注我](https://erdogant.medium.com/)

# 参考文献

1.  E. Taskesen，*如何找到最适合你数据的理论分布*，2023 年 2 月，*Towards Data Science, Medium*。

1.  E. Taskesen，*通过显著关联网络探索和理解你的数据*，2021 年 8 月，*Towards Data Science, Medium*

1.  *E. Taskesen,* [*图像聚类的逐步指南*](https://medium.com/towards-data-science/a-step-by-step-guide-for-clustering-images-4b45f9906128)*，2021 年 12 月，Towards Data Science, Medium。*
