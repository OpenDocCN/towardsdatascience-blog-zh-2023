# 在 SQL 中使用 HAVING 和 DISTINCT 子句

> 原文：[https://towardsdatascience.com/using-the-having-and-distinct-clauses-in-sql-d9e3be67b4be?source=collection_archive---------13-----------------------#2023-01-25](https://towardsdatascience.com/using-the-having-and-distinct-clauses-in-sql-d9e3be67b4be?source=collection_archive---------13-----------------------#2023-01-25)

## 两个你应该知道的重要 SQL 子句

[](https://mgcodesandstats.medium.com/?source=post_page-----d9e3be67b4be--------------------------------)[![Michael Grogan](../Images/af9ce19e2f61efb07664124e664c7e81.png)](https://mgcodesandstats.medium.com/?source=post_page-----d9e3be67b4be--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d9e3be67b4be--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d9e3be67b4be--------------------------------) [Michael Grogan](https://mgcodesandstats.medium.com/?source=post_page-----d9e3be67b4be--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Feec017a8b178&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-the-having-and-distinct-clauses-in-sql-d9e3be67b4be&user=Michael+Grogan&userId=eec017a8b178&source=post_page-eec017a8b178----d9e3be67b4be---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d9e3be67b4be--------------------------------) ·4分钟阅读·2023年1月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd9e3be67b4be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-the-having-and-distinct-clauses-in-sql-d9e3be67b4be&user=Michael+Grogan&userId=eec017a8b178&source=-----d9e3be67b4be---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd9e3be67b4be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-the-having-and-distinct-clauses-in-sql-d9e3be67b4be&source=-----d9e3be67b4be---------------------bookmark_footer-----------)![](../Images/04e7bbeda036a77a666723d5d07fbc91.png)

来源：照片由 [geralt](https://pixabay.com/users/geralt-9301/) 提供，来自 [Pixabay](https://pixabay.com/photos/binary-binary-system-data-2728117/)

SQL 是一个强大的工具，用于从数据库中提取数据——无论是从一个还是多个表中。

尽管如此，在有效分析数据时，有一些子句尤其重要。

本文讨论的两个子句是**HAVING**和**DISTINCT**子句。

## 为什么我们需要 HAVING 子句？

HAVING 子句的目的是在使用 GROUP BY 函数时充当 WHERE 子句的等效物。

如果你经常使用 SQL，你会知道 GROUP BY 子句在表格中聚合值时是一个非常重要的功能，例如获取某个数据组的平均值、计算某个数据组的最大值或最小值——以及众多其他功能。

假设数据库中存在以下表格：

![](../Images/fb97e22db18d280609903930b65e0400.png)

来源：表格由作者使用 PostgreSQL 创建。表格显示在 pgAdmin4 中。

我们可以看到该表格包含：

+   由字母表示的品牌

+   每个品牌的产品，如…
