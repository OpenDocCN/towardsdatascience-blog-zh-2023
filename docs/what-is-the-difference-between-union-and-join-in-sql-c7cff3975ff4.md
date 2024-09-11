# SQL 中 UNION 和 JOIN 的区别是什么？

> 原文：[https://towardsdatascience.com/what-is-the-difference-between-union-and-join-in-sql-c7cff3975ff4?source=collection_archive---------5-----------------------#2023-05-01](https://towardsdatascience.com/what-is-the-difference-between-union-and-join-in-sql-c7cff3975ff4?source=collection_archive---------5-----------------------#2023-05-01)

## SQL 中 UNION、EXCEPT 和 INTERSECT 的 5 分钟指南

[](https://mikehuls.medium.com/?source=post_page-----c7cff3975ff4--------------------------------)[![Mike Huls](../Images/8f9f55a0d25db00799c5d37383b7f5b6.png)](https://mikehuls.medium.com/?source=post_page-----c7cff3975ff4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c7cff3975ff4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c7cff3975ff4--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----c7cff3975ff4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ffb62c607ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-the-difference-between-union-and-join-in-sql-c7cff3975ff4&user=Mike+Huls&userId=7ffb62c607ee&source=post_page-7ffb62c607ee----c7cff3975ff4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c7cff3975ff4--------------------------------) ·7分钟阅读·2023年5月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc7cff3975ff4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-the-difference-between-union-and-join-in-sql-c7cff3975ff4&user=Mike+Huls&userId=7ffb62c607ee&source=-----c7cff3975ff4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc7cff3975ff4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-the-difference-between-union-and-join-in-sql-c7cff3975ff4&source=-----c7cff3975ff4---------------------bookmark_footer-----------)![](../Images/21ede7b38e5fd0033160d7bb0cdfbaea.png)

我们有大量数据需要处理（图片来源：[Simon Berger](https://unsplash.com/@8moments) 于 [Unsplash](https://unsplash.com/photos/twukN12EN7c)）

在本文中，我们将深入探讨三个经常被忽视的 SQL 运算符：`EXCEPT`、`INTERSECT` 和 `UNION`。我们将：

1.  [通过清晰的示例和视觉效果彻底理解这些概念](#59cd)

1.  [理解运算符的“规则”以真正掌握如何使用它们](#ea42)

1.  [探索一个更复杂的示例](#28fb)

1.  [理解与](#9223) `[JOIN](#9223)` [的区别，并讨论何时使用哪种](#9223)

在本文的结尾，你将掌握一些强大的新工具，让我们开始编码吧！

## 在我们开始之前..

在一些数据库中，如 SQL Server、PostgreSQL 和 SQLite，我们使用 `EXCEPT` 操作符。在其他数据库（例如 MySQL 和 Oracle）中，这个操作符有不同的名称：`MINUS`。`MINUS` 和 `EXCEPT` 的工作方式完全相同。

> TL;DR: `*EXCEPT == MINUS*`

在本文的其余部分，我们将使用 SQL Server 的示例，配合 `EXCEPT`；如果你使用其他数据库，只需在需要的地方将其替换为 `MINUS`。
