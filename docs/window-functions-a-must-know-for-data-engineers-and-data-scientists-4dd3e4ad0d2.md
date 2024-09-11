# 窗口函数：数据工程师和数据科学家必知的知识

> 原文：[`towardsdatascience.com/window-functions-a-must-know-for-data-engineers-and-data-scientists-4dd3e4ad0d2?source=collection_archive---------1-----------------------#2023-06-14`](https://towardsdatascience.com/window-functions-a-must-know-for-data-engineers-and-data-scientists-4dd3e4ad0d2?source=collection_archive---------1-----------------------#2023-06-14)

## 回到基础 | 揭秘 SQL 窗口函数

[](https://iffatm.medium.com/?source=post_page-----4dd3e4ad0d2--------------------------------)![Iffat Malik](https://iffatm.medium.com/?source=post_page-----4dd3e4ad0d2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4dd3e4ad0d2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4dd3e4ad0d2--------------------------------) [Iffat Malik](https://iffatm.medium.com/?source=post_page-----4dd3e4ad0d2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F88491120e677&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwindow-functions-a-must-know-for-data-engineers-and-data-scientists-4dd3e4ad0d2&user=Iffat+Malik&userId=88491120e677&source=post_page-88491120e677----4dd3e4ad0d2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4dd3e4ad0d2--------------------------------) ·13 分钟阅读·2023 年 6 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4dd3e4ad0d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwindow-functions-a-must-know-for-data-engineers-and-data-scientists-4dd3e4ad0d2&user=Iffat+Malik&userId=88491120e677&source=-----4dd3e4ad0d2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4dd3e4ad0d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwindow-functions-a-must-know-for-data-engineers-and-data-scientists-4dd3e4ad0d2&source=-----4dd3e4ad0d2---------------------bookmark_footer-----------)![](img/14add0fff137d8af56d3fda795403145.png)

数据增长在过去几年中非常迅速，尽管我们拥有各种工具和技术，但 SQL 仍然是大多数工具的核心。它是数据分析的基本语言之一，被各规模公司广泛用于解决与数据相关的挑战。

我一直相信，你不需要仅仅为了通过面试或解决特定问题而去了解某些概念。如果你愿意去学习这些概念及其底层架构，它将帮助你在任何工作中取得成功。窗口函数有些复杂，刚开始可能会感到有些不安，但一旦掌握了它们，它们会是非常有趣的。

如果你对 SQL 聚合函数有一定了解，那么理解窗口函数的概念会更容易。聚合函数对一组值进行计算并返回一个值；当与*GROUP BY*子句配合使用时，它为每个组返回一个单一的值。你可以在这里了解更多信息：
