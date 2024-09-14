# 初级开发者编写多页 SQL 查询；高级开发者使用窗口函数

> 原文：[`towardsdatascience.com/junior-developers-write-multi-page-sql-queries-seniors-use-window-functions-f82dfeb8e378`](https://towardsdatascience.com/junior-developers-write-multi-page-sql-queries-seniors-use-window-functions-f82dfeb8e378)

## 在记录的上下文中执行计算的一种优雅方法

[](https://thuwarakesh.medium.com/?source=post_page-----f82dfeb8e378--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----f82dfeb8e378--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f82dfeb8e378--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f82dfeb8e378--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----f82dfeb8e378--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f82dfeb8e378--------------------------------) ·阅读时间 7 分钟·2023 年 3 月 13 日

--

![](img/5ecd316079b782495a6f38876a60a489.png)

图片由 [R Mo](https://unsplash.com/@mooo3721?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

想象一下你经营一个全国性的电子商店。你必须确定每个商店对州销售额的贡献。

如果你刚开始学习 SQL，你可能会考虑一下，开发一个极其出色的查询，可能看起来像下面这样。

```py
SELECT
 s.state,
 s.store_name,
 s.total_sales,
 s.total_sales / t.state_total_sales AS sales_contribution
FROM
 (
 SELECT
  state,
  store_name,
  SUM(sales) AS total_sales
 FROM
  store_sales
 GROUP BY
  state,
  store_name
) s
INNER JOIN (
 SELECT
  state,
  SUM(sales) AS state_total_sales
 FROM
  store_sales
 GROUP BY
  state
) t ON
 s.state = t.state
ORDER BY
 s.state,
 sales_contribution DESC;
```

```py
|state|store_name|total_sales|sales_contribution|
|-----|----------|-----------|------------------|
|California|MegaMart|82500.00|0.61797752808988764045|
|California|ABC Mart|51000.00|0.38202247191011235955|
|New York|SuperMart|57000.00|0.60638297872340425532|
|New York|Corner Shop|37000.00|0.39361702127659574468|
|Texas|XYZ Store|35000.00|0.67961165048543689320|
|Texas|My Store|16500.00|0.32038834951456310680|
```

这肯定能完成工作。但如果你把这个解释给别人，他们会跟你过得很痛苦。

相反，如果你让一位高级开发者来做同样的事情，他们会写出如下的查询：

```py
SELECT
 state,
 store_name,
 SUM(sales) AS total_sales,
 SUM(sales) / SUM(SUM(sales)) OVER (PARTITION BY state) AS sales_contribution
FROM
 store_sales
GROUP BY
 state,
 store_name
ORDER BY
 state,
 sales_contribution DESC;
```

这个更加简洁的版本使用了窗口函数。

窗口函数创建一个与当前记录相关的记录窗口，并在该窗口内进行操作。在这个例子中，窗口是状态。

我第一次遇到窗口函数是在几年前，当时我为一个零售客户工作。他们的客户数据集非常庞大，他们希望查看每个顾客群体的购买模式变化——比如 80 年代和 90 年代出生的人群等。

[](/fast-load-data-to-sql-from-python-2d67aea946c0?source=post_page-----f82dfeb8e378--------------------------------) ## Python 到 SQL — 我现在可以快 20 倍地加载数据

### 上传大量数据的好坏丑方法

towardsdatascience.com [](/5-advanced-sql-techniques-for-real-life-projects-f2db9b6680e2?source=post_page-----f82dfeb8e378--------------------------------) ## 这 5 种 SQL 技巧涵盖了大约 80%的实际项目

### 加速你的 SQL 学习曲线。

towardsdatascience.com

从那时起，窗口函数成为了我最喜欢的工具，我在几乎每个分析项目中至少使用一次。这是因为我们经常需要在记录的上下文中进行计算。

本文是 SQL 中窗口函数的简单介绍。最重要的是，我们需要了解如何定义窗口以及我们在窗口上执行什么操作。

如果你打算跟随本文中的示例，让我们创建一个表并插入一些虚拟值。

```py
CREATE TABLE store_sales (
  store_id INT,
  store_name VARCHAR(50),
  state VARCHAR(50),
  month DATE,
  sales DECIMAL(10, 2)
);

INSERT INTO store_sales VALUES
(1, 'ABC Mart', 'California', '2022-01-01', 10000.00),
(2, 'XYZ Store', 'Texas', '2022-01-01', 7500.00),
(3, 'Corner Shop', 'New York', '2022-01-01', 12000.00),
(4, 'MegaMart', 'California', '2022-01-01', 25000.00),
(5, 'My Store', 'Texas', '2022-01-01', 5000.00),
(6, 'SuperMart', 'New York', '2022-01-01', 18000.00),
(7, 'ABC Mart', 'California', '2022-02-01', 15000.00),
(8, 'XYZ Store', 'Texas', '2022-02-01', 9000.00),
(9, 'Corner Shop', 'New York', '2022-02-01', 10000.00),
(10, 'MegaMart', 'California', '2022-02-01', 30000.00),
(11, 'My Store', 'Texas', '2022-02-01', 6000.00),
(12, 'SuperMart', 'New York', '2022-02-01', 20000.00),
(13, 'ABC Mart', 'California', '2022-03-01', 12000.00),
(14, 'XYZ Store', 'Texas', '2022-03-01', 10500.00),
(15, 'Corner Shop', 'New York', '2022-03-01', 15000.00),
(16, 'MegaMart', 'California', '2022-03-01', 27500.00),
(17, 'My Store', 'Texas', '2022-03-01', 5500.00),
(18, 'SuperMart', 'New York', '2022-03-01', 19000.00),
(19, 'ABC Mart', 'California', '2022-04-01', 14000.00),
(20, 'XYZ Store', 'Texas', '2022-04-01', 8000.00);
```

## SQL 窗口函数的基础

这是一个查询，显示每个州每家商店的平均销售额和总销售额。

```py
SELECT 
    store_name,
    state,
    SUM(sales) AS total_sales,
    AVG(SUM(sales)) OVER (PARTITION BY state) AS state_average
FROM 
    store_sales
GROUP BY 
    store_name, state;
```

```py
|store_name|state|total_sales|state_average|
|----------|-----|-----------|-------------|
|ABC Mart|California|51000.00|66750.000000000000|
|MegaMart|California|82500.00|66750.000000000000|
|Corner Shop|New York|37000.00|47000.000000000000|
|SuperMart|New York|57000.00|47000.000000000000|
|XYZ Store|Texas|35000.00|25750.000000000000|
|My Store|Texas|16500.00|25750.000000000000|
```

让我们仔细研究窗口函数。

![](img/eb6dfcda371a45a207d650ffbc49c455.png)

作者插图。

每个窗口函数都有这两个部分。我们在 OVER 关键字后定义窗口。在此示例中，我们仅使用州列对数据集进行分区。操作将在共享相同州的记录之间执行。

此外，你可以使用 ORDER BY 关键字重新排列窗口记录。以下查询使用它来获取每家商店在其州内的排名。

```py
SELECT
 store_name,
 state,
 sales,
 DENSE_RANK() OVER (PARTITION BY state
ORDER BY
 sales DESC) AS store_sales_rank
FROM
 store_sales;
```

```py
|store_name|state|sales|store_sales_rank|
|----------|-----|-----|----------------|
|MegaMart|California|30000.00|1|
|MegaMart|California|27500.00|2|
|MegaMart|California|25000.00|3|
|ABC Mart|California|15000.00|4|
|ABC Mart|California|14000.00|5|
|ABC Mart|California|12000.00|6|
|ABC Mart|California|10000.00|7|
|SuperMart|New York|20000.00|1|
|SuperMart|New York|19000.00|2|
|SuperMart|New York|18000.00|3|
|Corner Shop|New York|15000.00|4|
|Corner Shop|New York|12000.00|5|
|Corner Shop|New York|10000.00|6|
|XYZ Store|Texas|10500.00|1|
|XYZ Store|Texas|9000.00|2|
|XYZ Store|Texas|8000.00|3|
|XYZ Store|Texas|7500.00|4|
|My Store|Texas|6000.00|5|
|My Store|Texas|5500.00|6|
|My Store|Texas|5000.00|7|
```

你可以使用[RANK](https://www.sqlshack.com/overview-of-sql-rank-functions/)或[ROW_NUMBER](https://www.simplilearn.com/tutorials/sql-tutorial/row-number-funtion-in-sql)代替[DENSE_RANK](https://learn.microsoft.com/en-us/sql/t-sql/functions/dense-rank-transact-sql?view=sql-server-ver16)。这三个关键字之间的区别在于它们处理平局的方式。

ROW_NUMBER 会为平局分配一个顺序号，不重视平局。RANK 会为平局分配相同的排名并跳过下一个。例如，如果两个商店的销售值相同，它们都会得到第 1 名。但是第 2 名会被跳过，下一个则得到第 3 名。DENSE_RANK 也会为平局分配相同的编号，但不会跳过下一个编号。下一个记录将获得紧接着的排名。

## 我们可以使用窗口函数的有趣方法

窗口函数有许多令人兴奋的应用。正如我之前提到的，我在几乎每个 SQL 项目中都使用它。以下是我们可以使用窗口函数的一些常见方式。

由于我们已经在基础示例中查看了排名，这里不再重新讨论。但排名是窗口函数最常见的用例之一。

**1\. 计算累计总数**

我们也可能需要在有时间数据的地方计算累计总数。换句话说，我们应该将所有先前的值累加到某一点。

在我们的商店销售示例中，这可能是每家商店自年初以来每个月底的销售额。这是完成此任务的查询。

```py
SELECT
 store_name,
 MONTH,
 sales,
 SUM(sales) OVER (PARTITION BY store_name
ORDER BY
 "month") AS running_total
FROM
 store_sales;
```

```py
|store_name|month|sales|running_total|
|----------|-----|-----|-------------|
|ABC Mart|2022-01-01|10000.00|10000.00|
|ABC Mart|2022-02-01|15000.00|25000.00|
|ABC Mart|2022-03-01|12000.00|37000.00|
|ABC Mart|2022-04-01|14000.00|51000.00|
|Corner Shop|2022-01-01|12000.00|12000.00|
|Corner Shop|2022-02-01|10000.00|22000.00|
|Corner Shop|2022-03-01|15000.00|37000.00|
|MegaMart|2022-01-01|25000.00|25000.00|
|MegaMart|2022-02-01|30000.00|55000.00|
|MegaMart|2022-03-01|27500.00|82500.00|
|My Store|2022-01-01|5000.00|5000.00|
|My Store|2022-02-01|6000.00|11000.00|
|My Store|2022-03-01|5500.00|16500.00|
|SuperMart|2022-01-01|18000.00|18000.00|
|SuperMart|2022-02-01|20000.00|38000.00|
|SuperMart|2022-03-01|19000.00|57000.00|
|XYZ Store|2022-01-01|7500.00|7500.00|
|XYZ Store|2022-02-01|9000.00|16500.00|
|XYZ Store|2022-03-01|10500.00|27000.00|
|XYZ Store|2022-04-01|8000.00|35000.00|
```

上述查询将按月份列对窗口中的记录进行排序。在任何月份，销售数据只会累积到该月份。

**2\. 与组统计数据比较。**

可能会有需要将每条记录与其组平均值进行比较的情况。例如，我们可能会对查看每个店铺的状态平均值感兴趣。

这是用于此的 SQL 查询。

```py
SELECT
 store_name,
 state ,
 MONTH,
 sales,
 AVG(sales) OVER (PARTITION BY state, "month") AS running_total
FROM
 store_sales;
```

```py
|store_name|state|month|sales|running_total|
|----------|-----|-----|-----|-------------|
|MegaMart|California|2022-01-01|25000.00|17500.000000000000|
|ABC Mart|California|2022-01-01|10000.00|17500.000000000000|
|ABC Mart|California|2022-02-01|15000.00|22500.000000000000|
|MegaMart|California|2022-02-01|30000.00|22500.000000000000|
|ABC Mart|California|2022-03-01|12000.00|19750.000000000000|
|MegaMart|California|2022-03-01|27500.00|19750.000000000000|
|ABC Mart|California|2022-04-01|14000.00|14000.0000000000000000|
|SuperMart|New York|2022-01-01|18000.00|15000.000000000000|
|Corner Shop|New York|2022-01-01|12000.00|15000.000000000000|
|Corner Shop|New York|2022-02-01|10000.00|15000.000000000000|
|SuperMart|New York|2022-02-01|20000.00|15000.000000000000|
|Corner Shop|New York|2022-03-01|15000.00|17000.000000000000|
|SuperMart|New York|2022-03-01|19000.00|17000.000000000000|
|XYZ Store|Texas|2022-01-01|7500.00|6250.0000000000000000|
|My Store|Texas|2022-01-01|5000.00|6250.0000000000000000|
|My Store|Texas|2022-02-01|6000.00|7500.0000000000000000|
|XYZ Store|Texas|2022-02-01|9000.00|7500.0000000000000000|
|My Store|Texas|2022-03-01|5500.00|8000.0000000000000000|
|XYZ Store|Texas|2022-03-01|10500.00|8000.0000000000000000|
|XYZ Store|Texas|2022-04-01|8000.00|8000.0000000000000000|
```

**3\. 计算移动平均**

移动平均在处理时间序列数据时非常典型。移动平均通常比单个数据点噪声更小。你总是能在金融数据分析中看到它，比如股市数据。但我们也可以在任何领域找到类似的应用。

这是针对我们店铺销售数据的 SQL 查询。它计算了每个店铺的 3 点移动平均。

```py
SELECT
 store_name ,
 MONTH,
 sales,
 AVG(sales) OVER (PARTITION BY store_name
ORDER BY
 MONTH ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS moving_avg_sales
FROM
 store_sales;
```

```py
|store_name|month|sales|moving_avg_sales|
|----------|-----|-----|----------------|
|ABC Mart|2022-01-01|10000.00|10000.0000000000000000|
|ABC Mart|2022-02-01|15000.00|12500.0000000000000000|
|ABC Mart|2022-03-01|12000.00|12333.3333333333333333|
|ABC Mart|2022-04-01|14000.00|13666.666666666667|
|Corner Shop|2022-01-01|12000.00|12000.0000000000000000|
|Corner Shop|2022-02-01|10000.00|11000.0000000000000000|
|Corner Shop|2022-03-01|15000.00|12333.3333333333333333|
|MegaMart|2022-01-01|25000.00|25000.000000000000|
|MegaMart|2022-02-01|30000.00|27500.000000000000|
|MegaMart|2022-03-01|27500.00|27500.000000000000|
|My Store|2022-01-01|5000.00|5000.0000000000000000|
|My Store|2022-02-01|6000.00|5500.0000000000000000|
|My Store|2022-03-01|5500.00|5500.0000000000000000|
|SuperMart|2022-01-01|18000.00|18000.0000000000000000|
|SuperMart|2022-02-01|20000.00|19000.000000000000|
|SuperMart|2022-03-01|19000.00|19000.000000000000|
|XYZ Store|2022-01-01|7500.00|7500.0000000000000000|
|XYZ Store|2022-02-01|9000.00|8250.0000000000000000|
|XYZ Store|2022-03-01|10500.00|9000.0000000000000000|
|XYZ Store|2022-04-01|8000.00|9166.6666666666666667|
```

仔细观察我们如何定义窗口。除了通常出现的 PARTITION BY 和 ORDER BY 关键字，我们还使用了一些其他关键字。我们告诉 SQL 只考虑前 2 条记录和当前记录。通过更改参数，你甚至可以计算不同点的移动平均。

同样，你还可以计算**前瞻性移动平均**。以下是用于此的 SQL 查询：

```py
SELECT
 store_name ,
 MONTH,
 sales,
 AVG(sales) OVER (PARTITION BY store_name 
ORDER BY
 MONTH ROWS BETWEEN CURRENT ROW AND 2 FOLLOWING) AS moving_avg_sales
FROM
 store_sales;
```

```py
|store_name|month|sales|moving_avg_sales|
|----------|-----|-----|----------------|
|ABC Mart|2022-01-01|10000.00|12333.3333333333333333|
|ABC Mart|2022-02-01|15000.00|13666.666666666667|
|ABC Mart|2022-03-01|12000.00|13000.0000000000000000|
|ABC Mart|2022-04-01|14000.00|14000.0000000000000000|
|Corner Shop|2022-01-01|12000.00|12333.3333333333333333|
|Corner Shop|2022-02-01|10000.00|12500.0000000000000000|
|Corner Shop|2022-03-01|15000.00|15000.0000000000000000|
|MegaMart|2022-01-01|25000.00|27500.000000000000|
|MegaMart|2022-02-01|30000.00|28750.000000000000|
|MegaMart|2022-03-01|27500.00|27500.000000000000|
|My Store|2022-01-01|5000.00|5500.0000000000000000|
|My Store|2022-02-01|6000.00|5750.0000000000000000|
|My Store|2022-03-01|5500.00|5500.0000000000000000|
|SuperMart|2022-01-01|18000.00|19000.000000000000|
|SuperMart|2022-02-01|20000.00|19500.000000000000|
|SuperMart|2022-03-01|19000.00|19000.0000000000000000|
|XYZ Store|2022-01-01|7500.00|9000.0000000000000000|
|XYZ Store|2022-02-01|9000.00|9166.6666666666666667|
|XYZ Store|2022-03-01|10500.00|9250.0000000000000000|
|XYZ Store|2022-04-01|8000.00|8000.0000000000000000|
```

## 结论

窗口函数在意识到它们的实用性后，已经成为我的最爱。因此，通过这篇文章，我分享了基础知识和一些常用的窗口函数应用。

但你可以通过窗口函数做更多的事情。虽然这篇博客文章没有广泛涉及，但基础知识应该能让你编写出精彩的 SQL 语句。

> 感谢阅读，朋友！如果你喜欢我的文章，欢迎在[**LinkedIn**](https://www.linkedin.com/in/thuwarakesh/)、[**Twitter**](https://twitter.com/Thuwarakesh)和[**Medium**](https://thuwarakesh.medium.com/)上保持联系。
> 
> 还不是 Medium 会员？请使用此链接[**成为会员**](https://thuwarakesh.medium.com/membership)，因为你无需额外付费，我可以通过推荐你获得少量佣金。
