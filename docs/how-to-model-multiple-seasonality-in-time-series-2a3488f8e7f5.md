# 如何对时间序列建模多重季节性

> 原文：[`towardsdatascience.com/how-to-model-multiple-seasonality-in-time-series-2a3488f8e7f5?source=collection_archive---------14-----------------------#2023-07-25`](https://towardsdatascience.com/how-to-model-multiple-seasonality-in-time-series-2a3488f8e7f5?source=collection_archive---------14-----------------------#2023-07-25)

## **处理多个周期的季节性效应**

[](https://vcerq.medium.com/?source=post_page-----2a3488f8e7f5--------------------------------)![Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----2a3488f8e7f5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2a3488f8e7f5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2a3488f8e7f5--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----2a3488f8e7f5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-model-multiple-seasonality-in-time-series-2a3488f8e7f5&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----2a3488f8e7f5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2a3488f8e7f5--------------------------------) · 5 分钟阅读 · 2023 年 7 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2a3488f8e7f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-model-multiple-seasonality-in-time-series-2a3488f8e7f5&user=Vitor+Cerqueira&userId=efb5f27c836d&source=-----2a3488f8e7f5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2a3488f8e7f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-model-multiple-seasonality-in-time-series-2a3488f8e7f5&source=-----2a3488f8e7f5---------------------bookmark_footer-----------)![](img/b0ff9a93e1a11fae7f8848871a893fcf.png)

图片由 [Joshua Woroniecki](https://unsplash.com/@joshua_j_woroniecki?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，你将学习如何对时间序列建模多重季节性。我们将涵盖：

+   如何使用 MSTL 分解时间序列

+   创建捕捉复杂季节性的解释变量

+   使用现成的方法，以*orbit*的预测包为例。

# 复杂季节性

[季节性指的是以规律的周期性重复的系统变化](https://medium.com/towards-data-science/8-techniques-to-model-seasonality-2f81d739710)。这些模式与观察时间序列的频率有关。低频时间序列通常包含一个单一的季节性周期。例如，每月时间序列表现出每年的季节性。

越来越多的时间序列在更高的采样频率下被收集，例如每日或每小时。这导致数据集变得更大，并且季节性更复杂。每日时间序列可能会显示每周、每月和每年的重复模式。

这是一个具有每日和每周季节性的小时时间序列示例：
