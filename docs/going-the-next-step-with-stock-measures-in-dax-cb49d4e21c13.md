# Going the Next Step with Stock Measures in DAX

> 原文：[`towardsdatascience.com/going-the-next-step-with-stock-measures-in-dax-cb49d4e21c13?source=collection_archive---------13-----------------------#2023-11-28`](https://towardsdatascience.com/going-the-next-step-with-stock-measures-in-dax-cb49d4e21c13?source=collection_archive---------13-----------------------#2023-11-28)

## *在使用 Power BI 中的 Stock Measures 时，我们可能会遇到一些奇怪的效果。让我们看看这种情况是如何出现的以及如何解决它。*

[](https://medium.com/@salvatorecagliari?source=post_page-----cb49d4e21c13--------------------------------)![Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----cb49d4e21c13--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cb49d4e21c13--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cb49d4e21c13--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----cb49d4e21c13--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39cccb39e92a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoing-the-next-step-with-stock-measures-in-dax-cb49d4e21c13&user=Salvatore+Cagliari&userId=39cccb39e92a&source=post_page-39cccb39e92a----cb49d4e21c13---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cb49d4e21c13--------------------------------) ·7 分钟阅读·2023 年 11 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcb49d4e21c13&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoing-the-next-step-with-stock-measures-in-dax-cb49d4e21c13&user=Salvatore+Cagliari&userId=39cccb39e92a&source=-----cb49d4e21c13---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcb49d4e21c13&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoing-the-next-step-with-stock-measures-in-dax-cb49d4e21c13&source=-----cb49d4e21c13---------------------bookmark_footer-----------)![](img/de6275a5ca5ee665dbfaa76b41735898.png)

Photo by [Firmbee.com](https://unsplash.com/@firmbee?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

当数据无法随时间聚合时，通常会使用股票计算。

例如，随着时间的推移累加我的银行账户余额会是一个不好的主意。对我来说是个好主意，但不是对我的银行。

目前，我正在进行一个客户项目，为人力资源数据创建报告解决方案。

一个关键的指标是 Headcount，它也是一个 stock measure，因为我们在 Fact 表中按月份存储了 Headcount 的数据。

这看起来是个简单的任务。

但在某些情况下，我们可能需要多走一步才能获得正确的结果。

现在，让我们深入了解一下。

# 基础 Stock Measure

首先，Stock Measure 的正确名称是 Semi-Additive Measures。这是因为，如上所述，它们不会随时间聚合数据，而是对所有其他维度进行聚合。
