# SQL 窗口函数的结构

> 原文：[`towardsdatascience.com/anatomy-of-sql-window-functions-7256d8cf509a`](https://towardsdatascience.com/anatomy-of-sql-window-functions-7256d8cf509a)

## 回到基础 | SQL 初学者基础

[](https://iffatm.medium.com/?source=post_page-----7256d8cf509a--------------------------------)![Iffat Malik](https://iffatm.medium.com/?source=post_page-----7256d8cf509a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7256d8cf509a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7256d8cf509a--------------------------------) [Iffat Malik](https://iffatm.medium.com/?source=post_page-----7256d8cf509a--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7256d8cf509a--------------------------------) ·阅读时间 9 分钟·2023 年 3 月 30 日

--

![](img/cb45c6657e13eb4af98d3e3c9d64e15e.png)

作者提供的图片，创建于[canva](https://www.canva.com)

为了理解企业数据，你必须进行大量查询。当我说“大量”时，我是认真的。处理陌生的数据集常常让人感到畏惧，花时间探索和理解数据本身总是一个好习惯。掌握基本的数据检索技能是好的，但了解分析函数以从数据中提取有用的见解则是锦上添花，而且也很有趣！

我来自数据可视化背景，对我而言，不仅理解数据至关重要，还需要找出值得注意的发现，以便向更广泛的团队展示。此外，构建复杂的仪表板通常是一个反复的过程，你需要回到数据源核对数据，而 SQL 窗口函数始终伴随我进行数据分析。

尽管窗口函数对于数据分析非常有用，但仍有一些混淆，人们通常害怕使用它们。在编写 SQL 窗口函数的详细指南时，我意识到它变得过于描述性，但我又不想跳过细节，特别是关于语法和使用的子句。了解构建模块是重要的，对吧？所以，我将尝试在这篇文章中分解窗口函数的构建模块，以便处理和实现时不会感到压倒。

一如既往，我们将使用[**classicmodels**](https://www.mysqltutorial.org/mysql-sample-database.aspx) MySQL 样本数据库进行演示，该数据库包含汽车零售商的商业数据。下面是供参考的 ER 图，

![](img/c8eecf27187711291ef0341d63121b71.png)

作者提供的图片

## 首先，什么是窗口函数？

窗口函数的教科书定义是，

> [窗口函数](https://www.postgresql.org/docs/9.1/tutorial-window.html)在与当前行相关的一组表行上执行计算。

![](img/6c89fab2d61fd11e8d9029890b4fcae4.png)

图片来源: [Masterfile](https://www.masterfile.com)

你认为这个小家伙从窗户里看到什么？是从这个房间或建筑物的窗户外景的部分视图，对吧？这正是窗口函数的作用。它允许你对数据子集进行计算，而不对当前行进行聚合。

## 为什么需要？为什么使用窗口函数？聚合函数哪里不足？

这是来自*PRODUCTS* 表的示例数据，为了演示目的，我将其限制在*PRODUCTLINEs - 飞机*、*船只* 和 *火车*。

```py
--sample data from table PRODUCTS.
SELECT
    *
FROM
    CLASSICMODELS.PRODUCTS
WHERE PRODUCTLINE IN ('Planes','Ships','Trains');
```

![](img/61e40d6525d4e266551d94eb62cdfef5.png)

图片来自作者

现在找出每个*PRODUCTLINE* 的总库存量，

```py
--total quantity in stock for each productline
SELECT 
    PRODUCTLINE,
    SUM(QUANTITYINSTOCK) AS TOTAL_QUNATITY
FROM 
    CLASSICMODELS.PRODUCTS
WHERE PRODUCTLINE IN ('Planes','Ships','Trains')
GROUP BY PRODUCTLINE; 
```

![](img/11041bbf676e50c4a5526c793f1d6d57.png)

图片来自作者

这很简单。*SUM(QUANTITYINSTOCK)* 将来自多行的数据总结为 3 行，因为我们使用了*GROUP BY* 子句，它为每个产品线提供了一行。很好！这正是预期的结果。

![](img/7d95aec1dafadd042a3a9cfde8cd8f46.png)

图片来自作者

现在我们来重新审视需求，

1.  显示每个*PRODUCTLINE* 中每种产品的数量，以及该特定*PRODUCTLINE* 的总库存量。

1.  按*PRODUCTLINE* 分组排列结果集。

那么这可以通过聚合函数来完成吗？我们确实想要每个*PRODUCTLINE* 的总库存量，但我们不希望将所有数据汇总。这就是窗口函数的用武之地，

```py
--sum() as a window function
SELECT 
    PRODUCTNAME,
    PRODUCTLINE,
    QUANTITYINSTOCK,
    SUM(QUANTITYINSTOCK) OVER (PARTITION BY PRODUCTLINE) AS TOTAL_QUANTITY
FROM
    CLASSICMODELS.PRODUCTS
WHERE PRODUCTLINE IN ('Planes','Ships','Trains');
```

![](img/bd67da55ea2f89167ec341134c4353bf.png)

图片来自作者

在这里，*SUM()* 作为窗口函数对一组行进行了计算，但与聚合函数不同的是，它不会将结果集总结为一行。相反，所有的行（*PRODUCTNAME, PRODUCTLINE* 和 *QUANTITYINSTOCK*）保持其原始形式/身份，并且计算的行（*TOTAL_QUANTITY*）在每行的结果集中添加。

![](img/8071ad99b79e89cf4af44ca4e479ca99.png)

图片来自作者

## 窗口函数的类型

说实话，窗口函数没有官方分类，但根据使用情况，我们可以大致分为 3 种方式，

![](img/7b2246679c08af9fb929adf6d2aaa803.png)

图片来自作者

+   **聚合函数** - 常规的聚合函数可以用作窗口函数，以计算窗口分区内的数值列的聚合，如运行总销售额、分区内的最小值或最大值等。

+   **排名函数** - 这些函数为分区中的每一行返回一个排名值。

+   **值函数** - 这些函数对于生成简单的统计数据或时间序列分析非常有用。

## 窗口函数的语法

窗口函数的常见语法是，

![](img/7231354dacbfef3c29dd5f72b36ffd52.png)

图片来自作者

在深入之前，我们首先要理解其中每个子句的重要性，

## OVER() 子句

*OVER()* 子句指定了一个窗口函数，因此它必须始终包含在语句中。它定义了一个用户指定的行子集（窗口），窗口函数将在其上应用。如果你在 *OVER()* 内不提供任何内容，窗口函数将应用于整个结果集。

继续上面的示例，

```py
--empty OVER() clause
SELECT 
    PRODUCTNAME,
    PRODUCTLINE,
    QUANTITYINSTOCK,
    SUM(QUANTITYINSTOCK) OVER () AS TOTAL_QUANTITY
FROM
    CLASSICMODELS.PRODUCTS
WHERE PRODUCTLINE IN ('Planes','Ships','Trains');
```

![](img/d74f9f4fa90c0a31e3ff7e24bcd1aa5b.png)

图片来源于作者

在这里，由于我们提供了一个空的 *OVER()* 子句，窗口函数 *SUM(QUANTITYINSTOCK)* 被应用于 *PRODUCTLINE-* *飞机*、*船只* 和 *火车* 的所有记录，结果是 *TOTAL_QUANTITY* 为 105816。

## PARTITION BY 子句

*PARTITION BY* 与 *OVER* 子句一起使用。它根据用户指定的表达式将查询结果集分成分区或桶，然后窗口函数应用于每个分区或桶。

这是可选的，如果你不指定 *PARTITION BY* 子句，那么函数将所有行视为一个单一的分区。正如我们在上面的示例中所做的那样，我们仅指定了一个空的 *OVER()* 子句而没有 *PARTITION BY* 子句，因此总量是针对所有 *PRODUCTLINE* 计算的。

如果我们指定了一个会发生什么，

```py
--OVER() with PARTITION BY
SELECT 
    PRODUCTNAME,
    PRODUCTLINE,
    QUANTITYINSTOCK,
    SUM(QUANTITYINSTOCK) OVER (PARTITION BY PRODUCTLINE) AS TOTAL_QUANTITY
FROM
    CLASSICMODELS.PRODUCTS
WHERE PRODUCTLINE IN ('Planes','Ships','Trains');
```

![](img/2b43c6994f508b1ce2f5a98750af1a81.png)

图片来源于作者

由于我们要求 *OVER()* 子句基于 *PRODUCTLINE* 对 *TOTAL_QUANTITY* 结果集进行分区，现在 *SUM(QUANTITYINSTOCK)* 将计算每个分区的库存数量。

那么，*PARTITION BY* 和 *GROUP BY* 子句有什么关系？它们是相似的还是不同的？

***GROUP BY* 子句,**

+   它将多行根据一个或多个列/表达式分组为汇总行（每组返回 1 行）。简单来说，它减少了结果集中的行数。

+   它与 SUM()、AVG()、MAX() 等聚合函数一起使用。

+   它位于 *WHERE* 子句之后，但在 *HAVING, ORDER BY* 子句之前。常见语法是，

![](img/dc9b0ef034c60a5544de757233b80705.png)

图片来源于作者

***PARTITION BY* 子句,**

+   它与窗口函数中的 *OVER()* 子句一起使用。它将查询结果集划分为分区，然后窗口函数应用于每个分区。

+   *PARTITION BY* 类似于 *GROUP BY*，因为它基于表达式对结果进行聚合；然而，主要区别在于，它不会减少结果集的行数。

+   这是可选的，如果你不指定 *PARTITION BY* 子句，那么函数将所有行视为一个单一的分区。

+   常见语法是，

![](img/c17211dca2471d1fe6fa023ded8ac7b6.png)

图片来源于作者

如果我们需要找出每个 PRODUCTLINE 的 MSRP 的最小值和最大值，那么对于 GROUP BY 和 PARTITION BY 子句，结果集会有什么不同，

![](img/892742f271023e4c70a9a45996289c6e.png)

图片来源于作者

## ORDER BY 子句

它用于在结果集的每个分区内按升序或降序排序结果集。默认情况下，它是升序的。

## ROWS/RANGE 子句

现在我们已经知道窗口函数的关键特性是使用*PARTITION BY*子句创建结果集的窗口或分区，然后在每个分区上执行计算。如果我们进一步想在这些分区内创建子集呢？哇！分区内的分区？是的，这就是我们需要*FRAME*子句的原因。

*FRAME*子句进一步定义了当前分区的一个子集。它使用*ROW*或*RANGE*来定义这个子集的起点和终点。它需要*ORDER BY*子句。

框架是相对于当前行来确定的，这意味着你将当前行的位置作为基准点，并以此为参考定义你在分区内的框架。

+   *ROWS* — 通过指定当前行之前或之后的行数来定义框架的开始和结束。

+   *RANGE* — 与*ROWS*相对，*RANGE*指定与当前行值相比的值范围，以在分区内定义框架。

通用语法是，

> **{ROWS | RANGE} BETWEEN <frame_starting> AND <frame_ending>**

![](img/3849aa0e5961ada988e8b08e89b4b5b5.png)

作者提供的图片

在进一步讨论之前，让我们先了解一些定义窗口的基本术语。

![](img/388516b7b3eee5b99234ba06166d2117.png)

图片来源：[mysqltutorials](https://www.mysqltutorial.org)

+   *UNBOUNDED PRECEDING* - 这指定了分区中当前行之前的所有行（从第一行开始）。

+   *N PRECEDING* - 这指定了分区中当前行之前的‘N’行数。

+   *UNBOUNDED FOLLOWING* - 这指定了分区中当前行之后的所有行（一直到最后一行）。

+   *M FOLLOWING* - 这指定了分区中当前行下方的‘M’行数。

让我们通过一个例子快速理解一下，

```py
SELECT 
    PRODUCTNAME,
    PRODUCTLINE,
    QUANTITYINSTOCK,
    SUM(QUANTITYINSTOCK) OVER (PARTITION BY PRODUCTLINE ROWS BETWEEN 1 PRECEDING AND CURRENT ROW) AS TOTAL
FROM
    CLASSICMODELS.PRODUCTS
WHERE PRODUCTLINE IN ('Planes','Ships','Trains');
```

![](img/93267b9084ee0c5dc812607836e8ab52.png)

作者提供的图片

***ROWS BETWEEN 1 PRECEDING AND CURRENT ROW*** 表示在分区内需要计算*SUM(QUANTITYINSTOCK)*的框架大小。

![](img/1b7de0a4150d4228d546a8e58868152f.png)

作者提供的图片

这里有一些*FRAME*子句的示例，

+   *ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING* — 这意味着考虑从分区的第一行到分区的最后一行的框架。

+   *ROWS BETWEEN UNBOUNDED PRECEDING AND 4 FOLLOWING* - 这意味着考虑从分区的第一行到当前行之后的 4 行的框架。

+   *ROWS BETWEEN 4 PRECEDING AND 1 PRECEDING* - 窗口将是当前行之前的 4 行到当前行之前的 1 行。

默认框架根据是否存在*ORDER BY*子句而有所不同；如果存在，默认框架是，

> *{ROWS/RANGE} BETWEEN* **UNBOUNDED PRECEDING** *AND* **CURRENT ROW**

这意味着将窗口范围视为从第一行到当前行的所有行。

如果没有*ORDER BY*子句，则默认的窗口范围为，

> *{ROWS/RANGE}* BETWEEN **UNBOUNDED PRECEDING** AND **UNBOUNDED FOLLOWING**

这意味着整个分区。

## 定义窗口别名，

如果查询中有多个窗口函数使用相同的窗口，则可能需要使用窗口别名。

```py
--finding out minimum and maximum MSRP for each productline
SELECT
    PRODUCTNAME,
    PRODUCTLINE,
    MSRP,
    MIN(MSRP) OVER(PARTITION BY PRODUCTLINE) AS MIN_MSRP,
    MAX(MSRP) OVER(PARTITION BY PRODUCTLINE) AS MAX_MSRP
FROM
    CLASSICMODELS.PRODUCTS
WHERE PRODUCTLINE IN ('Planes','Ships','Trains');
```

使用窗口别名编写相同查询的另一种方式是，

```py
--using window alias
SELECT
    PRODUCTNAME,
    PRODUCTLINE,
    MSRP,
    MIN(MSRP) OVER MSRP_WINDOW AS MIN_MSRP,
    MAX(MSRP) OVER MSRP_WINDOW AS MAX_MSRP
FROM
    CLASSICMODELS.PRODUCTS
WHERE PRODUCTLINE IN ('Planes','Ships','Trains')
WINDOW MSRP_WINDOW AS (PARTITION BY PRODUCTLINE);
```

📌 **附注**

在查询执行过程中，窗口函数会在结果集中执行，

+   **在** *JOIN*、*WHERE*、*GROUP BY* 和 *HAVING* 子句之后，

+   **在** *ORDER BY* 子句、*LIMIT* 和 *SELECT DISTINCT* 之前。

## 结论

你可能希望从 100 种不同的方式中探索数据，而窗口函数正适合这种分析。本文只是了解基本语法和子句的入门，以便窗口函数不会显得过于复杂，通过练习，肯定会更好。

这是最常用窗口函数的详细指南，

[窗口函数 — 数据工程师和数据科学家必知](https://www.example.org/window-functions-a-must-know-for-data-engineers-and-data-scientists-4dd3e4ad0d2?source=post_page-----7256d8cf509a--------------------------------)

### 回归基础 | 初学者的 SQL 基础

[towardsdatascience.com](https://www.towardsdatascience.com/window-functions-a-must-know-for-data-engineers-and-data-scientists-4dd3e4ad0d2?source=post_page-----7256d8cf509a--------------------------------)

一些其他有用的资源，

+   [SQL 窗口函数速查表](https://learnsql.com/blog/sql-window-functions-cheat-sheet/)

+   [窗口子句](https://dev.mysql.com/doc/refman/8.0/en/window-functions-frames.html)

+   [HackerRank](https://www.hackerrank.com) 或 [LeetCode](https://leetcode.com) 来练习基础/中级/高级 SQL 问题。

学习愉快！
