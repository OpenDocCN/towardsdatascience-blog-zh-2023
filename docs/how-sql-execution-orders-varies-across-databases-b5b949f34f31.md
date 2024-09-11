# SQL 执行顺序在不同数据库中的差异

> 原文：[`towardsdatascience.com/how-sql-execution-orders-varies-across-databases-b5b949f34f31?source=collection_archive---------0-----------------------#2023-12-07`](https://towardsdatascience.com/how-sql-execution-orders-varies-across-databases-b5b949f34f31?source=collection_archive---------0-----------------------#2023-12-07)

## *为什么在 SQL Server 中不能按顺序位置进行 GROUP BY，但其他数据库可以*

[](https://medium.com/@tobisam?source=post_page-----b5b949f34f31--------------------------------)![Tobi Sam](https://medium.com/@tobisam?source=post_page-----b5b949f34f31--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b5b949f34f31--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b5b949f34f31--------------------------------) [Tobi Sam](https://medium.com/@tobisam?source=post_page-----b5b949f34f31--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F92fab82e0c7a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-sql-execution-orders-varies-across-databases-b5b949f34f31&user=Tobi+Sam&userId=92fab82e0c7a&source=post_page-92fab82e0c7a----b5b949f34f31---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b5b949f34f31--------------------------------) ·4 min 阅读·2023 年 12 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb5b949f34f31&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-sql-execution-orders-varies-across-databases-b5b949f34f31&user=Tobi+Sam&userId=92fab82e0c7a&source=-----b5b949f34f31---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb5b949f34f31&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-sql-execution-orders-varies-across-databases-b5b949f34f31&source=-----b5b949f34f31---------------------bookmark_footer-----------)![](img/8fc3fd8afa97a4a245a0cc1f3bff5fc7.png)

Transact-SQL 与 MySQL 执行顺序（作者提供的图片）

在与 MySQL 和 PostgreSQL 等开源数据库定期工作之后，我最近有机会参与一个 SQL Server 项目，并发现了 SQL 环境中的一个微妙但重要的区别。我观察到在 SQL Server 中，我无法按顺序位置进行 GROUP BY（GROUP BY 1, 2, 3…），而这是我在其他数据库中经常使用的功能，特别是用于快速测试。

这一发现让我探讨了这两个数据库系统的几个细微差别，特别是**SQL 执行顺序**，这将是本文的重点。

为什么这很重要？在使用数据库系统时，了解这些细微差别可以极大地影响你的工作流程，提高你的生产力。它可以节省你大量的故障排除时间。此外，**通过** **理解各种数据库的 SQL 执行顺序，你可以根据你所使用的系统编写更优化的 SQL 查询**。

在本文中，我们将探讨一个主要的用例——GROUP BY——并调查其原因。然而，这些见解也可以应用于 HAVING、WHERE 或任何其他 SQL 命令子句。

# 让我们开始吧

让我们看看下面这个查询示例。虽然在 MySQL 中它可以正常工作，但在 SQL Server 中这将**不起作用**：

```py
SELECT
    DATEPART(year, day) AS order_date,
    SUM(cost) as cost
FROM clean
GROUP BY 1;
```

如果你运行这个，你可能会得到类似这样的错误：

`每个 GROUP BY 表达式必须包含至少一个不是外部引用的列。`

然而，在将 GROUP BY 序号引用替换为显式表达式后，这个修订的查询可以正常工作。你还会注意到，在 ORDER BY 子句中可以引用序号位置，这让我觉得很奇怪：

```py
SELECT
    datepart(year, day),
    sum(cost) as cost
from clean
GROUP BY datepart(year, day)
ORDER BY 1;
```

在 SQL Server 中，我很快了解到我必须在 GROUP BY 子句中使用显式的列名或表达式。**这被认为是一种最佳实践，因为它使代码更易于理解**。然而，我对为什么不同数据库之间会有这种行为差异感到好奇。此外，我发现 SQL Server 中的`ORDER BY`子句与序号位置一起使用，这进一步激起了我的好奇心。

# **探索 SELECT 语句执行顺序**

为了了解情况，我们来看一下`SELECT`语句在 SQL Server 和其他数据库中的执行/处理顺序。需要注意的是，在 SQL 数据库中，每部分查询都是按顺序执行的，这一顺序与书写的顺序不同。

例如，在 SQL Server 中，我们可以从下面的图片和[Microsoft 文档](https://learn.microsoft.com/en-us/sql/t-sql/queries/select-transact-sql?view=sql-server-ver16&redirectedfrom=MSDN#logical-processing-order-of-the-select-statement)中看到，FROM 子句是第一个被评估的命令。此外，**SELECT 子句在 GROUP BY 子句之后运行**。这就是为什么在第一个示例中我们无法在 GROUP BY 子句中引用列的位置或其别名的原因！

然而，我们可以在 ORDER BY 子句中自由引用序号位置和/或别名，因为这是在 SELECT 子句之后被评估的。SELECT 子句告诉数据库哪些列将被返回，因此此时位置是已知的。很酷，对吧？

## SQL Server 执行顺序

![](img/a122deac3dc1a4cc59ea0163ac976429.png)

SQL Server SELECT 语句处理顺序（图片由作者提供）

# MySQL

然而，在 MySQL 中，我发现很难找到明确的文档说明 SQL 查询的执行顺序。执行顺序似乎依赖于查询的内容以及查询优化器定义的最佳路径。

但从我们从 MySQL 文档中看到的情况来看，这些线索显示了执行顺序可能的方式，即**在评估 GROUP BY 子句之前，SELECT 子句先被评估**：

> 对于 GROUP BY 或 HAVING 子句，它在搜索**select_expr**值之前先搜索 FROM 子句。（对于 GROUP BY 和 HAVING，这与 MySQL 5.0 之前的行为不同，后者使用与 ORDER BY 相同的规则。）

# GoogleSQL

如果我们再看看 GoogleSQL（前称标准 SQL）的[文档](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#implicit_aliases)，这是 Google BigQuery 中使用的语法，你会看到与 SQL Server 查询执行方式不同的类似偏差：

> GROUP BY 和 ORDER BY 还可以引用第三组：整数文字，它们指的是 SELECT 列表中的项。整数 1 指的是 SELECT 列表中的第一项，2 指的是第二项，依此类推。

如你所见，这种行为在 SQL Server 中不受支持。Google 文档还提到，GROUP BY、ORDER BY 和 HAVING 可以引用来自 SELECT 列表的别名。

综上所述，我们可以有很大把握认为其他数据库的执行顺序与下图类似：

## MySQL、PostgreSQL 和 BigQuery 可能的执行顺序

![](img/083618d9b2e8b3c3ff2dfe2af4db2ce8.png)

MySQL SELECT 语句执行顺序（作者提供的图片）

## 结论

这篇文章简短地介绍了 MySQL、GoogleSQL 和其他数据库 SQL 语法的执行顺序，与 SQL Server 的区别，基于观察到的行为和文档。SQL Server 强调在 GROUP BY 子句中显式声明以提高代码清晰度，而 MySQL 的执行顺序明确将 SELECT 子句评估在 GROUP BY 子句之前，允许我们引用其中的序号位置。

欢迎分享您对这个主题的想法，并期待在下一篇中与您见面。

> 您可以[成为 Medium 会员](https://medium.com/@tobisam/membership)来支持我，享受更多类似的故事。

## 参考资料

+   [SELECT — Transact-SQL](https://learn.microsoft.com/en-us/sql/t-sql/queries/select-transact-sql?view=sql-server-ver16&redirectedfrom=MSDN#logical-processing-order-of-the-select-statement)

+   [ROWNUM](https://blogs.oracle.com/connect/post/on-rownum-and-limiting-results) — Oracle

+   [MySQL 参考文档](https://dev.mysql.com/doc/refman/8.0/en/select.html)

+   [PostgreSQL](https://www.postgresql.org/docs/current/using-explain.html) - EXPLAIN
