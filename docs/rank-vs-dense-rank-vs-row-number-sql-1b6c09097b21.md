# SQL 中的 RANK() 与 DENSE_RANK() 和 ROW_NUMBER()

> 原文：[`towardsdatascience.com/rank-vs-dense-rank-vs-row-number-sql-1b6c09097b21`](https://towardsdatascience.com/rank-vs-dense-rank-vs-row-number-sql-1b6c09097b21)

## 理解 SQL 中这些窗口函数之间的区别

[](https://gmyrianthous.medium.com/?source=post_page-----1b6c09097b21--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----1b6c09097b21--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1b6c09097b21--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1b6c09097b21--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----1b6c09097b21--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1b6c09097b21--------------------------------) ·6 分钟阅读·2023 年 3 月 6 日

--

![](img/14a0fd288583774a244801d6caf38161.png)

图片由 [Nik](https://unsplash.com/@helloimnik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/UNCQklgSUd4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在 SQL 的世界里，窗口函数是一个强大的构造，允许用户以精确的方式对数据进行分段和操作。通过根据特定列和排序标准对数据进行分组，窗口函数使得在分区内进行高级计算成为可能。

在这篇全面的教程中，我们将深入探讨三种最常用的窗口函数：`ROW_NUMBER()`、`DENSE_RANK()` 和 `RANK()`。无论你是经验丰富的 SQL 专家还是刚刚入门，本指南将提供你掌握这些关键工具所需的知识和实际示例。

所有包含的代码片段均已在 MySQL 数据库上测试，这些函数在几乎任何 SQL 方言中都应运行，只需最小或无需修改。

[**订阅 Data Pipeline**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的新闻通讯**

首先，让我们创建一个示例表格，在整个教程中将参考这个表格，以演示几个不同的概念。

```py
CREATE TABLE employees (
  id integer,
  first_name varchar(20),
  last_name varchar(20),
  position varchar(20),
  salary varchar(20)
)

INSERT INTO employees VALUES 
(1, 'Andrew', 'Brown', 'Manager', 100000),
(2, 'Maria', 'Johnson', 'Manager', 105000),
(3, 'John', 'Anderson', 'Senior Manager', 130000),
(4, 'Alex', 'Purple', 'Associate', 50000),
(5, 'George', 'Bull', 'Senior Associate', 65000),
(6, 'Jess', 'Fridman', 'Associate', 48000),
(7, 'Marion', 'White', 'Senior Associate', 65000),
(8, 'Andreea', 'Berton', 'Manager', 102000),
(9, 'Bob', 'Johanson', 'Associate', 45000),
(10, 'Georgia', 'Hoffman', 'Senior Associate', 66000),
(11, 'Johan', 'Peterson', 'Senior Associate', 58000);
```

这是我们的示例表格的样子：

```py
SELECT * FROM employees;

| id  | first_name | last_name | position         | salary |
| --- | ---------- | --------- | ---------------- | ------ |
| 1   | Andrew     | Brown     | Manager          | 100000 |
| 2   | Maria      | Johnson   | Manager          | 105000 |
| 3   | John       | Anderson  | Senior Manager   | 130000 |
| 4   | Alex       | Purple    | Associate        | 50000  |
| 5   | George     | Bull      | Senior Associate | 65000  |
| 6   | Jess       | Fridman   | Associate        | 48000  |
| 7   | Marion     | White     | Senior Associate | 65000  |
| 8   | Andreea    | Berton    | Manager          | 102000 |
| 9   | Bob        | Johanson  | Associate        | 45000  |
| 10  | Georgia    | Hoffman   | Senior Associate | 66000  |
| 11  | Johan      | Peterson  | Senior Associate | 58000  |
```

## 什么是窗口函数？

窗口函数用于 SQL 中，对一组行执行计算，并为每一行返回一个值。注意与聚合函数的区别，后者用于返回每组的单个值。窗口函数通常用于计算累计总和、排名以及移动平均值。

窗口函数的有效语法应包括

+   `**OVER**` **子句**在函数之后指定，用于引用窗口

+   和**窗口规范**，指定了行应如何分组（该规范可以包括`PARTITION BY`和/或`ORDER BY`子句）

## RANK()窗口函数

`RANK()`函数返回指定组或分区内每行的排名。如果在分区内有多行具有相同的值，那么所有这些行将被分配相同的排名。在这种情况下，排名中会出现间隙，因为随后的行将被分配与其实际排名相对应的编号（而不是下一个可用的排名）。

现在我们考虑创建一个`employees`记录的排名，使得在相同职位上的员工根据收入进行排名。这可以通过以下查询实现：

```py
SELECT 
  *,
  RANK() OVER (PARTITION BY position  ORDER BY salary DESC) AS emp_pos_rank
FROM 
  employees;

| id  | first_name | last_name | position         | salary | emp_pos_rank |
| --- | ---------- | --------- | ---------------- | ------ | ------------ |
| 4   | Alex       | Purple    | Associate        | 50000  | 1            |
| 6   | Jess       | Fridman   | Associate        | 48000  | 2            |
| 9   | Bob        | Johanson  | Associate        | 45000  | 3            |
| 2   | Maria      | Johnson   | Manager          | 105000 | 1            |
| 8   | Andreea    | Berton    | Manager          | 102000 | 2            |
| 1   | Andrew     | Brown     | Manager          | 100000 | 3            |
| 10  | Georgia    | Hoffman   | Senior Associate | 66000  | 1            |
| 5   | George     | Bull      | Senior Associate | 65000  | 2            |
| 7   | Marion     | White     | Senior Associate | 65000  | 2            |
| 11  | Johan      | Peterson  | Senior Associate | 58000  | 4            |
| 3   | John       | Anderson  | Senior Manager   | 130000 | 1            |
```

注意`Senior Associate`职位的排名出现了间隙。两名员工被分配了第二名，这意味着紧随这两名记录的记录将被分配为`4`（而不是`3`）。

## DENSE_RANK()窗口函数

`DENSE_RANK()`函数返回指定组或分区内每行的排名。与`RANK()`不同，`DENSE_RANK()`不会出现间隙：

```py
SELECT 
  *,
  DENSE_RANK() OVER (PARTITION BY position  ORDER BY salary DESC) AS emp_pos_rank
FROM 
  employees;

| id  | first_name | last_name | position         | salary | emp_pos_rank |
| --- | ---------- | --------- | ---------------- | ------ | ------------ |
| 4   | Alex       | Purple    | Associate        | 50000  | 1            |
| 6   | Jess       | Fridman   | Associate        | 48000  | 2            |
| 9   | Bob        | Johanson  | Associate        | 45000  | 3            |
| 2   | Maria      | Johnson   | Manager          | 105000 | 1            |
| 8   | Andreea    | Berton    | Manager          | 102000 | 2            |
| 1   | Andrew     | Brown     | Manager          | 100000 | 3            |
| 10  | Georgia    | Hoffman   | Senior Associate | 66000  | 1            |
| 5   | George     | Bull      | Senior Associate | 65000  | 2            |
| 7   | Marion     | White     | Senior Associate | 65000  | 2            |
| 11  | Johan      | Peterson  | Senior Associate | 58000  | 3            |
| 3   | John       | Anderson  | Senior Manager   | 130000 | 1            |
```

注意第 4 名 Senior Associate 现在会获得 3 名排名，因为其他两名员工共享第二名排名，并且`DENSE_RANK`窗口函数不会创建间隙。

## ROW_NUMBER()窗口函数

最后，`ROW_NUMBER`窗口函数会为每个分区内的每一行分配一个从索引`1`开始的编号。

```py
SELECT 
  *,
  ROW_NUMBER() OVER (PARTITION BY position  ORDER BY salary DESC) AS emp_pos_rank
FROM 
  employees;

| id  | first_name | last_name | position         | salary | emp_pos_rank |
| --- | ---------- | --------- | ---------------- | ------ | ------------ |
| 4   | Alex       | Purple    | Associate        | 50000  | 1            |
| 6   | Jess       | Fridman   | Associate        | 48000  | 2            |
| 9   | Bob        | Johanson  | Associate        | 45000  | 3            |
| 2   | Maria      | Johnson   | Manager          | 105000 | 1            |
| 8   | Andreea    | Berton    | Manager          | 102000 | 2            |
| 1   | Andrew     | Brown     | Manager          | 100000 | 3            |
| 10  | Georgia    | Hoffman   | Senior Associate | 66000  | 1            |
| 5   | George     | Bull      | Senior Associate | 65000  | 2            |
| 7   | Marion     | White     | Senior Associate | 65000  | 3            |
| 11  | Johan      | Peterson  | Senior Associate | 58000  | 4            |
| 3   | John       | Anderson  | Senior Manager   | 130000 | 1            |
```

## 最终思考

总之，窗口函数是 SQL 的一个强大功能，允许在结果集的特定子集上执行复杂的计算。它们可以用来计算累计总数、滚动平均值和其他需要参考邻近行的指标。

在今天的文章中，我们讨论了`RANK()`、`DENSE_RANK()`和`ROW_NUMBER()`函数之间的区别。通过理解这些窗口函数之间的差异，你可以选择最适合你特定用例的函数，并优化你的 SQL 查询性能。

👉 [**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，一份专注于数据工程的新闻通讯**

👇**相关的文章你可能也会喜欢** 👇

[](/dbt-55b35c974533?source=post_page-----1b6c09097b21--------------------------------) ## 什么是 dbt（数据构建工具）

### 对正在主导数据领域的 dbt 的温和介绍

towardsdatascience.com [](/etl-vs-elt-68e221d78719?source=post_page-----1b6c09097b21--------------------------------) ## ETL 与 ELT：有什么区别？

### 数据工程背景下 ETL 与 ELT 的比较

towardsdatascience.com [](/cte-sql-945e4b461de3?source=post_page-----1b6c09097b21--------------------------------) ## 什么是 SQL 中的 CTE

### 理解 SQL 中的公共表表达式（CTE）

towardsdatascience.com
