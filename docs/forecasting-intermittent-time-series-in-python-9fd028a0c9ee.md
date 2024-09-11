# 在 Python 中预测间歇性时间序列

> 原文：[`towardsdatascience.com/forecasting-intermittent-time-series-in-python-9fd028a0c9ee?source=collection_archive---------2-----------------------#2023-08-07`](https://towardsdatascience.com/forecasting-intermittent-time-series-in-python-9fd028a0c9ee?source=collection_archive---------2-----------------------#2023-08-07)

## 完整的 Python 中间歇性时间序列预测指南，包括一个综合项目

[](https://medium.com/@marcopeixeiro?source=post_page-----9fd028a0c9ee--------------------------------)![Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----9fd028a0c9ee--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9fd028a0c9ee--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9fd028a0c9ee--------------------------------) [Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----9fd028a0c9ee--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F741c1c8fcfbd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforecasting-intermittent-time-series-in-python-9fd028a0c9ee&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=post_page-741c1c8fcfbd----9fd028a0c9ee---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9fd028a0c9ee--------------------------------) · 15 分钟阅读 · 2023 年 8 月 7 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9fd028a0c9ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforecasting-intermittent-time-series-in-python-9fd028a0c9ee&source=-----9fd028a0c9ee---------------------bookmark_footer-----------)![](img/b059b7a1862602f8507d5feeee2ed7b8.png)

照片由 [Wexor Tmg](https://unsplash.com/@wexor?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

间歇性时间序列，或稀疏时间序列，是一种特殊情况，其中非零值在时间上偶尔出现，而其余的值为 0。

一个常见的稀疏时间序列示例是降雨量。可能会有连续很多天没有降雨，而下雨时，降水量会有所不同。

另一个现实生活中的间歇性序列示例是需求低速动或高价值物品，如航空航天或重型机械的备件。

一些时间序列的间歇性特征在预测中确实构成了挑战，因为传统模型对间歇性处理不佳。因此，我们必须转向适用于稀疏时间序列的替代预测方法。

在这篇文章中，我们探讨了预测间歇性时间序列的不同方法。像往常一样，我们首先理论性地探讨每种模型，然后在 Python 中实现它们。

一如既往，完整的源代码可在 [GitHub](https://github.com/marcopeix/intermittent_time_series) 上找到。

> **通过我的** [**免费时间序列备忘单**](https://www.datasciencewithmarco.com/pl/2147608294) **在 Python 中学习最新的时间序列分析技术！获取**…
