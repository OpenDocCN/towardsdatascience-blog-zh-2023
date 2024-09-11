# 理解时间序列趋势

> 原文：[https://towardsdatascience.com/understanding-time-series-trend-addfd9d7764e?source=collection_archive---------2-----------------------#2023-03-14](https://towardsdatascience.com/understanding-time-series-trend-addfd9d7764e?source=collection_archive---------2-----------------------#2023-03-14)

## 确定性趋势与随机趋势，以及如何处理它们

[](https://vcerq.medium.com/?source=post_page-----addfd9d7764e--------------------------------)[![Vitor Cerqueira](../Images/9e52f462c6bc20453d3ea273eb52114b.png)](https://vcerq.medium.com/?source=post_page-----addfd9d7764e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----addfd9d7764e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----addfd9d7764e--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----addfd9d7764e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-time-series-trend-addfd9d7764e&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----addfd9d7764e---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----addfd9d7764e--------------------------------) ·6 min 阅读·2023年3月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faddfd9d7764e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-time-series-trend-addfd9d7764e&user=Vitor+Cerqueira&userId=efb5f27c836d&source=-----addfd9d7764e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faddfd9d7764e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-time-series-trend-addfd9d7764e&source=-----addfd9d7764e---------------------bookmark_footer-----------)![](../Images/4c837fdc4590f7f0194f180a27dce0f4.png)

图片由 [Ali Abdul Rahman](https://unsplash.com/@_actually_?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

检测和处理趋势是时间序列建模的关键步骤。

在这篇文章中，我们将：

+   描述时间序列的趋势及其不同特征；

+   探索如何检测时间序列的趋势；

+   讨论处理趋势的方法；

# 理解趋势

## 趋势作为时间序列的一个构建块

在任何给定的时间点，时间序列可以分解为三个部分：趋势、季节性和剩余部分。

趋势代表时间序列水平的长期变化。这种变化可以是向上（水平上升）或向下（水平下降）。如果变化在一个方向上是系统性的，则该趋势是单调的。

![](../Images/55b9b6390229c5643ae542c9854c116a.png)

美国 GDP 时间序列显示出上升的单调趋势。数据来源见参考文献[1]。图片由作者提供。

## 趋势作为非平稳性的原因

如果时间序列的统计特性不变，则该时间序列是平稳的。这包括时间序列的水平…
