# 时间序列中的平稳性——综合指南

> 原文：[https://towardsdatascience.com/stationarity-in-time-series-a-comprehensive-guide-8beabe20d68?source=collection_archive---------0-----------------------#2023-04-11](https://towardsdatascience.com/stationarity-in-time-series-a-comprehensive-guide-8beabe20d68?source=collection_archive---------0-----------------------#2023-04-11)

## 如何在 Python 中检查时间序列是否平稳以及当它非平稳时可以做什么

[](https://medium.com/@iamleonie?source=post_page-----8beabe20d68--------------------------------)[![Leonie Monigatti](../Images/4044b1685ada53a30160b03dc78f9626.png)](https://medium.com/@iamleonie?source=post_page-----8beabe20d68--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8beabe20d68--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8beabe20d68--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----8beabe20d68--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstationarity-in-time-series-a-comprehensive-guide-8beabe20d68&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----8beabe20d68---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8beabe20d68--------------------------------) ·8分钟阅读·2023年4月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8beabe20d68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstationarity-in-time-series-a-comprehensive-guide-8beabe20d68&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----8beabe20d68---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8beabe20d68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstationarity-in-time-series-a-comprehensive-guide-8beabe20d68&source=-----8beabe20d68---------------------bookmark_footer-----------)![](../Images/55138648f455b73f978af21736d394cf.png)

时间序列中的平稳性（图像由作者绘制）

当未来类似于现在时，它更容易建模 [3]。平稳性描述了时间序列统计特征随时间不变的概念。因此，一些时间序列预测模型，如自回归模型，依赖于时间序列的平稳性。

在这篇文章中，你将学到：

+   [什么是平稳性](#e15a)，

+   [为何这很重要](#afc6)，

+   [检查平稳性的 3 种方法](#48f0)，以及

+   [当时间序列非平稳时可以应用的 3 种技术](#b9bb)

# 什么是平稳性？

稳定性描述了一个时间序列在未来如何变化的概念将保持不变[3]。用数学术语来说，当时间序列的**统计性质与时间无关**时，它就是稳定的[3]：

+   均值恒定，

+   方差恒定，

+   协方差与时间无关。

这就是**弱形式稳定性**的定义。另一种稳定性类型是**严格稳定性**。它意味着样本…
