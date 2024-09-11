# 在 Power BI 中将实际数据与预测数据合并为一条连续的线

> 原文：[`towardsdatascience.com/combining-actuals-and-forecasts-in-one-continuous-line-in-power-bi-dc5fd3a66c6f?source=collection_archive---------2-----------------------#2023-08-12`](https://towardsdatascience.com/combining-actuals-and-forecasts-in-one-continuous-line-in-power-bi-dc5fd3a66c6f?source=collection_archive---------2-----------------------#2023-08-12)

## *在许多业务中，我们有实际销售数据和预测数据。我们可以将这些数据添加到一条折线图中，从而看到两条线。但我的一个客户问我是否可以将实际数据绘制为选定月份之前的连续线，而之后的月份则用预测数据。这里是我如何做到的。*

[](https://medium.com/@salvatorecagliari?source=post_page-----dc5fd3a66c6f--------------------------------)![Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----dc5fd3a66c6f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dc5fd3a66c6f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dc5fd3a66c6f--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----dc5fd3a66c6f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39cccb39e92a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombining-actuals-and-forecasts-in-one-continuous-line-in-power-bi-dc5fd3a66c6f&user=Salvatore+Cagliari&userId=39cccb39e92a&source=post_page-39cccb39e92a----dc5fd3a66c6f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dc5fd3a66c6f--------------------------------) ·11 分钟阅读·2023 年 8 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdc5fd3a66c6f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombining-actuals-and-forecasts-in-one-continuous-line-in-power-bi-dc5fd3a66c6f&user=Salvatore+Cagliari&userId=39cccb39e92a&source=-----dc5fd3a66c6f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdc5fd3a66c6f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombining-actuals-and-forecasts-in-one-continuous-line-in-power-bi-dc5fd3a66c6f&source=-----dc5fd3a66c6f---------------------bookmark_footer-----------)![](img/bdf5c662a3ba1059b9c687aa83483651.png)

照片由 [Carlos Muza](https://unsplash.com/@kmuza?utm_source=medium&utm_medium=referral) 供稿，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

为了设置背景，让我们来看下面的图片：

![](img/d1021777052b3a9bd671e0dfdf451e48.png)

图 1 — 实际数据与预测数据的折线图（图由作者提供）

对于这张图表，我只是输入了一些数字到 Excel 中，并从这些数字创建了一个折线图。

你可以看到实际销售和预测销售的曲线在逐渐偏离，这也是预期之中的。

虽然这种做法在大多数情况下是可以的，但我的客户希望对他的数据有不同的视角：
