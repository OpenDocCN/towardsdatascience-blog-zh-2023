# 《维度灾难揭秘》

> 原文：[`towardsdatascience.com/the-curse-of-dimensionality-demystified-2fc9b0bb1126`](https://towardsdatascience.com/the-curse-of-dimensionality-demystified-2fc9b0bb1126)

## 理解维度灾难背后的数学直觉

[](https://reza-bagheri79.medium.com/?source=post_page-----2fc9b0bb1126--------------------------------)![Reza Bagheri](https://reza-bagheri79.medium.com/?source=post_page-----2fc9b0bb1126--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2fc9b0bb1126--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2fc9b0bb1126--------------------------------) [Reza Bagheri](https://reza-bagheri79.medium.com/?source=post_page-----2fc9b0bb1126--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2fc9b0bb1126--------------------------------) ·23 分钟阅读·2023 年 10 月 6 日

--

![](img/61dacdeae7a371398548e2bad19472fe.png)

图片来源: [`pixabay.com/illustrations/ancient-art-background-cosmos-dark-764930/`](https://pixabay.com/illustrations/ancient-art-background-cosmos-dark-764930/)

*维度灾难* 指的是在分析高维数据时出现的问题。数据集的*维度*或*维数*指的是数据集中线性独立特征的数量，因此*高维*数据集就是特征数量众多的数据集。这个术语首次由贝尔曼在 1961 年提出，他观察到，为了以一定的准确性估计一个任意函数，所需的样本数量会随着函数所取参数数量的增加而呈指数增长。

在这篇文章中，我们详细探讨了在分析高维数据集时出现的数学问题。尽管这些问题可能看起来违反直觉，但可以用直观的方式来解释它们。我们不进行纯理论讨论，而是使用 Python 创建并分析高维数据集，观察维度灾难在实践中的表现。*在这篇文章中，所有图片，除非另有说明，均由作者提供。*

**数据集的维度**

如前所述，数据集的维度定义为其具有的线性独立特征的数量。一个线性独立的特征不能表示为数据集中其他特征的线性组合。因此，如果数据集中的一个特征或列是其他特征的线性组合，它不会增加数据集的维度。例如，图 1 显示了两个数据集。第一个数据集有两个线性独立的列，因此其维度为 2。在第二个数据集中，一列是另一列的倍数，因此我们只有一个独立特征。正如该数据集的图示所示，尽管有两个特征，但所有数据点都在一条 1 维的线上。因此，这个数据集的维度是 1。

![](img/5ee523475faa332e048de327c6fe310b.png)

图 1

**维度对体积的影响**

维度诅咒的主要原因是维度对体积的影响。在这里，我们关注数据集的几何解释。通常，我们可以假设数据集是从一个群体中抽取的随机样本。例如，假设我们的群体是图 2 所示的正方形上的点集合。这个正方形是所有点(*x*₁, *x*₂)的集合，使得 0≤*x*₁≤1 和 0≤*x*₂≤1，且这个正方形的边长为 1。

每个群体都有其自己的分布，这里我们假设这个正方形上的点是均匀分布的。这意味着如果我们试图从这个群体中随机选择一个点，所有的点都有相等的被选中机会。现在，如果我们从这个群体中抽取大小为*n*的随机样本（即从这个正方形中随机选择*n*个点），这些点形成了一个具有*n*行和两个特征的数据集。因此，这个数据集的维度是 2。

![](img/05563aa0917fa4f78e1b857a610de8ca.png)

图 2

同样地，我们可以通过从一个边长为 1 的立方体中抽取随机样本来创建一个维度为 3 的数据集（图 2）。这个立方体是所有点(*x*₁, *x*₂, *x*₃)的集合，使得 0≤*x*₁≤1, 0≤*x*₂≤1, 0≤*x*₃≤1，并且这个立方体上的点是均匀分布的。

最终，我们可以扩展这个想法，通过从边长为 1 的*d*维超立方体中随机抽取大小为*n*的样本来创建一个*d*维数据集。超立方体定义为所有点(*x*₁, *x*₂,…, *x*_*d*)的集合，使得 0≤*xᵢ*≤1（对于 *i*=1…*d*）。同样，我们可以假设这些点在这个超立方体中是均匀分布的。生成的数据集包含*n*个示例（观测值）和*d*个特征。

边长为*L*的*d*维超立方体的体积是*L^d*。（请注意，如果*d*=2，该公式给出的就是边长为*L*的正方形的面积。然而，这里我们假设面积是二维对象的体积的特例）。因此，如果*L*=1，不管*d*的值是什么，体积都是 1。但重要的不是超立方体的总体积，而是体积在超立方体内部的分布。这里我们用一个例子来解释它。

我们可以将单位正方形分成 10 个壳体，如图 3 所示。第一个壳体实际上是单位正方形中心的小正方形，边长为 0.1。这个正方形的右下角位于*x*₁=0.55。第二个壳体的右下角位于*x*₁=0.6，因此其厚度为 0.05。所有其余的壳体都有相同的厚度，最外层壳体的右下角位于*x*₁=1。因此，它覆盖了单位正方形的边缘。图 3 展示了其中一个壳体作为示例，其右下角位于*x*₁=0.8。

![](img/bf90bcb98ad1d97a3bb80728ec67ab50.png)

图 3

我们可以轻松计算出底部右角位于*x*₁=*c*的壳体的体积（面积）：

![](img/2e38b6a7fbd1dc6d9cc9aedbba675e2b.png)

现在我们可以将此过程扩展到*d*维超立方体。我们将超立方体分成 10 个厚度相同的超立方壳体。

角落之一位于*x*₁=*c*的壳体的体积由以下公式确定：

![](img/85338605d33150377a6ddacee9ee810a.png)

列表 1 计算了不同*d*值下所有这些壳体的体积，并创建了体积的柱状图。结果如图 4 所示。

```py
# listing 1

import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import uniform, norm
from scipy.spatial import distance_matrix
from sklearn.neighbors import KNeighborsRegressor
%matplotlib inline

num_dims=[2, 3, 9, 15, 50, 100]
x_list = np.linspace(0.5, 1, 11)
edge_list = 2*(x_list - 0.5)
fig, axs = plt.subplots(3, 2, figsize=(14, 19))
plt.subplots_adjust(hspace=0.3)

for i, d in enumerate(num_dims):
    vols = [(edge_list[i+1])**d-(edge_list[i])**d \
            for i in range(len(x_list)-1)]
    axs[i//2, i%2].bar(x_list[1:], vols, width = 0.03)
    axs[i//2, i%2].set_title("d={}".format(d), fontsize=20)
    axs[i//2, i%2].set_xlabel('Shell corner $x_1$ coordinate', fontsize=18)
    axs[i//2, i%2].set_ylabel('Volume', fontsize=18)
    axs[i//2, i%2].set_xticks(x_list)
    axs[i//2, i%2].tick_params(axis='both', labelsize=12)

plt.show()
```

![](img/3fa0c41a2a21eca5abb97d9c10d914c3.png)

图 4

正如你所见，随着维度的增加，内层壳体的总体积份额迅速减少。事实上，对于*d≥*50，最外层壳体（其角落位于*x*₁=1）占据了单位超立方体总量的 99%以上。因此，我们得出结论，在高维超立方体中，几乎所有的体积都靠近超立方体的面，而不在其内部。

现在让我们计算在每个壳层中找到一个数据点的概率。记住我们假设这些超立方体中的点是均匀分布的。随机数据点在某个壳层内的概率可以通过对该壳层体积上的均匀分布概率密度函数（PDF）进行积分来计算。单位超立方体上的连续均匀分布的 PDF 是：

![](img/ed3843d964f94569d49e7110338837c3.png)

因此，随机数据点位于其中一个壳体（记作*S*）内的概率等于该壳体的体积：

![](img/a599abf83d92ad3e67cd565356e13064.png)

因此，根据图 4 的结果，如果我们从高维超立方体中随机抽取一个数据点，它很可能会位于外层壳。对于 *d*≥50，这个随机数据点位于内层壳的概率几乎为零。

我们还可以使用 Python 确认这些结果。列表 2 从定义在 *d* 维单位超立方体上的均匀分布中抽取了 5000 个样本。然后，它计算了每个壳层中的数据点数量，并创建了它们的条形图。它使用了与列表 1 中相同的 *d* 值。

```py
# Listing 2

np.random.seed(0)
num_dims=[2,3,9, 15, 50, 100]
x_list = np.linspace(0.5, 1, 11)
x_list[-1] = 1.001
x_list1 = np.linspace(0.5, 0, 11)
x_list1[-1] = -0.001
fig, axs = plt.subplots(3, 2, figsize=(14, 19))
plt.subplots_adjust(hspace=0.3)

for i, d in enumerate(num_dims):
    sample = uniform.rvs(size=5000*d).reshape(5000, d)
    count_list=[]
    for j in range(len(x_list)-1):
        inshell_count = (((x_list[j] <= sample) & \
                          (sample < x_list[j+1])) | \
                          ((x_list1[j+1] < sample) & \
                          (sample <= x_list1[j]))).sum(axis=1)
        outshell_count = ((x_list[j+1] <= sample) | \
                          (x_list1[j+1] >= sample)).sum(axis=1)
        count = ((inshell_count>0) & (outshell_count==0)).sum()
        count_list.append(count)

    axs[i//2, i%2].bar(x_list[1:], count_list, width = 0.03)
    axs[i//2, i%2].set_title("d={}".format(d), fontsize=20)
    axs[i//2, i%2].set_xlabel('Shell corner $x_1$ coordinate', fontsize=18)
    if i%2==0:
        axs[i//2, i%2].set_ylabel('Number of data \npoints within shell',
                                  fontsize=18)
    axs[i//2, i%2].set_xticks(np.linspace(0.5, 1, 11))
    axs[i//2, i%2].tick_params(axis='both', labelsize=12)

plt.show()
```

![](img/63ceec4bd4438b1b3e971e56f52afcb8.png)

图 5

图 5 中显示了图形，如你所见，条形图的形状类似于图 4。每个壳层中的数据点数量与方程 1 中给出的壳层的相应概率成比例，因此它们与该壳层的体积成比例。实际上，当 *d*=100 时，几乎所有的数据点都包含在最外层壳中，这体现了维度的诅咒。

在高维超立方体中，几乎所有随机样本的观察点都接近面（图 6）。如果超立方体是 *d* 维的，这些观察点也可以代表一个具有 *d* 特征的数据集。这样一个数据集显然不能代表其抽样的总体。这里的总体是超立方体中所有数据点的集合，但我们的样本仅包含接近超立方体面上的数据点。在实际中，我们没有来自超立方体中心附近区域的数据点。假设我们使用这样的样本作为机器学习模型的训练数据集。模型在训练过程中从未见过接近超立方体中心的数据点，因此对这样的数据点的预测不会可靠。实际上，模型无法将其预测推广到这样的数据点。

![](img/c00cf2a85f58f40a1a3ab72d50dc1350.png)

图 6

我们还可以找到这些结果的不同解释。假设我们在区间 [0,1] 上定义了均匀分布。这就像一个 1 维空间。如果我们从中抽取一个大小为 1 的随机样本，并将其记作 *X*₁，那么我们有：

![](img/6ef35f6c61c1832d7a8023c610c50195.png)![](img/f2ec1505d10c4ded47eb121f6c203a51.png)

因此，*X*₁ 在区间边缘 0.05 个单位内的概率仅为 10%。现在假设我们有一个来自 100 维均匀分布的样本，记作 *X*₁, *X*₂, … *X*₁₀₀。这是一个 IID（独立同分布）样本，这意味着所有的 *Xᵢ* 相互独立且每个 *Xᵢ* 在区间 [0,1] 上具有均匀分布。因此，我们可以写出：

![](img/38c52f31c75b947cbb2327b24ba7cec6.png)

我们得到：

![](img/e1a7aadbab1765c60d6168d70dd8a02f.png)

因此，至少有一个 *Xᵢ* 位于超立方体的某个面 0.05 单位以内的概率几乎为 1（这意味着该 *Xᵢ* 的观察值在该面 0.05 单位以内）。平均而言，在一个大小为 37594 的样本中，我们只能找到一个不在这个区域的数据点（1 / 2.66e-5 ≈ 37594）。这意味着我们需要一个非常大的样本才能得到仅有的几个不在超立方体面附近的数据点。

随着数据集中维度（或特征）的增加，为了准确概括机器学习模型所需的观察数量呈指数增长。维度诅咒使得训练数据集变得稀疏，模型预测的概括变得更加困难。因此，我们需要更多的训练数据（更多的观察）来概括模型。实际上，准备如此庞大的训练数据集可能不切实际。

图 7 显示了维度诅咒对随机采样影响的类比。最初，我们有一个二维射击目标和一个新手射手，他随机射击。射中最内圈的机会很高。在底部，我们有相同的目标，但现在具有维度诅咒。几乎所有的目标区域都属于最外圈，因此射中最内圈的机会几乎为零。

![](img/df39c780e35649c85a39993feba96b6e.png)

图 7

维度诅咒对随机采样的影响不仅限于均匀分布。实际上，无论其概率分布如何，这种现象都会发生在任何高维数据集中。让我们看看如果数据集中的特征具有正态分布会发生什么。假设我们数据集中的所有特征都是独立的，并且具有标准正态分布。因此，如果我们将这些特征组合成一个向量，该向量具有标准多元正态分布。图 8 显示了二维标准多元正态分布的 PDF。

![](img/478ffc7733f0722984516eb7203529ff.png)

图 8

标准正态分布的 PDF 是其边际分布 PDF 的乘积：

![](img/9d48abd9c07a60cae9708477face4697.png)

每个 *f*(*xᵢ*) 是标准正态分布的 PDF。使用这个方程，我们可以绘制标准正态分布在更高维度的 PDF。列表 3 绘制了 *d* 维标准正态分布在 *xᵢ* 轴上的 3 个 *d* 值的 PDF。请注意，PDF 具有对称形状，因此图形对于所有 *xᵢ* 和通过原点的所有线都是相同的。基于这些图，我们得出结论，对于任何 *d* 值，PDF 的最大值都在原点，且随着远离原点而下降。

```py
# Listing 3

np.random.seed(0)
num_dims=[2, 5, 10]
xi = np.linspace(-3, 3, 500)
fig, axs = plt.subplots(1, 3, figsize=(19, 5))
plt.subplots_adjust(wspace=0.25)

for i, d in enumerate(num_dims):
    pdf = norm.pdf(xi)**d
    axs[i].plot(xi, pdf)
    axs[i].set_xlabel('$x_i$', fontsize=28)
    axs[i].set_xlim([-3, 3])
    axs[i].set_title("d={}".format(d), fontsize=26)
    axs[i].tick_params(axis='both', labelsize=12)
axs[0].set_ylabel('PDF', fontsize=26)

plt.show()
```

![](img/880a2305cb0d4129894e07de0f5445a9.png)

图 9

列表 4 从*d*-维标准多元正态分布中抽取 5000 个样本。然后，它计算该样本中每个*d*-维数据点到原点的欧几里得距离。最后，它创建这些距离的直方图。它使用了列表 1 中使用的相同*d*值。结果如图 10 所示。

```py
# Listing 4

np.random.seed(0)
num_dims=[2,3,9, 15, 50, 100]
fig, axs = plt.subplots(2, 3, figsize=(19, 14))
plt.subplots_adjust(hspace=0.3)

for i, d in enumerate(num_dims):
    samples = norm.rvs(size=5000*d).reshape(5000, d)
    dist = np.linalg.norm(samples, axis=1)
    axs[i//3, i%3].hist(dist, bins=25)
    axs[i//3, i%3].set_title("d={}".format(d), fontsize=28)
    axs[i//3, i%3].set_xlabel('Distance from origin', fontsize=22)
    if i%3==0:
        axs[i//3, i%3].set_ylabel('Number of data points', fontsize=22)
    axs[i//3, i%3].set_xlim([0, 14])
    axs[i//3, i%3].tick_params(axis='both', labelsize=16)

plt.show()
```

![](img/ad5c3a2ff7fe4da7342959eb0b584888.png)

图 10

这里，直方图中每个区间的数据点数量与在该区间找到数据点的概率成正比。正如该图所示，即使在*d*=2 时，直方图的峰值也不在零处。在原点，PDF 具有最大值（图 8），然而，概率也依赖于我们进行积分的体积：

![](img/134ba93e7b7d834cb49e05816a5489f7.png)

通过增加维度，径向直方图的峰值从原点处移动得更远。因此，大多数数据点都位于一个薄环中，几乎没有其他地方找到数据点的概率。请注意，在任何值的*d*下，PDF 的最大值仍然位于原点（图 9），然而，在原点附近获得数据点的概率几乎为零。同样，数据集并不代表它所采样的总体，我们没有来自非常接近原点且具有高 PDF 值的区域的数据点。

重要的是要注意，维度的诅咒与数据集的概率分布的 PDF 无关。例如，均匀分布在其支持上具有相同的值，无论我们有多少维度。然而，由于概率依赖于 PDF 和体积，它会受到维度数量的影响。

**维度对距离函数的影响**

维度对距离也有重要影响。首先，让我们看看“距离”是什么意思。设***x***为*d*维向量：

![](img/a7c0fb88ed925daec18f7013be5d1ac1.png)

***x***的*p*-范数定义为：

![](img/fa367bf0a342736b0ba642290ef02323.png)

我们可以使用*p*-范数计算向量***x***和***y***之间的距离：

![](img/6bc5209a2f63ae3f1414f5ea102ee41a.png)

请注意，还有其他类型的距离函数，但在本文中，我们只关注这种类型。当*p*=2 时，我们得到熟悉的欧几里得距离：

![](img/0e1ac604e40df86e07c6c39042d5ddc9.png)

这是向量***x***-***y***的长度，也可以表示为***||x-y||* (**我们可以在*p*=2 时使用一个没有下标的*p*-范数**)**。现在，让我们深入探讨维度对距离函数的影响。假设我们数据集的维度是*d*。我们选择数据集中的一个点作为查询点，并用向量***x***_*q*表示。我们找出离查询点最近和最远的数据点，并分别用向量***x***_*n*和***x***_*f*表示（见图 11）。然后我们计算查询点与最近数据点和最远数据点之间的距离：

![](img/5cef2cd988e2af670a18fce72617ef58.png)![](img/7b12743f6e4614180b2102cb2b70c900.png)

现在可以证明，在对数据分布进行某些合理假设的情况下，我们有：

![](img/df2941e4dc1b0e0787690736e52d940a.png)

因此，随着维度的增加，查询点到最远数据点的距离逐渐接近其到最近数据点的距离。因此，将最近的数据点与其他数据点区分开来变得不可能。

![](img/96da5563bc5d560c4db5aee2e3c1262c.png)

图 11

列表 5 显示了维度对距离函数的影响。它从定义在*d*-维单位超立方体上的均匀分布中抽取了 500 个样本。然后它计算了每对数据点之间的欧几里得距离，并绘制了这些距离的直方图。它使用了列表 1 中相同的*d*值。图 12 显示了这些直方图。请注意，在*d*-维单位超立方体中，两点之间的最小欧几里得距离为零，最大可能距离为：

![](img/344e1ea40b8f87c08552ab2067c9932e.png)

因此，对于每个*d*值，直方图的*x*轴范围为 0 到√*n*。

```py
# Listing 5

np.random.seed(0)
num_dims=[2,3,9, 15, 50, 500]
fig, axs = plt.subplots(2, 3, figsize=(19, 14))
plt.subplots_adjust(hspace=0.3)

for i, d in enumerate(num_dims):
    samples = uniform.rvs(size=500*d).reshape(500, d)
    dist_amtrix = distance_matrix(samples, samples)
    dist_arr = dist_amtrix[np.triu_indices(len(dist_amtrix), k = 1)]

    axs[i//3, i%3].hist(dist_arr, bins=30)
    axs[i//3, i%3].set_title("d={}".format(d), fontsize=28)
    axs[i//3, i%3].set_xlabel('Pairwise distance', fontsize=22)
    if i%3==0:
        axs[i//3, i%3].set_ylabel('Number of data points', fontsize=22)
    axs[i//3, i%3].set_xlim([0, np.sqrt(d)])
    axs[i//3, i%3].tick_params(axis='both', labelsize=16)

plt.show()
```

![](img/2d1660125479e744b2eedce2a29c07a9.png)

图 12

随着维度（*d*）的增加，直方图变得更加尖锐，因此所有成对距离都在一个狭窄范围内。这表明，对于数据集中的每个数据点，DMAX（到最远数据点的距离）接近 DMIN（到最近数据点的距离）。

让我解释一下这种效应背后的直觉。图 13 显示了一个查询点（***x***_*q*）及其最近的（***x***_*n*）和最远的数据点（***x***_*f*）。在二维空间中，每个向量

![](img/608de6d4f3354b5df6375af64f4232ef.png)

和

![](img/2f460e3bec9694dcd188cb4f8d0e1dde.png)

有两个分量。***x***_*n-* ***x***_*q* 的分量用蓝色标记，***x***_*f-* ***x***_*q* 的分量用红色标记。同样，在*d*-维空间中，这些向量有*d*个分量。

![](img/a6efa42d16c9d2802e569fa6181ba6a5.png)

图 13

在二维空间中，从一个点到另一个点，我们应该通过 2 个维度。例如，在图 13 中，从***x***_*q*到***x***_*f*，我们应该向右移动*l*₁单位，向上移动*l*₂单位。请注意，*l*₁和*l*₂是向量***x***_*f-****x***_*q*的组件，这些点之间的距离是*l*₁²+*l*₂²的平方根。同样，在*d-*维空间中，从***x***_*q*到***x***_*f*，我们应该通过*d*个维度。现在，向量***x***_*f-****x***_*q*有*d*个组件。如果第*i*个组件用*lᵢ*表示，则意味着我们应该在第*i*个维度上移动*lᵢ*单位（图 13）。

列表 6 创建了一个从定义在*d*-维单位超立方体上的均匀分布中生成的大小为 500 的样本。然后，它随机选择一个查询数据点（***x***_*q*），并找到其最近的（***x***_*n*）和最远的（***x***_*f*）数据点。接下来，它计算向量***x***_*n-****x***_*q*和***x***_*f-****x***_*q*的组件。最后，这些组件的绝对值在*d*=2 和*d*=500 的两个条形图中绘制。结果显示在图 14 中。

```py
# Listing 6

np.random.seed(9)
num_dims=[2,500]
fig, axs = plt.subplots(2, 2, figsize=(17, 11))
plt.subplots_adjust(hspace=0.3)

for i, d in enumerate(num_dims):
    samples = uniform.rvs(size=500*d).reshape(500, d)
    query = samples[0]
    dist_amtrix = distance_matrix(samples, samples)
    ind_min = np.argmin(dist_amtrix[0, 1:])+1 
    ind_max = np.argmax(dist_amtrix[0, 1:])+1
    nearest = samples[ind_min]
    farthest = samples[ind_max]
    query_nearest_comps = np.abs(query - nearest)
    query_farthest_comps = np.abs(query - farthest)
    query_nearest_length = np.linalg.norm(query - nearest)
    query_farthest_length = np.linalg.norm(query - farthest)
    axs[0,i].bar(np.arange(1, d+1), query_nearest_comps,
               color="blue", width=0.45, label="$x_n-x_q$")
    axs[0,i].bar(np.arange(1, d+1), query_farthest_comps,
               color="red", alpha=0.5, width=0.45, label="$x_f-x_q$")
    axs[0,i].set_title("d={}".format(d), fontsize=26, pad=20)
    axs[0,i].set_xlabel('Index of component', fontsize=18)
    axs[0,i].set_ylabel('Abosulte value of component', fontsize=18)
    if i==0:
        axs[0,i].set_xticks(np.arange(1, d+1))
    axs[0,1].set_ylim([0, 1.2])
    axs[0,i].tick_params(axis='both', labelsize=16)
    axs[0,i].legend(loc='best', fontsize=17)
    axs[1,i].bar(["$||x_n-x_q||$", "$||x_f-x_q||$"],
                 [query_nearest_length, query_farthest_length],
                 width=0.2)
    axs[1,i].tick_params(axis='both', labelsize=22)
    axs[1,i].set_ylabel('Length', fontsize=18)

plt.show()
```

![](img/84c3b318650e84b71e274ac18c15d587.png)

图 14

向量***x***_*n-****x***_*q*的长度给出了***x***_*q*和***x***_*n*之间的距离。同样，||***x***_*f-****x***_*q||*给出了***x***_*q*和***x***_*f*之间的距离。这些距离也在图 14 中的两个条形图中显示。正如图中所示，在二维空间中，向量***x***_*n-****x***_*q*的组件的绝对值远小于***x***_*f-****x***_*q*的组件。因此，||***x***_*f-****x***_*q||*和||***x***_*n-****x***_*q||*之间的差异是显著的。在 500 维空间中，我们有 500 个组件。正如你所见，***x***_*n-****x***_*q*的某些组件的绝对值甚至大于***x***_*f-****x***_*q*。请记住，向量的长度是从这些组件的平方和中得到的。因此，||***x***_*f-****x***_*q||*和||***x***_*n-****x***_*q||*现在相对接近。

我们知道所有数据点都是随机选择的，因此每个数据点的组件都是随机数。当维度很高时，从一个点到另一个点，我们应该通过每个维度移动一个随机距离（图 13），由于我们有很多维度，每对点之间的总距离大致相同。因此，从查询点到其他任何点的距离大致相同。事实上，随着维度的增加，点之间的平均距离增加，但这些距离之间的相对差异减少（图 14）。

图 15 给出了这一效果的类比。假设两个地方之间的路径由几个段组成。在一维空间中，我们只有一个段。在二维空间中，每对点之间有 2 个段，而在 *d* 维空间中，我们有 *d* 个段。当只有一个段时，路径长度给出了两个点之间的实际水平距离。因此，我们可以很容易地区分较近的点 (*n*) 和较远的点 (*f*)。

随着段数的增加，路径长度（即所有段长度的总和）增加，并与点之间的实际距离发生偏离。现在点之间的水平距离对路径的影响不大。相反，沿着段的垂直移动决定了路径长度。随着段数趋向于无穷大，*q* 和 *n* 之间的路径长度接近于 *q* 和 *f* 之间的路径长度。

![](img/32d121eec4e5f41f2343308b03fb58f7.png)

图 15

一个具有 *d* 个段的路径类似于 *d* 维空间中两个点之间的距离，每个段可以表示连接这些点的向量的一个分量。随着维度的增加，我们有更多的段（分量），距离之间的相对差异接近于零。

**维度对学习模型的影响**

如前一节所述，在高维空间中，接近度或距离的概念不再有意义。因此，任何依赖距离函数的机器学习模型在高维空间中可能会崩溃。换句话说，当训练数据集中有很多特征时，这些模型将无法给出可靠的预测。一个例子是 *k-*NN (*k* 最近邻) 算法。在这一节中，我们将探讨维度诅咒如何影响 *k-*NN 模型的预测误差（这些例子是 [1] 中的修改版本）。

我们的数据集有 *d* 个特征和 1000 个示例（观察）。这些示例来自单位 *d* 维超立方体上的均匀分布。因此，这个数据集的每个示例可以写作：

![](img/f3dafba6893dcc66f093c48a595868f1.png)

这个观察的目标定义为：

![](img/c13516a3158afc739cdba662a0984641.png)

因此，目标仅仅是每个示例第一个特征的平方。我们使用一个 *k*-NN 模型，通过这个训练数据集预测测试数据点的目标。为了预测测试数据点的目标，*k*-NN 模型首先在训练数据集中找到该数据点的 *k* 个最近邻（*k* 个离测试点最近的数据点）。它使用距离函数（在这个例子中是欧几里得距离）来找到它们。测试点的预测目标是这 *k* 个最近邻的目标值的平均值。在这个例子中，我们使用 3 个最近邻（*k*=3）。

测试点位于超立方体的中心：

![](img/e244802b77e6ca577383293ef85ca940.png)

我们进行了 1000 次模拟。在每次模拟中，我们从 *d* 维均匀分布中抽取 1000 个数据点以创建训练数据集的示例，并计算它们的目标值。然后，我们在该训练数据集上训练一个 *k*-NN 模型（*k*=3）。最后，我们使用该模型预测测试数据点的目标（记作 *y*^_*t*），并与实际目标 0.25 比较。一旦我们获得所有模拟的预测目标，就可以计算这些预测的偏差、方差和均方误差（MSE）。偏差定义为：

![](img/8df1889a84fb998f23ed36446e643686.png)

这是在这些模拟中预测目标（*y*^_*t*）的均值与测试点的实际目标（*y*_*t*）之间的差异。方差是这些模拟中预测目标的方差：

![](img/77872b54f4e7b703d32cf9d23778c52a.png)

均方误差（MSE）定义为：

![](img/91d772e6230c670df79e940c7c1f900d.png)

可以证明：

![](img/214a61453af31445fe84a52be054218b.png)

列表 7 计算了该示例在不同 *d* 值下的偏差、方差和 MSE。结果绘制在图 16 中。

```py
# Listing 7

np.random.seed(2)
num_dims = np.arange(1, 11)
num_simulations = 1000

var_list = []
bias_list = []
for d in num_dims:
    pred_list = []
    for i in range(num_simulations):
        X = uniform.rvs(size=1000*d).reshape(1000, d)
        y = X[:,0]**2
        knn_model = KNeighborsRegressor(n_neighbors=3)
        knn_model.fit(X, y)
        pred_list.append(knn_model.predict(0.5*np.ones((1,d))))
    var_list.append(np.var(pred_list))
    bias_list.append((np.mean(pred_list)-0.25)**2)
error = np.array(bias_list) + np.array(var_list)
plt.plot(num_dims, var_list, 'o-', label="$Variance$")
plt.plot(num_dims, bias_list, 'o-', label="$Bias²$")
plt.plot(num_dims, error, 'o-', label="$MSE$")
plt.xlabel("d", fontsize=16)
plt.ylabel("$MSE, Bias², Variance$", fontsize=16)
plt.legend(loc='best', fontsize=14)
plt.xticks(num_dims)
plt.show()
```

![](img/93e6298bdbc2451a585fe973ea8ed8fa.png)

图 16

*k*-NN 模型假设相近的数据点也应具有相近的目标。然而，在高维数据集中，所有数据点之间的距离大致相同，因此 *k* 个最近邻可能距离测试点非常远。因此，这些邻居的平均目标不再是测试点实际目标的准确预测。

列表 8 显示了 *d*=2 和 *d*=10 的所有模拟中 *k*-NN 预测（*y*^_*t*）的比较（图 17）。它还用黑色虚线显示了测试点的实际目标（*y_t*），用蓝线显示了所有 *k*-NN 预测的均值。请注意，黑色虚线和蓝线之间的距离表示偏差。

```py
# Listing 8

np.random.seed(2)
num_dims = [2, 10]
num_simulations = 1000
fig, axs = plt.subplots(1, 2, figsize=(10, 5))
plt.subplots_adjust(wspace=0.3)

for i, d in enumerate(num_dims):
    pred_list = []
    for j in range(num_simulations):
        X = uniform.rvs(size=1000*d).reshape(1000, d)
        y = X[:,0]**2
        knn_model = KNeighborsRegressor(n_neighbors=3)
        knn_model.fit(X, y)
        pred_list.append(knn_model.predict(0.5*np.ones((1,d))))
    axs[i].scatter([0]*num_simulations, pred_list,
                   color="blue", alpha=0.2, label="$\hat{y}_t$")
    axs[i].axhline(y = 0.25, color = 'black',
                   linestyle='--', label="$y_t$", linewidth=3)
    axs[i].axhline(y = np.mean(pred_list),
                   color = 'blue', label="$E[\hat{y}_t]$")
    axs[i].set_ylim([0, 0.7])
    axs[i].set_title("d={}".format(d), fontsize=18)
    axs[i].set_ylabel('$\hat{y}_t$', fontsize=18)
    axs[i].legend(loc='best', fontsize=14)
    axs[i].set_xticks([])

plt.show()
```

![](img/3a309e8f2eacb4eb3fef91b6e3b90e69.png)

图 17

随着维度的增加，最近邻会接近超立方体的面，并且距离超立方体中心的测试点非常远。这些点的 *x*₁ 组件不一定接近测试点的 *x*₁，并且可能变化很大，因此预测的方差随着 *d* 的增加迅速增加。然而，平均来说，它们接近测试点的 *x*₁，所以偏差保持相对较小（图 16）。均方误差（MSE）与偏差和方差成正比，因此它随维度增加而增加。

列表 9 显示了另一个示例。这里的设置类似于列表 7，但数据集的目标定义为：

![](img/405529ce5ed95c2bad3cb536eab29810.png)

其中

![](img/1f25252327599653e36fe3137615bd24.png)

测试点再次位于超立方体的中心：

![](img/f1e6a9a8ea917375d242269142c7c00f.png)

这个例子的偏差、方差和 MSE 的图示如图 18 所示。在这里，偏差随着维度的增加迅速增加，而方差保持相对较小。

```py
# Listing 9

np.random.seed(2)
num_dims = np.arange(1,11)
num_simulations = 1000

var_list = []
bias_list = []
for d in num_dims:
    pred_list = []
    for i in range(num_simulations):
        X = uniform.rvs(size=1000*d).reshape(1000, d)
        y = np.exp(-5*np.linalg.norm(X-0.5*np.ones((1,d)), axis=1)**2)
        knn_model = KNeighborsRegressor(n_neighbors=3)
        knn_model.fit(X, y)
        pred_list.append(knn_model.predict(0.5*np.ones((1,d))))
    var_list.append(np.var(pred_list))
    bias_list.append((np.mean(pred_list)-1)**2)
error = np.array(bias_list) + np.array(var_list)
plt.plot(num_dims, var_list, 'o-', label="$Variance$")
plt.plot(num_dims, bias_list, 'o-', label="$Bias²$")
plt.plot(num_dims, error, 'o-', label="$MSE$")
plt.xlabel("d", fontsize=16)
plt.ylabel("$MSE, Bias², Variance$", fontsize=16)
plt.legend(loc='best', fontsize=14)
plt.xticks(num_dims)
plt.show()
```

![](img/19d32c989bdcc44647c446d0778e0e0a.png)

图 18

记住，随着维度的增加，每对点之间的距离增加，但这些距离之间的相对差异接近于零。因此，包括最近邻在内的所有数据点，||***x***|| 增加，而 *y* 变为零。因此，最近邻的平均 *y^* 与测试点的目标偏离。结果是偏差和 MSE 增加，但方差保持较小。

总结来说，维度的增加会导致均方误差（MSE）增加。然而，它对偏差或方差的影响取决于数据集目标的定义。这些例子清楚地表明，*k*-最近邻（k-NN）对于高维数据并不是一个可靠的模型。

在本文中，我们讨论了维度诅咒及其对体积和距离函数的影响。我们看到，在高维空间中，几乎所有数据点都将远离原点。因此，如果我们使用高维数据集来训练模型，那么对于接近原点的数据点，模型的预测将不可靠。此外，距离的概念也不再有意义。在高维数据集中，所有数据点之间的成对距离非常接近。因此，依赖于距离函数的机器学习模型将无法为高维数据集提供准确的预测。

**参考文献**

[1] Hastie, T., et al. The Elements of Statistical Learning. Springer, 2009.
