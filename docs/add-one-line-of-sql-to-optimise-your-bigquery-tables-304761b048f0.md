# 添加一行 SQL 以优化您的 BigQuery 表

> 原文：[`towardsdatascience.com/add-one-line-of-sql-to-optimise-your-bigquery-tables-304761b048f0?source=collection_archive---------2-----------------------#2023-12-08`](https://towardsdatascience.com/add-one-line-of-sql-to-optimise-your-bigquery-tables-304761b048f0?source=collection_archive---------2-----------------------#2023-12-08)

## 聚类：一种简单的方法来分组相似的行，防止不必要的数据处理

[](https://medium.com/@mattchapmanmsc?source=post_page-----304761b048f0--------------------------------)![Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----304761b048f0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----304761b048f0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----304761b048f0--------------------------------) [Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----304761b048f0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbf7d13fc53db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadd-one-line-of-sql-to-optimise-your-bigquery-tables-304761b048f0&user=Matt+Chapman&userId=bf7d13fc53db&source=post_page-bf7d13fc53db----304761b048f0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----304761b048f0--------------------------------) · 5 分钟阅读 · 2023 年 12 月 8 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F304761b048f0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadd-one-line-of-sql-to-optimise-your-bigquery-tables-304761b048f0&user=Matt+Chapman&userId=bf7d13fc53db&source=-----304761b048f0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F304761b048f0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadd-one-line-of-sql-to-optimise-your-bigquery-tables-304761b048f0&source=-----304761b048f0---------------------bookmark_footer-----------)

在我之前的文章中，我解释了如何使用分区来优化 SQL 查询：

[](/use-the-partitions-luke-a-simple-and-proven-way-to-optimise-your-sql-queries-43e24ea4c5d0?source=post_page-----304761b048f0--------------------------------) ## 使用分区，卢克！一种简单而有效的 SQL 查询优化方法

### 如果你曾经写过运行时间长的 SQL 查询，这篇文章适合你

towardsdatascience.com

现在，我正在写**续集**！ (来点爸爸笑话吗？)

本文将探讨**聚簇**：在 BigQuery 中另一个强大的优化技术。像分区一样，聚簇可以帮助您编写性能更高、运行速度更快、成本更低的查询。如果您想发展 SQL 工具包并建立更高级的数据科学技能，这是一个很好的起点。

# 什么是聚簇表？

在 BigQuery 中，聚簇表是将相似的行物理上“分组”在一起的表。

例如，想象一个名为`user_signups`的表，用于跟踪所有在一个虚构网站上注册账号的人。它有四列：

+   `registration_date`: 用户创建账号的日期

+   `country`: 用户所在的国家

+   `tier`: 用户的计划（“免费”或“付费”）
