# 构建高性能实时数据模型指南

> 原文：[`towardsdatascience.com/a-guide-to-building-performant-real-time-data-models-d60b37bb07dc`](https://towardsdatascience.com/a-guide-to-building-performant-real-time-data-models-d60b37bb07dc)

[](https://medium.com/@marietruong?source=post_page-----d60b37bb07dc--------------------------------)![Marie Truong](https://medium.com/@marietruong?source=post_page-----d60b37bb07dc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d60b37bb07dc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d60b37bb07dc--------------------------------) [Marie Truong](https://medium.com/@marietruong?source=post_page-----d60b37bb07dc--------------------------------)

·发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d60b37bb07dc--------------------------------) ·阅读时间 7 分钟·2023 年 8 月 12 日

--

![](img/3861b61d9bafd196194be9f60407c51b.png)

照片由[Lukas Blazek](https://unsplash.com/@goumbik?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

数据已成为决策的重要工具。为了便于操作，数据需要经过清洗、转换和建模。

这个过程通常是 ELT 管道的一部分，按给定频率运行，例如每天。

另一方面，为了快速调整和做出决策，利益相关者有时需要访问最新数据，以便能够迅速做出反应。

例如，如果网站用户数量大幅下降，他们需要迅速了解此问题，并获得必要的信息以理解问题。

第一次被要求构建一个实时数据仪表盘时，我直接将其连接到实时的原始表，并提供了一些简单的 KPI，比如用户数量和崩溃次数。对于月度图表和更深入的分析，我创建了另一个仪表盘，连接到我们的数据模型，每天更新一次。

这种策略并不理想：我在数据仓库和 BI 工具之间重复逻辑，因此维护起来更困难。此外，实时仪表盘只能在几天的数据下表现良好，因此利益相关者不得不切换到历史仪表盘以查看早期日期。

我知道我们必须对此采取措施。我们需要实时数据模型，同时不影响性能。

在这篇文章中，我们将探讨构建实时模型的不同解决方案及其优缺点。

# 视图

SQL 视图是一个虚拟表，包含查询结果。与表不同，视图不存储数据。它们由一个每次有人查询视图时都会执行的查询定义。

这是一个视图定义的示例：

```py
CREATE VIEW orders_aggregated AS (
  SELECT 
    order_date, 
    COUNT(DISTINCT order_id) AS orders,
    COUNT(DISTINCT customer_id) AS customers
  FROM orders
  GROUP BY order_date
 )
```

即使在表中添加了新行，视图也会保持最新。然而，如果表很大，视图可能会变得非常慢，因为没有数据被存储。

如果你在一个小项目中工作，它们应该是首选。

如果逻辑复杂，包含连接和窗口函数，你可能会遇到仪表板加载时间极长的问题。

## 优点

✅ 它们确实很容易设置

✅ 它们始终保持最新

## 缺点

❌ 对大数据量或复杂计算的性能较差

# 频繁刷新的表

如果你的数据需要非常近期但不完全是实时的，一个好的解决方案是非常频繁地刷新表。

下面是如何定义一个查询，以便每两小时刷新一次表：

```py
DELETE FROM orders_aggregated 
WHERE order_date BETWEEN "2023-07-17 08:00:00" AND "2023-07-17 08:30:00";
INSERT INTO orders_aggregated (
  SELECT 
    order_date, 
    COUNT(DISTINCT order_id) AS orders,
    COUNT(DISTINCT customer_id) AS customers
  FROM orders
  WHERE order_date BETWEEN "2023-07-17 08:00:00" AND "2023-07-17 08:30:00"
  GROUP BY order_date
 )
```

在 Scopely，我们有一些每半小时刷新一次的模型。这些模型具有很好的性能，并提供当天的信息。这个话题的重要性不需要数据来自最近的 30 分钟。

我们在流水线中增加了一些复杂性：它运行得很频繁，有时会失败。当我们每日的流水线失败时，我们只是手动重新运行。但对于一个一天运行 48 次的模型来说，这将是噩梦。因此，我们添加了一段额外的代码，以确保如果一次运行失败，下一次运行会整合前一次运行的数据。

## 优点

✅ 它们能实现非常好的性能

## 缺点

❌ 数据不是完全实时的

❌ 流水线运行频繁，需要密切监视

# 物化视图

大多数现代云数据仓库都有一个称为物化视图的对象。物化视图是一个将查询结果存储在物理表中的对象。

在某些数据仓库中，物化视图需要通过触发器刷新，而在其他如 BigQuery 的数据仓库中，它们可以在新增行时自动刷新。这确保了数据的良好质量，因为增量逻辑由仓库本身处理。

使用物化视图的缺点是它们附带了许多限制。

如果我们尝试使用之前的相同查询来构建 BigQuery 中的物化视图：

```py
CREATE MATERIALIZED VIEW orders_aggregated AS (
  SELECT 
    order_date, 
    COUNT(DISTINCT order_id) AS orders,
    COUNT(DISTINCT customer_id) AS customers
  FROM orders
  GROUP BY order_date
 )
```

我们遇到了一个错误：

```py
Incremental materialized views do not support the DISTINCT clause for aggregation function 'count'
```

物化视图通常支持受限的语法。相反，我们可以使用函数 APPROX_COUNT_DISTINCT：

```py
CREATE MATERIALIZED VIEW orders_aggregated AS (
  SELECT 
    order_date, 
    APPROX_COUNT_DISTINCT(order_id) AS orders,
    APPROX_COUNT_DISTINCT(customer_id) AS customers
  FROM orders
  GROUP BY order_date
 )
```

## 优点

✅ 它们结合了表的性能和视图的简单性

✅ 无需设计增量逻辑

## 缺点

❌ 允许的语法极其有限

# Lambda 视图

这个想法是在我们意识到逻辑非常简单时产生的：我们使用视图来处理实时数据，使用表来处理历史数据。那么，为什么不能使用一个**UNION ALL**视图来组合这两者呢？

经过一点研究，我们发现这个概念已经有了名字：lambda 架构。

Lambda 架构是一个结合批处理（增量表部分）和流处理（视图部分）的系统。

我非常热情，尝试基于我的**orders_aggregated**表构建一个 lambda 视图。我只是简单地在日期上使用一个筛选器，并每天更新该视图，将筛选器的值更改为当前日期。

```py
CREATE VIEW orders_aggregated_lv1 AS (
  -- Batch layer
  SELECT 
    *
  FROM orders_aggregated
  WHERE DATETIME(order_date) < "2023-07-17"

  UNION ALL 
  -- Stream layer
  SELECT 
    order_date, 
    COUNT (DISTINCT order_id) AS orders,
    COUNT(DISTINCT customer_id) AS customers
  FROM orders
  WHERE DATETIME(order_date) >= "2023-07-17"
  GROUP BY order_date

 )
```

```py
SELECT *
FROM orders_aggregated_lv1
WHERE DATETIME(order_date) = "2023-07-15"
```

但执行时间非常令人失望。查看图表后，我明白了原因：BigQuery 的计划器仍在重新计算整个视图，尽管它只应该查看表。

![](img/4d87abc52231434405d4518080935e5d.png)

作者提供的图像

与其从 WHERE 筛选器中知道它不需要查看视图，不如查看整个视图中**order_date**列的值。

所以我使用了一个小的变通方法，并为视图硬编码了日期值：

```py
CREATE  VIEW orders_aggregated_lv2 AS (
  -- Batch layer
  SELECT 
    *
  FROM orders_aggregated
  WHERE DATETIME(order_date) < "2023-07-17"

  UNION ALL 
  -- Stream layer
  SELECT 
    DATE("2023-07-17") as order_date, 
    COUNT (DISTINCT order_id) AS orders,
    COUNT(DISTINCT customer_id) AS customers
  FROM orders
  WHERE DATETIME(order_date) = "2023-07-17"
  GROUP BY order_date
 )
```

```py
SELECT *
FROM orders_aggregated_lv2
WHERE DATETIME(order_date) = "2023-07-15"
```

这一次，执行时间非常快，计划器只读取了表中的数据！

![](img/00e5a663a84d08a733c289ddedd995eb.png)

作者提供的图像

然而，我们担心如果查询没有在正午夜运行，或者我们的每日管道失败，我们可能会遗漏部分数据。

所以我们添加了一天的边际时间：

```py
CREATE VIEW orders_aggregated_lv3 AS (
  -- Batch layer
  SELECT 
    *
  FROM orders_aggregated
  WHERE DATETIME(order_date) < "2023-07-17"

  UNION ALL 
  -- Stream layer
  SELECT 
    DATE("2023-07-17") as order_date, 
    COUNT (DISTINCT order_id) AS orders,
    COUNT(DISTINCT customer_id) AS customers
  FROM orders
  WHERE DATETIME(order_date) = "2023-07-17"
  GROUP BY order_date

    UNION ALL 
  -- Stream layer
  SELECT 
    DATE("2023-07-18") as order_date, 
    COUNT (DISTINCT order_id) AS orders,
    COUNT(DISTINCT customer_id) AS customers
  FROM orders
  WHERE DATETIME(order_date) = "2023-07-18"
  GROUP BY order_date

 )
```

如果我们的数据管道失败而我们没有立即修复，我们仍然拥有数据，只是性能略有下降。

由于我们使用的是**数据构建工具**，我们能够用 Jinja2 语法编写那一迭代逻辑，避免了重复。

## 优点

✅ Lambda 视图表现出色

✅ 可以定义几天的边际时间，以确保它们在管道失败时保持最新

## 缺点

❌ 使查询计划器高效的逻辑很复杂

❌ 一个 lambda 视图依赖于至少两个数据库对象

# 哪一个是最佳解决方案？

没有绝对最好的解决方案；这完全取决于你的数据和使用案例。不过，如果你在了解了四个选项后仍然难以决定，这里有一个小的决策树来帮助你做出决定：

![](img/4db526ffa4b8c98161313ef92c51b940.png)

作者提供的图像

# 资源

+   [BigQuery 的物化视图文档](https://cloud.google.com/bigquery/docs/materialized-views-create#supported-mvs)

+   [如何仅使用 dbt + SQL 创建接近实时的模型](https://discourse.getdbt.com/t/how-to-create-near-real-time-models-with-just-dbt-sql/1457)

我希望你喜欢这篇文章！如果你喜欢，请关注我，获取更多关于 Python、SQL 和分析的内容，例如这个关于 ELT 管道的教程：

[](/how-to-build-an-elt-with-python-8f5d9d75a12e?source=post_page-----d60b37bb07dc--------------------------------) ## 如何使用 Python 构建 ELT

### 提取、加载和转换数据

towardsdatascience.com
