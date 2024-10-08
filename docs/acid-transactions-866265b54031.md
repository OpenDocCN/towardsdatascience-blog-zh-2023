# 什么是 ACID 事务？

> 原文：[`towardsdatascience.com/acid-transactions-866265b54031`](https://towardsdatascience.com/acid-transactions-866265b54031)

## 在数据库事务的背景下理解 ACID 属性

[](https://gmyrianthous.medium.com/?source=post_page-----866265b54031--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----866265b54031--------------------------------)[](https://towardsdatascience.com/?source=post_page-----866265b54031--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----866265b54031--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----866265b54031--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----866265b54031--------------------------------) ·阅读时间 3 分钟·2023 年 7 月 5 日

--

![](img/3d32ef5f17f84fc39dd7972aa55efea2.png)

[照片由 Michal Matlon](https://unsplash.com/@michalmatlon?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，来源于 [Unsplash](https://unsplash.com/photos/_4VlY7cW86M?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在数据库操作的背景下，事务指的是被视为单一逻辑工作单元的操作，旨在使底层系统保持一致的状态。

一致性通过确保所有操作要么在成功完成后执行，要么在任何操作因某种原因失败时不执行来维护。

## ACID 属性

数据库事务应遵循所谓的 ACID 属性。这些系统被称为事务型系统，确保每个操作，包括读取、写入或更新，都符合 ACID 属性。ACID 缩写代表 **A**tomiciy（原子性）、**C**onsistency（一致性）、**I**solation（隔离性）和 **D**urability（持久性）。

**原子性**：该属性指的是一个事务被视为一个单一的工作单元。如果事务中定义的任何操作失败，则不会提交任何操作，之前执行的操作将被回滚（如果有的话），底层系统将恢复到其先前的状态。原子性防止数据丢失或数据损坏，因为事务只有在每个语句都成功应用后才会提交。

**一致性**：该属性确保所有事务遵循预定义规则，使每个事务以可预测的方式执行，这将始终使底层系统在提交后保持一致状态。

**隔离性**：一个事务性数据库系统允许多个用户同时与其交互。这个属性确保由多个用户发起的并发事务不会相互干扰，从而保持数据完整性。单个事务中的任何操作在事务提交之前都不会在外部可见。

**持久性**：这个最后的属性确保每当事务被提交时，它将被保存并且其效果是永久性的，即使在系统故障的情况下也是如此。

## 如何在 SQL 中编写事务

现在我们对四个 ACID 属性有了基本了解，让我们编写一个 SQL 事务来观察这些属性的实际效果。请注意，我将使用 BigQuery 语法，这可能也适用于许多其他 SQL 方言，但无法保证。

在下面分享的示例中，我们创建了一个包含两个语句的事务。第一个语句将向表`mydataset.mytable`中插入一条新记录，而第二个语句将（故意）引发错误，因为它尝试进行除以零的操作。

在我们代码片段的第二部分中，我们定义了回滚逻辑，它将撤销所有在故障之前所做的更改。由于 `SELECT 1/0` 语句将引发异常，`INSERT INTO` 语句将被回滚，新记录不会被添加到目标表中。

```py
BEGIN

  BEGIN TRANSACTION;

  INSERT INTO mydataset.mytable VALUES (1, 100, 'pending');

  -- Raise an exception by attempting a division by zero 
  SELECT 1/0;

  COMMIT TRANSACTION;

  EXCEPTION WHEN ERROR THEN
    -- Roll back the transaction if an exception is raised
    SELECT @@error.message;
    ROLLBACK TRANSACTION;
END;
```

## 结论

ACID 属性是事务性数据库系统必须遵守的最基本的概念之一，以确保和维护数据的完整性、一致性和可靠性。此外，这些概念使多个用户能够同时与底层系统进行交互，而不危及这些特性。

如果你的日常工作需要与事务性数据库系统进行交互，那么熟悉这些概念是非常重要的。数据库事务提供的 ACID 属性将确保数据的完整性，并允许你以适当的方式处理故障和错误，从而不会影响其他操作和用户。

👉 [**成为会员**](https://gmyrianthous.medium.com/membership) **并阅读 Medium 上的每一个故事。你的会员费直接支持我和其他你阅读的作者。你还将获得对 Medium 上每个故事的完全访问权限。**

[](https://gmyrianthous.medium.com/membership?source=post_page-----866265b54031--------------------------------) [## 使用我的推荐链接加入 Medium — Giorgos Myrianthous

### 作为 Medium 的会员，你的部分会员费用会分配给你阅读的作者，并且你可以完全访问每个故事……

gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership?source=post_page-----866265b54031--------------------------------)

👇**你可能也喜欢的相关文章** 👇

[](/etl-vs-elt-68e221d78719?source=post_page-----866265b54031--------------------------------) ## ETL 与 ELT：有什么区别？

### 在数据工程的背景下，ETL 与 ELT 的比较

towardsdatascience.com [](/dbt-55b35c974533?source=post_page-----866265b54031--------------------------------) ## 什么是 dbt（数据构建工具）

### 对正在主宰数据世界的 dbt 的温和介绍

towardsdatascience.com [](/diagrams-as-code-python-d9cbaa959ed5?source=post_page-----866265b54031--------------------------------) ## Python 中的图表作为代码

### 使用 Python 创建云系统架构图

towardsdatascience.com
