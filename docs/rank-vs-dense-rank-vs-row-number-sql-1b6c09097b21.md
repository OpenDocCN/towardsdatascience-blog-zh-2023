# SQL 中的 RANK() 与 DENSE_RANK() 与 ROW_NUMBER()

> 原文：[`towardsdatascience.com/rank-vs-dense-rank-vs-row-number-sql-1b6c09097b21?source=collection_archive---------3-----------------------#2023-03-06`](https://towardsdatascience.com/rank-vs-dense-rank-vs-row-number-sql-1b6c09097b21?source=collection_archive---------3-----------------------#2023-03-06)

## 理解这些 SQL 窗口函数之间的差异

[](https://gmyrianthous.medium.com/?source=post_page-----1b6c09097b21--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----1b6c09097b21--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1b6c09097b21--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1b6c09097b21--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----1b6c09097b21--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frank-vs-dense-rank-vs-row-number-sql-1b6c09097b21&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----1b6c09097b21---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1b6c09097b21--------------------------------) ·6 分钟阅读·2023 年 3 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1b6c09097b21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frank-vs-dense-rank-vs-row-number-sql-1b6c09097b21&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----1b6c09097b21---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1b6c09097b21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frank-vs-dense-rank-vs-row-number-sql-1b6c09097b21&source=-----1b6c09097b21---------------------bookmark_footer-----------)![](img/14a0fd288583774a244801d6caf38161.png)

照片由 [Nik](https://unsplash.com/@helloimnik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/UNCQklgSUd4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在 SQL 的世界里，窗口函数是一个强大的构造，它允许用户以精确的方式对数据进行分段和处理。通过基于特定列和排序标准对数据进行分组，窗口函数能够在分区内进行高级计算。

在这篇全面的教程中，我们将深入探讨三种最常用的窗口函数：`ROW_NUMBER()`、`DENSE_RANK()` 和 `RANK()`。无论你是经验丰富的 SQL 专家还是刚刚入门，这个指南都会为你提供掌握这些基本工具所需的知识和实际示例。

所有包含的代码片段都在 MySQL 数据库上进行了测试，这些函数应该在几乎任何 SQL 变体上运行，只需最少或无需修改。

[**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的通讯**

首先，让我们创建一个示例表格，我们将在整个教程中引用它，以演示几个不同的概念。

```py
CREATE TABLE employees (
  id integer,
  first_name varchar(20),
  last_name varchar(20),
  position varchar(20),
  salary varchar(20)
)

INSERT INTO employees VALUES 
(1, 'Andrew', 'Brown'…
```
