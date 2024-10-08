# 如何在 BigQuery 中比较两个表的相等性

> 原文：[`towardsdatascience.com/compare-tables-bigquery-1419ff1b3a2c`](https://towardsdatascience.com/compare-tables-bigquery-1419ff1b3a2c)

## 使用标准 SQL 比较表并提取其差异

[](https://gmyrianthous.medium.com/?source=post_page-----1419ff1b3a2c--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----1419ff1b3a2c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1419ff1b3a2c--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----1419ff1b3a2c--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----1419ff1b3a2c--------------------------------)

·发布于[数据科学前沿](https://towardsdatascience.com/?source=post_page-----1419ff1b3a2c--------------------------------) ·阅读时长 6 分钟·2023 年 1 月 26 日

--

![](img/d91f79f374c08c8dffd598f1df953ed3.png)

图片由[Zakaria Ahada](https://unsplash.com/@zakariahada?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，来源于[Unsplash](https://unsplash.com/photos/0xOCVe7nUU0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在 BigQuery 中比较表格是测试数据管道和查询结果的关键任务，特别是在将它们投入生产之前。比较表格的能力可以检测数据中的任何变化或差异，确保数据保持准确和一致。

在本文中，我们将演示如何在 BigQuery 上比较两个（或更多）表，并提取不同的记录（如果有）。更具体地说，我们将展示如何比较具有相同列的表以及列数不同的表。

首先，让我们创建两个具有一些虚拟值的表，然后在本教程中引用这些表，以演示几个不同的概念。

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
  `no_of_purchases` INT
)
INSERT `temp.tableB` (first_name, last_name, is_active, no_of_purchases)
VALUES 
  ('Bob', 'Anderson', True, 12),
  ('Maria', 'Brown', False, 0),
  ('Andrew', 'White', True, 6),
  ('John', 'Down', False, 0)
```

## 比较具有相同列的表记录

现在我们已经创建了两个示例表，你应该已经注意到它们之间有几个差异。

```py
SELECT * FROM `temp.tableA`;

+------------+-----------+-----------+-----------------+
| first_name | last_name | is_active | no_of_purchases |
+------------+-----------+-----------+-----------------+
| Bob        | Anderson  | true      | 12              |
| Andrew     | White     | true      | 4               |
| Maria      | Brown     | false     | 0               |
+------------+-----------+-----------+-----------------+
```

```py
SELECT * FROM `temp.tableB`;

+------------+-----------+-----------+-----------------+
| first_name | last_name | is_active | no_of_purchases |
+------------+-----------+-----------+-----------------+
| Bob        | Anderson  | true      | 12              |
| Andrew     | White     | true      | 6               |
| Maria      | Brown     | false     | 0               |
| John       | Down      | false     | 0               |
+------------+-----------+-----------+-----------------+
```

现在假设表`temp.tableB`是某个数据集的最新版本，而`temp.tableA`是旧版本，我们希望查看这两个表之间的实际差异（记录方面），我们只需使用以下查询：

```py
WITH
  table_a AS (SELECT * FROM `temp.tableA`),
  table_b AS (SELECT * FROM `temp.tableB`),
  rows_mismatched AS (
    SELECT
      'tableA' AS table_name,
      *
    FROM (
      SELECT
        *
      FROM
        table_a EXCEPT DISTINCT
      SELECT
        *
      FROM
        table_b 
    )

    UNION ALL

    SELECT
      'tableB' AS table_name,
      *
    FROM (
      SELECT
        *
      FROM
        table_b EXCEPT DISTINCT
      SELECT
        *
      FROM
        table_a 
    )
  )

SELECT * FROM rows_mismatched
```

现在，结果将包含所有观察到的表之间的差异以及记录发现的表名称的参考。

在我们的具体示例中，表 A 和表 B 在两个记录上存在差异；第一个记录似乎是`Andrew White`的记录，因为此人的`no_of_purchases`字段的值不同。此外，表`tableB`有一个在表`tableA`中不存在的额外记录。

```py
+------------+------------+-----------+-----------+-----------------+
| table_name | first_name | last_name | is_active | no_of_purchases |
+------------+------------+-----------+-----------+-----------------+
| tableB     | John       | Down      | false     | 0               |
| tableB     | Andrew     | White     | true      | 6               |
| tableA     | Andrew     | White     | true      | 4               |
+------------+------------+-----------+-----------+-----------------+
```

*注意：如果你不熟悉* `*WITH*` *子句和 SQL 中的公共表表达式（CTEs），请务必阅读以下文章：*

[](/cte-sql-945e4b461de3?source=post_page-----1419ff1b3a2c--------------------------------) ## 什么是 SQL 中的 CTEs

### 理解 SQL 中的公共表表达式（CTE）

towardsdatascience.com

## 比较具有不同列的表记录

现在假设你想比较两个列数不同的表中的记录。显然，我们需要进行等价比较，即我们需要从两个表中提取出共同的字段，以便进行有意义的比较。

让我们重新创建我们的表，以生成一些不匹配的列，这样我们就可以演示如何处理这些情况：

```py
-- Create the first table
CREATE TABLE `temp.tableA` (
  `first_name` STRING,
  `last_name` STRING,
  `is_active` BOOL,
  `dob` STRING
)
INSERT `temp.tableA` (first_name, last_name, is_active, dob)
VALUES 
  ('Bob', 'Anderson', True, '12/02/1993'),
  ('Maria', 'Brown', False, '10/05/2000'),
  ('Andrew', 'White', True, '14/12/1997')

-- Create the second table
CREATE TABLE `temp.tableB` (
  `first_name` STRING,
  `last_name` STRING,
  `is_active` BOOL,
  `no_of_purchases` INT
)
INSERT `temp.tableB` (first_name, last_name, is_active, no_of_purchases)
VALUES 
  ('Bob', 'Anderson', True, 12),
  ('Maria', 'Brown', True, 0),
  ('Andrew', 'White', True, 6),
  ('John', 'Down', False, 0)
```

现在我们的新表只有三个共同的列，即`first_name`、`last_name`和`is_active`。

```py
SELECT * FROM `temp.tableA`;

+------------+-----------+-----------+--------------+
| first_name | last_name | is_active | dob          |
+------------+-----------+-----------+--------------+
| Bob        | Anderson  | true      | '12/02/1993' |
| Andrew     | White     | true      | '10/05/2000' |
| Maria      | Brown     | false     | '14/12/1997' |
+------------+-----------+-----------+--------------+
```

```py
SELECT * FROM `temp.tableB`;

+------------+-----------+-----------+-----------------+
| first_name | last_name | is_active | no_of_purchases |
+------------+-----------+-----------+-----------------+
| Bob        | Anderson  | true      | 12              |
| Andrew     | White     | true      | 6               |
| Maria      | Brown     | false     | 0               |
| John       | Down      | false     | 0               |
+------------+-----------+-----------+-----------------+
```

现在，如果我们尝试运行上一节中执行的查询，而这两个表具有相同的列，我们将遇到以下错误：

```py
Column 4 in EXCEPT DISTINCT has incompatible types: STRING, INT64 at [13:7]
```

鉴于我们的表不再有匹配的列，这种情况是完全正常的。我们需要稍微修改我们最初的查询，使得最初的 CTEs 只选择每个表的共同列。我们的查询将如下所示：

```py
WITH
  table_a AS (
    SELECT 
      first_name,
      last_name,
      is_active
    FROM 
      `temp.tableA`
  ),
  table_b AS (
    SELECT 
      first_name,
      last_name,
      is_active 
    FROM 
      `temp.tableB`
  ),
  rows_mismatched AS (
    SELECT
      'tableA' AS table_name,
      *
    FROM (
      SELECT
        *
      FROM
        table_a EXCEPT DISTINCT
      SELECT
        *
      FROM
        table_b 
    )

    UNION ALL

    SELECT
      'tableB' AS table_name,
      *
    FROM (
      SELECT
        *
      FROM
        table_b EXCEPT DISTINCT
      SELECT
        *
      FROM
        table_a 
    )
  )

SELECT * FROM rows_mismatched
```

在本节中创建的表存在以下不匹配情况（仅考虑其共同列时）：

+   `Maria Brown`的记录在`is_active`列上存在差异

+   表`tableB`有一条额外的记录（`John Down`），而在`tableA`中不存在

这些差异可以从下面共享的查询结果中观察到：

```py
+------------+------------+-----------+-----------+
| table_name | first_name | last_name | is_active |
+------------+------------+-----------+-----------+
| tableB     | Maria      | Brown     | false     |
| tableB     | John       | Down      | false     | 
| tableA     | Maria      | Brown     | true      | 
+------------+------------+-----------+-----------+
```

## 结论

在这篇文章中，我们提供了一个全面的指南，说明如何在 BigQuery 中比较表格。我们强调了这项任务在确保数据准确性和一致性方面的重要性，并演示了多种比较具有相同列的表格以及具有不同列数的表格的技术。我们还介绍了提取表格间不同记录的过程（如果有的话）。

总体而言，这篇文章旨在为读者提供有效和高效比较 BigQuery 表格所需的工具和知识。希望你觉得它有用！

[**成为会员**](https://gmyrianthous.medium.com/membership) **并阅读 Medium 上的每一个故事。你的会员费用直接支持我和其他你阅读的作者。你还将完全访问 Medium 上的每一个故事。**

[](https://gmyrianthous.medium.com/membership?source=post_page-----1419ff1b3a2c--------------------------------) [## 通过我的推荐链接加入 Medium — Giorgos Myrianthous

### 作为 Medium 会员，你的一部分会员费用会分配给你阅读的作者，同时你可以完全访问每一个故事…

[gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership?source=post_page-----1419ff1b3a2c--------------------------------)

**相关的文章你可能也会喜欢**

[](/etl-vs-elt-68e221d78719?source=post_page-----1419ff1b3a2c--------------------------------) ## ETL 与 ELT：有什么区别？

### 在数据工程背景下对 ETL 和 ELT 进行比较

[towardsdatascience.com ## 什么是 dbt（数据构建工具）

### 对 dbt 的温和介绍，它正在主导数据世界

[towardsdatascience.com
