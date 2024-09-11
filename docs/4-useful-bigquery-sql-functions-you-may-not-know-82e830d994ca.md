# 4 个你可能不知道的实用 BigQuery SQL 函数

> 原文：[https://towardsdatascience.com/4-useful-bigquery-sql-functions-you-may-not-know-82e830d994ca?source=collection_archive---------2-----------------------#2023-01-26](https://towardsdatascience.com/4-useful-bigquery-sql-functions-you-may-not-know-82e830d994ca?source=collection_archive---------2-----------------------#2023-01-26)

## 以及如何使用它们

[](https://madfordata.medium.com/?source=post_page-----82e830d994ca--------------------------------)[![Vicky Yu](../Images/54a32f45ebd13a18811912877f60f2f7.png)](https://madfordata.medium.com/?source=post_page-----82e830d994ca--------------------------------)[](https://towardsdatascience.com/?source=post_page-----82e830d994ca--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----82e830d994ca--------------------------------) [Vicky Yu](https://madfordata.medium.com/?source=post_page-----82e830d994ca--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcd08464b29cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-useful-bigquery-sql-functions-you-may-not-know-82e830d994ca&user=Vicky+Yu&userId=cd08464b29cc&source=post_page-cd08464b29cc----82e830d994ca---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----82e830d994ca--------------------------------) ·4 min read·Jan 26, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F82e830d994ca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-useful-bigquery-sql-functions-you-may-not-know-82e830d994ca&user=Vicky+Yu&userId=cd08464b29cc&source=-----82e830d994ca---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F82e830d994ca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-useful-bigquery-sql-functions-you-may-not-know-82e830d994ca&source=-----82e830d994ca---------------------bookmark_footer-----------)![](../Images/1c2be453429ef62ac777a6d882068008.png)

图片由 [Priscilla Du Preez](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

作为一个长期的 SQL 用户，我总是寻找更简单的方式来用 SQL 分析数据。在之前的文章中，我回顾了 [6 个 BigQuery SQL 函数](/6-bigquery-sql-functions-every-user-should-know-9ed97b1cf72e) 我希望我早些知道的，今天我想分享另外 4 个我希望你会觉得有用的函数。

## 1\. PERCENTILE_CONT

[PERCENTILE_CONT](https://cloud.google.com/bigquery/docs/reference/standard-sql/navigation_functions#percentile_cont) 计算一列值的百分位数。BigQuery 没有**MEDIAN**函数，但你可以使用**PERCENTILE_CONT**来计算[中位数，因为它等同于第50百分位数](https://www.statisticshowto.com/probability-and-statistics/percentiles-rank-range/)。计算中位数和百分位数对于了解数据分布和确定可能影响分析的异常值很有用。

在下面的示例中，我有6个数字（1, 3, 5, 8, 10和1000）在一个数组中，这些数字使用**UNNEST**函数扩展成行。第4行使用0.5作为参数来计算中位数，表示第50百分位数，第5行使用0.95来计算第95百分位数。注意结果显示**95th percentile**是**752**，而**25th percentile**是**3.5**，**median**是**6.5**。这表明异常值可能需要在分析中移除，因为差异非常大。
