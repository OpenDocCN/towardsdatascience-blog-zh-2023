# 如何为你的数据找到最佳的理论分布

> 原文：[`towardsdatascience.com/how-to-find-the-best-theoretical-distribution-for-your-data-a26e5673b4bd`](https://towardsdatascience.com/how-to-find-the-best-theoretical-distribution-for-your-data-a26e5673b4bd)

## 了解基础数据分布是数据建模的一个重要步骤，并且有许多应用，例如异常检测、合成数据创建和数据压缩。

[](https://erdogant.medium.com/?source=post_page-----a26e5673b4bd--------------------------------)![Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----a26e5673b4bd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a26e5673b4bd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a26e5673b4bd--------------------------------) [Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----a26e5673b4bd--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a26e5673b4bd--------------------------------) ·19 分钟阅读·2023 年 2 月 3 日

--

![](img/88e75fd6c664a30e948bcf9ad06e421d.png)

作者提供的图片。

了解数据的基础（概率）分布有许多建模上的优势。确定基础分布的最简单方法是通过直方图可视化随机变量。利用候选分布，可以创建各种图形，例如概率分布函数图（PDF/CDF）和 QQ 图。然而，为了确定精确的分布参数（例如 loc、scale），必须使用定量方法。在本博客中，我将描述*为什么确定数据集的基础概率分布很重要。参数分布和非参数分布之间的区别是什么。如何使用定量方法确定最佳拟合，并通过可视化检查来确认。* 分析使用*distfit*库进行，提供了一个笔记本，方便访问和实验。

*如果你觉得这篇文章对你有帮助，欢迎* [*关注我*](https://erdogant.medium.com/subscribe) *，因为我会写更多关于数据科学的内容。如果你考虑购买 Medium 会员，你可以通过使用我的推荐链接来稍微支持我的工作。这和一杯咖啡的价格一样，但这允许你每月阅读无限的文章。*

# 分布拟合和概率密度函数的重要性。

***概率密度函数*** 是统计学中的一个基本概念。简言之，*对于一个给定的随机变量 X，我们旨在指定函数 f，以自然描述 X 的分布。有关概率密度函数的更多信息，请参见底部的术语部分。* 尽管有大量优质材料描述这些概念[1]*，* 但了解为何知道数据集的基础数据分布仍然具有挑战性。让我用一个小的类比来解释其重要性。*假设你需要从地点 A 前往地点 B，你会更倾向于选择哪种类型的车？* 答案很简单。你可能会首先探查地形。有了这些信息，你可以选择最适合的车（跑车、四轮驱动等）。从逻辑上讲，跑车更适合平滑的、平坦的地形，而四轮驱动更适合崎岖的山地地形。换句话说，没有地形的探索分析，很难选择最合适的车。***然而，这样的探索步骤在数据建模中往往容易被忽视或遗忘。***

> 在做建模决策之前，你需要了解数据的基础分布。

在数据方面，探索数据的基本特征也很重要，例如偏斜度、峰度、异常值、分布形状、单变量、双峰等。根据这些特征，决定使用哪些模型会更容易，因为大多数模型对数据有先决条件。例如，一个众所周知且流行的技术是主成分分析（PCA）。该方法计算协方差矩阵，并要求数据在进行 PCA 时必须是多变量正态分布。此外，PCA 对异常值也很敏感。因此，在进行 PCA 步骤之前，你需要知道数据是否需要（对数）归一化，或者是否需要去除异常值。有关 PCA 的更多细节，请参见这里 [2]。

# 直方图可以帮助建立直观感。

直方图是数据分析中一个众所周知的图表，它是数据集分布的图形表示。直方图总结了落在各个区间内的观察次数。使用如`matplotlib hist()`等库，能够轻松地进行数据的可视化检查。改变区间数量的范围将有助于识别直方图的形状是否像常见的概率分布。检查还会提供数据是否对称或偏斜，是否有多个峰值或异常值的线索。在大多数情况下，你会观察到如图 1 所示的分布形状。

+   ***正态分布*** 的钟形。

+   ***指数分布或帕累托分布*** 的递减或递增形状。

+   ***均匀分布*** 的平坦形状。

+   ***复杂的形状***，即不符合任何理论分布的形状（例如，多峰）。

![](img/5c878e2d91f9d023650ec749040482de.png)

图 1\. 常见分布类型。图形使用 distfit 库创建，并可以在 Colab 笔记本中进一步探索（链接见底部）。(作者提供的图片)。

如果你发现具有多个峰值的分布（二峰或多峰），这些峰值在不同的箱数下不应消失。双峰分布通常提示混合人群。此外，如果你观察到某个值或小范围值的密度出现大幅度峰值，这可能指向潜在的异常值。异常值预计会远离其他密度。

直方图是检查相对较少样本（随机变量或数据点）的一个好方法。然而，当样本数量增加或绘制多个直方图时，视觉效果可能会变得麻烦，与理论分布的视觉比较也会很困难。相反，累积分布函数（CDF）图或分位数-分位数图（QQ 图）可能更具洞察力。但这些图需要与经验数据分布最匹配（或拟合）的候选理论分布。因此，让我们在下一节中确定最佳理论分布！*有关随机变量和理论分布的更多信息，请参见底部的术语部分。*

# 确定理论分布的四个步骤。

可以通过四个步骤为经验数据分布发现 PDF 拟合。

1.  **从直方图中计算密度和权重。** 第一步是将数据展平成数组，并通过将观测值分组到箱中并计算每个箱中的事件数来创建直方图。箱数的选择很重要，因为它控制了分布的粗糙程度。尝试不同的箱宽可以为相同的数据提供多个视角。在*distfit*中，箱宽可以手动定义或根据观测值自身数学确定。后者是默认选项。

1.  **从数据中估计分布参数。** 在参数化方法中，下一步是根据（选择的）理论分布估计形状、位置和尺度参数。这通常涉及诸如最大似然估计（MLE）等方法，以确定最能拟合数据的参数值。例如，如果选择了正态分布，MLE 方法将估计数据的均值和标准差。

1.  **检查拟合优度**。一旦参数被估计出来，理论分布的拟合程度需要评估。这可以通过拟合优度检验来完成。常用的统计检验包括*残差平方和（RSS，也称为 SSE）、Wasserstein 检验、Kolmogorov-Smirnov 检验以及能量检验（distfit 中也可用）*。

1.  **选择最佳理论分布。** 此时，通过拟合优度检验统计量对理论分布进行测试和评分。现在可以对这些评分进行排序，并选择评分最高的理论分布。

最后一步，可以使用交叉验证、重采样或保留数据集等方法验证模型。检查模型是否有很好的泛化能力，并检查假设，如独立性和正态性是否满足。一旦理论分布经过拟合和验证，它可以用于许多应用（请继续阅读下面的部分）。

> 在数据科学领域，分布拟合有很大好处。它不仅可以更好地理解、探索和准备数据，还可以带来快速而轻量的解决方案。

# distfit 库为你的数据找到最佳拟合。

***Distfit*** 是一个用于 *单变量分布的概率密度拟合* 的 Python 包。它可以找到参数化、非参数化和离散分布的最佳拟合。此外，它还通过各种图表提供可视化洞察，以帮助做出更好的决策。以下是最重要功能的总结：

> 使用 distfit 库，你可以仅用几行代码轻松找到最佳理论分布。

+   **找到最佳拟合** *参数化、非参数化和离散分布*。

+   **预测** 异常值/新奇样本（新）[[10]](/outlier-detection-using-distribution-fitting-in-univariate-data-sets-ac8b7a14d40e)。

+   **基于拟合的分布生成合成数据**。

+   **绘图：** 直方图、概率密度函数图、累积分布函数图（CDF）、直方图、分位数-分位数图（QQ 图）、概率图以及总结图。

+   **保存和加载** 模型。

**安装** 简单明了，可以通过 PyPi 完成。

```py
pip install distfit
```

# 如何使用参数拟合来确定最佳拟合？

使用参数拟合时，我们对输入数据的总体分布参数做出假设。换句话说，直方图的形状应与任何已知的理论分布匹配。***参数拟合的优点*** 是计算效率高，结果易于解释。***缺点*** 是当样本量较小时，对异常值较为敏感。*distfit* 库可以在 89 种理论分布中确定最佳拟合，这些理论分布来自 *scipy* 库。为了评分拟合效果，有四种 ***拟合优度*** 统计检验：*残差平方和（RSS 或 SSE）、Wasserstein、Kolmogorov-Smirnov（KS）和 Energy*。对于每个拟合的理论分布，返回 loc、scale 和 arg 参数，如正态分布的均值和标准差。

> 找到数据集的最佳匹配理论分布需要进行拟合优度统计检验。

在下面的示例中，我们将从正态分布中生成数据，`mean=2` 和 `standard deviation=4`。我们将使用*distfit*从数据本身来估计这两个参数。如果你已经知道分布的家族（例如，钟形分布），你可以指定一个分布子集。默认情况下是常见分布的子集（如图 1 所示）。*注意，由于随机成分，结果可能与我展示的有所不同。*

```py
# Import libraries
import numpy as np
from distfit import distfit

# Create random normal data with mean=2 and std=4
X = np.random.normal(2, 4, 10000)

# Initialize using the parametric approach.
dfit = distfit(method=’parametric’, todf=True)

# Alternatively limit the search for only a few theoretical distributions.
dfit = distfit(method='parametric', todf=True, distr=['norm', 'expon'])

# Fit model on input data X.
dfit.fit_transform(X)

# Print the bet model results.
dfit.model
# 'stats': 'RSS',
# 'name': 'loggamma',
# 'params': (761.2276, -725.194369, 109.61),
# 'score': 0.0004758991728293508,
# 'loc': -725.1943699246272,
# 'scale': 109.61710960155318,
# 'arg': (761.227612981012,),
# 'CII_min_alpha': -4.542099829801259,
# 'CII_max_alpha': 8.531658573964933
# 'distr': <scipy.stats._continuous_distns.loggamma_gen>,
# 'model': <scipy.stats._distn_infrastructure.rv_continuous_frozen>,
```

检测到的最佳拟合（即，具有最低 RSS 分数）是`loggamma`分布。最佳拟合的结果存储在`dfit.model`中，但我们也可以检查所有其他 PDF 的拟合情况，如`dfit.summary`中所示（见下面的代码部分），并创建一个图（图 2）。

```py
# Print the scores of the distributions:
dfit.summary[['name', 'score', 'loc', 'scale']]
#         name     score                loc         scale
#0     loggamma  0.000476         -725.19437     109.61711
#1            t   0.00048           2.036554      3.970414
#2         norm   0.00048           2.036483      3.970444
#3         beta  0.000481         -72.505842    133.797587
#4        gamma  0.000498        -304.071325      0.051542
#5      lognorm  0.000507        -325.188197    327.201051
#6   genextreme  0.001368           0.508856      3.947172
#7     dweibull  0.005371           2.102396      3.386271
#8      uniform  0.079545         -12.783659     30.766669
#9        expon  0.108689         -12.783659     14.820142
#10      pareto  0.108689 -1073741836.783659  1073741824.0

# Plot the RSS of the fitted distributions.
dfit.plot_summary()
```

![](img/fd5776a05784387f5612c73344cd2dc6.png)

图 2\. PDF 按拟合优度评分测试排序。（作者提供的图片）

> **但是，为什么正态分布在我们生成了随机正态数据的情况下，仍然没有最低的残差平方和呢？**
> 
> 首先，我们的输入数据集总是有限的，范围在一个（狭窄的）区间内。相反，理论（正常）分布在两个方向上都趋向于无限。其次，所有的统计分析都基于模型，而所有模型只是对现实世界的简化。换句话说，为了逼近理论分布，我们需要使用多种统计测试，每种测试都有其自身的（不）优点。最后，一些分布具有非常灵活的特性，其中（对数）伽马分布就是一个明显的例子。对于大的 k，伽马分布会趋近于正态分布 [4]。

结果是，排名前 7 的分布具有类似且较低的 RSS 分数，其中包括正态分布。我们可以在总结统计中看到，正态分布的估计参数为`loc=2.036` 和 `scale=3.97`，这非常接近我们最初生成的随机样本（`mean=2`，`std=4`）。综合来看，这是一个非常好的结果。

## 进行自助法以增加信心。

我们可以进一步使用自助法（bootstrapping）和 Kolmogorov-Smirnov (KS) 测试来验证我们拟合的模型，以评估拟合的优度 [9]。如果模型过拟合，KS 测试将揭示自助样本和原始数据之间的显著差异，表明模型不能代表潜在分布。在*distfit*中，`n_bootst` 参数可以在初始化时或之后设置（见代码部分）。

```py
# Set bootstrapping during initialization.
# dfit = distfit(method='parametric', n_boots=100)

# Bootstrapping
dfit.bootstrap(X, n_boots=100)

# Print
print(dfit.summary[['name', 'score', 'bootstrap_score', 'bootstrap_pass']])

#           name     score  bootstrap_score bootstrap_pass
# 0     loggamma  0.000218             0.42           True
# 1         beta  0.000217             0.36           True
# 2            t  0.000248             0.08           True
# 3         norm  0.000247             0.07           True
# 4      lognorm  0.000311             0.04          False
# 5        gamma   0.00029             0.01          False
# 6   genextreme  0.000874             0.00          False
# 7     dweibull  0.001651             0.00          False
# 8      uniform  0.073118             0.00          False
# 9        expon  0.104076             0.00          False
# 10      pareto  0.104076             0.00          False

# Plot the RSS and bootstrap scores of the fitted distributions.
dfit.plot_summary()
```

![](img/ee1e1d06985f73ff5ebfa8a0cdb4e71b.png)

图 2B. 自助法结果。PDF 按自助分数和拟合优度测试排序。（作者提供的图片）

图中左侧坐标轴显示了拟合优度检验，右侧坐标轴（橙色线）显示了自助法结果。自助法分数是介于 [0, 1] 之间的值，描绘了自助法样本与 PDF 的拟合成功率。此外，绿色和红色点表示自助样本与原始数据之间是否存在显著差异。自助法测试现在排除了几个显示不稳定结果的 PDF。

现在明白统计测试只是帮助你朝正确方向看待问题是很好的，并且***选择最佳模型不仅仅是一个统计问题；它也是一个建模决策*** *[5]****。*** 请思考这个问题：`loggamma` 分布的尾部严重右偏，而 `normal` 分布是对称的（两个尾部相似）。这在使用置信区间和预测尾部的异常值时可能会产生巨大差异。明智地选择分布，以确保它与应用程序匹配。

> 选择最佳模型不是一个统计问题，而是一个建模决策。

# 绘图指导我们做出更好的决策。

最佳实践是同时使用统计数据和可视化策划来决定最佳的分布拟合。使用 PDF/CDF 和 QQ 图可以是指导这些决策的最佳工具之一。例如，图 2 说明了拟合优度检验统计量，其中前 7 个 PDF 的 RSS 分数非常相似且较低。`dweibull` 分布排在第 8 位，也具有较低的 RSS 分数。然而，视觉检查会告诉我们，尽管 RSS 分数相对较低，它仍然不是一个好的拟合。

让我们开始使用直方图和 PDF*.* 绘制经验数据。这些绘图将帮助直观地指导分布是否合适。我们可以在图 3 中看到左侧的 PDF 和置信区间，右侧是 CDF 图。置信区间自动设置为 95% CII，但可以在初始化时使用 `alpha` 参数进行更改。在使用绘图功能时，它会自动显示直方图，包括条形图以及 PDF/CDF 和置信区间。这些属性都可以手动自定义（见下面的代码部分）。

```py
import matplotlib.pyplot as plt
# Create subplot
fig, ax = plt.subplots(1,2, figsize=(25, 10))
# Plot PDF with histogram
dfit.plot(chart='PDF', ax=ax[0])
# Plot the CDF
dfit.plot(chart='CDF', ax=ax[1])

# Change or remove properties of the chart.
dfit.plot(chart='PDF', 
          emp_properties=None,
          bar_properties=None,
          pdf_properties={'color': 'r'},
          cii_properties={'color': 'g'})
```

![](img/1b5de20e931be2604de625409e4946f3.png)

图 3\. 两个面板中都显示了带有置信区间的经验数据分布。左侧面板：带 PDF 的直方图。右侧面板：CDF。 （作者提供的图片）

我们还可以使用`n_top`参数绘制所有其他估计的理论分布。通过视觉检查可以确认，前 8 个分布与实际数据非常接近，仅有少数例外。*注意到引导法揭示了并非所有的拟合都是稳定的。* 图例中的分布从最佳拟合（最高）到最差拟合（最低）进行排名。在这里我们可以看到`dweibull`分布的拟合非常差，中间有两个峰值。仅使用 RSS 分数很难判断是否使用这个分布。分布`uniform`、`exponent`和`pareto`显示出较差的 RSS 分数，现在通过图示确认了这一点。

```py
# Create subplot
fig, ax = plt.subplots(1,2, figsize=(25, 10))
# Plot PDF with histogram
dfit.plot(chart='PDF', n_top=11, ax=ax[0])
# Plot the CDF
dfit.plot(chart='CDF', n_top=11, ax=ax[1])
```

![](img/3d3b292cd542e10a9ee75f3eb400542b.png)

图 4\. 左侧：PDF，右侧：CDF。所有拟合的理论分布以不同颜色显示。（作者提供的图片）

## 分位数-分位数图。

还有一个我们可以检查的图是 QQ 图。QQ 图通过将经验概率分布与理论概率分布的分位数进行对比，来比较它们。如果两个分布相等，则 QQ 图上的点将完美地位于直线`y = x`上。我们可以使用`qqplot`函数生成 QQ 图（图 5）。左侧面板显示最佳拟合，右侧面板包含所有拟合的理论分布。有关如何解读 QQ 图的更多细节，请参阅这个博客[3]。

```py
# Create subplot
fig, ax = plt.subplots(1,2, figsize=(25, 10))
# Plot left panel with best fitting distribution.
dfit.qqplot(X, ax=ax[0])
# plot right panel with all fitted theoretical distributions
dfit.qqplot(X, n_top=11, ax=ax[1])
```

![](img/77cdd694f8964fa9afc95df5a8e7c31f.png)

图 5\. 左侧面板：经验分布与最佳理论分布的比较。右侧面板：经验分布与所有其他理论分布的比较。（作者提供的图片）

# 使用非参数拟合识别最佳分布拟合。

非参数密度估计是当总体样本是“*无分布的*”时，即数据不类似于常见的理论分布。在*distfit*中，实施了两种非参数方法用于非参数密度拟合：***分位数***方法和***百分位数***方法。这两种方法都假设数据不遵循特定的概率分布。对于*分位数方法*，对数据的分位数进行建模，这对于具有偏斜分布的数据可能很有用。对于*百分位数方法*，对百分位数进行建模，这对于数据包含多个峰值时可能很有用。在这两种方法中，*优点*是对异常值具有鲁棒性，并且不对潜在分布做出假设。在下面的代码部分，我们使用`method='quantile'`或`method='percentile'`进行初始化。所有功能，如预测和绘图，可以以与前面的代码部分相同的方式使用。

```py
# Load library
from distfit import distfit

# Create random normal data with mean=2 and std=4
X = np.random.normal(2, 4, 10000)

# Initialize using the quantile or percentile approach.
dfit = distfit(method='quantile')
dfit= distfit(method='percentile')

# Fit model on input data X and detect the best theoretical distribution.
dfit.fit_transform(X)
# Plot the results
fig, ax = dfit.plot()
```

# 识别离散数据的最佳分布。

如果随机变量是离散的，*distfit 库* 包含 ***离散拟合*** 的选项。最佳拟合是通过使用二项分布得到的。问题可以总结如下：给定一组非负整数，我们能否为离散分布拟合一个概率分布，并比较拟合的质量？对于离散量，正确的术语是概率质量函数（PMF）。就离散分布而言，一个整数列表的 PMF 的形式为 P(k)，只能拟合到二项分布中，使用适当的 `n` 和 `p` 值，这种方法在 *distfit* 中实现。请参见下面的代码部分，其中创建了一个离散数据集，`n=8` 和 `p=0.5`。随机变量作为输入传递给 *distfit*，它检测到参数 `n=8` 和 `p=0.501366`，表明拟合非常好。

```py
# Load library
from scipy.stats import binom
from distfit import distfit

# Parameters for the test-case:
n = 8
p = 0.5
# Generate 10000 randon discrete data points of the distribution of (n, p)
X = binom(n, p).rvs(10000)
# Initialize using the discrete approach.
dfit = distfit(method='discrete')
# Find the best fit.
dfit.fit_transform(X)

# print results
print(dfit.model)
# 'name': 'binom',
# 'score': 0.00010939074999009602,
# 'chi2r': 1.5627249998585145e-05,
# 'n': 8,
# 'p': 0.501366,
# 'CII_min_alpha': 2.0,
# 'CII_max_alpha': 6.0}
# 'distr': <scipy.stats._discrete_distns.binom_gen at 0x14350be2230>,
# 'model': <scipy.stats._distn_infrastructure.rv_discrete_frozen at 0x14397a2b640>,

# Make predictions
results = dfit.predict([0, 2, 8])
```

使用 `plot` 功能绘制结果。

```py
# Plot the results
dfit.plot()

# Change colors or remove parts of the figure.
# Remove emperical distribution
dfit.plot(emp_properties=None)
# Remove PDF
dfit.plot(pdf_properties=None)
# Remove histograms
dfit.plot(bar_properties=None)
#Remove confidence intervals
dfit.plot(cii_properties=None)
```

![](img/1339c5e518c284cf21766c01752e2316.png)

图 7。顶部图展示了输入数据（黑点）和拟合的分布（蓝线）。检测到的参数（n=8 和 p=0.501366）非常贴合，考虑到生成的随机变量（n=8，p=0.5）。红色垂直条表示置信区间，设置为 0.05（默认）。底部图展示了在 n 次扫描中的 RSS 分数。最佳拟合通过最低的 RSS 来检测。（图片由作者提供）

# 分布拟合的应用。

知道数据集中的基础分布在许多应用中是关键。我将总结几个应用。

+   **异常/新颖性检测** 是密度估计的一个明显应用。这可以通过计算给定分布和参数的置信区间来实现。*distfit* 库计算置信区间，以及给定拟合分布的样本为异常值/新颖性的概率。代码部分展示了一个小示例，但请跟随链接深入了解。

## 使用分布拟合进行单变量数据集的异常检测

### 学习如何使用概率密度函数来检测异常值，这种方法适用于快速轻量级的模型，并且具有解释性……

towardsdatascience.com](/outlier-detection-using-distribution-fitting-in-univariate-data-sets-ac8b7a14d40e?source=post_page-----a26e5673b4bd--------------------------------)

```py
# Import libraries
import numpy as np
from distfit import distfit

# Create random normal data with mean=2 and std=4
X = np.random.normal(2, 4, 10000)

# Initialize using the parametric approach (default).
dfit = distfit(multtest='fdr_bh', alpha=0.05)

# Fit model on input data X.
dfit.fit_transform(X)
# With the fitted model we can make predictions on new unseen data.
y = [-8, -2, 1, 3, 5, 15]
dfit.predict(y, todf=True)

# Print results
print(dfit.results['df'])
#      y   y_proba y_pred         P
# 0  -8.0  0.017455   down  0.005818
# 1  -2.0  0.312256   none  0.156128
# 2   1.0  0.402486   none  0.399081
# 3   3.0  0.402486   none  0.402486
# 4   5.0  0.340335   none  0.226890
# 5  15.0  0.003417     up  0.000569

# Plot the results
dfit.plot()
```

+   **合成数据**：概率分布拟合可以用于生成模拟现实数据的合成数据。通过使用现实数据拟合概率分布，可以生成合成数据，用于测试假设和评估算法性能。下面的代码部分展示了如何通过估计分布参数从正态分布中生成随机变量的一个小例子。深入细节可以在这个博客中找到：

[## 逐步指南生成合成数据，通过从单变量分布中抽样](https://example.org/step-by-step-guide-to-generate-synthetic-data-by-sampling-from-univariate-distributions-6b0be4221cb1?source=post_page-----a26e5673b4bd--------------------------------)

### 了解如何创建合成数据，以备项目数据不足时使用，或用于模拟。

[towardsdatascience.com](https://example.org/step-by-step-guide-to-generate-synthetic-data-by-sampling-from-univariate-distributions-6b0be4221cb1?source=post_page-----a26e5673b4bd--------------------------------)

```py
# Import libraries
import numpy as np
from distfit import distfit

# Create random normal data with mean=2 and std=4
X = np.random.normal(2, 4, 10000)

# Initialize using the parametric approach (default).
dfit = distfit()

# Fit model on input data X.
dfit.fit_transform(X)

# The fitted distribution can now be used to generate new samples.
X_synthetic = dfit.generate(n=1000)
```

+   **优化和压缩**：概率分布拟合可以用于优化概率分布的各种参数，如均值和方差，以最适合数据。找到最佳参数有助于更好地理解数据。此外，如果数十万次观察只需用 loc、scale 和 arg 参数描述，那么这是对数据的强大压缩。

+   **非正式调查**输入数据集的属性是密度估计的非常自然的应用。密度估计可以提供数据偏斜和多模态的有价值指示。在某些情况下，它们将得出可能被认为是自明的结论，而在其他情况下，它们将指引进一步分析和数据收集的方向。

+   **检验假设**：概率分布拟合可以用于检验关于数据集潜在概率分布的假设。例如，可以使用拟合优度检验将数据与正态分布进行比较，或使用卡方检验将数据与泊松分布进行比较。

+   **建模**：概率分布拟合可以用于建模复杂系统，如天气模式、股票市场趋势、生物学、种群动态和预测性维护。通过将概率分布拟合到历史数据中，可以提取有价值的见解，并创建一个可以用来预测未来行为的模型。

# 结束语。

我涉及了参数化、非参数化和离散随机变量的概率密度拟合概念。使用*distfit*库，可以轻松检测 89 种理论分布中的最佳理论分布。它管道化了直方图的密度估计、分布参数的估计、拟合优度测试，并返回最佳拟合分布的参数。最佳拟合可以通过各种绘图功能进行探索，如直方图、CDF/PDF 图和 QQ 图。所有图表均可自定义并轻松组合。此外，还可以对新的未见样本进行预测。另一项功能是使用拟合模型参数生成合成数据。

在数据科学领域，了解分布拟合具有重要的好处。这不仅有助于更好地理解、探索和准备数据，还能提供快速而轻量的解决方案。要认识到统计测试只是帮助你朝正确方向查看，而*选择最佳模型不是一个统计问题；它是一个建模决策。* *明智地选择你的模型。*

*保持安全，保持冷静。*

***致敬 E.***

*如果你觉得这篇文章有帮助，欢迎* [*关注我*](https://erdogant.medium.com/subscribe) *，因为我写更多关于数据科学的内容。如果你考虑购买 Medium 会员，可以通过我的推荐链接支持我的工作。价格与一杯咖啡相同，但这允许你每月阅读无限数量的文章。*

# 软件

+   [Distfit Github/文档](https://erdogant.github.io/distfit/)

+   [Colab Notebook 示例](https://erdogant.github.io/distfit/pages/html/Documentation.html#colab-notebook)

## 让我们联系吧！

+   [在 LinkedIn 上联系我](https://www.linkedin.com/in/erdogant/)

+   [在 Github 上关注我](https://github.com/erdogant)

+   [在 Medium 上关注我](https://erdogant.medium.com/)

# 术语。

本博客中的定义/术语：

> **“随机变量**是其值未知的变量或一个将值分配给实验结果的函数。随机变量可以是离散的（具有特定值）或连续的（在连续范围内的任何值）” [1]。它可以是数据集中某个特定特征的单列，例如人类身高。也可以是通过传感器测量的整个数据集，包含成千上万个特征。
> 
> **概率密度函数**（PDF）是一种统计表达式，定义了连续随机变量的[概率分布](https://www.investopedia.com/terms/p/probabilitydistribution.asp)（结果的可能性）。正态分布是 PDF 的一个常见示例（著名的钟形曲线）。PDF 有时也被称为“分布函数”或“概率函数”。
> 
> **理论分布**是 PDF 的一种形式。理论分布的示例包括正态分布、二项分布、指数分布、泊松分布等 [7]。distfit 库包含 89 种理论分布。
> 
> **经验分布**（或数据分布）是观察到的随机变量（输入数据）的频率基础分布 [8]。直方图常用于可视化经验分布。

# 参考文献

1.  *W. Kenton,* [*概率密度函数（PDF）的基础知识及示例*](https://www.investopedia.com/terms/p/pdf.asp)*, 2022, Investopedia.*

1.  *E. Taskesen,* *PCA 负载是什么以及如何有效使用双图*? *Medium 2022.*

1.  *P. Varshney,* *Q-Q 图解**, Medium 2020.*

1.  [*伽玛分布*](https://en.wikipedia.org/wiki/Gamma_distribution)*，维基百科。*

1.  *A. Downey,* [*你的数据是否符合正态分布？提示：不符合。*](http://allendowney.blogspot.com/2013/08/are-my-data-normal.html) *2018，博客。*

1.  [*概率密度函数*](https://en.wikipedia.org/wiki/Probability_density_function)*，维基百科。*

1.  [*概率分布列表*](https://en.wikipedia.org/wiki/List_of_probability_distributions)*，维基百科*

1.  [*经验分布函数*](https://en.wikipedia.org/wiki/Empirical_distribution_function)*，维基百科*

1.  *G. Jogesh Babu, Eric D. Feigelson,* [*天体统计学：拟合优度及其他*](https://articles.adsabs.harvard.edu/pdf/2006ASPC..351..127B)*，ASP 会议系列，第 351 卷，2006 年*

1.  E. Taskesen, *通过分布拟合在单变量数据集中检测异常值*，Medium 2023

1.  E. Taskesen, *逐步指南：通过从单变量分布中抽样生成合成数据*，Medium 2023
