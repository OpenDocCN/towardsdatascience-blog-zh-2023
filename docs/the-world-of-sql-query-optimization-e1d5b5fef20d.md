# SQL 查询优化的世界

> 原文：[`towardsdatascience.com/the-world-of-sql-query-optimization-e1d5b5fef20d?source=collection_archive---------10-----------------------#2023-03-27`](https://towardsdatascience.com/the-world-of-sql-query-optimization-e1d5b5fef20d?source=collection_archive---------10-----------------------#2023-03-27)

![](img/83a4adc3c26c7145297a344045db80f7.png)

图片由 [Jake Blucker](https://unsplash.com/@jakeblucker?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

## 数据工程

## 探索不同的查询优化器及其工作原理

[](https://kovidrathee.medium.com/?source=post_page-----e1d5b5fef20d--------------------------------)![Kovid Rathee](https://kovidrathee.medium.com/?source=post_page-----e1d5b5fef20d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e1d5b5fef20d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e1d5b5fef20d--------------------------------) [Kovid Rathee](https://kovidrathee.medium.com/?source=post_page-----e1d5b5fef20d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F13d513db037&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-world-of-sql-query-optimization-e1d5b5fef20d&user=Kovid+Rathee&userId=13d513db037&source=post_page-13d513db037----e1d5b5fef20d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e1d5b5fef20d--------------------------------) · 6 分钟阅读 · 2023 年 3 月 27 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe1d5b5fef20d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-world-of-sql-query-optimization-e1d5b5fef20d&user=Kovid+Rathee&userId=13d513db037&source=-----e1d5b5fef20d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe1d5b5fef20d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-world-of-sql-query-optimization-e1d5b5fef20d&source=-----e1d5b5fef20d---------------------bookmark_footer-----------)

SQL 是一种简单的语言，规则很少，这也是它如此受欢迎的原因。它还包含大量的关键字和功能，允许你以各种方式与数据进行交互。正是这种灵活性带来了查询编写风格和选择上的诸多变异。

一旦你向数据库发出查询，它必须解析你的查询以理解其流程，但这并不是数据库工作的全部。数据库引擎还有一个组件，用来查看你的查询，并在不改变查询功能的前提下对其进行重写，以便提高性能和响应时间。这个强大的组件，毫无疑问，被称为**查询优化器**。

但是优化器如何重写查询呢？它具有什么额外的信息来源来处理查询？这些问题的答案有助于区分不同类型的查询优化器。广义上讲，有**四种查询优化技术**，如下所述：

+   **基于启发式的优化**根据数据库中的预定义规则进行，例如优先考虑聚簇索引。

+   **基于成本的优化**工作…
