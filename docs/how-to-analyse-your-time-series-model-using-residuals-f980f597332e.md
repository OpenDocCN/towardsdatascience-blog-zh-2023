# 如何通过残差分析你的时间序列模型

> 原文：[`towardsdatascience.com/how-to-analyse-your-time-series-model-using-residuals-f980f597332e`](https://towardsdatascience.com/how-to-analyse-your-time-series-model-using-residuals-f980f597332e)

## 学习如何使用预测模型的残差来提高其性能

[](https://medium.com/@egorhowell?source=post_page-----f980f597332e--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----f980f597332e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f980f597332e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f980f597332e--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----f980f597332e--------------------------------)

·发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f980f597332e--------------------------------) ·阅读时间 5 分钟·2023 年 1 月 3 日

--

![](img/1b43c40b3df3d4128f518ab3b0f3669f.png)

照片由[regularguy.eth](https://unsplash.com/@moneyphotos?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

# 背景

能够分析你的时间序列模型对于诊断其性能至关重要。一个实现这一点的方法是通过拟合模型的残差。在这篇文章中，我们将讨论什么是残差以及如何利用它们来改进你的模型，并提供一个 Python 示例。

附加视频。

# 什么是残差？

在时间序列分析中，残差，***r***，是*拟合值* ***ŷ***和*实际值* ***y***之间的差异：

![](img/327097209e99f18cfa4b70641abb412c.png)

方程由作者使用 LaTeX 生成。

重要的是要区分[残差](https://en.wikipedia.org/wiki/Residual_(numerical_analysis))和[误差](https://en.wikipedia.org/wiki/Forecast_error)。误差是*实际*值和*预测*值之间的差异。然而，正如上面所示，残差是*实际*值和*拟合值*之间的差异。这些*拟合值*是模型在拟合训练数据时做出的预测。由于模型知道所有观测值，它不再是技术上的预测，而是*拟合值*。

如果你想了解更多关于预测误差及其指标的信息，请查看我之前的相关文章：

[](/an-overview-of-forecasting-performance-metrics-ef548dad0134?source=post_page-----f980f597332e--------------------------------) ## 预测性能指标概述

### 预测时间序列的基本性能指标概述

towardsdatascience.com

# 残差分析

我们可以使用残差来分析我们的模型捕捉数据特征的效果。一般来说，残差应该：

+   显示非常少或**没有**[*自相关*](https://www.youtube.com/watch?v=4VmfTaoomWk&t=23s)或[*偏自相关*](https://medium.com/towards-data-science/partial-autocorrelation-for-time-series-481a9cfa7526)*。* 如果它们有任何形式的相关性，那么模型**遗漏了一些数据中的信息**。我们可以使用[*Ljung–Box*](https://en.wikipedia.org/wiki/Ljung%E2%80%93Box_test)统计检验和[*相关图*](https://en.wikipedia.org/wiki/Correlogram)来确定残差是否确实相关。

+   残差的均值应该是**零**，否则预测将是**有偏的**。实际上，通过简单地加或减去偏差来调整预测值是非常容易的。

> 作为背景，[零假设](https://en.wikipedia.org/wiki/P-value)在 Ljung–Box 检验中假设残差不相关。因此，我们希望无法拒绝零假设，并且[ p 值](https://en.wikipedia.org/wiki/P-value)大于 5%。

现在让我们在 Python 中进行一些残差分析吧！

# Python 中的残差分析

## 拟合 Holt-Winters 模型

在这次简短的演示中，我们将*指数平滑 Holt-Winters*模型拟合到著名的美国航空乘客数据集。如果你想了解更多关于 Holt-Winters 模型的工作原理，确保阅读我之前关于它的博客：[链接]

[](/time-series-forecasting-with-holt-winters-b78ffc322f24?source=post_page-----f980f597332e--------------------------------) ## 使用 Holt-Winters 模型进行时间序列预测

### 对最强大和最有用的指数平滑模型的讨论和实现

towardsdatascience.com

> 数据 [来自 Kaggle](https://www.kaggle.com/datasets/ashfakyeafi/air-passenger-data-for-time-series-analysis) 的 CC0 许可证。

GitHub Gist 由作者提供。

![](img/eae71268a248260ba3c44bb2a0cdd9c9.png)

图表由作者在 Python 中生成。

该模型的预测效果看起来相当好。让我们通过将*实际值*、*拟合值*和*残差*插入训练数据集中来分析残差：

GitHub Gist 由作者提供。

![](img/ffa01c3dd43e7433917ae7c23289e258.png)

图像由作者在 Python 中生成。

正如我们所看到的，残差确实是*实际值*（*#Passengers*）和*拟合值*之间的差异，如之前所述。

## 残差相关性

残差的相关性可以通过绘制它们的[自相关](https://www.youtube.com/watch?v=GcJ__g_cimA)和[偏自相关](https://www.youtube.com/watch?v=4VmfTaoomWk&t=23s)函数来计算：

作者的 GitHub Gist。

![](img/b265087d12ea5f0e855e1bc55b0c8438.png)

由作者在 LaTeX 中生成的图表。

大多数相关性都在**统计上不显著**的蓝色区域内，这表明残差**不相关**。然而，你可能会注意到相关性中存在一些重复模式。这表明模型可能没有完全考虑某些季节性成分。

如果你想深入了解自相关和偏自相关，请参考我之前关于这些内容的帖子：

[时间序列分析中的自相关](https://towardsdatascience.com/autocorrelation-for-time-series-analysis-86e68e631f77?source=post_page-----f980f597332e--------------------------------) [## 时间序列分析中的自相关

### 解释什么是自相关以及它在时间序列分析中的重要性。

[偏自相关在时间序列分析中的应用](https://towardsdatascience.com/autocorrelation-for-time-series-analysis-86e68e631f77?source=post_page-----f980f597332e--------------------------------) [## 偏自相关在时间序列分析中的应用

### 解释什么是偏自相关及其在时间序列分析中的重要性

[偏自相关在时间序列分析中的应用](https://towardsdatascience.com/partial-autocorrelation-for-time-series-481a9cfa7526?source=post_page-----f980f597332e--------------------------------)

## Ljung-Box 检验

确定残差是否相关的一个更量化的方法是进行[*Ljung–Box*](https://en.wikipedia.org/wiki/Ljung%E2%80%93Box_test)统计检验：

作者的 GitHub Gist。

![](img/e64f4f176b43b0edc758f3b68e4b8bca.png)

由作者在 Python 中生成的图像。

这显示了前**10**个滞后的 p 值。它们都低于[*显著性水平*](https://statisticsbyjim.com/glossary/significance-level/)**0.05**，因此我们**拒绝没有自相关的零假设**。因此，我们的残差中存在**相关性**，在重新拟合模型时需要重新审视。

如果你想了解更多关于统计检验和 p 值的内容，我推荐阅读我之前的相关文章：

[Z 检验简单解释](https://towardsdatascience.com/z-test-simply-explained-80b346e0e239?source=post_page-----f980f597332e--------------------------------) [## Z 检验简单解释

### 对 Z 检验在统计假设检验中的直观解释

[Z 检验简单解释](https://towardsdatascience.com/z-test-simply-explained-80b346e0e239?source=post_page-----f980f597332e--------------------------------)

## 残差的直方图

一个[直方图](https://en.wikipedia.org/wiki/Histogram)将确定残差是否具有**零均值**且**对称**（无偏差）：

作者的 GitHub Gist。

![](img/7ccdc8978274989043c398524913c680.png)

由作者在 Python 中生成的图表。

在这种情况下，残差**主要分布在零附近**，均值为`-0.023`，甚至可能略微负偏差。这表明我们可能不需要为计算的预测提供偏移量。

# 总结与进一步思考

在这篇文章中，我们展示了如何使用模型的残差来诊断时间序列预测模型。判断模型是否适合的两个关键指标是：残差没有或只有很少的相关性，并且其均值接近零。

本博客中使用的完整代码可在我的 GitHub 上找到：

[](https://github.com/egorhowell/Medium-Articles/blob/main/Time%20Series/Time%20Series%20Tools/residual_analysis.py?source=post_page-----f980f597332e--------------------------------) [## Medium-Articles/residual_analysis.py at main · egorhowell/Medium-Articles

### 我在我的中等博客/文章中使用的代码。通过创建一个帐户来贡献 egorhowell/Medium-Articles 的开发…

github.com](https://github.com/egorhowell/Medium-Articles/blob/main/Time%20Series/Time%20Series%20Tools/residual_analysis.py?source=post_page-----f980f597332e--------------------------------)

# 另外一件事！

我有一个免费的新闻通讯，[**Dishing the Data**](https://dishingthedata.substack.com/)，在这里我每周分享成为更好数据科学家的技巧。没有“废话”或“诱饵”，只有来自实践数据科学家的纯粹可操作的见解。

[](https://newsletter.egorhowell.com/?source=post_page-----f980f597332e--------------------------------) [## Dishing The Data | Egor Howell | Substack

### 如何成为更好的数据科学家。点击阅读《Dishing The Data》，由 Egor Howell 编写，是 Substack 出版的内容…

newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----f980f597332e--------------------------------)

# 与我联系！

+   [**YouTube**](https://www.youtube.com/@egorhowell?sub_confirmation=1)

+   [**LinkedIn**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)

# 参考文献与进一步阅读

+   *预测：原理与实践：* [`otexts.com/fpp3/accuracy.html`](https://otexts.com/fpp3/accuracy.html)

+   [`www.statology.org/ljung-box-test-python/`](https://www.statology.org/ljung-box-test-python/)
