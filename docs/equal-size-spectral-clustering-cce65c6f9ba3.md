# 等规模谱聚类

> 原文：[https://towardsdatascience.com/equal-size-spectral-clustering-cce65c6f9ba3?source=collection_archive---------2-----------------------#2023-02-06](https://towardsdatascience.com/equal-size-spectral-clustering-cce65c6f9ba3?source=collection_archive---------2-----------------------#2023-02-06)

## 对这种流行算法的一种修改，构建了点数平衡的簇

[](https://anamabo3.medium.com/?source=post_page-----cce65c6f9ba3--------------------------------)[![卡门·阿德里安娜·马丁内斯·巴博萨博士](../Images/caad66f044af1131e17dc28ea2f48863.png)](https://anamabo3.medium.com/?source=post_page-----cce65c6f9ba3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cce65c6f9ba3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cce65c6f9ba3--------------------------------) [卡门·阿德里安娜·马丁内斯·巴博萨博士](https://anamabo3.medium.com/?source=post_page-----cce65c6f9ba3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa0526bfe8d0e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fequal-size-spectral-clustering-cce65c6f9ba3&user=Carmen+Adriana+Mart%C3%ADnez+Barbosa%2C+PhD.&userId=a0526bfe8d0e&source=post_page-a0526bfe8d0e----cce65c6f9ba3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cce65c6f9ba3--------------------------------) ·7 min read·2023年2月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcce65c6f9ba3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fequal-size-spectral-clustering-cce65c6f9ba3&user=Carmen+Adriana+Mart%C3%ADnez+Barbosa%2C+PhD.&userId=a0526bfe8d0e&source=-----cce65c6f9ba3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcce65c6f9ba3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fequal-size-spectral-clustering-cce65c6f9ba3&source=-----cce65c6f9ba3---------------------bookmark_footer-----------)![](../Images/df096faa71e7869134449b85e5f2fd07.png)

阿姆斯特丹餐馆的等规模聚类。所有图像由作者创建。

聚类是一种用于将具有相似特征的数据点分组的方法。它广泛应用于探索性数据分析，并且在许多应用中已被证明非常重要，如模式识别、市场和客户细分、推荐系统、数据压缩和生物数据分析等。

尽管有大量的聚类算法，但没有一种能够生成点数平衡的聚类。这种等大小的聚类在某些领域非常重要，例如在最后一公里配送领域，其中大量订单可以聚合成等大小的组，以改善配送路线并最大化车辆容量利用率。

鉴于需要进行等大小的聚类，我和几位同事扩展了所谓的[spectral clustering](https://en.wikipedia.org/wiki/Spectral_clustering)算法，以生成点数平衡的聚类。这个新算法基于数据点的地理信息构建具有相似点数的聚类。

完整代码可以在这个[GitHub 仓库](https://github.com/anamabo/Equal-Size-Spectral-Clustering)中找到。它旨在为数据科学社区做出贡献。如果你需要在地图上创建等大小的点聚类，可以试试！

# 等大小谱聚类：算法

等大小聚类由三个步骤组成：聚类初始化、计算每个聚类的邻居，以及平衡每个聚类中的点。我们来详细回顾一下每一步。

## 聚类初始化

在第一步中，我们通过使用[Scikit-learn的谱聚类算法实现](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.SpectralClustering.html#sklearn.cluster.SpectralClustering)来创建聚类。谱聚类在汇聚空间数据方面非常强大，因为它基于连接节点的边识别图中的社区。谱聚类算法特别适用于聚类遵循圆形对称性的点。如果你想了解更多关于这种方法的信息，可以阅读William Fleshman写的这篇精彩文章：

[](/spectral-clustering-aba2640c0d5b?source=post_page-----cce65c6f9ba3--------------------------------) [## 谱聚类

### 基础和应用

towardsdatascience.com](/spectral-clustering-aba2640c0d5b?source=post_page-----cce65c6f9ba3--------------------------------)

进行聚类初始化需要两个超参数：`nclusters`*，*即所需的聚类数量，以及`nneighbors`*，*即每个数据点的邻居数量。最后一个参数由谱聚类用于构建相似度矩阵。`nneighbors`的良好值在数据点的 7% 到 15% 之间。

![](../Images/0e6f34b8d39502322e755e6c5b352a7d.png)

步骤 1：通过谱聚类算法创建聚类。这些聚类的点数不相等。

## 计算每个聚类的邻居

一旦创建了簇，算法的第二步是计算每个簇的邻居。这一计算是如何进行的呢？通过估计每个数据点最近邻居的簇标签的众数。例如，如果点*x*属于簇*A*，而其大多数最近邻居属于簇*B*，这意味着簇*B*是簇*A*的一个邻居。

计算每个簇的邻居极其重要，因为簇的平衡是通过在邻近簇之间交换点来实现的。

![](../Images/7f193ec0ad84298e84cac32883213929.png)

步骤2：左侧面板：估计每个簇的邻居。在这个示例中，我们可以看到簇A和簇B是簇C的邻居。右侧面板：通过在邻近簇之间交换点来平衡簇。

## 平衡每个簇上的点

算法的最后一步是平衡每个簇上的点。如上所述，我们通过在邻近簇之间交换点来实现这一点。大簇将点转移到较小的邻近簇。在平衡过程中，我们的目标是使簇的大小大致等于*N/*`ncluster`，其中*N*是数据点的总数。

为了平衡簇的大小，我们定义了超参数`equity_fraction`。`equity_fraction`是一个定义在区间(0,1]内的数值，它限制了结果簇的平等程度。如果`equity_fraction`为零，则簇将保持初始大小。如果`equity_fraction`为一，则结果簇的大小将大致等于*N/*`ncluster`。

![](../Images/1062512ad4d5b3f02ba13e02d345b4e6.png)

步骤3：最终的簇大小取决于`equity_fraction`。左侧：如果`equity_fraction`为零，簇保持初始大小。右侧：如果`equity_fraction`为一，簇的点数大致相同。

让我们做一个小小的插入，定义一个叫做*簇分散*的量。簇分散被定义为簇内点距离的标准差。你可以将其视为簇内距离的一个稍作修改的版本。

`equity_fraction`影响初始簇的分散，因为点的交换增加了簇内点之间的距离。在这种情况下，我建议使用优化算法来找到最优的簇超参数，以最小化簇的分散。在下一节中，我会提到如何从Python代码中获取簇的分散。

## 其他功能

**重要的是要记住，相等大小的谱聚类可以用来创建空间点的聚合**。[这个仓库](https://github.com/anamabo/Equal-Size-Spectral-Clustering)提供了一个绘图功能，可以在你拥有数据点坐标的情况下使用。在下一节中，我们将看到这个功能的实际应用。

# 使用案例：阿姆斯特丹餐馆的聚类

假设你是荷兰的一家农场的主人，你希望将新鲜的高品质食物送到阿姆斯特丹的许多餐厅。你有6辆容量相同的车辆，这意味着它们能够大致送到相同数量的餐厅。

为了充分利用车辆的容量，你可以使用均等大小聚类来对餐厅进行分组，使得每辆车在不同餐厅之间的旅行距离不会过长。

首先，让我们读取餐厅的位置：

文件*restaurants_in_amsterdam.csv*包含了阿姆斯特丹中央车站8公里范围内的餐厅位置列表。你可以在[GitHub仓库](https://github.com/anamabo/Equal-Size-Spectral-Clustering)的*datasets*文件夹中找到这个文件。

从`coords`数据框中列出的地点，可以估算出一个包含每对点之间旅行距离的矩阵。这个矩阵的形状是(n_samples, n_samples)，并且它必须是对称的。**这个矩阵是均等大小聚类所接收的输入。**

现在我们可以运行均等大小光谱聚类。这跟调用`SpectralEqualSizeClustering`类一样简单：

在这个例子中，我们创建了6个簇。我们选择邻居数量`nneighbors`为输入数据集中点数量的10%。由于我们希望簇尽可能相等，因此我们将`equity_fraction`设置为1。

你可以看到如何通过调用`fit`方法获得每个数据点的簇标签。**重要提示：** 预测未包含在原始数据中的点的簇标签的函数尚未实现。如果你发现这段代码对你的工作有帮助，我鼓励你开发这个功能！

上述获得的簇标签可以添加到数据框`coords`中，以便在地图上绘制结果簇：

运行上述代码后，我们得到一个包含所有簇的交互式图，如图所示：

![](../Images/572519a10fafc61245b48c27cad8d5d1.png)

使用均等大小光谱聚类代码创建的簇。

在那个图中，你可以选择每个簇以单独进行可视化：

![](../Images/2d4c30467f2d84701c5e34a40ece74a9.png)

在上述用例中，超参数的优化是不必要的。然而，正如我之前提到的，如果需要，可以使用优化方法。在这种情况下，你可以使用作为优化指标的属性`clustering.total_cluster_dispersion`，它是所有簇离散度的总和。通过最小化这个量，得到的簇会更紧凑。

# 主要信息

在这篇博客中，我介绍了一个修改过的光谱聚类代码，该代码生成在点数上平衡的簇。这个算法可以用来生成空间点的均等聚合，并且在最后一公里配送领域的一些流程改进中可能也会有用。

等大小谱聚类的重要考虑因素如下：

+   输入数据必须是一个与数据点坐标相关的对称距离矩阵。

+   聚类代码的超参数包括期望的簇数（`nclusters`）、每个数据点的邻居数（`nneighbors`）以及确定簇大小是否相等的一个比例（`equity_fraction`）。你可以使用任何优化算法来找到最小化总簇离散度（`total_cluster_dispersion`）的最佳参数。

+   等大小聚类也可以用于非空间数据，但尚未对这一目的进行测试。如果你想进行实验，请定义一个度量来创建代码所需的对称距离矩阵作为输入。务必先对变量进行归一化或标准化。

+   该代码目前还没有`prediction`方法，但如果你觉得这个代码有用，欢迎贡献。

希望你喜欢这篇文章。再次感谢阅读！

*致谢：等大小谱聚类是与 Mor Verbin、Lilia Angelova 和 Ula Grzywna 合作开发的。真是一个了不起的数据团队！*
