# 3 种季节性及其检测方法

> 原文：[`towardsdatascience.com/3-types-of-seasonality-and-how-to-detect-them-4e03f548d167?source=collection_archive---------2-----------------------#2023-06-30`](https://towardsdatascience.com/3-types-of-seasonality-and-how-to-detect-them-4e03f548d167?source=collection_archive---------2-----------------------#2023-06-30)

## 理解时间序列的季节性

[](https://vcerq.medium.com/?source=post_page-----4e03f548d167--------------------------------)![Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----4e03f548d167--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4e03f548d167--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4e03f548d167--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----4e03f548d167--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-types-of-seasonality-and-how-to-detect-them-4e03f548d167&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----4e03f548d167---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----4e03f548d167--------------------------------) ·6 分钟阅读·2023 年 6 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4e03f548d167&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-types-of-seasonality-and-how-to-detect-them-4e03f548d167&user=Vitor+Cerqueira&userId=efb5f27c836d&source=-----4e03f548d167---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4e03f548d167&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-types-of-seasonality-and-how-to-detect-them-4e03f548d167&source=-----4e03f548d167---------------------bookmark_footer-----------)![](img/3ad81f63db00083e99537b91371779a3.png)

照片来源：[insung yoon](https://unsplash.com/ja/@insungyoon?utm_source=medium&utm_medium=referral)于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

分析和处理季节性是时间序列分析中的一个关键练习。

在这篇文章中，我们将描述三种季节性及其检测方法。

# 什么是季节性？

季节性是构成时间序列的关键组件之一。季节性指的是在给定周期内以相似强度重复的系统性波动。

季节性变化可能由各种因素引起，如天气、日历或经济条件。在各种应用中都有很多例子。由于假期和旅游，夏季航班更贵。另一个例子是消费者支出，在 12 月因节假日而增加。

季节性意味着某些时期的平均值与其他时间的平均值不同。这个问题[导致序列变得非平稳](https://medium.com/towards-data-science/understanding-time-series-trend-addfd9d7764e)。因此，在建立模型时分析季节性非常重要。

# 三种季节性

时间序列中可能出现三种季节性模式。季节性可以是确定性的，也可以是随机性的。在随机性方面，季节性模式可以是平稳的或…
