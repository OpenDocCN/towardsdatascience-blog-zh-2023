# 以 SQL 为思维方式 — 避免自上而下的 SQL 编写方法

> 原文：[`towardsdatascience.com/think-in-sql-avoid-writing-sql-in-a-top-to-bottom-approach-476a67f53a59?source=collection_archive---------5-----------------------#2023-02-02`](https://towardsdatascience.com/think-in-sql-avoid-writing-sql-in-a-top-to-bottom-approach-476a67f53a59?source=collection_archive---------5-----------------------#2023-02-02)

## 通过理解逻辑查询处理顺序来编写清晰的 SQL

[](https://chengzhizhao.medium.com/?source=post_page-----476a67f53a59--------------------------------)![Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----476a67f53a59--------------------------------)[](https://towardsdatascience.com/?source=post_page-----476a67f53a59--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----476a67f53a59--------------------------------) [Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----476a67f53a59--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff956c63a9571&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthink-in-sql-avoid-writing-sql-in-a-top-to-bottom-approach-476a67f53a59&user=Chengzhi+Zhao&userId=f956c63a9571&source=post_page-f956c63a9571----476a67f53a59---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----476a67f53a59--------------------------------) · 7 min read · 2023 年 2 月 2 日

--

![](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F476a67f53a59&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthink-in-sql-avoid-writing-sql-in-a-top-to-bottom-approach-476a67f53a59&source=-----476a67f53a59---------------------bookmark_footer-----------)![](img/4b40ea0f0eb8d5415ec9881ec750cd1a.png)

照片由[Jeffrey Brandjes](https://unsplash.com/es/@jeffreyfotografie?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布在[Unsplash](https://unsplash.com/photos/7cLqEYJws8E?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上。

你可能会觉得写 SQL 很有挑战性，因为它是声明式的。尤其是对那些熟悉像 Python、Java 或 C 这样的命令式语言的工程师来说，SQL 是一种切换和思维转变。SQL 的思维方式不同于任何命令式语言，学习和发展方式也不应相同。

在使用 SQL 时，你是否采用自上而下的方法？你是否从 SQL 中的“SELECT”子句开始开发？在本文中，我们将讨论 SQL 逻辑查询处理的顺序，以帮助你理解为什么你可能需要改变自上而下编写 SQL 的方式。这也可以帮助你更清晰地思考 SQL，并更有效地开发查询。

# SQL 是一种声明式语言

声明式语言的核心概念非常棒：人类传达的是高层结构和逻辑指令，而不是精确的流程。作为开发者，这意味着我们不需要编写逐步指南来**实现目标**。我们会编写**要实现的目标**，这样我们可以更多地关注结果，并让底层查询解析器和引擎来决定如何执行指令。
