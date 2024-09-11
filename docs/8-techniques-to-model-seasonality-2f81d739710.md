# 8 种季节性建模技巧

> 原文：[`towardsdatascience.com/8-techniques-to-model-seasonality-2f81d739710?source=collection_archive---------1-----------------------#2023-07-14`](https://towardsdatascience.com/8-techniques-to-model-seasonality-2f81d739710?source=collection_archive---------1-----------------------#2023-07-14)

## 如何处理季节性预测

[](https://vcerq.medium.com/?source=post_page-----2f81d739710--------------------------------)![Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----2f81d739710--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2f81d739710--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2f81d739710--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----2f81d739710--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F8-techniques-to-model-seasonality-2f81d739710&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----2f81d739710---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2f81d739710--------------------------------) · 8 分钟阅读 · 2023 年 7 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2f81d739710&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F8-techniques-to-model-seasonality-2f81d739710&user=Vitor+Cerqueira&userId=efb5f27c836d&source=-----2f81d739710---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2f81d739710&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F8-techniques-to-model-seasonality-2f81d739710&source=-----2f81d739710---------------------bookmark_footer-----------)![](img/f4f0a31997052cf1d398d56f6c0843f8.png)

图片由[Clark Young](https://unsplash.com/@cbyoung?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文是对[上一篇文章](https://medium.com/towards-data-science/3-types-of-seasonality-and-how-to-detect-them-4e03f548d167)的跟进。在那篇文章中，我们识别了三种季节性模式。

在这里，我们将：

+   学习如何描述时间序列的季节性。

+   探讨你可以用来建模季节性的 8 种方法。

# 建模季节性模式

季节性指的是在某些时间段内重复出现的模式。这是一个重要的变异来源，需要建模。

![](img/6a4af74cbdac7dfb5c56d993334d96ea.png)

一个时间序列及其季节性调整版本。数据来源见下一节。图像由作者提供。

处理季节性的方法有几种。一些方法在建模之前移除季节性成分。季节性调整的数据（一个时间序列减去季节性成分）[突出长期效果如趋势或商业周期](https://otexts.com/fpp2/components.html)。其他方法则添加额外的变量来捕捉季节性的周期性特征。

在探讨不同的方法之前，让我们创建一个时间序列并描述其季节性模式。

## 分析示例
