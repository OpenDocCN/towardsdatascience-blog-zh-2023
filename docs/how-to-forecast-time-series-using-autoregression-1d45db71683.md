# 如何使用自回归进行时间序列预测

> 原文：[`towardsdatascience.com/how-to-forecast-time-series-using-autoregression-1d45db71683`](https://towardsdatascience.com/how-to-forecast-time-series-using-autoregression-1d45db71683)

## 关于如何在 Python 中使用自回归模型进行预测的教程

[](https://medium.com/@egorhowell?source=post_page-----1d45db71683--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----1d45db71683--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1d45db71683--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1d45db71683--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----1d45db71683--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1d45db71683--------------------------------) ·阅读时间 7 分钟·2023 年 1 月 17 日

--

![](img/081074115ceaa168aa8c4a20d048276e.png)

[Aron Visuals](https://unsplash.com/@aronvisuals?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供的照片

# 背景

预测是一个广泛的领域，提供了许多模型来模拟你的时间序列。在我之前的文章中，我们介绍了一些 [基本预测模型](https://medium.com/towards-data-science/basic-forecasting-techniques-ef4295248e46) 并探讨了流行的 [指数平滑模型](https://medium.com/towards-data-science/forecasting-with-simple-exponential-smoothing-dd8f8470a14c)。

在这篇文章中，我们将开始探索另一类预测模型，从 [自回归](https://en.wikipedia.org/wiki/Autoregressive_model) 开始。我们将讨论预测这种模型所需的理论和背景，然后深入 Python 教程。

补充视频。

# 什么是自回归？

## 概述

自回归是指使用时间序列的前几值（滞后值）的某些线性加权组合来预测时间序列。由于我们是在对目标值进行自我回归，这被称为*自*回归。

从数学上讲，我们可以将自回归写作：

![](img/c89fcef7348c5e8fd54582102b3ddbaf.png)

方程由作者在 LaTeX 中生成。

其中 ***y*** 是我们在不同时间步预测的时间序列， ***ϕ*** 是时间序列的滞后值的拟合系数， ***ε*** 是 [误差项](https://en.wikipedia.org/wiki/White_noise)（通常 [**服从正态分布**](https://en.wikipedia.org/wiki/Normal_distribution)），***p*** 是模型中包含的滞后组件的数量，这也称为 ***阶数***。

从这个自回归方程中可以得出一些著名的模型：

+   如果我们没有系数或它们全为零，那么这就是 [***白噪声***](https://en.wikipedia.org/wiki/White_noise)

+   如果我们只有 ***ϕ_1 = 1*** 且其他系数为零，那么这就是一个 [***随机游走***](https://en.wikipedia.org/wiki/Random_walk)。

## 要求

要建立自回归模型，建议使用 [***平稳的***](https://en.wikipedia.org/wiki/Stationary_process) 时间序列。平稳性意味着时间序列不表现出任何长期趋势或明显的季节性。我们需要平稳性的原因是为了确保时间序列的统计属性在时间上是一致的，从而使建模变得更容易（稍后会详细解释）。

可以通过 [***差分***](https://otexts.com/fpp2/stationarity.html) 稳定趋势，并通过 [***对数或 Box-Cox 变换***](https://en.wikipedia.org/wiki/Power_transform) 稳定方差，从而实现平稳性。如果你想了解更多关于平稳性和这些变换的内容，可以查看我之前的相关文章：

[](/time-series-stationarity-simply-explained-125269968154?source=post_page-----1d45db71683--------------------------------) ## 时间序列平稳性简单解释

### 对时间序列建模中平稳性需求的简单直观解释。

towardsdatascience.com [](/box-cox-transform-for-time-series-cc45f26082c6?source=post_page-----1d45db71683--------------------------------) ## 时间序列的 Box-Cox 变换

### 如何使用 Box-Cox 变换创建平稳的时间序列。

towardsdatascience.com

你还可以进行平稳性的统计检验。最流行的检验是 [***扩展的迪基-福勒***](https://en.wikipedia.org/wiki/Augmented_Dickey%E2%80%93Fuller_test) ***(ADF)*** 检验，其原假设是数据 **不** 平稳。

## 估计

在训练模型时，平稳性的必要性变得更加明确。平稳数据具有恒定的统计属性，如均值和方差。因此，所有数据点都属于相同的统计概率分布，我们可以基于该分布建立模型。此外，预测被视为随机变量，将属于与训练数据（滞后）相同的分布。这基本上 **保证了未来的数据会在某种程度上类似于过去的数据**。

> 请参见这个 [statsexchange](https://stats.stackexchange.com/questions/19715/why-does-a-time-series-have-to-be-stationary) 讨论线程，了解自回归建模对平稳性要求的多个深入原因。

由于平稳数据属于某种分布（通常是正态分布），我们经常使用***最大似然估计 (MLE)***来估计自回归模型的系数和参数。MLE 推导出使我们获得时间序列数据的概率最高的参数和系数的最优值。对于正态分布的数据，MLE 的结果与进行[**普通最小二乘法**](https://en.wikipedia.org/wiki/Ordinary_least_squares)的结果相同。因此，最小二乘法也被频繁使用。

> 这里有一个关于 MLE 的绝佳和详细解释的链接：here。

还有其他选择最佳参数和系数的方法，例如[***赤池信息量准则 (AIC)***](https://en.wikipedia.org/wiki/Akaike_information_criterion)、[***贝叶斯信息量准则***](https://en.wikipedia.org/wiki/Bayesian_information_criterion)和[***汉南-奎因信息量准则 (HQIC)***](https://en.wikipedia.org/wiki/Hannan%E2%80%93Quinn_information_criterion)。

## 阶数选择

在拟合和估计模型之前，我们需要知道要包含多少个滞后（阶数），***p***。一种方法是绘制时间序列的[**偏自相关函数 (PACF)**](https://online.stat.psu.edu/stat510/lesson/2/2.2)。这测量了某些滞后如何**直接**彼此相关。因此，我们可以推导出哪些滞后是最[***统计显著***](https://en.wikipedia.org/wiki/Statistical_significance)的，并在构建模型时删除不显著的滞后。我们将在文章后面的 Python 教程中详细介绍如何执行这个过程。

如果你想了解更多关于 PACF 的信息，可以查看我之前关于它的文章：

[](/partial-autocorrelation-for-time-series-481a9cfa7526?source=post_page-----1d45db71683--------------------------------) ## 时间序列分析的偏自相关

### 描述什么是偏自相关及其在时间序列分析中的重要性

towardsdatascience.com

然而，另一种更彻底的技术是简单地遍历所有可能的滞后组件组合，并选择在 AIC 上得分最高的模型。这类似于常规的超参数调整，绝对是更稳健的方法，但当然受限于计算约束。

# Python 演练

现在我们将使用美国航空公司乘客数据集进行一个简单的自回归建模演练！

> 数据 [来自 Kaggle](https://www.kaggle.com/datasets/ashfakyeafi/air-passenger-data-for-time-series-analysis) ，具有 CC0 许可。

## 数据

首先让我们绘制时间序列：

作者的 GitHub Gist。

![](img/87f5dd5f5d2da117e26234afff4d36d3.png)

图表由作者在 Python 中生成。

时间序列具有明确的趋势和明显的年度季节性，并且这种季节性随着时间的推移而增加。因此，我们需要通过进行差分和应用 Box-Cox 变换来使其平稳：

GitHub Gist 由作者提供。

![](img/292a8d8d14a569b5a44375d2d96cc7b4.png)

图表由作者在 Python 中生成。

时间序列现在看起来平稳，但是我们可以通过之前描述的 ADF 测试以更定量的方式确认这一点：

GitHub Gist 由作者提供。

![](img/9ec36c714586d235d65add659d6b8dab.png)

图像由作者在 Python 中生成。

`P-Value`低于**5%**，因此有理由拒绝原假设，我们可以说时间序列是令人满意的平稳。为了使其更加平稳，我们本可以进行二阶差分和季节差分。

## 建模

我们通过绘制 PACF 来开始建模阶段，以查看哪些滞后在统计上显著：

GitHub Gist 由作者提供。

![](img/e95c33edafdb5b80a2f102aa3498b711.png)

图表由作者在 Python 中生成。

蓝色阴影区域之外的滞后被归类为显著的，应该作为自回归模型的特征包含在内。从上面的图中，似乎滞后 1、2、4、7、8、9、10、11、12 和 13 是显著的。注意滞后 12 具有最大的峰值。这是因为我们的时间序列按月份索引，并且具有年度季节性，因此滞后 12 是确切的一年差。

然而，为了构建我们的模型，我们将使用推荐的方法，即简单地遍历所有可能的滞后组合，并从该分析中选择最佳模型。由于我们的数据集相当小，这在计算上是完全可行的。

GitHub Gist 由作者提供。

在这里，我们使用 statsmodels 的`ar_select_order`来确定在自回归模型中包含的最佳滞后数量。在这种情况下，我们将模型设置为尝试滞后最多到 15。然后，使用来自 statsmodels 的`AutoReg`类将`ar_select_order`的结果拟合到模型中。

## 结果

从这个拟合模型中产生的预测是针对之前生成的差分和 Box-Cox 变换后的时间序列。因此，我们必须对预测进行反差分和逆 Box-Cox 变换，以获得实际的航空乘客预测量：

GitHub Gist 由作者提供。

![](img/8579774a2852090f5847f0f09575d159.png)

图表由作者在 Python 中生成。

> **预测结果非常好！**

我们的自回归模型预测已经充分捕捉了时间序列中的趋势和季节性。然而，季节性的捕捉是由于模型具有 13 的阶数（滞后）。这意味着它包含了过去一年中的所有滞后（每个月一个），从而容易捕捉到季节性，因为它非常规律。

# 摘要与进一步思考

在这篇文章中，我们深入探讨了自回归的常见预测模型。这就像线性回归，只不过特征只是目标在不同时间步的前值。使用自回归模型时，你的数据必须是平稳的，这意味着它需要具有恒定的均值和方差。使用自回归进行预测非常简单，可以通过 statsmodels Python 包来完成。

本文中使用的完整代码可以在我的 GitHub 上找到：

[](https://github.com/egorhowell/Medium-Articles/blob/main/Time%20Series/ARIMA/autoregression.py?source=post_page-----1d45db71683--------------------------------) [## Medium-Articles/autoregression.py 在主分支 · egorhowell/Medium-Articles

### 我在我的 Medium 博客/文章中使用的代码。通过创建一个账户来贡献于 egorhowell/Medium-Articles 的开发...

github.com](https://github.com/egorhowell/Medium-Articles/blob/main/Time%20Series/ARIMA/autoregression.py?source=post_page-----1d45db71683--------------------------------)

# 另一件事！

我有一个免费的新闻通讯，[**Dishing the Data**](https://dishingthedata.substack.com/)，在其中我每周分享成为更好的数据科学家的技巧。没有“空话”或“吸引点击”，只有来自实践中的数据科学家的纯粹可操作的见解。

[](https://newsletter.egorhowell.com/?source=post_page-----1d45db71683--------------------------------) [## Dishing The Data | Egor Howell | Substack

### 如何成为更好的数据科学家。点击阅读 Egor Howell 的《Dishing The Data》，这是一个 Substack 出版物...

newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----1d45db71683--------------------------------)

# 与我联系！

+   [**YouTube**](https://www.youtube.com/@egorhowell)

+   [**LinkedIn**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)

# 参考文献及进一步阅读

+   *预测：原则与实践:* [`otexts.com/fpp2/`](https://otexts.com/fpp3/accuracy.html)
