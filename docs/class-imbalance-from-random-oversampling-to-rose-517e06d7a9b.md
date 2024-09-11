# 类不平衡：ROSE 和随机游走过采样（RWO）

> 原文：[https://towardsdatascience.com/class-imbalance-from-random-oversampling-to-rose-517e06d7a9b?source=collection_archive---------7-----------------------#2023-08-29](https://towardsdatascience.com/class-imbalance-from-random-oversampling-to-rose-517e06d7a9b?source=collection_archive---------7-----------------------#2023-08-29)

## 让我们正式定义类不平衡问题，并直观地推导解决方案！

[](https://essamwissam.medium.com/?source=post_page-----517e06d7a9b--------------------------------)[![Essam Wisam](../Images/6320ce88ba2e5d56d70ce3e0f97ceb1d.png)](https://essamwissam.medium.com/?source=post_page-----517e06d7a9b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----517e06d7a9b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----517e06d7a9b--------------------------------) [Essam Wisam](https://essamwissam.medium.com/?source=post_page-----517e06d7a9b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fccb82b9f3b87&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclass-imbalance-from-random-oversampling-to-rose-517e06d7a9b&user=Essam+Wisam&userId=ccb82b9f3b87&source=post_page-ccb82b9f3b87----517e06d7a9b---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----517e06d7a9b--------------------------------) ·5 分钟阅读·2023年8月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F517e06d7a9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclass-imbalance-from-random-oversampling-to-rose-517e06d7a9b&user=Essam+Wisam&userId=ccb82b9f3b87&source=-----517e06d7a9b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F517e06d7a9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclass-imbalance-from-random-oversampling-to-rose-517e06d7a9b&source=-----517e06d7a9b---------------------bookmark_footer-----------)

在这一系列故事中，我们解释了用于处理类别不平衡的各种重采样技术；特别是那些最初在[Imbalance.jl](https://github.com/JuliaAI/Imbalance.jl) Julia包中实现的技术，包括朴素随机过采样、随机过采样示例（ROSE）、随机游走过采样（RWO）、合成少数类过采样技术（SMOTE）、SMOTE-名义型、SMOTE-名义型连续以及许多欠采样技术。在这个故事中，我们将假设对类别不平衡问题有一定了解，如[之前正式解释](https://essamwissam.medium.com/class-imbalance-and-oversampling-a-formal-introduction-c77b918e586d)的，并解释两种有助于解决这一问题的有趣算法；即ROSE和RWO。

## 目录

∘ [随机过采样示例（ROSE）](#2a1c)

∘ [随机游走过采样（RWO）](#0f3f)

## 随机过采样示例（ROSE）

我们知道，我们收集的任何额外数据都遵循少数类的基础数据分布，那么如何近似这种概率分布，然后从中采样以模拟收集真实示例呢？这就是随机过采样示例（ROSE）算法的作用。

因此，ROSE试图为每个类别k估计概率分布*P(x|y=k)*，然后从中抽取所需的*N_k*样本。众所周知，估计这种密度的一种方法是通过核密度估计，你可以从更粗糙的版本如直方图分析中推导或直观理解。以下描述了KDE：

**给定：** 数据点*x* **所需：** *P(x)*的估计 **操作：** 选择一个核函数*K(x)*，然后将*P(x)*估计为

![](../Images/48e3a2758a22f4d3ea2b516e8b408a14.png)

通常，我们希望能够控制核函数的尺度（即，压缩或扩展），因为这可以改善对*P(x)*的估计，因此一般而言，我们有

![](../Images/04d53a5b8dd60bb9756bd70ea0cdf1b9.png)

从本质上讲，它是将核函数放置在每个点上，然后对它们进行求和和归一化，使其积分为1。

![](../Images/b6aadf6515a2b00f605cff98eb427051.png)

将KDE应用于分布估计，作者：Drleft，维基媒体公有领域（[许可证](https://commons.wikimedia.org/wiki/Commons:GNU_Free_Documentation_License,_version_1.2)）

核函数本身是一个超参数；可能，一个已被证明不那么重要的超参数，只要它满足平滑性和对称性等基本属性。简单的高斯核，尺度为σ，是一个常见的选择，ROSE也使用这种KDE。

![](../Images/a9ccdeb86107ad9b0eb140abd2a5be94.png)

正态分布公式中的标准差作为h，因此未写出

ROSE从对任何类别k的分布估计中抽取*N_k*点（生成*P(x|y=k)*），具体操作如下：

+   随机选择一个点

+   在其上放置高斯核

+   从高斯分布中抽取一个点

这与随机过采样类似，只不过在随机选择一个点后，它在该点上放置一个高斯分布，并从高斯分布中采样生成新点，而不是重复选择的点。

在这个过程中，ROSE使用一个称为Silverman的经验规则来设置带宽h（或者在更高维度中是平滑矩阵，即正态分布中的协方差矩阵参数），以便[均值积分平方误差最小化](https://en.wikipedia.org/wiki/Mean_integrated_squared_error)。特别地，

![](../Images/a470d9207ad7e19e4611995aa72bc051.png)

其中D_σ是每个特征标准差的对角矩阵，d是特征的数量，N是点的数量。

在Imbalance.jl包中，这个值乘以另一个常数**s**以允许可选控制超参数。对于s=1，保持原样；对于s=0，ROSE等同于随机过采样。以下使用该包生成的动画展示了增加s对生成点的影响。

![](../Images/304f20ed1081ff8989ebb7791ebdb905.png)

作者所绘图。合成点以钻石形状显示。

注意，随着s的增加，由每个随机选择的原始点生成的合成点与其距离变得更远。

## 随机游走过采样（RWO）

当中心极限定理适用时，可以证明只需*n*个样本，就可以以95%的概率满足均值估计x̄=µ*±1.96**σ/√n。假设少数类的σ较小（例如，σ=10），并且我们收集了n=1000个样本，那么x̄=µ*±0.6*的概率为95%，换句话说，如果我们的估计x̄足够大，那么基本上x̄≈µ。类似的论点可以用来证明估计的标准差*S_x*将非常接近σ；至少在数据是正态分布的情况下。

这表明可以生成合成数据，以使估计的x̄和*S_x*得以保持，以模拟收集新数据。可以证明，如果属于某类k的数据中的特征是独立的，其中x̄是该类所有点的均值，*S_x*是它们的标准差（都是向量），那么如果通过应用

![](../Images/bc93bf51bc3f3aa9ceda5a0dd1105784.png)

x是属于某个类别的点；当对所有点进行k次这种操作时，会引入k*n个新样本。

然后渐近地，这些点不会改变原始的*x̄*和*S_x*。对于*x̄*很容易看到，因为当生成的点数N_k=k*n非常大时，会有*sum(x_new)=k*sum(x)*，因为*sum(r)=0*。因此，新数据集中包含旧例子和新例子的*x̄*是*x̄*(k+1)/(k+1)=x̄*，所以均值得以保持。同样可以证明标准差也得以保持。

![](../Images/d4963915e941cf78fdbee1f5d8073cbb.png)

动画由作者制作。钻石形状代表合成生成的点。

动画中展示的实现方法适用于任何比例，通过从需要过采样的类别中随机选择点，而不是遍历所有点。

![](../Images/b2d3b2a827d7a41e4a2189a3742f7c3d.png)

作者绘图

该方法称为随机游走过采样，因为在图中看起来像是通过在现有点上随机行走并放置新点来生成这些点。

希望这个故事能让你对机器学习中的类别不平衡问题及其解决方法有更多了解。让我们考虑SMOTE论文中提出的算法，以便了解[下一个故事](https://medium.com/towards-data-science/class-imbalance-from-smote-to-smote-n-759d364d535b)。

**参考文献：** [1] G Menardi, N. Torelli, “使用不平衡数据训练和评估分类规则，” 数据挖掘与知识发现，28(1)，第92–122页，2014年。

[2] Zhang, H., & Li, M. (2014). RWO-采样：一种用于不平衡数据分类的随机游走过采样方法。信息融合，25，第4–20页。
