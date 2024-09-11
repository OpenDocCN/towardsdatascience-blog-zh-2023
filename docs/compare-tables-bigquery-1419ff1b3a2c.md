# 如何在 BigQuery 中比较两个表的相等性

> 原文：[`towardsdatascience.com/compare-tables-bigquery-1419ff1b3a2c?source=collection_archive---------0-----------------------#2023-01-26`](https://towardsdatascience.com/compare-tables-bigquery-1419ff1b3a2c?source=collection_archive---------0-----------------------#2023-01-26)

## 使用标准 SQL 比较表格并提取它们的差异

[](https://gmyrianthous.medium.com/?source=post_page-----1419ff1b3a2c--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----1419ff1b3a2c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1419ff1b3a2c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1419ff1b3a2c--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----1419ff1b3a2c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcompare-tables-bigquery-1419ff1b3a2c&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----1419ff1b3a2c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1419ff1b3a2c--------------------------------) ·6 分钟阅读·2023 年 1 月 26 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1419ff1b3a2c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcompare-tables-bigquery-1419ff1b3a2c&source=-----1419ff1b3a2c---------------------bookmark_footer-----------)![](img/d91f79f374c08c8dffd598f1df953ed3.png)

图片由 [Zakaria Ahada](https://unsplash.com/@zakariahada?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/0xOCVe7nUU0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在 BigQuery 中比较表格是测试数据管道和查询结果的关键任务，尤其是在将它们投入生产之前。比较表格的能力可以检测数据中的任何变化或不一致，确保数据的准确性和一致性。

在这篇文章中，我们将演示如何在 BigQuery 上比较两个（或多个）表格，并提取出不同的记录（如果有的话）。更具体地说，我们将展示如何比较具有相同列的表格以及具有不同列数的表格。

首先，让我们通过创建两个具有一些虚拟值的表格来开始，这些表格将在本教程中被引用，以演示几个不同的概念。

```py
-- Create the first table
CREATE TABLE `temp.tableA` (
  `first_name` STRING,
  `last_name` STRING,
  `is_active` BOOL,
  `no_of_purchases` INT
)
INSERT `temp.tableA` (first_name, last_name, is_active, no_of_purchases)
VALUES 
  ('Bob', 'Anderson', True, 12),
  ('Maria', 'Brown', False, 0),
  ('Andrew', 'White', True, 4)

-- Create the second table
CREATE TABLE `temp.tableB` (
  `first_name` STRING,
  `last_name` STRING,
  `is_active` BOOL,
  `no_of_purchases` INT…
```
