# 使用 Delta 表处理缓慢变化的维度（SCD）

> 原文：[`towardsdatascience.com/handling-slowly-changing-dimensions-scd-using-delta-tables-511122022e45?source=collection_archive---------3-----------------------#2023-01-23`](https://towardsdatascience.com/handling-slowly-changing-dimensions-scd-using-delta-tables-511122022e45?source=collection_archive---------3-----------------------#2023-01-23)

## 使用 Delta 框架处理缓慢变化的维度挑战

[](https://mkukreja1.medium.com/?source=post_page-----511122022e45--------------------------------)![Manoj Kukreja](https://mkukreja1.medium.com/?source=post_page-----511122022e45--------------------------------)[](https://towardsdatascience.com/?source=post_page-----511122022e45--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----511122022e45--------------------------------) [Manoj Kukreja](https://mkukreja1.medium.com/?source=post_page-----511122022e45--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4076fc16cb6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhandling-slowly-changing-dimensions-scd-using-delta-tables-511122022e45&user=Manoj+Kukreja&userId=4076fc16cb6a&source=post_page-4076fc16cb6a----511122022e45---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----511122022e45--------------------------------) ·10 分钟阅读·2023 年 1 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F511122022e45&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhandling-slowly-changing-dimensions-scd-using-delta-tables-511122022e45&user=Manoj+Kukreja&userId=4076fc16cb6a&source=-----511122022e45---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F511122022e45&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhandling-slowly-changing-dimensions-scd-using-delta-tables-511122022e45&source=-----511122022e45---------------------bookmark_footer-----------)

长期以来，Kimball 方法一直是维度数据建模技术的标准。正如 Kimball 所说，“*时间的概念渗透到数据仓库的每一个角落*”。在数据分析的背景下，这意味着什么？从高层次看，现代分析可以被视为随着时间的推移对不断变化的数据进行汇总。问题在于，不断变化的数据不仅包括新的添加，还包括对以前数据集的更改。

总体维度数据建模将数据分为两个主要类别：

**事实** — 这些数据代表了*无限*的数据集，存储了实体的测量值。它包含了进行定量分析和决策制定所必需的数据。事实表通常有与其他表（维度）连接的列，以供参考。

**维度** — 这些数据代表了相对*有限*的数据集，提供有关在事实表中进行的测量的描述性信息。与事实表相比，维度的演变速度要慢得多。这就是它们常被称为“*缓慢变化的维度*”的原因。

金宝尔（Kimball）的方法涉及基于事实和维度创建星型模式。由于其非规范化的结构，星型模式非常适合分析用例……不……
