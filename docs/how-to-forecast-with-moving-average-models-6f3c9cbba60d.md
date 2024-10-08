# 如何使用移动平均模型进行预测

> 原文：[`towardsdatascience.com/how-to-forecast-with-moving-average-models-6f3c9cbba60d`](https://towardsdatascience.com/how-to-forecast-with-moving-average-models-6f3c9cbba60d)

## 关于如何使用移动平均模型进行时间序列分析预测的教程和理论

[](https://medium.com/@egorhowell?source=post_page-----6f3c9cbba60d--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----6f3c9cbba60d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6f3c9cbba60d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6f3c9cbba60d--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----6f3c9cbba60d--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----6f3c9cbba60d--------------------------------) ·6 分钟阅读·2023 年 1 月 23 日

--

![](img/6fb0947aa4194f06a35db89cc3cb7f8e.png)

照片由[Sigmund](https://unsplash.com/de/@sigmund?utm_source=medium&utm_medium=referral)提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

在我之前的帖子中，我们讨论了[***自回归***](https://medium.com/towards-data-science/how-to-forecast-time-series-using-autoregression-1d45db71683)。这是通过对该时间序列的先前观测值的某种线性加权组合来预测未来值的方法。我们也可以使用过去的***预测误差***来进行预测。这被称为[***移动平均***](https://en.wikipedia.org/wiki/Moving-average_model) ***(MA)***模型。

> 这与[**滚动均值模型**](https://en.wikipedia.org/wiki/Moving_average)不应混淆，后者也被称为移动平均模型。

在这篇帖子中，我想讲解移动平均预测模型背后的理论和框架，然后深入讲解如何在 Python 中实现它！

补充视频。

# 移动平均模型理论

## 概述

如上所述，移动平均模型通过拟合系数***θ***到之前预测的误差***ε***（也称为[***白噪声误差***](https://en.wikipedia.org/wiki/White_noise)），并加上一个常数项，即均值***μ***，具有*回归-like*特性：

![](img/cdf6a947a5b15204b7124d76b4dbe89e.png)

方程由作者在 LaTeX 中生成。

这是一种***MA(q)模型***，其中***q***是误差项的数量，称为**阶数**。

## 要求

MA(q)模型的一个关键要求是，与自回归模型类似，它需要数据是***平稳的***。这意味着数据具有**时间上的常数方差和均值**。这可以通过[***差分***](https://otexts.com/fpp2/stationarity.html)和通过***对数或 Box-Cox 变换***来稳定方差。

如果你想了解更多关于平稳性和这些变换的内容，请查看我之前关于这些主题的文章：

[](/time-series-stationarity-simply-explained-125269968154?source=post_page-----6f3c9cbba60d--------------------------------) ## 时间序列平稳性简明解释

### 对时间序列建模中平稳性需求的简单直观解释。

towardsdatascience.com [](/box-cox-transform-for-time-series-cc45f26082c6?source=post_page-----6f3c9cbba60d--------------------------------) ## Box-Cox 变换用于时间序列

### 如何使用 Box-Cox 变换创建一个平稳的时间序列。

towardsdatascience.com

## 阶数选择

选择 MA 模型的阶数可以通过查看[***自相关函数***](https://medium.com/towards-data-science/autocorrelation-for-time-series-analysis-86e68e631f77)来完成。自相关度量了**时间序列在不同时间步（滞后）的相关性**。如果一个滞后具有较高的相关性，那么它在描述时间序列当前值时具有影响力。这个‘影响’的概念可以用于进行预测。

我们可以使用自相关来绘制时间序列的[***自相关图***](https://en.wikipedia.org/wiki/Correlogram)，以确定哪些滞后值对预测有[***统计显著性***](https://en.wikipedia.org/wiki/Statistical_significance)。如果现在很难可视化，不用担心，我们稍后会进行关于这一过程的 Python 教程！

如果你想进一步了解自相关，请查看我关于它的上一篇文章：

[](/autocorrelation-for-time-series-analysis-86e68e631f77?source=post_page-----6f3c9cbba60d--------------------------------) ## 时间序列分析中的自相关

### 描述自相关是什么以及它在时间序列分析中为何有用。

towardsdatascience.com

然而，另一种更常见的方法是简单地遍历不同的组合/阶数，并使用给出最佳性能的模型。最佳性能通常通过返回最佳 [***Akaike 信息准则***](https://en.wikipedia.org/wiki/Akaike_information_criterion) ***(AIC)*** 值的模型来评估，这个值基于 [***最大似然估计***](https://en.wikipedia.org/wiki/Maximum_likelihood_estimation)。这个过程类似于经典机器学习中的 [***超参数调优***](https://medium.com/towards-data-science/optimise-your-hyperparameter-tuning-with-hyperopt-861573239eb5)。

## 估计

现在我们已经确定了 MA 项的顺序，我们需要拟合它们的系数！ 不幸的是，这不像线性模型或自回归模型那样简单，因为误差不可观察。那么，我们该怎么做呢？

好吧，这并不那么直接，我会在 [***这里***](https://online.stat.psu.edu/stat510/lesson/2/2.1) 留一个链接，提供完整的数学演示，详细解释这个过程。然而，概括来说，**每个滞后期的自相关值与其系数直接相关**。因此，一旦你知道了预测误差的自相关值，你就可以向后推导出其系数。

例如，对于 ***MA(1)*** 模型（具有一个预测误差项的模型），其自相关系数 ***c_1*** 为：

![](img/d94a18f3d1ee44307c0a118e948c2307.png)

作者在 LaTeX 中生成的公式。

可以看到，如果我们知道 ***c_1***，那么这只是一个简单的二次方程，我们可以解出系数。这种想法可以扩展到 ***MA(q)*** 模型，但由于多个误差和系数，复杂性自然增加。

# Python 教程

现在让我们在 Python 中实现移动平均模型吧！

> 数据 [来自 Kaggle](https://www.kaggle.com/datasets/ashfakyeafi/air-passenger-data-for-time-series-analysis) 并具有 CC0 许可证。

## 数据

首先绘制数据：

作者的 GitHub Gist。

![](img/87f5dd5f5d2da117e26234afff4d36d3.png)

作者在 Python 中生成的图。

很明显，时间序列具有年度季节性和一致的上升趋势。因此，我们需要通过差分和 Box-Cox 变换来使其平稳：

作者的 GitHub Gist。

![](img/292a8d8d14a569b5a44375d2d96cc7b4.png)

作者在 Python 中生成的图。

数据现在似乎是平稳的。我们本可以通过进行二阶差分或季节性差分来进一步使其平稳，但我认为这里的结果已经令人满意。

如果你想了解更多关于这些更复杂的差分方法，请查看我之前的博客文章：

[](/seasonality-of-time-series-5b45b4809acd?source=post_page-----6f3c9cbba60d--------------------------------) ## 时间序列的季节性

### 直观地理解季节性如何影响时间序列分析

towardsdatascience.com

## 模型

我们现在可以通过找到最优的阶数来开始建模阶段。为此，我们绘制一个自相关图，如上所述：

作者的 GitHub Gist。

![](img/c356ffd6710bdfaec3d2ca0783da0ea9.png)

由作者在 Python 中生成的图。

蓝色区域表示值不再具有统计显著性。从上面的图中，我们可以看到最后一个显著的滞后期是***第 13 个***。因此，我们的**MA 模型的阶数将是 13**。

不幸的是，Python 中不存在直接的 MA 模型函数或包，所以我们将使用 *[***ARIMA***](https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average)* 函数，并将每个组件的设置为零，除了移动平均订单。

> 不用担心 ARIMA 是什么或代表什么，我会在我的下一篇文章中介绍它！

作者的 GitHub Gist。

## 分析

现在让我们通过**去差分**数据并进行**逆** **Box-Cox 变换**来绘制预测图：

作者的 GitHub Gist。

![](img/4f621537ff8b5d210b0a5859c56122d3.png)

由作者在 Python 中生成的图。

我们可以看到预测结果大多很好地捕捉了趋势，但未能注意到季节性。在我上一篇文章中，训练在相同数据上的自回归模型能够捕捉到趋势和季节性。你可以在这里查看自回归建模的详细讲解：

[](/how-to-forecast-time-series-using-autoregression-1d45db71683?source=post_page-----6f3c9cbba60d--------------------------------) ## 如何使用自回归模型进行时间序列预测

### 关于如何在 Python 中使用自回归模型进行预测的教程

towardsdatascience.com

# 总结与进一步思考

在这篇文章中，我们学习了如何使用移动平均模型进行预测。这个模型通过前期预测误差的线性组合来预测未来的值。它在 Python 中实现起来很简单，但效果不如我上一篇文章中看到的自回归模型。

本文中使用的完整代码可以在我的 GitHub 上找到：

[](https://github.com/egorhowell/Medium-Articles/blob/main/Time%20Series/ARIMA/moving_average.py?source=post_page-----6f3c9cbba60d--------------------------------) [## Medium-Articles/moving_average.py at main · egorhowell/Medium-Articles

### 我在 Medium 博客/文章中使用的代码。通过在…上创建账户来贡献给 egorhowell/Medium-Articles 的开发。

github.com](https://github.com/egorhowell/Medium-Articles/blob/main/Time%20Series/ARIMA/moving_average.py?source=post_page-----6f3c9cbba60d--------------------------------)

# 另一个事项！

我有一个免费的新闻通讯，[**Dishing the Data**](https://dishingthedata.substack.com/)，我每周分享成为更好的数据科学家的技巧。没有“虚饰”或“点击诱饵”，只有来自实际数据科学家的纯粹可操作的见解。

[## Dishing The Data | Egor Howell | Substack](https://newsletter.egorhowell.com/?source=post_page-----6f3c9cbba60d--------------------------------)

### 如何成为更好的数据科学家。点击阅读《Dishing The Data》，由 Egor Howell 撰写，Substack 出版物...

[newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----6f3c9cbba60d--------------------------------)

# 与我联系！

+   [**YouTube**](https://www.youtube.com/@egorhowell?sub_confirmation=1)

+   [**领英**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**推特**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)

# 参考资料与进一步阅读

+   *预测：原理与实践:* [`otexts.com/fpp2/`](https://otexts.com/fpp3/accuracy.html)
