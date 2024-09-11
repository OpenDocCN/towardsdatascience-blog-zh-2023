# QQ 图绘制你的数据启蒙：分布的银河系旅行指南

> 原文：[https://towardsdatascience.com/qq-plotting-your-way-to-data-enlightenment-a-hitchhikers-guide-to-the-galaxies-of-distribution-398352c9199?source=collection_archive---------12-----------------------#2023-04-26](https://towardsdatascience.com/qq-plotting-your-way-to-data-enlightenment-a-hitchhikers-guide-to-the-galaxies-of-distribution-398352c9199?source=collection_archive---------12-----------------------#2023-04-26)

## 你的数据正常吗？终极指南：使用 R 绘制 QQ 图

[](https://namanagr03.medium.com/?source=post_page-----398352c9199--------------------------------)[![Naman Agrawal](../Images/6bb885397aec17f5029cfac7f01edad9.png)](https://namanagr03.medium.com/?source=post_page-----398352c9199--------------------------------)[](https://towardsdatascience.com/?source=post_page-----398352c9199--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----398352c9199--------------------------------) [Naman Agrawal](https://namanagr03.medium.com/?source=post_page-----398352c9199--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5bbb90aa727&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fqq-plotting-your-way-to-data-enlightenment-a-hitchhikers-guide-to-the-galaxies-of-distribution-398352c9199&user=Naman+Agrawal&userId=5bbb90aa727&source=post_page-5bbb90aa727----398352c9199---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----398352c9199--------------------------------) · 15分钟阅读 · 2023年4月26日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F398352c9199&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fqq-plotting-your-way-to-data-enlightenment-a-hitchhikers-guide-to-the-galaxies-of-distribution-398352c9199&user=Naman+Agrawal&userId=5bbb90aa727&source=-----398352c9199---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F398352c9199&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fqq-plotting-your-way-to-data-enlightenment-a-hitchhikers-guide-to-the-galaxies-of-distribution-398352c9199&source=-----398352c9199---------------------bookmark_footer-----------)![](../Images/b5168e4df2a891b3242b01347e9ac486.png)

图片来源于 [Pete Linforth](https://pixabay.com/users/thedigitalartist-202249/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4851119) [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4851119)

# 介绍

统计学是一个神秘的领域。在其广泛的理论、公式和框架中，蕴含着对各个学科领域都有深刻应用的知识。然而，尽管统计学取得了巨大的成功，但它也可能显得不那么友好，因为它的很多能力来自它所做的假设。一些最常见的假设包括独立同分布假设（i.i.d.）和正态性假设（数据可以近似或收敛于正态分布）。一旦这些假设开始破裂，理论就会变得越来越复杂，即使它们工作，它们也往往会失去很多力量。幸运的是，有支持这些假设的定理和法则，比如中心极限定理，它表明对于大样本量的各种分布的估计量，正态性假设是成立的。然而，为了充分实现统计学的潜力并利用其力量，必须在使用统计框架或测试得出结论之前，验证数据是否符合这些假设至关重要。通过仔细评估数据是否满足必要的假设，并相应调整我们的方法，我们可以确保统计分析提供准确可靠的见解，帮助我们在广泛的学科领域做出明智的决策。

在本文中，我们将探讨一种最强大的方法，即 QQ 图，用于测试你的数据是否符合正态分布。顾名思义，这是一种需要通过视觉检查来得出结论的图形方法，以判断数据是否正态分布。在统计学中，并没有太多的视觉方法，因为大多数假设检验框架依赖于估计量、临界区域和 p 值来获得数值估计。测试正态性的框架（如 Kolmogorov Smirnov 检验）也存在，但与使用 QQ 图的简单方法相比，这些框架被发现要弱得多。此外，需要承认的是，QQ 图可以用来测试数据是否遵循任何分布，而不仅仅是正态分布。例如，如果你认为收入是指数分布的，你仍然可以使用 QQ 图来检查。只是该测试在正态分布的背景下使用最广泛，因为正态分布在统计学中有着巨大的实用价值。

# 为什么不使用直方图？

这是一个有效的问题：如果构建 QQ 图也是一种图形方法，为什么不简单地绘制你的数据的直方图，并将其与正态分布的直方图进行比较呢？然而，这里有一些需要考虑的问题。

1.  你会选择什么样的分 bin 尺寸呢？虽然很大的 bin 尺寸可能会导致你错过分布中的一些重要特征，但很小的 bin 宽度可能会显示出很多噪音，使解释变得困难。选择适当的 bin 宽度可以显著改变你对底层分布的理解和认知。

1.  此外，直观检查直方图与正态分布曲线之间的拟合情况可能会显得繁琐且主观。你可以覆盖一个正态分布曲线（如图1所示），查看它与条形的高度匹配程度，但这个过程容易出错。

![](../Images/2b51f05061002df48b745410e350b2ac.png)

图1：使用直方图的难度 [图像由作者提供]

使用QQ图要简单得多，因为绘制图形不需要调整任何参数。你只需输入数据并生成图形。此外，视觉检查要容易得多：你只需检查两条线的匹配程度，而不必比较沿着钟形曲线不同位置的条形，这在数据偏斜或多峰时尤其具有挑战性。

# QQ图是什么？

QQ图（分位数-分位数图）是将给定数据的样本（或观察到的）分位数与理论（或预期）分位数进行绘图。由于我们将使用这些图来检查数据是否呈正态分布，因此理论分位数必须与正态分布的分位数相对应。但是，这些分位数是什么？

分位数是将分布划分为相等部分的值。例如，中位数是第50百分位数（或分位数），将分布分为两个相等的部分。

样本分位数是基于样本数据估计的分位数。给定一个包含n个观察值的样本，第k个样本分位数是将样本分为两部分的值：第一部分为k/n，剩余部分为(n-k)/n。例如，假设我们有以下数据：

![](../Images/f85e65dab419cdaf360c0e32df546ce6.png)

如何计算第50个分位数？第一步是将数据按升序排列，得到：

![](../Images/46cd5aa539aaf30dd5014ebaeed8effd.png)

注意到值37将数据分成了两个相等的部分。5，…，21构成了第一部分，占剩余数据的5/10。43，…，88构成了第二部分，占剩余数据的5/10。一般来说，第k小的样本被称为数据的第k阶统计量，表示为X₍ₖ₎。实际上，第k阶统计量是数据的100*k/(n + 1)分位数。

例如，在给定数据中，10是第3小的值。因此，X₍₃₎ = 10。这个值代表数据的100*3/(11 + 1) = 第25个分位数。上述一般化的证明稍微复杂，因为它利用了贝塔分布的性质和阶统计量的一般化CDF。为了完整性，下面给出了证明。然而，如果你不想了解这些内容，可以选择跳过并继续讨论样本分位数。

# 理论 [供感兴趣者参考]

**定理：** 对于大小为n的独立同分布数据X₁, X₂, · · · , Xₙ，第k阶统计量X₍ₖ₎是100*p分位数的无偏估计量，其中p由下式给出：

![](../Images/de4f1733c12b95a884ef3614a675f257.png)

换句话说，如果F是Xᵢ的CDF，

![](../Images/639743a3a05fbd6ad5b7a9185a2cf6e6.png)

（回顾一下，F表示累积分布。如果某个值的累积分布函数期望为k/(n +1)，则该值是k/(n +1)分位数的无偏估计量）

**证明：** 首先，我们找到均匀分布第k阶统计量的分布：回顾一下，标准均匀分布的概率密度函数和累积分布函数由下式给出：

![](../Images/193b6c9ab5f0601cb6d88db3ea792eba.png)

我们现在可以找到均匀分布第k阶统计量的CDF。设W为小于x的Xᵢ的数量。显然，W服从参数为n = n，p = Fᵤ(x)的二项分布。因此，第k阶统计量的CDF为：

![](../Images/4ba9f58ac13a4f0dd41347709633d164.png)

注：I是指示函数，当大括号内的条件满足时取值为1，否则为0。现在，我们可以对上述函数关于x进行微分，以得到均匀分布第k阶统计量的概率密度函数：

![](../Images/0d62bccb6921b56965c7518a60cf2cd2.png)

上述分布类似于贝塔分布的结构。回顾一下，Y ∼ Beta(a, b) 的密度函数及相应的期望值由下式给出：

![](../Images/4ee63803872f5a0557feefabf14f05cc.png)

通过比较均匀分布第k阶统计量的概率分布和贝塔分布的PDF，我们得出结论：

![](../Images/656c65a62b305979b1c7f9c64f0dc887.png)

接下来，我们依赖于统计学中最重要的公式之一的证明：我们展示了如果X是具有CDF Fₓ(x)的随机变量，则Fₓ(X)的PDF是标准均匀分布。设Y = Fₓ(X)。

![](../Images/2951e2c97d8f94d7cd24f5ca20f611c6.png)

最后，我们计算Fₓ(X₍ₖ₎)的分布。我们利用随机变量的CDF总是一个递增函数这一事实。因此，

![](../Images/b5acaacb2c123ce8c160433b909c07fd.png)

现在，让我们讨论理论分位数。理论分位数是指理论分布（如正态分布）的分位数。与基于给定数据估计的样本分位数不同，理论分位数由分布的参数（如均值和标准差）决定。其值不依赖于提供的样本，而仅依赖于用于检验数据的分布。例如，标准正态分布的5%理论分位数大约为-1.645，这意味着标准正态曲线下的5%面积位于-1.645的左侧。一般来说，如果我们假设给定数据具有分布N(µ, σ²)，则100*p（对于p在0和1之间）的理论分位数（πₚ）由下式给出：

![](../Images/38cc93241bc63e5a79e09f119da3cb5d.png)

这里Φ和Φ⁻¹分别指正态分布的累积分布函数和反函数。从上述讨论中可以明显看出，给定一个包含n个值的数据集，我们可以做两件事：

1.  我们可以确定数据集中每个值的相应的样本分位数。我们通过升序排列值来做到这一点，并得出第k个最小样本是第p = k/(n + 1)个样本分位数。

1.  对于每个给定的样本分位数，我们可以计算相应的理论分位数。特别是，如果我们想要测试我们的数据是否符合分布N(µ, σ²)，我们可以得出第p个理论分位数为 πₚ = µ+σΦ⁻¹(p)。

这正是构建QQ图的方案。让我们通过一个示例来理解这个过程。

# 构建QQ图：一个示例

考虑以下包含9个观察值的数据集：

![](../Images/ba864a2b472fb1ca4babe55c6dd11c43.png)

现在我们将为给定的数据集构建QQ图，以测试其是否符合N(1, 2)的分布。为简单起见，我们只取了10个观察值。然而，使用R或任何其他编程语言，可以将其扩展到任意数量的点，我们将在下一节中看到。为解决这个问题，我们考虑上述两个步骤：

**A) 我们可以确定数据集中每个值的相应的样本分位数。我们通过升序排列值来做到这一点，并得出第k个最小样本是第p = k/(n + 1)个样本分位数：** 首先，我们将数据集按升序排列，并分配相应的顺序统计值：

![](../Images/24f302319098a00d5a5edbf3c36c3a16.png)

表1 [作者提供的图像]

注意我们如何使用表格格式来表示数据。这在手工构建QQ图时可能非常有用，因为它允许您清晰地封装必要的信息。接下来，我们记录每个这些顺序统计量所估计的分位数：

![](../Images/00669b40ba197f2490d2bc8710960fa4.png)

表2 [作者提供的图像]

B) 对于每个给定的样本分位数，我们可以计算相应的理论分位数。特别是，如果我们想要测试我们的数据是否符合分布N(µ, σ²)，我们可以得出第p个理论分位数为 πₚ = µ + σΦ⁻¹(p)：利用导出的公式，我们可以估算如下正态分布的相应理论分位数：

![](../Images/79712b6039fb187d43c07244a5fa4a3e.png)

表3 [作者提供的图像]

Φ⁻¹的值可以使用z表或统计软件（例如R中的qnorm函数）找到。因此，我们得到了样本分位数值（ˆπp = X(k)）和理论分位数值πₚ = µ + σΦ⁻¹(p)。最后一步是将它们一起绘制在散点图中。这给出了以下图表：

![](../Images/3b2a314d5d33a4a6959da096840d7b52.png)

[作者提供的图像]

这就是给定数据的 QQ 图！是的，这只是理论分位数与样本分位数的图示。注意，我还添加了一条虚线 y = x，以便将给定的 QQ 图与理论分位数和样本分位数相同的情况进行比较。如果数据是正态分布的，那么 QQ 图上的点将沿着直线分布。点距离直线越近，数据越正态。相反，如果数据不是正态分布的，那么 QQ 图上的点将偏离直线。例如，在上述图中，样本分位数值总是低于相应的理论分位数值，这支持了数据不是正态分布的假设。实际上，上述数据集实际上是从均匀分布中获得的。因此，我们能够使用 QQ 图的概念来解释给定数据的正态性。

现在，让我们来看一个数据实际呈正态分布的示例。这次考虑一个大小为 14 的数据集：

![](../Images/5b554838d5b9e46b2bdf25f56a2e3c02.png)

第一步是绘制一个表格，将值按升序排列，并计算相应的顺序统计量及其代表的分位数。随后，我们填入理论分位数值。完成的表格如下所示：

![](../Images/44e335b34f41b0fa41fba4a35d1530a5.png)

表 4 [作者提供的图像]

如前所述，将上述点绘制在图表上得到如下 QQ 图：

![](../Images/926536cdbfad28b27fb25bd3afef80b2.png)

[作者提供的图像]

如你所见，这些点趋向于沿直线分布，这表明数据的分布与均值为 1 和方差为 2 的正态分布非常相似。请注意，如果数据集的样本点很少，QQ 图可能不会很有帮助。然而，如果样本数量足够多，QQ 图将成为测试数据集分布的强大工具，它将是你通往概率分布银河系的终极指南！

# 大样本：R 代码

我们将使用基础 R 中的 ToothGrowth 数据集，该数据集包含有关维生素 C 对豚鼠牙齿生长影响的信息。特别是，我们将查看 60 只豚鼠的牙本质细胞（负责牙齿生长的细胞）的长度，并测试牙齿长度的分布是否正态。我们可以如下加载数据集：

```py
# Load Packages
library(tidyverse)

# Load Dataset
data("ToothGrowth")
x = ToothGrowth$len
n = length(x)

# Length:
print(n)
> [1] 60
# First few observations
print(x[1:10])
> [1]  4.2 11.5  7.3  5.8  6.4 10.0 11.2 11.2  5.2  7.0
```

我们感兴趣的是测试数据集是否符合以下参数 µ 和 σ 的正态分布：

```py
# Mean of target distribution
mu <- mean(x)
print(mu)
> [1] 18.81333

# Variance of target distribution
sigma <- sd(x)
print(sigma)
> [1] 7.649315
```

**步骤 1：计算样本分位数** 为此，我们只需将数据按升序排列。相应的分位数值将从 1/(n + 1) 到 n/(n + 1)，如下所示：

```py
# Sample Quantiles: Sort Sample Provided
sq = sort(x)
print(sq[1:10])
> [1] 4.2 5.2 5.8 6.4 7.0 7.3 8.2 9.4 9.7 9.7

# Corresponding Quantile Values: k/(n + 1), k = 1, 2, ..., n
p = seq(1/(n + 1), n/(n + 1), by = 1/(n + 1))
print(p[1:5])
> [1] 0.01639344 0.03278689 0.04918033 0.06557377 0.08196721 
```

**步骤 2：计算理论分位数** 使用公式 πₚ = µ + σΦ⁻¹(p)，我们计算映射到每个样本分位数的理论分位数。注意：R 中的 qnorm() 函数允许我们计算正态分布的逆 CDF：

```py
# Theoretical Quantiles: Use Derived Formula
tq = mu + sigma*qnorm(p)
print(tq[1:5])
> [1] 2.484468 4.728449 6.170135 7.265988 8.165790
```

**步骤 3：将理论分位数与样本分位数绘图并与 y = x 比较**

```py
data.frame(tq, sq) %>%
  ggplot(aes(x = tq, y = sq)) +
  # Plot original points
  geom_point(size = 2.5, color = "blue") +
  # To plot the y = x line
  geom_abline(slope = 1, intercept = 0, linewidth = 0.8, linetype = 2) +
  # Plot styling [Optional]
  theme_minimal() +
  labs(x = "Theoretical Quantiles", y = "Sample Quantiles", title = "QQ Plot") +
  theme(plot.title = element_text(hjust = 0.5))
```

这产生了以下 QQ 图：

![](../Images/d07ab7a7dbd4169078b3b7226812446b.png)

[图像由作者提供]

如你所见，点趋向于沿线分布，表明数据的分布与均值为 18.81333 和方差为 7.649315² = 58.51202 的正态分布非常相似。最后，为了展示 QQ 图的巨大实用性，我们来看看另一个例子，我们尝试测试数据是否呈指数分布。特别是，我们将从均匀分布中抽取样本，并使用 QQ 图检查它是否呈指数分布。我们可以按如下方式加载样本：

```py
# Load Sample
x = runif(10000)
n = length(x)

# First few observations
print(x[1:5])
> [1] 0.07251043 0.20345894 0.20417683 0.48878998 0.50945799
```

我们感兴趣的是测试数据集是否呈指数分布，其速率参数等于均值的倒数（速率参数的最大似然估计量）。

**步骤 1：计算样本分位数** 为此，我们只需将数据按升序排列。相应的分位数值将从 1/(n + 1) 到 n/(n + 1)，如下面计算所示：

```py
# Sample Quantiles: Sort Sample Provided
sq = sort(x)
print(sq[1:5])
> [1] 6.094808e-05 9.398628e-05 3.439812e-04 3.590921e-04 4.317588e-04

# Corresponding Quantile Values: k/(n + 1), k = 1, 2, ..., n
p = seq(1/(n + 1), n/(n + 1), by = 1/(n + 1))
print(p[1:5])
> [1] 0.00009999 0.00019998 0.00029997 0.00039996 0.00049995
```

**步骤 2：计算理论分位数** 这次，我们使用公式 πₚ = Φ⁻¹(p, λ = 1/mean(X))，来计算映射到每个样本分位数的理论分位数，其中 Φ⁻¹(p, λ) 是指数分布的 CDF 的逆函数，速率为 λ。注意：R 中的 qexp() 函数允许我们计算指数分布的逆 CDF：

```py
# Theoretical Quantiles: Use Derived Formula
tq = qexp(p, rate = 1/mean(x))
print(tq[1:5])
> [1] 5.010184e-05 1.002087e-04 1.503205e-04 2.004374e-04 2.505593e-04
```

**步骤 3：将理论分位数与样本分位数绘图并与 y = x 比较**

```py
data.frame(tq, sq) %>%
  ggplot(aes(x = tq, y = sq)) +
  # Plot original points
  geom_point(size = 2.5, color = "blue") +
  # To plot the y = x line
  geom_abline(slope = 1, intercept = 0, linewidth = 0.8, linetype = 2) +
  # Plot styling [Optional]
  theme_minimal() +
  labs(x = "Theoretical Quantiles", y = "Sample Quantiles", title = "QQ Plot") +
  theme(plot.title = element_text(hjust = 0.5))
```

这产生了以下 QQ 图：

![](../Images/7613dbdd87cf218e005c02ceb87b75f8.png)

[图像由作者提供]

如你所见，点远离直线，表明数据的分布与指数分布不相似。事实上，我们可以绘制样本与均匀分布的 QQ 图，以展示样本确实适合标准均匀分布：

```py
# Theoretical Quantiles
tq = qunif(p)
print(tq[1:5])

data.frame(tq, sq) %>%
  ggplot(aes(x = tq, y = sq)) +
  # Plot original points
  geom_point(size = 2.5, color = "blue") +
  # To plot the y = x line
  geom_abline(slope = 1, intercept = 0, linewidth = 0.8, linetype = 2) +
  # Plot styling [Optional]
  theme_minimal() +
  labs(x = "Theoretical Quantiles", y = "Sample Quantiles", title = "QQ Plot") +
  theme(plot.title = element_text(hjust = 0.5))
```

这产生了以下 QQ 图，确实是一个完美的匹配！

![](../Images/ae4d983d89a7f534708ed8e500cccb63.png)

[图像由作者提供]

# 结论

总结来说，我们对QQ图的研究揭示了其在统计分析中的关键作用。通过将我们的数据分布与理论分布进行比较，QQ图使我们能够更深入地理解数据的行为，并识别出任何偏离预期分布的情况。此外，QQ图的多功能性使我们可以在各种场景中应用它们，例如识别异常值、比较数据集以及模型诊断。这些应用展示了QQ图的广泛用途，并强调了它们在统计分析领域的重要性。QQ图提供了一种强大且易于理解的数据分布探索方法，其在统计文献中的重要性证明了其价值。通过在我们的分析中利用QQ图，我们可以获得对数据的新见解，并基于对数据行为的更深入理解做出明智的决策。

如果你有任何疑问或建议，请在评论框中回复。请随时通过[邮件](mailto:naman.agr03@gmail.com)与我联系。

如果你喜欢我的文章并想阅读更多，请访问这个[链接](https://medium.com/@namanagr03)。

注意：所有包含表格、图表和方程式的图片均由作者制作。
