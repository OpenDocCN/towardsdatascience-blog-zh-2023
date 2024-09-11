# 2023 年你需要了解的 2 个重要 SQL CASE WHEN 示例

> 原文：[https://towardsdatascience.com/2-important-sql-case-when-examples-you-need-to-know-in-2023-cb5d50e59daa?source=collection_archive---------4-----------------------#2023-07-05](https://towardsdatascience.com/2-important-sql-case-when-examples-you-need-to-know-in-2023-cb5d50e59daa?source=collection_archive---------4-----------------------#2023-07-05)

## 数据科学

## 掌握解决真实 SQL CASE WHEN 问题的方法

[](https://medium.com/@17.rsuraj?source=post_page-----cb5d50e59daa--------------------------------)[![Suraj Gurav](../Images/f5dca32861f8c1c428e66fbe2174c04b.png)](https://medium.com/@17.rsuraj?source=post_page-----cb5d50e59daa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cb5d50e59daa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cb5d50e59daa--------------------------------) [Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----cb5d50e59daa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fdda183cca2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2-important-sql-case-when-examples-you-need-to-know-in-2023-cb5d50e59daa&user=Suraj+Gurav&userId=1fdda183cca2&source=post_page-1fdda183cca2----cb5d50e59daa---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----cb5d50e59daa--------------------------------) ·10分钟阅读·2023年7月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcb5d50e59daa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2-important-sql-case-when-examples-you-need-to-know-in-2023-cb5d50e59daa&user=Suraj+Gurav&userId=1fdda183cca2&source=-----cb5d50e59daa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcb5d50e59daa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2-important-sql-case-when-examples-you-need-to-know-in-2023-cb5d50e59daa&source=-----cb5d50e59daa---------------------bookmark_footer-----------)![](../Images/a16a9cb652552c6bea20bec356ccbe77.png)

图片由[Vincent van Zalinge](https://unsplash.com/@vincentvanzalinge?utm_source=medium&utm_medium=referral)拍摄，发布于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**使用 SQL 中 CASE WHEN 的真实面试题！**

在文章[**5 个高级 SQL 概念**](/5-advanced-sql-concepts-you-should-know-in-2022-b50efe6c99)中，你可以探讨 CASE..WHEN 语句的基础知识，以及[**用例**](/5-advanced-sql-case-examples-that-will-make-you-use-case-when-easily-1040c1eb8857)。然而，在这些文章中，我没有提及 CASE WHEN 的实际应用。

因此，我通过 LinkedIn 与多位数据科学专业人士在体育和电子商务公司联系，并收集了这 2 个在求职面试中最常被问到的 SQL CASE WHEN 示例。

> 为什么 CASE WHEN 是最常被提问的概念之一？

因为，SQL 中的 CASE WHEN 语句有助于在查询数据时实现 `If..Else` 逻辑。

你经常需要根据某些条件提取或汇总数据。当然，你可以使用 WHERE 子句来应用这些条件，但当你想基于这些条件创建新列时，`**CASE..WHEN**` 非常方便，你必须使用它。

在本文中，你将看到 2 个真实且常被问到的面试问题，你可以使用 SQL CASE WHEN 来解决这些问题。
