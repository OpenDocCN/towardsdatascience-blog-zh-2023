# 线性判别分析（LDA）可以如此简单

> 原文：[https://towardsdatascience.com/linear-discriminant-analysis-lda-can-be-so-easy-b3f46e32f982?source=collection_archive---------8-----------------------#2023-02-20](https://towardsdatascience.com/linear-discriminant-analysis-lda-can-be-so-easy-b3f46e32f982?source=collection_archive---------8-----------------------#2023-02-20)

## 为你准备的互动可视化工具

[](https://medium.com/@frederikho?source=post_page-----b3f46e32f982--------------------------------)[![Frederik Holtel](../Images/adc1632104df83db4449ca56d0a2c2ad.png)](https://medium.com/@frederikho?source=post_page-----b3f46e32f982--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b3f46e32f982--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b3f46e32f982--------------------------------) [Frederik Holtel](https://medium.com/@frederikho?source=post_page-----b3f46e32f982--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fde281205c742&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-discriminant-analysis-lda-can-be-so-easy-b3f46e32f982&user=Frederik+Holtel&userId=de281205c742&source=post_page-de281205c742----b3f46e32f982---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b3f46e32f982--------------------------------) ·7 分钟阅读·2023年2月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb3f46e32f982&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-discriminant-analysis-lda-can-be-so-easy-b3f46e32f982&user=Frederik+Holtel&userId=de281205c742&source=-----b3f46e32f982---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb3f46e32f982&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-discriminant-analysis-lda-can-be-so-easy-b3f46e32f982&source=-----b3f46e32f982---------------------bookmark_footer-----------)![](../Images/5f710a7c3c6c2686a4fca1620a2c6619.png)

图片由 Arus Nazaryan 使用 Midjourney 创建。提示词：“`无人机拍摄的两群羊，明亮的蓝色和深红色，被一条中间分隔群体的围栏分开，干净、逼真的羊，绿草地上，照片级真实感`”

分类是机器学习中的核心话题。然而，理解不同算法的工作原理可能会很具挑战性。在这篇文章中，我们将通过一个互动图表使线性判别分析变得生动，你可以进行实验。准备好深入数据分类的世界吧！

**交互式图** 👇🏽 点击添加和移除数据点，使用拖动来移动它们。改变人口参数并生成新的数据样本。

如果你在应用或研究分类方法，可能会遇到各种方法，如朴素贝叶斯、K最近邻（KNN）、二次判别分析（QDA）和线性判别分析（LDA）。然而，理解不同算法的工作原理并不总是直观的。LDA是最初需要学习和考虑的方法之一，本文展示了该技术的工作原理。

让我们从头开始。所有分类方法都是为了回答这个问题：这个观测值属于哪种类型的类别？

在上面的图中，有两个独立变量，`x_1` 在水平轴上，`x_2` 在垂直轴上。把独立变量看作是两个学科的分数，例如物理和文学（范围从0到20）。因变量 `y` 是类别，在图中表示为红色或蓝色。将其视为我们想要预测的二元变量，例如申请人是否被大学录取（“是”或“否”）。一组给定的观测值以圆圈的形式显示。每个圆圈由 `x_1` 和 `x_2` 值以及类别特征。

现在，如果添加一个新的数据点，它应该被分配到哪个类别呢？

LDA允许我们绘制一个边界，将空间分成两部分（或者多个部分，但在这种情况下是两个类别）。例如，在下图1中，我在 `(x_1 = 4, x_2 = 14)` 处标记了一个新的数据点。由于它落在空间的“红色侧”，这个观测值将被分配到红色类别。

![](../Images/03f247a59ed97037b70563a516d9fdc9.png)

图1：交叉点 (x_1=4, x_2=14) 被分配到红色类别。

# 它是如何工作的——背后的数学

所以，给定LDA边界，我们可以进行分类。但是我们如何使用LDA算法绘制边界线呢？

这条线将图形分成红色类别和蓝色类别的概率各为50%的区域。往一侧移动，红色的概率更高；往另一侧移动，蓝色的概率更高。我们需要找出一种方法来计算新观测值属于每个类别的概率。

新观测值属于红色类别的概率可以这样写： `P(Y = red| X = (x_1 = 4, x_2 = 14))`。为了便于阅读，我将以下用 `x` 代替 `x_1 = 4, x_2 = 14`，它是包含两个值的向量。

根据贝叶斯定理，我们可以将条件概率表示为 `P(Y = red | X = x) = P(Y = red ) * P(X = x | Y = red) / P(X = x)`。因此，为了计算给定x值的新观测值是“红色”的概率，我们需要知道另外三个概率。

我们一步步来。`P(Y = red)` 和 `P(X = x)` 被称为“先验概率”。在图中，`P(Y = red)` 可以通过将所有“红色”观察值占总观察值数量的比例（图1中为27,27%）来简单计算。

计算 `x` 的先验概率 `P(X = x)` 更为困难。我们在初始数据集中没有 `x_1 = 4` 和 `x_2 = 14` 的观察值，那么它的概率是多少？零？不完全是。

`P(X = x)` 可以理解为联合概率 `P(Y = red, X = x)` 和 `P(Y = blue, X = x)` 的总和（另见 [这里](https://stats.stackexchange.com/questions/294080/proof-of-a-modified-bayes-theorem/601390#601390)）：

![](../Images/799fcbe610da7c6556a979336e1d23ab.png)

每个联合概率可以用贝叶斯定理表示为：

![](../Images/24085fa6839e75ef86e639e814ace775.png)![](../Images/c151ed9d4013af48cf428953eb9cf508.png)

你会看到先验 `P(red)` 和 `P(blue)` 再次出现，我们已经知道如何确定它们。剩下的工作是找到计算条件概率 `P(X = x | Y = red)` 和 `P(X = x | Y = blue)` 的方法。

即使我们在原始数据集中从未看到位置 `x` 的任何红色数据点，我们也可以找到一个概率，如果我们将 `x` 看作是从某种形式的总体中抽取的。通常在使用现实世界数据时，总体是不可直接观察的，因此我们不知道总体分布的真实形式。LDA的做法是假设 `x` 值在 `x_1` 和 `x_2` 轴上是正态分布的。对于现实世界的数据，这个假设可能或多或少是合理的。然而，由于我们处理的是生成的数据集，因此我们不必担心这个问题。这些数据来源于遵循正态（即高斯）分布的总体，其特征由两个参数（每个独立变量）：`~N(mean, variance)`。

使用多元正态分布的公式，我们可以将属于“红色”类别的数据的分布描述为

![](../Images/4b4031dc61db91365394f8ba1779263f.png)

其中 Σ 表示协方差矩阵。LDA 仅使用一个共同的协方差矩阵，意味着它假设所有类别具有相同的 `variance(x_1)`、`variance(x_2)` 和 `covariance(x_1, x_2)`。

现在我们得到了 `P (X = x | Y = red)` 的公式，我们可以将其代入上面的方程中以得到 `P (Y = red|X = x)`。这会得到一个庞大的方程，我在这里略过。幸运的是，从这里开始会更简单。我们的目标是找到分隔红色和蓝色区域的分界线，即我们要找到那些红色或蓝色的概率相等的点：

![](../Images/67b338ecddf9e092b8ae3e60bb63bd78.png)

进行一些变换后，可以证明最小化这个等价于最小化以下方程：

![](../Images/108550b5f8c0466072a963be46d35546.png)

对 `x_2` 求解，我们得到一条类似于

![](../Images/100c7df1aaca3863d287b1c2f5f0ed2a.png)

其中`β_0`和`β_1`是依赖于均值µ_red和µ_blue、共同协方差矩阵Σ以及红色和蓝色的先验概率P（“red”）和P（“blue”）的参数。

# 探索图表

现在你已经了解它是如何工作的，你可以使用不同的参数来探索图表。

当你点击“创建新样本”时，图表会显示两个边界。贝叶斯边界是使用“真实”总体参数计算的。相对而言，估计边界则基于样本数据对参数进行估计。

探索的一些指导问题：

+   使用哪些参数时，估计边界与贝叶斯边界偏差较大？（见图2，左侧图像）

+   边界对异常值有多敏感？（见图2，中间图像）

+   哪些参数产生了你没有预料到的结果？

![](../Images/649cf2e543b218c92974395aae814f8b.png)![](../Images/a0465c6200f9b51fbca9d93cb6399d9f.png)![](../Images/bf0cdda45ba59b21e6f72ba1f0ecfcaf.png)

图2\. 左侧：估计边界与贝叶斯边界偏差较大。中间：边界适应异常值。右侧：边界对移动的数据点适应良好。

# 最终备注

LDA是机器学习分类方法领域中的一个重要工具。然而，了解LDA的理论通过方程和公式是一回事，而通过实践探索获得实际理解则是另一回事。这里提供的互动工具提供了一个独特的机会，可以以更直观的方式实验和理解LDA。

你希望看到哪些其他统计方法的互动可视化？请在评论中告诉我！

别忘了**关注我**，以便获取更多文章的最新动态！

除非另有说明，否则所有图像均由作者使用Observable创建。

如果你想了解LDA，可以看看由James、Witten、Hastie和Tibshirani编写的精彩书籍《统计学习导论》，你可以在他们的网站上免费下载：[https://www.statlearning.com/](https://www.statlearning.com/)
