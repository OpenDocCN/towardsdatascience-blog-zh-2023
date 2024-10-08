# 特征变换：PCA 和 LDA 教程

> 原文：[`towardsdatascience.com/feature-transformations-a-tutorial-on-pca-and-lda-1ac160088092`](https://towardsdatascience.com/feature-transformations-a-tutorial-on-pca-and-lda-1ac160088092)

## 使用 PCA 等方法减少数据集的维度

[](https://medium.com/@PadraigC?source=post_page-----1ac160088092--------------------------------)![Pádraig Cunningham](https://medium.com/@PadraigC?source=post_page-----1ac160088092--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1ac160088092--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ac160088092--------------------------------) [Pádraig Cunningham](https://medium.com/@PadraigC?source=post_page-----1ac160088092--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ac160088092--------------------------------) ·阅读时间 7 分钟·2023 年 7 月 14 日

--

![](img/4bb1b05b0d0163b3ecd9ab788f291fdb.png)

照片由 [Nicole Cagnina](https://unsplash.com/@nicole_c?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/projection?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 介绍

在处理高维数据时，通常使用主成分分析（PCA）等方法来降低数据的维度。这将数据转换为不同的（较低维度的）特征集。这与特征子集选择形成对比，后者选择原始特征的子集（参见[[1]](https://medium.com/towards-data-science/feature-subset-selection-6de1f05822b0)了解特征选择的教程）。

PCA 是一种将数据线性变换到较低维空间的方法。本文首先解释了什么是线性变换。然后，我们通过 Python 示例展示了 PCA 的工作原理。文章最后描述了线性判别分析（LDA），这是一种*监督*的线性变换方法。文中介绍的方法的 Python 代码可在 [GitHub](https://github.com/PadraigC/FeatTransTutorial) 上找到。

## 线性变换

想象一下，假设在假期之后，比尔欠玛丽£5 和$15，需要用欧元（€）支付。汇率为：£1 = €1.15 和 $1 = €0.93。因此，债务的€金额为：

![](img/c0dd42037c086d8bb90eb484a162cb76.png)

在这里，我们将两维（£，$）的债务转换为一维（€）。图 1 中展示了三个例子，包括原始债务（£5，$15）和另外两个债务（£15，$20）以及（£20，$35）。绿色的点是原始债务，红色的点是投影到单一维度的债务。红色的线代表这个新维度。

![](img/cbf16cd8b8dd7f74d2227ab0570161a3.png)

**图 1。** 一个将英镑和美元债务转换为欧元的线性变换的示例。图片作者提供。

在图的左侧，我们可以看到如何将其表示为矩阵乘法。原始数据集是一个 3 x 2 的矩阵（3 个样本，2 个特征），汇率形成一个 1D 矩阵的两个分量，输出是一个 1D 矩阵的 3 个分量。汇率矩阵*是*变换；如果汇率改变，则变换也会改变。

我们可以使用下面的代码在 Python 中执行矩阵乘法。这些矩阵被表示为 numpy 数组；最后一行调用`dot`方法在`cur`矩阵上执行矩阵乘法（点积）。这将返回矩阵`[19.7, 35.85, 55.55]`。

这种数据变换的一般格式如图 2 所示。**Y** 是原始数据集（*n* 个样本，*d* 个特征）；通过乘以具有 *d* x *k* 维的变换矩阵 **P**，将其减少到 *k* 个特征的 **X’** 中。

![](img/f5cdbb8952098a66272d10d823b4a790.png)

**图 2。** 如果我们有一个由 *n* 个样本和 *d* 个特征描述的数据集 **Y**，则可以通过乘以维度为 *d* x *k* 的变换矩阵 ***P*** 将其减少到 *k* 个特征（****X’***）。图片作者提供。

## 主成分分析

一般来说，变换矩阵 **P** 决定了变换。在图 1 的示例中，变换的细节由汇率决定，这些汇率是给定的。如果我们希望使用 PCA 来减少数据集的维度，我们如何决定变换的性质？嗯，PCA 由三个原则驱动：

1.  选择一种保留数据扩展的变换，即选择保留数据点之间距离的维度。

1.  选择彼此正交的维度（无冗余）。

1.  选择 *k* 个维度，以捕获数据中大部分的方差（例如 90%）。

这些原理在图 3 中得到了说明。我们有一个由两个特征描述的个体数据集：腰围和体重。这些特征彼此相关，因此目标是将数据投影到不同的、不相关的维度中，而不丢失数据中的‘扩展’。这些新维度就是主成分，赋予了 PCA 其名称。作为投影的替代思路，你可以将其视为将数据云*旋转*以对齐图 3 中的红色坐标轴。无论哪种方式，新轴都是 PC1，第一个主成分和 PC2，它与 PC1 垂直。如果认为 PC1 捕获了足够的数据变化，则可以舍弃 PC2。

![](img/30cb90ec4da05433963e882d8ae62a79.png)

**图 3.** 一个二维数据集，其中的特征是体重和腰围。第一个主成分（PC1）应在捕获数据方差最多的方向上。PC2 应与 PC1 正交，以便它们独立。图像由作者提供。

对数据矩阵**Y**执行 PCA 的步骤如图 2 所示：

1.  计算**Y**的列的均值和标准差。

1.  从**Y**的每一行中减去列均值，并除以标准差以创建*标准化中心矩阵* **Z**。

1.  计算协方差矩阵**C** = 1/(n-1) **Zᵀ Z**，其中**Zᵀ**是**Z**的转置。

1.  计算协方差矩阵**C**的特征向量和特征值。

1.  检查特征值的降序以确定保留的维度数*k* — 这是主成分的数量。

1.  顶部的*k*特征向量构成了转换矩阵**P**的列，该矩阵的维度为（*p* × *k*）。

1.  数据通过**X′ = ZP**进行转换，其中**X**′的维度为（*n* × *k*）。

在以下示例中，我们将使用在[Github](https://github.com/PadraigC/FeatTransTutorial)上共享的哈利·波特 TT 数据集。数据的格式如下所示。共有 22 行和五个描述性特征。我们将使用 PCA 将其压缩为两个维度。

下面显示了执行此操作的 Python 代码。 `Y_df`是包含数据集的 Pandas 数据框。特征值和特征向量（`ev,evec`）在第 8 行计算。如果我们检查特征值，它们告诉我们每个主成分（PC）捕获的方差量；[49%，31%，11%，5%，4%]。前两个主成分将保留数据中 80%（49% + 31%）的方差，因此我们决定使用两个主成分。 `X_dash`包含投影到二维的这些数据。二维投影的数据如图 4 所示。可以说，PC1 维度代表能力/无能力，而 PC2 维度代表优良程度。弗雷德和乔治·韦斯利（双胞胎）被绘制在相同的点，因为他们在原始数据集中具有完全相同的特征值。

![](img/8140bd9901ba3831fbe1ba6a5950c832.png)

**图 4.** 哈利·波特数据集投影到二维（2 个主成分）。图像由作者提供。

如果我们使用 scikit learn 中的 PCA 实现，可以用三行代码完成这项工作：

在第 4 行中数据被标准化，第 5 行中设置了 PCA 对象，第 6 行中完成了数据转换。再次说明，这些代码可以在[Github](https://github.com/PadraigC/FeatTransTutorial)的笔记本中找到。

## 线性判别分析

应该明确的是，PCA 本质上是一种*无监督*的机器学习技术，因为它不考虑任何类别标签。实际上，在监督学习的背景下，PCA 不一定会有效——考虑到其重点是保持数据的*分布*而不考虑类别标签，这一点并不令人惊讶。在图 4 中，我们可以看到 PCA 在企鹅数据集 [2] 上的表现。这是一个由四个特征描述的三类数据集（也可在[GitHub](https://github.com/PadraigC/FeatTransTutorial)上找到）。

![](img/e6187650d55b3a67a19b4b44252761e0.png)

**图 5\.** 这些散点图比较了 PCA 和 LDA 在企鹅数据集上的表现。PCA 由于未考虑类别标签信息，其表现不如 LDA。图片由作者提供。

在图 5 右侧，我们可以看到线性判别分析 (LDA) 在相同数据集上的表现。LDA 考虑了类别标签，并寻求一个最大化类别之间分离的投影。目标是揭示一个能够最大化类别间分离并最小化类别内分离的变换。这些可以在两个矩阵中计算：**S***ᵦ* 代表类别间分离，**S***𝓌* 代表类别内分离：

![](img/5eb6c85583a67a29b962b5f60edf9f40.png)![](img/64a85f1afb154f360b51171e931a292e.png)

其中 *n*𝒸 是类别 *c* 中对象的数量，*μ* 是所有示例的均值，*μ*𝒸 是类别 *c* 中所有示例的均值：

![](img/b3fdafe3b26e98a1d1681038917d8179.png)

这些求和中的组件 *μ, μ*𝒸*, xⱼ* 是维度为 *p* 的向量，因此 **S***ᵦ* 和 **S***𝓌* 是维度为 *p* × *p* 的矩阵。最大化类别间分离和最小化类别内分离的目标可以结合成一个称为 Fisher 判别准则的单一最大化：

![](img/ad7cc6716c5572688b96e8ea797df542.png)

这种表述以及找到最佳 **W***ˡᵈᵃ* 矩阵的任务在 [3] 中有更详细的讨论。现在我们只需认识到 **W***ˡᵈᵃ* 的作用与 PCA 中的 **P** 矩阵相同。它的维度是 *p* × *k*，将数据投影到一个 *k* 维空间中，以最大化类别间分离并最小化类别内分离。目标由两个 **S** 矩阵表示。我们可以看到图 5 右侧，它做得相当不错。

虽然 LDA 的内部工作机制可能看起来复杂，但在 scikit-learn 中它非常简单，如下面的代码块所示。这与上述的 PCA 代码非常相似；主要区别在于在拟合 LDA 时会考虑 `y` 目标变量；而 PCA 并非如此。

## 结论

本文的目标是解释数据转换的基本原理，展示 PCA 和 LDA 在 scikit-learn 中的工作方式，并展示这些方法的一些实际示例。

这些示例的代码和数据可以在 [GitHub](https://github.com/PadraigC/FeatTransTutorial) 上找到。关于这些方法的更深入处理在此 *arXiv* 报告 [3] 中进行了介绍。

## 参考文献

[1] P. Cunningham, “特征子集选择”，Towards Data Science，2022 年，[在线]，`towardsdatascience.com/feature-subset-selection-6de1f05822b0`

[2] A.M. Horst, A.P. Hill, K.B. Gorman KB, palmerpenguins: Palmer Archipelago（南极洲）企鹅数据，2020 年，[doi:10.5281/zenodo.3960218](https://doi.org/10.5281/zenodo.3960218)，R 包版本 0.1.0，[`allisonhorst.github.io/palmerpenguins/`](https://allisonhorst.github.io/palmerpenguins/)

[3] P. Cunningham, B. Kathirgamanathan, & S.J. Delany, 特征选择教程（带有 Python 示例），2021 年，[*arXiv 预印本 arXiv:2106.06437*](https://arxiv.org/abs/2106.06437)
