# 3 个可能影响 SQL 查询性能的细微错误

> 原文：[https://towardsdatascience.com/3-subtle-mistakes-that-can-undermine-your-sql-query-performance-47e897688977?source=collection_archive---------9-----------------------#2023-05-11](https://towardsdatascience.com/3-subtle-mistakes-that-can-undermine-your-sql-query-performance-47e897688977?source=collection_archive---------9-----------------------#2023-05-11)

## 以及如何缓解这些问题

[](https://sonery.medium.com/?source=post_page-----47e897688977--------------------------------)[![Soner Yıldırım](../Images/c589572e9d1ee176cd4f5a0008173f1b.png)](https://sonery.medium.com/?source=post_page-----47e897688977--------------------------------)[](https://towardsdatascience.com/?source=post_page-----47e897688977--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----47e897688977--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----47e897688977--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-subtle-mistakes-that-can-undermine-your-sql-query-performance-47e897688977&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----47e897688977---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----47e897688977--------------------------------) ·4 分钟阅读·2023 年 5 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F47e897688977&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-subtle-mistakes-that-can-undermine-your-sql-query-performance-47e897688977&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----47e897688977---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F47e897688977&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-subtle-mistakes-that-can-undermine-your-sql-query-performance-47e897688977&source=-----47e897688977---------------------bookmark_footer-----------)![](../Images/f327d905bfad84f115568823cff75142.png)

图片来源：[Brett Jordan](https://unsplash.com/it/@brett_jordan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/JZgA2mPEY6c?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

SQL 是一个适用于所有处理数据密集型产品的人的通用工具。无论你是数据分析师、数据科学家还是软件开发人员，你可能都需要编写 SQL 查询以从关系型数据库中提取数据。

在编写查询时，我们往往关注于获取所需的数据。查询性能通常是第二个考虑因素。这通常没问题，除非数据规模非常庞大。

此外，数据库管理系统在实际执行查询之前会创建一个执行计划以优化查询，在某些情况下，这可以视为对查询的改进。

然而，我们仍需记住查询性能，并应致力于编写更优和更高效的查询。我们的应用程序目前可能没有查询性能问题，但我们处理的数据可能会突然增加。

在这篇文章中，我们将了解3个微妙的错误，这些错误可能会降低SQL查询性能，特别是在处理大型数据集时。虽然这些错误可能不会导致不正确的结果，实际上可能会产生期望的数据，但在查询效率和可维护性方面仍有相当大的改进空间。
