# BigQuery 优化策略 3：表格扁平化

> 原文：[`towardsdatascience.com/bigquery-optimization-strategies-3-table-flattening-373d34c08cea?source=collection_archive---------7-----------------------#2023-02-01`](https://towardsdatascience.com/bigquery-optimization-strategies-3-table-flattening-373d34c08cea?source=collection_archive---------7-----------------------#2023-02-01)

## 关于爆炸表和收缩数组

[](https://medium.com/@martin.weitzmann?source=post_page-----373d34c08cea--------------------------------)![Martin Weitzmann](https://medium.com/@martin.weitzmann?source=post_page-----373d34c08cea--------------------------------)[](https://towardsdatascience.com/?source=post_page-----373d34c08cea--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----373d34c08cea--------------------------------) [Martin Weitzmann](https://medium.com/@martin.weitzmann?source=post_page-----373d34c08cea--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F25d277ee60df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbigquery-optimization-strategies-3-table-flattening-373d34c08cea&user=Martin+Weitzmann&userId=25d277ee60df&source=post_page-25d277ee60df----373d34c08cea---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----373d34c08cea--------------------------------) · 9 分钟阅读 · 2023 年 2 月 1 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F373d34c08cea&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbigquery-optimization-strategies-3-table-flattening-373d34c08cea&user=Martin+Weitzmann&userId=25d277ee60df&source=-----373d34c08cea---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F373d34c08cea&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbigquery-optimization-strategies-3-table-flattening-373d34c08cea&source=-----373d34c08cea---------------------bookmark_footer-----------)

数组是你可以想到的分析数据库中最酷的特性之一，因为它可以在事件发生的地点和时间存储额外的信息。让我们先探索一些基本示例，然后深入了解 Google Analytics 4 中的数组。

![](img/076ff3f2da5c8b766f6edf93ed5c02e2.png)

照片由 [Torsten Dederichs](https://unsplash.com/@tdederichs?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

例如，在存储销售历史时，我们可以将购买的产品与购买事件一起存储在数组中，而不是在单独的表中——这样可以避免在后续分析中处理所有 SQL 连接的麻烦。

尽管数组不是特别直观，但我认为 SQL 连接更糟糕（主要是因为人们[使用了错误的思维模型来解释它们](https://medium.com/towards-data-science/explain-sql-joins-the-right-way-f6ea784b568b)）。

在查询优化方面，有一个简单的规则来处理 SQL 中的数组：

+   **如果你可以汇总数组——就这样做！**

让我解释一下——当你需要从数组中获取信息时，你有两个选择：

+   *汇总*数组 / 将其减少为一个值

+   *横向连接*数组到表格 / “扁平化”表格

如果你只需要 *一个值*，那么汇总——如果你确实需要 *多个* *值*，则过滤、过滤再过滤，然后再进行昂贵的横向连接。
