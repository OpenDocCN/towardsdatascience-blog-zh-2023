# 晋级下一轮所需的前三种 SQL 技能

> 原文：[`towardsdatascience.com/the-top-3-sql-skills-needed-to-get-to-the-next-round-51ad1699a213`](https://towardsdatascience.com/the-top-3-sql-skills-needed-to-get-to-the-next-round-51ad1699a213)

## 数据专业人士的技术面试帮助

[](https://medium.com/@violante.andre?source=post_page-----51ad1699a213--------------------------------)![Andre Violante](https://medium.com/@violante.andre?source=post_page-----51ad1699a213--------------------------------)[](https://towardsdatascience.com/?source=post_page-----51ad1699a213--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----51ad1699a213--------------------------------) [Andre Violante](https://medium.com/@violante.andre?source=post_page-----51ad1699a213--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----51ad1699a213--------------------------------) ·阅读时间 6 分钟·2023 年 8 月 28 日

--

![](img/584c4b12a0d4da972d8f858eddd3f5f9.png)

图片来源：[Arnold Francisa at Unsplash](https://unsplash.com/@clark_fransa)

如果你有志于从事数据科学家、数据分析师和数据工程师等角色，并且正在面试，那么你可能会遇到一个或多个需要现场编码的技术面试，通常涉及 SQL。虽然后续面试可能需要其他编程语言，如 Python，这在数据领域很常见，但让我们专注于我在这些面试中遇到的典型 SQL 问题。为了讨论的目的，我会假设你已经熟悉基础的 SQL 概念，如 `SELECT`、`FROM`、`WHERE` 以及聚合函数如 `SUM` 和 `COUNT`。让我们进入具体细节吧！

## 1\. 掌握连接和表类型

毫无疑问，最常见的 SQL 问题是关于表连接的。这个问题可能看起来太显而易见，但我参与过的每一次面试都围绕这个话题展开。你应该对内连接和左连接感到游刃有余。此外，处理自连接和并集的熟练程度也是非常重要的。执行这些连接操作时，特别是在***事实表***和***维度表***等不同表类型之间的能力同样重要。以下是我对这两个术语的宽松定义：

***事实表：*** 一种包含大量行但属性或列相对较少的表。比如，在线零售商维护一个“*订单*”表，包含如下列：`date, customer_id, order_id, product_id, units, amount`。这个表属性较少，但记录量巨大。

***维度表：*** 具有较少行但许多属性的维度表。例如，某在线零售商的“*customer*”表可能包含每个客户的一行，具有`customer_id, first_name, last_name, ship_street_addr, ship_zip_code`等属性。

理解这两种主要表类型是很重要的。掌握如何合并事实表和维度表以确保准确结果是至关重要的。让我们考虑一个实际的例子：面试问题提出了两个表（*“orders”*和*“customer”*）并询问：

> 有多少客户在其一生中至少购买了 3 个单位并且邮政编码为 90210？

仅仅运行内连接后再进行计数、求和和/或过滤可能会由于重复计算产生巨大的差异。即使在这个看似简单的问题中，许多部分也需要仔细思考。为了有效地将你的思路传达给面试官，请将这些事项考虑清楚并说出来：

+   哪个表包含所有客户？*“orders”*事实表仅包括已确认购买的客户，而*“customer”*维度表包括所有客户，包括那些已注册但未购买的客户。通过快速的`count(distinct )`对比，可以看到哪个表有更多独特的客户。

+   计算每个客户购买的单位总数或计数需要使用带有`GROUP BY`子句的聚合函数。

+   需要与*“customer”*表进行连接，以缩小邮政编码为 90210 的客户范围。在使用连接时，我建议为表创建别名，这可能需要你在 select 语句中添加该别名以及相关属性。

概述了思路后，让我们将其转化为代码！

```py
-- cte of sum of units per customer from orders fact table
WITH customer_units_agg as (
    SELECT
        customer_id
        , SUM(units) as UNIT_SUM
    FROM
        orders
    GROUP BY
        customer_id
)

-- join CTE table with customers dim table to filter by units and zip
SELECT
    COUNT(DISTINCT ca.customer_id) as CUSTOMER_CNT
FROM customer_units_agg ca
    INNER JOIN customer ctmr on ca.customer_id = ctmr.customer_id
WHERE 1=1
    AND ca.UNIT_SUM >= 3
    AND ctmr.ship_zip_code = 90210;
```

这个解决方案使用了公共表表达式（CTE），但请记住，有很多不同的方法可以在 SQL 中实现相同的结果。只要你能得到正确的结果并且能够解释你的方法，你就走在了正确的道路上。

## 2\. 使用子查询、临时表和 CTE 处理复杂性

与之前的例子一样，几乎每次 SQL 编码面试都需要多步骤程序。这时候子查询、临时表和 CTE 就非常有用。熟练掌握这些技术或至少对它们有所了解是必须的。让我们深入探讨每种方法：

+   **子查询：**这些嵌套查询涉及像`select * from (select * from table)`或过滤`select * from table1 where table1_value < (select max(table2_value) from table2)`这样的构造，包含内查询和外查询。子查询是有用的，但它们的语法可能对编写者和阅读者都变得困难或混乱。对于复杂场景，避免过多的子查询。

+   **临时表：** 正如其名称所示，临时表仅在会话期间存在。你可以逐步创建它们，一个接一个地构建，以帮助故障排除或逻辑排序。它们有助于将复杂问题分解成较小、更易管理的步骤。

```py
-- mysql and others
CREATE TEMPORARY TABLE my_temp_table as
    SELECT
        column1
        , column2
        , column3
    FROM original_table
    WHERE some_conditions;

-- sql server
SELECT
    column1
    , column2
    , column3
INTO #my_temp_table -- this is the new temp table created
FROM original_table
WHERE some_conditions;
```

+   **公共表表达式（CTE）：** 如连接示例所示，CTE 提供了一种灵活的查询结构方式。它们类似于临时表，但不会在下一个`SELECT`语句之后持续存在。这要求将 CTE“链式”连接（见下方代码示例），如果有很多步骤的话。在链式连接 CTE 时，请记住只使用一个`WITH`语句，后接 CTE，最后是主查询。此外，分号位于 CTE 链中的最终`SELECT`语句之后。由于其有限的作用域，CTE 不能在这一点之外被引用。

```py
-- chaining multiple cte together
WITH cte_tabl_1 as (
    SELECT * FROM table1
),
cte_tabl_2 as (
    SELECT * FROM table2
),
cte_tabl_3 as (
    SELECT * FROM table3
)

select * from cte_tabl_1, cte_tabl_2, cte_tabl_3;
```

实际上，一旦你被聘用，你可以决定最佳的方法或其他人正在使用的方案。目前，CTE 似乎是更受欢迎的选择。鉴于面试情境通常不需要链式 CTE，融入 CTE 应该相对简单且有效。

## 3\. 高级分析中的窗口函数导航

几乎每次面试中都会出现的一个反复话题是窗口函数。 [PostgreSQL 文档](https://www.postgresql.org/docs/9.1/tutorial-window.html) 有效地解释了窗口函数，并突出了与分组操作的区别。

> 窗口函数在一组与当前行相关的表行上执行计算。这类似于可以通过聚合函数完成的计算。但与常规的聚合函数不同，使用窗口函数不会将行分组为单个输出行——行仍然保持各自的独立性。在幕后，窗口函数能够访问的不仅仅是查询结果的当前行。

关键在于最后两句话。行的非分组和窗口函数在幕后访问当前行以上的更多内容。面试问题通常涵盖从理论性问题，例如“你使用过哪些类型的窗口函数？”到需要使用窗口函数的编码问题。考虑诸如“显示每个部门中收入最高的三名员工”或“显示每位客户购买的最新三项商品”等场景。根据我的经验，最常见的窗口函数包括`rank()`、`row_number()`和`dense_rank()`，它们都使用`OVER`函数。值得注意的是，大多数聚合函数可以与`OVER`函数一起使用。有关排名函数差异的良好说明可以参见这个 [Stack Overflow 示例](https://stackoverflow.com/questions/7747327/sql-rank-versus-row-number)。此外，LearnSQL 提供的这个 [窗口函数备忘单](https://learnsql.com/blog/sql-window-functions-cheat-sheet/Window_Functions_Cheat_Sheet_Letter.pdf) 是一个很好的视觉参考，展示了不同类型的函数以及一些输出视觉效果。

## 结论

这些是过去一个月中面试官在编码面试中反复询问的前三大 SQL 技能。掌握这些领域应能轻松帮助你应对几乎所有 SQL 面试场景。

最后，这里是我的最后一个建议：如果你在编码面试中对语法或术语不确定，请写下逐步的大纲来展示你的问题解决过程。强调你的思维过程和创造力，因为它们往往比严格的语法更重要。祝你在即将到来的数据相关面试中好运！
