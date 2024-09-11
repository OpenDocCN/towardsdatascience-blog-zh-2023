# 数据分析中你需要知道的三种经验证的 SQL 最佳实践

> 原文：[https://towardsdatascience.com/3-proven-sql-best-practices-you-need-to-know-in-data-analysis-e778a0577060?source=collection_archive---------0-----------------------#2023-05-16](https://towardsdatascience.com/3-proven-sql-best-practices-you-need-to-know-in-data-analysis-e778a0577060?source=collection_archive---------0-----------------------#2023-05-16)

## 数据分析

## 学习三种最佳方法，以编写易于阅读、调试和修改的 SQL 查询

[](https://medium.com/@17.rsuraj?source=post_page-----e778a0577060--------------------------------)[![Suraj Gurav](../Images/f5dca32861f8c1c428e66fbe2174c04b.png)](https://medium.com/@17.rsuraj?source=post_page-----e778a0577060--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e778a0577060--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e778a0577060--------------------------------) [Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----e778a0577060--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fdda183cca2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-proven-sql-best-practices-you-need-to-know-in-data-analysis-e778a0577060&user=Suraj+Gurav&userId=1fdda183cca2&source=post_page-1fdda183cca2----e778a0577060---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e778a0577060--------------------------------) ·5 min read·2023年5月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe778a0577060&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-proven-sql-best-practices-you-need-to-know-in-data-analysis-e778a0577060&user=Suraj+Gurav&userId=1fdda183cca2&source=-----e778a0577060---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe778a0577060&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-proven-sql-best-practices-you-need-to-know-in-data-analysis-e778a0577060&source=-----e778a0577060---------------------bookmark_footer-----------)![](../Images/d46797d817049ddf474c63fbabf401c9.png)

照片由 [Kobby Mendez](https://unsplash.com/@kobbymendez?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

编写 SQL 查询是数据分析中的重要步骤。即使是基本知识的人也可以编写 SQL 查询。

那么，是什么让你脱颖而出？

编写易于理解和维护的 SQL 查询可以让你脱颖而出。写出计算机能够理解的查询非常简单，但并非总是如此。

无论你是否与其他 SQL 开发人员或用户合作，你的代码必须易于阅读、调试和修改。这不是一项困难的任务，只需遵循编写 SQL 查询的最佳方法。

使用这三种简单而有效的 SQL 最佳实践将大幅提高你的 SQL 查询质量。

无论是 RDBMS，如 SQL Server、MySQL Workbench 还是 DB Browser，这些都是编写可维护查询的三种最佳方法。

我不是告诉你传统的最佳实践，那些你在 Google 上到处看到的。但我已经测试过这些编写 SQL 查询的方法，我的团队也采纳了它。所以，它对你也可能有帮助。
