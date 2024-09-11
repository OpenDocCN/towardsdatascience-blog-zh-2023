# 提升 Spark 联合操作符性能：优化提示以提高查询速度

> 原文：[`towardsdatascience.com/boosting-spark-union-operator-performance-optimization-tips-for-improved-query-speed-9123ae6ada80?source=collection_archive---------11-----------------------#2023-04-20`](https://towardsdatascience.com/boosting-spark-union-operator-performance-optimization-tips-for-improved-query-speed-9123ae6ada80?source=collection_archive---------11-----------------------#2023-04-20)

## 解密 Spark 联合操作符的性能

[](https://chengzhizhao.medium.com/?source=post_page-----9123ae6ada80--------------------------------)![Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----9123ae6ada80--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9123ae6ada80--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9123ae6ada80--------------------------------) [Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----9123ae6ada80--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff956c63a9571&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fboosting-spark-union-operator-performance-optimization-tips-for-improved-query-speed-9123ae6ada80&user=Chengzhi+Zhao&userId=f956c63a9571&source=post_page-f956c63a9571----9123ae6ada80---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9123ae6ada80--------------------------------) ·6 min read·2023 年 4 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9123ae6ada80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fboosting-spark-union-operator-performance-optimization-tips-for-improved-query-speed-9123ae6ada80&user=Chengzhi+Zhao&userId=f956c63a9571&source=-----9123ae6ada80---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9123ae6ada80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fboosting-spark-union-operator-performance-optimization-tips-for-improved-query-speed-9123ae6ada80&source=-----9123ae6ada80---------------------bookmark_footer-----------)![](img/bd00c06827eb22a6b64bcc2112e3d7d8.png)

图片由 [Fahrul Azmi](https://unsplash.com/@fahrulazmi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/zN4mtLHkHn4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

联合操作符是将两个输入数据框合并为一个的集合操作符之一。Union 是 Apache Spark 中一种方便的操作，用于合并具有相同列顺序的行。一个常见的用例是应用不同的转换，然后将它们联合起来。

在 Spark 中使用 union 操作的方法常被广泛讨论。然而，一个较少讨论的隐藏事实是与 union 操作符相关的性能陷阱。如果我们没有理解 Spark 中 union 操作符的陷阱，可能会陷入执行时间翻倍的陷阱。

在这个故事中，我们将专注于 Apache Spark DataFrame 的 union 操作符，展示物理查询计划，并分享优化技巧。

# Spark 中的 Union Operator 101

与关系数据库（RDBMS）SQL 类似，union 是直接合并行的一种方法。在处理 union 操作符时，需要注意的一点是确保行遵循相同的结构：

+   **列数应该相同**。联合操作不会静默执行或填充……
