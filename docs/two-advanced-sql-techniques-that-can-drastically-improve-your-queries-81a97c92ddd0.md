# 两个可以大幅提升查询效率的高级 SQL 技巧

> 原文：[https://towardsdatascience.com/two-advanced-sql-techniques-that-can-drastically-improve-your-queries-81a97c92ddd0?source=collection_archive---------0-----------------------#2023-06-30](https://towardsdatascience.com/two-advanced-sql-techniques-that-can-drastically-improve-your-queries-81a97c92ddd0?source=collection_archive---------0-----------------------#2023-06-30)

## 了解公共表表达式（CTE）和窗口函数

[](https://chongjason.medium.com/?source=post_page-----81a97c92ddd0--------------------------------)[![Jason Chong](../Images/af940189f50be491653b6469eef6fb46.png)](https://chongjason.medium.com/?source=post_page-----81a97c92ddd0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----81a97c92ddd0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----81a97c92ddd0--------------------------------) [Jason Chong](https://chongjason.medium.com/?source=post_page-----81a97c92ddd0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdc66e2ca621a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-advanced-sql-techniques-that-can-drastically-improve-your-queries-81a97c92ddd0&user=Jason+Chong&userId=dc66e2ca621a&source=post_page-dc66e2ca621a----81a97c92ddd0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----81a97c92ddd0--------------------------------) · 11分钟阅读·2023年6月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F81a97c92ddd0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-advanced-sql-techniques-that-can-drastically-improve-your-queries-81a97c92ddd0&user=Jason+Chong&userId=dc66e2ca621a&source=-----81a97c92ddd0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F81a97c92ddd0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-advanced-sql-techniques-that-can-drastically-improve-your-queries-81a97c92ddd0&source=-----81a97c92ddd0---------------------bookmark_footer-----------)![](../Images/b1f706d22404a4cbcab78d47197c01d4.png)

照片由 [Karina Szczurek](https://unsplash.com/@karinaszczurek?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

SQL 是每个数据专业人士的基本技能。无论你是数据分析师、数据科学家还是数据工程师，你都需要对如何编写干净高效的 SQL 查询有扎实的理解。

这是因为在任何严谨的数据分析或复杂的机器学习模型背后，都是底层的数据，而这些数据必须来自某个地方。

希望在阅读我的介绍性[博客文章](https://medium.com/towards-data-science/10-most-important-sql-commands-every-data-analyst-needs-to-know-f0f568914b98)之后，你已经了解到 SQL 代表结构化查询语言，它是一种用于从关系数据库中检索数据的语言。

在那篇博客文章中，我们讲解了一些基本的 SQL 命令，如 `SELECT`、`FROM` 和 `WHERE`，这些命令应该涵盖了你在使用 SQL 时遇到的大多数基本查询。

但是，如果这些简单的命令不够用会发生什么呢？如果你想要的数据需要一种更强大的查询方法怎么办？

好吧，不用再找了，因为今天我们将探讨两种新的 SQL 技术，你可以将它们添加到你的工具箱中，这些技术将使你的查询达到一个新的水平。这些技术称为 Common…
