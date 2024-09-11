# 用 BigQuery SQL 用户定义函数简化数据清理

> 原文：[`towardsdatascience.com/simplify-data-cleaning-with-bigquery-sql-user-defined-functions-41c0560ea6?source=collection_archive---------8-----------------------#2023-04-20`](https://towardsdatascience.com/simplify-data-cleaning-with-bigquery-sql-user-defined-functions-41c0560ea6?source=collection_archive---------8-----------------------#2023-04-20)

## 简介和用例

[](https://madfordata.medium.com/?source=post_page-----41c0560ea6--------------------------------)![Vicky Yu](https://madfordata.medium.com/?source=post_page-----41c0560ea6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----41c0560ea6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----41c0560ea6--------------------------------) [Vicky Yu](https://madfordata.medium.com/?source=post_page-----41c0560ea6--------------------------------)

·

[跟进](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcd08464b29cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplify-data-cleaning-with-bigquery-sql-user-defined-functions-41c0560ea6&user=Vicky+Yu&userId=cd08464b29cc&source=post_page-cd08464b29cc----41c0560ea6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----41c0560ea6--------------------------------) ·5 分钟阅读·2023 年 4 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F41c0560ea6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplify-data-cleaning-with-bigquery-sql-user-defined-functions-41c0560ea6&user=Vicky+Yu&userId=cd08464b29cc&source=-----41c0560ea6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F41c0560ea6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplify-data-cleaning-with-bigquery-sql-user-defined-functions-41c0560ea6&source=-----41c0560ea6---------------------bookmark_footer-----------)![](img/e4af66655e15b1d6abe99b4aae5a16c3.png)

[Brooke Cagle](https://unsplash.com/@brookecagle?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片

数据相关工作的大部分是数据清理，但编写 SQL 语句常常会很繁琐，尤其是在表中多个列上编写相同的 SQL 逻辑。直到我发现可以在[BigQuery 中创建用户定义函数 (UDFs)](https://cloud.google.com/bigquery/docs/reference/standard-sql/user-defined-functions)来满足我的特定数据清理用例。今天我想分享一些数据清理用例，你可以应用 UDFs 来简化你的 SQL 查询。

## 介绍

由于不同公司数据库权限不同，我将讨论使用临时 UDFs 的数据清理示例，因为永久 UDFs 可能需要数据库管理员不允许的额外访问权限。临时 UDFs 在 SQL 查询完成时过期，而持久 UDFs 则保存在数据库中，可以在多个 SQL 查询中使用。

我将使用我创建并上传到[BigQuery 沙盒](https://cloud.google.com/bigquery/docs/sandbox)的虚假电影数据，任何有 Google 账号的人都可以免费使用。我几年前在数据分析师面试的家庭作业中收到了类似的数据，并将使用在作业中执行的数据清理示例，但这次使用 UDFs。

## 用例 1：用于报告的值分组
