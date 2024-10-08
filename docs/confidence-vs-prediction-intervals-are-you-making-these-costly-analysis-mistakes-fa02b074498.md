# 置信区间与预测区间

> 原文：[`towardsdatascience.com/confidence-vs-prediction-intervals-are-you-making-these-costly-analysis-mistakes-fa02b074498`](https://towardsdatascience.com/confidence-vs-prediction-intervals-are-you-making-these-costly-analysis-mistakes-fa02b074498)

## 探索置信区间与预测区间之间的关键区别

[](https://medium.com/@egorhowell?source=post_page-----fa02b074498--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----fa02b074498--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fa02b074498--------------------------------)![数据科学探索](https://towardsdatascience.com/?source=post_page-----fa02b074498--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----fa02b074498--------------------------------)

·发布于 [数据科学探索](https://towardsdatascience.com/?source=post_page-----fa02b074498--------------------------------) ·阅读时间 6 分钟·2023 年 6 月 4 日

--

![](img/0431f8f5d270f3410c3ddff11082e605.png)

图片由 [Jarosław Kwoczała](https://unsplash.com/@sumekler?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

“[***置信区间***](https://medium.com/towards-data-science/confidence-intervals-simply-explained-58b0b11e985f)”和“[***预测区间***](https://en.wikipedia.org/wiki/Prediction_interval)”这两个术语在数据科学会议中经常被混用。我必须承认，有时候我也这样做过，只是为了显得聪明。

然而，这很危险。置信区间和预测区间指的是非常不同的概念，你可能会被那些了解区别的人抓住。这将是非常尴尬的。

但不要绝望！在这篇文章中，我将直观地解释这两种区间之间的区别，让你自信地运用它们。

# 置信区间

## 概述

更为人熟知的是置信区间，所以我们从这里开始。置信区间是对一些采样参数（例如样本均值或回归模型中的一个系数）不确定性的度量。它帮助我们理解估计值与真实总体值的接近程度。如果你有兴趣了解更多关于置信区间的内容，可以查看我之前关于这个主题的博客文章：

[](/confidence-intervals-simply-explained-58b0b11e985f?source=post_page-----fa02b074498--------------------------------) ## 置信区间简单解释

### 置信区间的简要解释。

towardsdatascience.com

## 理论

考虑 [**线性回归**](https://en.wikipedia.org/wiki/Linear_regression) 方程的两种形式：

![](img/abd2e8a1de7e995842dde5f5c8f5bde7.png)

公式由作者提供，使用 LaTeX 排版。

+   **y**: *响应*

+   **x**: *协变量或特征*

+   **β₀:** *斜率系数*

+   **β₁:** *截距*

+   **ε:** *高斯随机噪声*

+   **E[y | x]**: *给定 x 的 y 的均值响应*

线性回归的目标是根据一些数据确定未知的系数 **β₀** 和 **β₁**，这些数据由变量 **x** 和相应的输出或响应 **y** 表示。然而，在实际应用中，我们通常只能访问整个总体的随机样本。因此，**β₀** 和 **β₁** 的值会存在不确定性，因为它们是从这个随机样本中估计的，而非总体。这种不确定性是回归模型中的“误差”来源。

在置信区间的情况下，它表示与估计的均值响应 **E[y | x]** 相关的不确定性水平。假设我们有一个比较工资和年龄的数据集，发现 23 岁时的平均工资为 28,000 英镑，E[salary | 23] = 28,000 英镑。如果我们计算 95%的置信区间，并得到 26,000 到 30,000 英镑的范围，这表明我们可以以 95%的概率预期，所有 23 岁人群的真实平均工资会落在此区间内。虽然这个解释可能无法涵盖所有细节，但它提供了一个总体的直观理解。

> 查看 [这里](https://robjhyndman.com/hyndsight/intervals/) 以获取置信区间及其含义的严格定义。

一句话总结：

> 置信区间 = 均值响应中的不确定性

## 公式

回归中置信区间的公式相当复杂，但幸运的是大多数统计软件可以为我们计算！然而，为了完整性，让我们陈述公式并突出一些关键特征：

![](img/69df99eb4de6b67338c4bf56364b258e.png)

公式由作者提供，使用 LaTeX 排版。

![](img/7ba99feb36ec2c3e00ed7406b1fd60ff.png)

公式由作者提供，使用 LaTeX 排版。

其中：

+   **x**: *特征或协变量值*

+   **ŷ:** *x 的预测值*

+   ***t_(1-α/2, n-2)** = ***T 分布****置信水平为 1-α**的临界值，*自由度为 n-2* [***自由度***](https://en.wikipedia.org/wiki/Degrees_of_freedom_(statistics))*

+   ***n**: *样本中的数据点数量**

+   ***y**: *真实/实际响应**

+   ***MSE**: [*均方误差*](https://en.wikipedia.org/wiki/Mean_squared_error)*

*再次提醒，不要过于纠结于这些数学细节——我当然没有！目的是提供对公式来源的一些见解。正如你所见，它主要是统一了数据的分布和预测中的误差。如果你对全面的推导感兴趣，可以查看这个链接[这里](https://online.stat.psu.edu/stat415/lesson/7/7.5)。*

# *预测区间*

## *概述*

*置信区间衡量了估计均值响应的不确定性，那么预测区间又是做什么的呢？预测区间衡量了新样本值可能落在何处的不确定性。它通过考虑当前样本中的所有观测响应来实现这一点。它考虑了样本中的变异性和估计回归模型的不确定性。*

## *理论*

*记住我们上面的简单线性回归方程：*

*![](img/abd2e8a1de7e995842dde5f5c8f5bde7.png)*

*方程由作者在 LaTeX 中编写。*

*置信区间表达了**E[y | x]**的采样不确定性，而预测区间表达了**y**范围内的不确定性。由于它考虑了整个范围而不仅仅是均值，预测区间自然比置信区间要大。*

*置信区间表示与估计**E[y | x]**（**y**在**x**上的期望值）相关的采样不确定性。而预测区间则捕捉了**y**的整个可能值范围中的不确定性。由于预测区间考虑了整个范围而不仅仅是均值，它自然比置信区间要宽。*

## *公式*

*预测区间的公式与置信区间的公式密切相关：*

*![](img/18d4fd6b26b51c34417a401842af586a.png)*

*但是，你是否观察到一个关键的区别？没错，还有一个涉及 MSE 的额外项。通过引入这个额外的正项，我们可以从数学上看出，预测区间将大于置信区间。*

# *示例图*

*为了更好地理解置信区间和预测区间，让我们通过一个可视化的例子来演示它们。以下是一个生成显示薪资与经验关系的图的代码片段。该图包括数据点、回归线以及 95%的置信区间和预测区间：*

> *数据来自[Kaggle](https://www.kaggle.com/datasets/abhishek14398/salary-dataset-simple-linear-regression)，使用 CC0 许可证。*

*作者在 GitHub Gist 上提供。*

*![](img/1ec88e4d34ae09a5b36708e1eceedaaa.png)*

*图由作者在 Python 中生成。*

*正如我们所见，预测区间远比置信区间宽。*

# *总结与进一步思考*

*在本文中，我们探讨了置信区间和预测区间之间的差异。要总结关键点，请参见下表：*

*![](img/4efda2d6fcbf679acd975cd38a6d08a4.png)*

*表由作者制作。*

*现在你明白了区别，你可以纠正那些错误地将这两个术语互换使用的人！*

*本帖中使用的完整代码可以在我的 GitHub 上找到：*

*[](https://github.com/egorhowell/Medium-Articles/blob/main/Statistics/General/confidence_vs_prediction_intervals.py?source=post_page-----fa02b074498--------------------------------) [## Medium-Articles/confidence_vs_prediction_intervals.py at main · egorhowell/Medium-Articles

### 我在 Medium 博客/文章中使用的代码。通过创建账户参与 egorhowell/Medium-Articles 的开发…

github.com](https://github.com/egorhowell/Medium-Articles/blob/main/Statistics/General/confidence_vs_prediction_intervals.py?source=post_page-----fa02b074498--------------------------------)

# 另一个话题！

我有一个免费的新闻通讯，[**Dishing the Data**](https://dishingthedata.substack.com/)，在其中我每周分享成为更好数据科学家的小贴士。没有“虚 fluff”或“点击诱饵 clickbait”，只有来自实际数据科学家的纯粹可操作见解。

[](https://newsletter.egorhowell.com/?source=post_page-----fa02b074498--------------------------------) [## Dishing The Data | Egor Howell | Substack

### 如何成为更好的数据科学家。点击阅读《Dishing The Data》，由 Egor Howell 撰写，是一个 Substack 出版物…

newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----fa02b074498--------------------------------)

# 与我联系！

+   [**YouTube**](https://www.youtube.com/@egorhowell)

+   [**LinkedIn**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)

# 参考资料与进一步阅读

+   关于这两个区间的更深入讨论：[`online.stat.psu.edu/stat501/lesson/3/3.3`](https://online.stat.psu.edu/stat501/lesson/3/3.3)

+   更多讨论和对容忍区间的相关性：[`blog.minitab.com/en/adventures-in-statistics-2/when-should-i-use-confidence-intervals-prediction-intervals-and-tolerance-intervals`](https://blog.minitab.com/en/adventures-in-statistics-2/when-should-i-use-confidence-intervals-prediction-intervals-and-tolerance-intervals)

+   代码教程的灵感来源：[`lmc2179.github.io/posts/confidence_prediction.html`](https://lmc2179.github.io/posts/confidence_prediction.html)*
