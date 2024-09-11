# 通过解缠改进k-Means聚类

> 原文：[https://towardsdatascience.com/improving-k-means-clustering-with-disentanglement-caf59a8c57bd?source=collection_archive---------4-----------------------#2023-11-26](https://towardsdatascience.com/improving-k-means-clustering-with-disentanglement-caf59a8c57bd?source=collection_archive---------4-----------------------#2023-11-26)

## 学习数据集类别邻域结构可以改善聚类效果

[](https://medium.com/@afagarap?source=post_page-----caf59a8c57bd--------------------------------)[![Abien Fred Agarap](../Images/8f616478044e721a31c1c1df3d8e8b62.png)](https://medium.com/@afagarap?source=post_page-----caf59a8c57bd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----caf59a8c57bd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----caf59a8c57bd--------------------------------) [Abien Fred Agarap](https://medium.com/@afagarap?source=post_page-----caf59a8c57bd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F782adfd45f71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimproving-k-means-clustering-with-disentanglement-caf59a8c57bd&user=Abien+Fred+Agarap&userId=782adfd45f71&source=post_page-782adfd45f71----caf59a8c57bd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----caf59a8c57bd--------------------------------) · 8分钟阅读 · 2023年11月26日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcaf59a8c57bd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimproving-k-means-clustering-with-disentanglement-caf59a8c57bd&source=-----caf59a8c57bd---------------------bookmark_footer-----------)

> 这篇文章是关于 A.F. Agarap 和 A.P. Azcarraga 于2020年国际联合神经网络大会（IJCNN）上发表的论文“[通过解缠内部表示改进k-Means聚类](https://arxiv.org/abs/2006.04535)”的配套文章

# 背景

聚类是一种无监督学习任务，它将一组对象分组，使得同一组中的对象相互之间的相似性高于其他组中的对象。这是一个被广泛研究的任务，其应用包括但不限于数据分析和可视化、异常检测、序列分析以及自然语言处理。

与其他机器学习方法类似，聚类算法严重依赖特征表示的选择。在我们的工作中，我们通过解缠结提高了特征表示的质量。

我们将*解缠结*定义为类别不同的数据点相对类别相似的数据点之间的距离。这类似于[Frosst et al. (2019)](https://arxiv.org/abs/1902.01889)中对上述术语的处理。因此，在表示学习过程中最大化解缠结意味着类别相似的数据点之间的距离被最小化。

![](../Images/da008ac7da864e5fa797bb0eb272a952.png)

图由作者提供。

这样做可以保留数据集中示例的类别成员，即数据点在特征空间中如何根据其类别或标签分布。如果类别成员被保留，我们将拥有一个特征表示空间，其中最近邻分类器或聚类算法的表现将会很好。

## 聚类

聚类是一种机器学习任务，旨在发现数据点的分组，其中组内的点在彼此之间的相似性比不同组的点之间的相似性更高。

![](../Images/07b4c01ef371936a078d891401134dca.png)

图由作者提供。

像其他机器学习算法一样，聚类算法的成功依赖于特征表示的选择。对于使用的数据集，一种表示可能优于另一种。然而，在深度学习中情况并非如此，因为特征表示作为神经网络的隐式任务被学习。

## 深度聚类

因此，近年来的工作如 [Deep Embedding Clustering 或 DEC](https://arxiv.org/abs/1511.06335) 和 [Variational Deep Embedding 或 VADE](https://arxiv.org/abs/1611.05148)（2016年），以及 [ClusterGAN](https://arxiv.org/abs/1809.03627)（2018年），都利用了神经网络的特征表示学习能力。

![](../Images/7426b046529309ac105122d3f9e8e05f.png)

图源自 [DEC](https://arxiv.org/abs/1511.06335)（Xie 等人，2016年）。DEC的网络结构。

我们不会在本文中详细讨论它们，但这些工作的基本思想本质上是相同的，即通过深度神经网络同时学习特征表示和聚类分配。这种方法被称为*深度聚类*。

# 动机

> 我们能否在聚类之前保留数据点的类别成员？

尽管深度聚类方法同时学习聚类分配和特征表示，但它们并没有明确设定保留数据集的类别邻域结构。这成为了我们研究的动机，即我们能否在学习到的深度网络表示上执行聚类，同时保留数据集的类别邻域结构。

2019年，[Not Too Deep](https://ieeexplore.ieee.org/abstract/document/9413131/)或N2D聚类方法被提出，他们学习了数据集的潜在代码表示，进一步使用t-SNE、Isomap和UMAP等技术搜索潜在流形。所得到的流形是数据集的聚类友好表达。因此，在流形学习之后，他们将学习到的流形用作聚类的数据集特征。通过这种方法，他们能够获得良好的聚类性能。与深度聚类算法相比，N2D是一个相对简单的方法，我们提出了类似的方法。

# 学习解耦表示

我们还使用自编码器网络学习数据集的潜在代码表示，然后将该表示用于聚类。我们在如何学习更友好于聚类的表示方面有所不同。我们提议解耦自编码器网络学习到的表示，而不是使用流形学习技术。

![图片](../Images/6ba7a98d115d4688d3a2cde8c5743698.png)

作者绘制的图。最小化类似类别数据点之间的距离，从而更好地分离类别不同的数据点。

为了解开学习到的表示，我们使用软最近邻损失或SNNL来衡量类似类别数据点的缠结程度。这个损失函数的作用是在神经网络的每个隐藏层中最小化类似类别数据点之间的距离。Frosst、Papernot和Hinton在他们2019年的论文中使用了一个被*T*表示的固定温度值。温度因子决定了如何控制对点对距离的重视程度，例如，在低温下，损失由小距离主导，而在实际上，远离的表示之间的距离变得不太相关。他们在他们的论文中用SNNL进行了判别和生成任务。

![图片](../Images/a2f9b75901e8cacb14d6da99992afd35.png)

作者绘制的图。我们从[Neelakantan等人，2015](https://arxiv.org/abs/1511.06807)得到了指数，但它可以是任何值。

在我们的工作中，我们使用了SNNL进行聚类，并引入了一个退火温度的使用，而不是固定温度。我们的退火温度是关于训练时期数的反函数，由τ表示。

![图片](../Images/475eee7c704fde65f7a6181d3d176e96.png)

作者提供的图。比较了使用退火温度和固定温度的软最近邻损失。我们从高斯分布中采样并随机标记了300个数据点，并对它们进行软最近邻损失的梯度下降。左侧的图展示了标记点的初始状态。我们可以看到，从第20个周期到第50个周期，潜在编码中的簇分离情况，使得类之间更加孤立。我们在[论文](https://arxiv.org/abs/2006.04535)中展示了在基准数据集上的解耦表示。

在从高斯分布中随机抽取并标记的300个数据点上运行梯度下降，我们可以看到使用我们的退火温度进行SNNL时，与使用固定温度相比，我们发现了解耦速度更快。正如我们所见，即使在第20个周期，使用退火温度时，类相似的数据点会比使用固定温度时更聚集在一起或纠缠在一起，这一点也通过SNNL值的数值结果得到了证明。

# 我们的方法

我们的贡献包括使用SNNL进行特征表示的解耦，用于SNNL的退火温度，以及相比于深度聚类方法的更简单的聚类方法。

我们的方法可以总结如下，

1.  我们训练了一个自动编码器，其复合损失包括二进制交叉熵作为重建损失，以及软最近邻损失作为正则化器。为了保持数据集的类邻域结构，我们最小化了自动编码器每个隐藏层的SNNL。

1.  训练后，我们使用数据集的潜在编码表示作为数据集特征进行聚类。

# 在解耦表示上的聚类

我们的实验配置如下，

1.  我们使用了[MNIST](https://paperswithcode.com/dataset/mnist)、[Fashion-MNIST](https://github.com/zalandoresearch/fashion-mnist)和[EMNIST](https://paperswithcode.com/dataset/emnist)平衡基准数据集。数据集中的每个图像都被展平为一个784维的向量。我们使用其真实标签作为伪聚类标签，以衡量我们模型的聚类准确性。

1.  由于计算限制和保持方法简单，我们没有进行超参数调优或其他训练技巧。

1.  其他正则化器如dropout和batch norm被省略，因为它们可能会影响解耦过程。

1.  我们计算了模型在四次运行中的平均性能，每次运行有不同的随机种子。

## 聚类性能

然而，自动编码和聚类都是无监督学习任务，而我们使用的SNNL是一种利用标签来保持数据集类邻域结构的损失函数。

![](../Images/9b7eb3abb26050f8d44da63e00773af1.png)

作者提供的图。

鉴于此，我们通过使用基准数据集中的少量标记训练数据来模拟标记数据的缺乏。我们使用的标记示例数量是随意选择的。

我们从文献中获取了 DEC、VaDE、ClusterGAN 和 N2D 的报告聚类准确性作为基线结果，在上表中，我们可以看到我们的发现总结，其中我们的方法优于基线模型。

注意，这些结果是每个数据集四次运行中的最佳聚类准确性，因为文献中的基线结果也是各自作者报告的最佳聚类准确性。

## 可视化解耦表示

为进一步支持我们的发现，我们可视化了我们网络对每个数据集的解耦表示。

对于 EMNIST Balanced 数据集，我们随机选择了 10 个类别以便于更清晰的可视化。

从这些可视化中，我们可以看到，每个数据集的潜在代码表示确实通过具有明确的聚类变得更加聚类友好，这从聚类分散度中可以看出。

![](../Images/0cc91f7c9119650d1036dab1920bf6b3.png)

图由作者提供。3D 可视化比较了三个数据集的原始表示和解耦潜在表示。为了实现这种可视化，表示通过 t-SNE 编码，困惑度 = 50，学习率 = 10，优化了 5,000 次迭代，所有计算使用了相同的随机种子。然而，为了进行聚类，我们使用了更高的维度以获得更好的聚类性能。

## 在较少标记示例上的训练

我们还尝试在较少的标记示例上训练我们的模型。

![](../Images/0ff8dfcd895fbfd7171e96bbf48ccb3e.png)

图由作者提供。在使用少量标记数据进行训练时，测试了 MNIST 和 Fashion-MNIST 测试集上的聚类准确性。无论是原始表示还是基线自编码器都没有利用标记数据集。

在上图中，我们可以看到，即使在较少的标记训练示例下，解耦表示的聚类性能仍与我们文献中的基线模型相当。

这意味着在标记数据集稀缺的情况下，这种方法可以用于产生良好的结果。

# 结论

与深度聚类方法相比，我们通过使用自编码器重构损失和软最近邻损失的复合损失来学习更具聚类友好的表示，从而改善了 k-Means 聚类算法的性能。

我们扩展的软最近邻损失使用了退火温度，这有助于更快、更好地解耦，从而改善了基准数据集上的聚类性能。至此，我们的工作得出结论。

自我们的工作发表以来，[几篇其他论文](https://scholar.google.com/scholar?cites=11092372353656200973&as_sdt=2005&sciodt=0%2C5&hl=en&oi=gsb)已在软最近邻损失上进行了建设，或被认为是相似的。尤其是，来自Google的[监督对比（SupCon）学习论文](https://proceedings.neurips.cc/paper_files/paper/2020/file/d89a66c7c80a29b1bdbab0f2a1a94af8-Paper.pdf)，但其不同之处在于SupCon方法提出了嵌入的归一化，增加了数据增强的使用，有一个可丢弃的对比头部和两阶段训练。

另一方面，我们的工作需要相对较低的硬件资源，同时取得了良好的结果。

我们讨论如何在[下一篇文章](https://medium.com/@afagarap/implementing-soft-nearest-neighbor-loss-in-pytorch-b9ed2a371760)中实现软最近邻损失。

# **参考文献**

+   Frosst, Nicholas, Nicolas Papernot和Geoffrey Hinton的“使用软最近邻损失分析和改进表示”。*机器学习国际会议*。PMLR，2019年。

+   Goldberger, Jacob等人的“邻域分析组件”。*神经信息处理系统进展*。2005年。

+   Khosla, Prannay等人的“监督对比学习”。*神经信息处理系统进展* 33 (2020): 18661–18673。

+   Salakhutdinov, Ruslan和Geoff Hinton的“通过保持类邻域结构学习非线性嵌入”。*人工智能和统计*。2007年。
