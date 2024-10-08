# 这 5 种 SQL 技术涵盖了 ~80% 的实际项目

> 原文：[`towardsdatascience.com/5-advanced-sql-techniques-for-real-life-projects-f2db9b6680e2`](https://towardsdatascience.com/5-advanced-sql-techniques-for-real-life-projects-f2db9b6680e2)

## 加快你的 SQL 学习曲线

[](https://thuwarakesh.medium.com/?source=post_page-----f2db9b6680e2--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----f2db9b6680e2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f2db9b6680e2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2db9b6680e2--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----f2db9b6680e2--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2db9b6680e2--------------------------------) ·8 分钟阅读·2023 年 3 月 8 日

--

![](img/9fa3d4a6f9a1bcd3cf96e5b4e65cbdf3.png)

由 [Possessed Photography](https://unsplash.com/es/@possessedphotography?utm_source=medium&utm_medium=referral) 提供的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你是否对 SQL 感到好奇但又犹豫不决？或者你已经熟悉 SQL 的基础知识，但在将其应用于实际项目时遇到困难。

我了解这种感觉。

当我第一次开始学习 SQL 时，我被大量的信息所吓倒。即使今天，我仍然不断学习和探索新技术。

当然，SQL 的基础知识，如连接、子查询、过滤和排序，很容易掌握。但是，对于复杂的实际问题，你需要高级技术。

在这篇文章中，我想分享我在日常工作中最常用的五种高级 SQL 技术。通过掌握这些技术，你将能够完成几乎 80% 的生产级 SQL 查询，使你成为任何数据驱动项目的宝贵资产。

[](/fast-load-data-to-sql-from-python-2d67aea946c0?source=post_page-----f2db9b6680e2--------------------------------) ## Python To SQL — 我现在可以将数据加载速度提高 20 倍

### 上传大量数据的好、坏和丑陋的方法

towardsdatascience.com

我故意没有包括一些其他常用的技术，例如事务。如果你在分析角色中，这份方法列表将非常有用，而不是在软件工程师角色中。

在整篇文章中，我假设我们使用的是 Postgres 数据库。但如今每个主要的关系数据库都提供类似的功能。

## 1. 窗口函数

窗口函数是一种分析函数，它在与当前行相关的一组行中执行计算。窗口函数的结果与原始行一起返回，而不改变底层数据。

[](/junior-developers-write-multi-page-sql-queries-seniors-use-window-functions-f82dfeb8e378?source=post_page-----f2db9b6680e2--------------------------------) ## 初级开发者编写多页 SQL 查询；高级开发者使用窗口函数

### 在记录的上下文中执行计算的一种优雅方法

[towardsdatascience.com

窗口函数的实际例子可能是计算特定产品随时间的销售收入累计总额。这对于识别销售趋势可能很有用，比如某些产品在一年中的某些时间最受欢迎。

这是一个如何使用 `SUM` 窗口函数计算特定产品随时间的销售收入累计总额的例子：

```py
SELECT
  product_id,
  date,
  SUM(revenue) OVER (PARTITION BY product_id ORDER BY date) AS running_total
FROM
  sales
WHERE
  product_id = 123;
```

在这个例子中，`SUM` 窗口函数用于计算特定 `product_id` 随时间的 `revenue` 累计总额。`PARTITION BY` 子句按 `product_id` 对数据进行分组，`ORDER BY` 子句按 `date` 对数据进行排序。`running_total` 列包含 `SUM` 窗口函数的结果。

**为什么不使用** `**group by**`**？**

当我第一次开始使用窗口函数时，这让我感到困惑。

是的，你可以在 PostgreSQL 中使用 `GROUP BY` 来聚合数据。然而，使用 `GROUP BY` 会提供与窗口函数不同的结果。

在计算特定产品随时间的销售收入累计总额的例子中，使用 `GROUP BY` 会按 `product_id` 和 `date` 对销售数据进行分组，然后计算每个组的收入总和。这将给你每一天的总收入，而不是随时间的累计收入。

这是一个使用 `GROUP BY` 按 `product_id` 和 `date` 聚合销售数据的例子：

```py
SELECT
  product_id,
  date,
  SUM(revenue) AS total_revenue
FROM
  sales
GROUP BY
  product_id,
  date
WHERE
  product_id = 123;
```

[了解更多关于 SQL 窗口函数的信息](https://mode.com/sql-tutorial/sql-window-functions/)

## 2\. CTE: 公共表表达式

CTE 是一个可以在单个 SQL 语句中引用的临时命名结果集。它定义了一个可以在庞大的查询中多次引用的子查询，从而简化了复杂查询。

让我们通过一个例子来更好地理解这一点。假设你有一个包含所有客户订单的表。你想找到各个产品在表现最佳的地区的销售情况。

没有 CTE，你必须编写复杂的查询，涉及子查询、连接和聚合函数。这可能使查询难以阅读和理解。然而，使用 CTE 可以简化查询，使其更易读。

这是一个经典例子，用于理解 CTE 的有用性。

```py
WITH regional_sales AS (
    SELECT region, SUM(amount) AS total_sales
    FROM orders
    GROUP BY region
), top_regions AS (
    SELECT region
    FROM regional_sales
    WHERE total_sales > (SELECT SUM(total_sales)/10 FROM regional_sales)
)
SELECT region,
       product,
       SUM(quantity) AS product_units,
       SUM(amount) AS product_sales
FROM orders
WHERE region IN (SELECT region FROM top_regions)
GROUP BY region, product;
```

在上面的查询中，我们使用了两个 CTE 来计算顶级销售区域中的畅销产品。

第一个 CTE，`regional_sales`，通过对`orders`表的`amount`列求和并按`region`分组，计算每个区域的总销售额。

第二个 CTE，`top_regions`，仅选择那些总销售额超过所有区域总销售额 10%的区域。这是通过一个子查询来计算所有区域的总销售额并将其除以 10 实现的。

主查询然后使用`IN`子句将`orders`表与`top_regions` CTE 连接，以仅包含来自顶级销售区域的订单。

这是没有使用 CTE 的重写查询：

```py
SELECT region,
       product,
       SUM(quantity) AS product_units,
       SUM(amount) AS product_sales
FROM orders
WHERE region IN (
    SELECT region
    FROM (
        SELECT region, SUM(amount) AS total_sales
        FROM orders
        GROUP BY region
    ) regional_sales
    WHERE total_sales > (
        SELECT SUM(total_sales)/10
        FROM (
            SELECT region, SUM(amount) AS total_sales
            FROM orders
            GROUP BY region
        ) regional_sales_sum
    )
)
GROUP BY region, product;
```

CTE 可以简化复杂的查询，使其更具可读性。它使在更大查询中多次重用相同的子查询变得更容易。

[了解更多关于 CTE 的信息](https://learnsql.com/blog/what-is-common-table-expression/)

## 3\. 递归查询

你是否曾经想从一个数据以层次结构或树状结构存储的数据库中检索数据？

例如，你可能有一个产品类别树，其中每个类别都有子类别，每个子类别还可以有进一步的子类别。在这种情况下，递归查询非常有用。

递归查询是指在定义中引用自身的查询。当遍历数据库中的树状或层次结构并检索所有相关数据时，它很有用。换句话说，它使你能够从一个依赖于同一表中数据的表中选择数据。

这是一个用于遍历类别树的递归查询示例：

```py
WITH RECURSIVE category_tree(id, name, parent_id, depth, path) AS (
  SELECT id, name, parent_id, 1, ARRAY[id]
  FROM categories
  WHERE parent_id IS NULL
  UNION ALL
  SELECT categories.id, categories.name, categories.parent_id, category_tree.depth + 1, path || categories.id
  FROM categories
  JOIN category_tree ON categories.parent_id = category_tree.id
)
SELECT id, name, parent_id, depth, path
FROM category_tree;
```

我们在这个例子中使用了带有`WITH`子句的 CTE 来定义递归查询。`RECURSIVE`关键字告诉 Postgres 这是一个递归查询。

`category_tree` CTE 由两个 SELECT 语句定义。第一个 SELECT 语句选择类别树的根节点（没有父节点的节点），第二个 SELECT 语句递归选择子节点。`UNION ALL`操作符结合了两个 SELECT 语句的结果。

`depth`列用于跟踪树中每个类别节点的深度。`path`列是一个数组，用于存储从根节点到当前节点的路径。

使用这个查询，我们可以检索树中所有类别及其各自的深度和路径。

[了解更多关于递归查询的信息](https://learnsql.com/blog/sql-recursive-cte/)

## 4\. 动态 SQL

如果你曾经使用过 SQL 查询，你可能遇到过一些非常复杂的查询，需要在运行时生成。编写这些查询可能令人望而却步，而执行它们则可能更加具有挑战性。

过去，我曾依靠 Python 生成复杂的 SQL 查询，并使用像 psycopg2 这样的数据库连接器执行它们。这种方法有效但不太优雅。

然而，我最近发现了 Postgres 中的动态 SQL，这使得生成和执行复杂查询变得更加可控。使用动态 SQL，你可以根据运行时条件动态创建查询，这在处理复杂的数据结构或业务逻辑时非常有用。

假设你想检索在特定日期下的所有订单。在静态查询中，你可能会写成这样：

```py
SELECT * FROM orders WHERE order_date = '2022-03-01';
```

但如果你允许用户选择一个日期范围呢？使用动态 SQL，你可以根据用户输入生成查询，如下所示：

```py
EXEC SQL BEGIN DECLARE SECTION;
const char *stmt = "SELECT * FROM orders WHERE order_date BETWEEN ? AND ?";
DATE start_date, end_date;
EXEC SQL END DECLARE SECTION;

EXEC SQL PREPARE mystmt FROM :stmt;

EXEC SQL EXECUTE mystmt USING :start_date, :end_date;
```

在这个例子中，我们创建了一个函数，该函数接受两个参数，`start_date` 和 `end_date`，并返回一个在该日期范围内的订单表。`EXECUTE` 语句允许我们根据输入参数动态生成查询，而 `USING` 子句则指定了查询的值。

这个例子很简单。但是在大规模项目中，你会需要大量动态生成的 SQL。

[了解更多关于动态 SQL 的信息](https://www.postgresql.org/docs/current/ecpg-dynamic.html)。

## 5\. 游标

我们的查询可能会在受限环境中运行。一次性对一个庞大的表进行密集操作可能并不总是最佳选择。或者我们可能需要对操作有更多的控制，而不是将其应用于整个表。

这就是游标派上用场的地方。游标允许你逐行检索和操作结果集中的数据。你可以使用游标遍历数据集，并对每一行执行复杂的操作。

假设你有一个名为“products”的表，其中包含所有产品的信息，包括产品 ID、产品名称和当前库存。你可以使用游标遍历所有包含特定产品的订单，并更新其库存。

```py
DECLARE
    cur_orders CURSOR FOR 
        SELECT order_id, product_id, quantity
        FROM order_details
        WHERE product_id = 456;

    product_inventory INTEGER;
BEGIN
    OPEN cur_orders;
    LOOP
        FETCH cur_orders INTO order_id, product_id, quantity;
        EXIT WHEN NOT FOUND;
        SELECT inventory INTO product_inventory FROM products WHERE product_id = 456;
        product_inventory := product_inventory - quantity;
        UPDATE products SET inventory = product_inventory WHERE product_id = 456;
    END LOOP;
    CLOSE cur_orders;

    -- do something after updating the inventory, such as logging the changes
END;
```

在这个例子中，我们首先声明了一个名为“cur_orders”的游标，选择所有包含特定产品 ID 的订单详细信息。然后我们定义了一个名为“product_inventory”的变量，用于存储该产品的当前库存。

在循环中，我们从游标中获取每个订单 ID、产品 ID 和数量，从当前库存中减去数量，并使用新的库存值更新产品表。

最后，我们关闭游标，并在更新库存后进行其他操作，例如记录更改。

## 结论

总之，SQL 是一种强大的语言，提供了许多处理复杂数据的技术。但是，初学时掌握所有这些技术可能会让你感到不知所措。

这篇博客文章探讨了五种最常用的高级 SQL 技术，包括 CTE、窗口函数、递归查询、动态查询和游标。虽然像联接和子查询这样的基本 SQL 概念对于处理数据是基础，但这些技术将帮助你处理几乎任何 SQL 项目。

虽然这篇文章提供了高级 SQL 技术的概述，但并不打算对每种技术进行详尽讨论。有关的链接已经提供，供那些希望深入探讨这些概念的人使用。未来，我计划对每种技术进行更深入的探讨，提供更全面的理解其能力和潜在应用。

> 感谢阅读，朋友！如果您喜欢我的文章，让我们在 [**LinkedIn**](https://www.linkedin.com/in/thuwarakesh/)、[**Twitter**](https://twitter.com/Thuwarakesh) 和 [**Medium**](https://thuwarakesh.medium.com/) 保持联系。
> 
> 还不是 Medium 会员？请使用这个链接[**成为会员**](https://thuwarakesh.medium.com/membership)，因为在没有额外费用的情况下，我会因推荐您而获得一小部分佣金。
