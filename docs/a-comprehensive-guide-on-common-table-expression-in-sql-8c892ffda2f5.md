# 《SQL中的通用表表达式全面指南》

> 原文：[https://towardsdatascience.com/a-comprehensive-guide-on-common-table-expression-in-sql-8c892ffda2f5?source=collection_archive---------5-----------------------#2023-08-22](https://towardsdatascience.com/a-comprehensive-guide-on-common-table-expression-in-sql-8c892ffda2f5?source=collection_archive---------5-----------------------#2023-08-22)

## 回归基础 | 简化复杂查询并提升可读性

[](https://iffatm.medium.com/?source=post_page-----8c892ffda2f5--------------------------------)[![Iffat Malik](../Images/7be3b651053507de2077b3c3c9d3a408.png)](https://iffatm.medium.com/?source=post_page-----8c892ffda2f5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8c892ffda2f5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8c892ffda2f5--------------------------------) [Iffat Malik](https://iffatm.medium.com/?source=post_page-----8c892ffda2f5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F88491120e677&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-guide-on-common-table-expression-in-sql-8c892ffda2f5&user=Iffat+Malik&userId=88491120e677&source=post_page-88491120e677----8c892ffda2f5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8c892ffda2f5--------------------------------) · 14 分钟阅读 · 2023年8月22日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8c892ffda2f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-guide-on-common-table-expression-in-sql-8c892ffda2f5&user=Iffat+Malik&userId=88491120e677&source=-----8c892ffda2f5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8c892ffda2f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-guide-on-common-table-expression-in-sql-8c892ffda2f5&source=-----8c892ffda2f5---------------------bookmark_footer-----------)![](../Images/b150ba7dccc63bf4a565984c86a8b0aa.png)

作者提供的图片

在编程中，将指令或语句分组到较小、更易管理的代码块中是一种常见做法。这种做法通常被称为[*代码块组织*](https://en.wikipedia.org/wiki/Block_(programming))。这基本上是将程序或程序的大部分拆分成较小且逻辑上相关的块。这些块旨在执行特定任务或仅仅是将相关功能分组在一起。这种方法不仅提高了代码的可读性，还使代码更加有序和易于维护。各种编程结构，如函数、方法、try-catch 块、循环和条件语句，通常用于此目的。

在*SQL* 中，实现相同功能的一种方法是使用*公共表表达式 (CTE)*。在本文中，我们将探讨*CTEs*如何显著简化和优化复杂的 SQL 查询。

## 什么是 CTE？

> CTE，即公共表表达式，是一种查询，它**暂时**存储结果集，以便在另一个查询中引用和使用。只要它在**相同的执行范围内**，CTE 就会保持可用。

简单来说，*CTE* 就像一个临时表，用于保存中间结果…
