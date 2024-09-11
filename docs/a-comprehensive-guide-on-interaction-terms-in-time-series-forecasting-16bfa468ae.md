# 时间序列预测中的交互项全面指南

> 原文：[`towardsdatascience.com/a-comprehensive-guide-on-interaction-terms-in-time-series-forecasting-16bfa468ae?source=collection_archive---------10-----------------------#2023-08-01`](https://towardsdatascience.com/a-comprehensive-guide-on-interaction-terms-in-time-series-forecasting-16bfa468ae?source=collection_archive---------10-----------------------#2023-08-01)

![](img/1fadc2980072fe9153e186992a828785.png)

图片由 Midjourney 创建

## 学习如何通过使线性模型更灵活以适应趋势变化，从而改善模型的拟合度

[](https://eryk-lewinson.medium.com/?source=post_page-----16bfa468ae--------------------------------)![Eryk Lewinson](https://eryk-lewinson.medium.com/?source=post_page-----16bfa468ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----16bfa468ae--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----16bfa468ae--------------------------------) [Eryk Lewinson](https://eryk-lewinson.medium.com/?source=post_page-----16bfa468ae--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F44bc27317e6b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-guide-on-interaction-terms-in-time-series-forecasting-16bfa468ae&user=Eryk+Lewinson&userId=44bc27317e6b&source=post_page-44bc27317e6b----16bfa468ae---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----16bfa468ae--------------------------------) ·7 分钟阅读·2023 年 8 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F16bfa468ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-guide-on-interaction-terms-in-time-series-forecasting-16bfa468ae&user=Eryk+Lewinson&userId=44bc27317e6b&source=-----16bfa468ae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F16bfa468ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-guide-on-interaction-terms-in-time-series-forecasting-16bfa468ae&source=-----16bfa468ae---------------------bookmark_footer-----------)

建模时间序列数据可能具有挑战性（同时也令人着迷），因为时间序列数据本身复杂且不可预测。例如，时间序列中的长期趋势可能会因某些事件而发生剧烈变化。回想一下全球疫情初期，像航空公司或实体店这样的企业迅速出现了客户和销售的急剧下降。相比之下，电子商务企业则继续正常运营，受到的干扰较少。

交互项有助于这种模式的建模。它们捕捉变量之间的复杂关系，从而导致更准确的预测。

本文探讨：

+   在时间序列预测中的交互项

+   在建模复杂关系时，交互项的好处

+   如何在你的模型中有效地实现交互项

# 交互项概述

交互项使你能够探究目标与特征之间的关系是否会根据…的值发生变化。
