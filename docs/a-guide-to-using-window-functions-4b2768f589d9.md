# 使用 BigQuery 的窗口函数指南

> 原文：[`towardsdatascience.com/a-guide-to-using-window-functions-4b2768f589d9`](https://towardsdatascience.com/a-guide-to-using-window-functions-4b2768f589d9)

## 在 BigQuery 中轻松创建累计总数、移动平均和排名。

[](https://medium.com/@thomas.ellyatt?source=post_page-----4b2768f589d9--------------------------------)![Tom Ellyatt](https://medium.com/@thomas.ellyatt?source=post_page-----4b2768f589d9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4b2768f589d9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4b2768f589d9--------------------------------) [Tom Ellyatt](https://medium.com/@thomas.ellyatt?source=post_page-----4b2768f589d9--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4b2768f589d9--------------------------------) ·16 分钟阅读·2023 年 7 月 21 日

--

![](img/e881dc2a741d886c4abc1ac4ba0686bf.png)

由 [Benjamin Voros](https://unsplash.com/@vorosbenisop?utm_source=medium&utm_medium=referral) 拍摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果你曾经搜索过或偶然发现过类似于‘*你需要知道的 6 个 SQL 技能，以通过面试*’或‘*我希望早几年知道的 SQL 概念*’这样的内容，窗口函数很可能在那个列表中获得了应有的提及。

> 窗口函数非常棒。

我写这篇文章的目标是帮助你理解这些窗口函数以及如何使用它们。一旦我们完成了教程，我准备了一些用例，你可以在你的项目中运行这些用例进行尝试，因为我在这些示例中使用了公开数据。

# 我们将涵盖：

+   **什么是** **窗口函数？**

+   **窗口函数的语法** — 即分区、排序和框架部分

+   详细了解如何创建 7 天的移动平均以及它是如何工作的

+   你可以使用的**聚合**和**窗口函数**是什么？

+   最后，我们将演示一些用例，以展示窗口函数如何应用。

## **什么是** 窗口函数？

‘窗口’这个词在 SQL 中使用可能看起来很奇怪（*或在计算中一般*）。通常，函数类型的名称会让你对其使用方式有个初步了解，例如：

+   **聚合函数** — 处理一堆数据并给出总结结果

+   **字符串函数** — 一个包含处理单词和句子的工具箱

+   **数组函数** — 一次性处理一组项目

等等…

![](img/124972bd1841e122622cafd4a7cb4cc4.png)

由 [Cosmic Timetraveler](https://unsplash.com/@cosmictimetraveler?utm_source=medium&utm_medium=referral) 拍摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

那么 SQL 中的窗口函数是什么？就像现实世界中的窗户一样，它允许你查看特定区域，而其余部分则不在视线范围内。你只关注窗户中显示的内容。

回到数据的世界，假设你有一个包含 IOWA 酒类店每月销售的表格。

> 这个示例中使用的数据集是公开访问的，由 Google 提供，并且在 BigQuery 中已存在，如果你想自己尝试这些示例。
> 
> ([link](https://console.cloud.google.com/marketplace/product/iowa-department-of-commerce/iowa-liquor-sales?pli=1) (CC0 license))

`bigquery-public-data.iowa_liquour_sales.sales`

以下示例提供了按年份和月份销售的简单视图。

![](img/355551bd99f2282322cfce2833435c68.png)

> 我将上述内容保存为视图，以便将来我们的查询尽可能简洁，专注于应用窗口函数。

如果你想使用这个视图，可以使用`spreadsheep-20220603.Dashboard_Datasets.iowa_liqour_monthly_sales`。

那么我们还想要每年的月平均值作为单独的列怎么办？

有几种方法可以实现这一点，如果你对窗口函数不熟悉，可以尝试先计算平均值作为子查询，然后再与原始表连接，如下所示。

![](img/5483e3292b9fbe584944a59fbb1b13f3.png)

这完全可以正常工作，但窗口函数将允许你在**没有子查询**的情况下得到相同的答案！

![](img/241cec270d76d3f1c68f63f7cf30833c.png)

上述窗口函数允许我们对**partition by year**定义的特定行组执行聚合函数，这里是**avg**函数。

回顾之前的窗口类比，*partition by*部分就是我们在这种情况下的窗口。确实，我们面前有整个数据集，但分区限制了我们的视图仅限于年份。

是时候深入研究语法了。

![](img/ed424eeb31d664fc70e4c0ff979ba791.png)

## 窗口语法

在上面的示例中，我们可以将函数拆分为两个部分：函数名和窗口。

在这种情况下，函数名是熟悉的聚合函数**AVG**。然而，窗口部分则有所不同。

一旦你指定了你的函数，你需要使用**over**关键字开始你的窗口函数，后面必须跟上圆括号()。

![](img/6576ec6fcf2c81ff578026e8fa53f7d0.png)

在圆括号内，你可以使用**partition by**关键字指定我们想要执行聚合的窗口，后跟你希望包括在窗口中的列列表。在这里，我们只包含了一列**year**，但稍后我们将引入另一列。

**Partition by 是可选的**；如果你不包括**partition by**，聚合将包含数据集中的所有行。由于这存在于 SELECT 语句中，值得注意的是 WHERE 子句会在此窗口函数之前执行。

我这是什么意思？使用我之前分享的示例，我通过**按年分区**来指定窗口。然而，在我的**WHERE 子句**中，我设置了一个仅返回**年份 = 2022**的过滤器。

这意味着数据集中只有一个年份——2022，当窗口函数运行时。因此，我的**按年分区窗口是多余的**，在这种情况下使用下面的行会得到相同的结果。

![](img/bdfe8b2a5d9507740ddb2520cf4fa0b8.png)

让我们重新运行之前的查询，这次去掉我们的 WHERE 子句。

![](img/f1edde0974c55fe87d072c1d97317dd0.png)

图片由[Nik](https://unsplash.com/@helloimnik?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这里我们可以看到 2023 年和 2022 年的不同值。这现在显示了每一年提供的平均月销售额。

例如，在**第 7 行**，我们有**2022 年**，其**平均月销售额为 3570 万**，而**2023 年（至今）的平均月销售额为 3580 万**。

![](img/57ad70ec62e55a26bece3f1c3384d864.png)

将报告修改为仅关注 2022 年，我们可以清晰地可视化该年度的月度平均收入与实际情况的对比。

访问月度平均数据使得可视化和分析销售趋势变得更容易。具体来说，明显可以看出，年度的下半年对销售有显著贡献。

我们使用了一个窗口函数来确定每年的平均月销售额。然后，这个函数将结果应用于所有具有该年份的行。这就像我们之前看到的左连接子查询。

![](img/468939b39525889d21c5d78895c4c801.png)

图片由[Daniel K Cheung](https://unsplash.com/fr/@danielkcheung?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## **Order By**

迄今为止，我们主要关注了聚合函数和如何指定窗口。我们还可以确定窗口应该如何执行任务，这是一部分排名或运行总计/平均值解决方案的关键。

返回到 Iowa 数据集，让我们扩展视图以包括 store_name，然后根据总销售额给商店一个编号的月度排名。

## 新视图

> spreadsheep-20220603.Dashboard_Datasets.iowa_liqour_monthly_sales_inc_store

与聚合函数不同，对于专门用于窗口函数的排名函数，你不会在函数内部指定列。

![](img/9aec38fbb3c644006f95d25c2190fb85.png)

然而，如果你尝试按照上述方式运行，你会遇到一个错误。

![](img/5f1210d976d25c820ad962c14c436839.png)

这里的问题是我们告诉 Bigquery 我们想对结果进行排序，但我们没有指定排序方式，这可以通过**ORDER BY**来实现。

![](img/f6b27be18dca7d9773e2741a4bd6319c.png)![](img/d17df45dc93f434430ace8de8051b988.png)

这为我们提供了一个按商店级别的月度销售视图，并且每个商店都有一个排名。你可以进一步分析，回答其他问题，比如*2022 年每个月的前三大商店是哪三家*？

在本文接近尾声的一个例子中，我们将使用一个新的子句，**QUALIFY**，它可以让你轻松地过滤窗口函数给出的结果。

到目前为止，我们的窗口函数已应用于每个分区中的所有行，但如果我们只想要分区的一个子集呢？例如，过去七天的每日销售平均值？为此，我们需要指定一个窗口帧。

## 窗口帧

现在引入一个新的数据集，介绍**芝加哥出租车**！这是另一个你可以用来实验的公共数据集（CC0 许可证）。([link](https://console.cloud.google.com/marketplace/product/city-of-chicago-public-data/chicago-taxi-trips?project=spreadsheep-20220603))

```py
bigquery-public-data.chicago_taxi_trips.taxi_trips
```

这个公共数据集很大，达到 75GB，很快就会消耗掉你每月 100GB 的免费查询配额。因此，我创建了一个只包含 2023 年数据的新表，这样我们可以在不产生高额费用的情况下玩转数据。

我已经将这个表公开，所以我建议你尝试使用我的数据集进行测试。

```py
spreadsheep-case-studies-2023.chicago_taxies_2023.trip_data
```

不管怎样，回到主题……什么是窗口帧？这个子句允许我们定义在分区内需要使用哪些行或范围。一个常见的用例是创建移动平均或累计总和。

```py
SELECT
  date(trip_start_timestamp) as trip_start_date,
  round(sum(trip_total),2) as trip_total_revenue
FROM
  `spreadsheep-case-studies-2023.chicago_taxies_2023.trip_data`
WHERE
  date(trip_start_timestamp) between "2023-05-01" and "2023-06-30"
GROUP BY
  trip_start_date
ORDER BY
  trip_start_date DESC
```

这个查询提供了 2023 年 5 月至 6 月的每日收入数据。

![](img/54d9e8504337ccab92e7b973ddb2b565.png)

移动平均在时间序列数据中非常常见，因为它允许你轻松地将特定日期或月份的表现与给定周期内通常看到的结果进行比较。

首先，我们来创建一个简单的移动平均，并且为了避免重复的日期转换和收入舍入，我将我们的初始查询放在了一个 CTE 中。

```py
WITH daily_data as(
SELECT
  date(trip_start_timestamp) as trip_start_date,
  round(sum(trip_total),2) as trip_total_revenue
FROM
  `spreadsheep-case-studies-2023.chicago_taxies_2023.trip_data`
WHERE
  date(trip_start_timestamp) between "2023-05-01" and "2023-06-30"
GROUP BY
  trip_start_date
ORDER BY
  trip_start_date DESC
)

SELECT
  trip_start_date,
  trip_total_revenue,
  avg(trip_total_revenue) over (order by trip_start_date asc) as moving_average
FROM
  daily_data
```

![](img/894507e9c1c09c4dbc3ab403b0ac916d.png)

如果我们查看前五行，可以看到第一个平均值等于 trip_total_revenue。这是因为它是窗口的开始，因为我们按 trip_start_date 升序排列了数据。因此，目前还没有任何内容可供平均计算。

然而，我们现在在第二行的第 1 行和第 2 行之间有一个每日平均值，而在第三行的第 1 行、第 2 行和第 3 行之间有一个每日平均值。

这是一个不错的开始，它显示了我们的移动平均正在工作，但我们可以更进一步。我们来创建一个仅包含最后七天收入的移动平均，如果窗口不包含七天，则显示空值，因为这是一个不完整的窗口。

要指定你的窗口范围，你需要记住三个关键词：

+   当前行

+   preceding

+   following

然后，你从行或范围开始构建你的窗口（稍后我会解释这两者之间的区别），接着在<<start>>和<<end>>之间。

```py
rows between 7 preceding and one preceding
```

上面的示例是我们问题所需的窗口框架。我们已经指定窗口从当前行之前的七行开始，到当前行之前的一行结束。

这是一个简单的示例，展示了这个窗口函数如何与求和聚合（累积总和）一起工作。

```py
select
  numbers,
  sum(numbers) over 
  (
    order by numbers asc 
    rows between 7 preceding and one preceding
  ) 
  as moving_sum_seven
from
  test_data
```

![](img/9928f4e257bd211e21957aefa50e4c42.png)

正如你所见，当我们到达第 8 行时，移动总和的值达到 7，此时窗口现在包含七行数据。如果你将窗口调整为 6 行之前和当前行，你会看到窗口已经移动，以包括当前行。

![](img/3b9275c9a55b62d1b88ea25785061c3b.png)

在本节的最后，我将提供一些用例示例来突出它们的使用方式，但现在先回到当前任务！

让我们把这个窗口范围应用到我们的移动平均中。

```py
with daily_data as (
SELECT
  date(trip_start_timestamp) as trip_start_date,
  round(sum(trip_total),2) as trip_total_revenue
FROM
  `spreadsheep-case-studies-2023.chicago_taxies_2023.trip_data`
WHERE
  date(trip_start_timestamp) between "2023-05-01" and "2023-06-30"
GROUP BY
  trip_start_date
ORDER BY
  trip_start_date DESC
)

SELECT
  trip_start_date,
  trip_total_revenue,
  avg(trip_total_revenue) over (order by trip_start_date asc rows between 7 preceding and one preceding) as moving_average
FROM
  daily_data
ORDER BY
  trip_start_date DESC
```

现在我们面临最后一个挑战：如何在窗口中数据行少于七行时将值设为 null？好吧，我们可以使用 IF 语句来进行检查。

```py
COUNT(*) OVER 
(
ORDER BY trip_start_date ASC ROWS BETWEEN 7 PRECEDING AND 1 PRECEDING
) = 7
```

```py
 if
  (
    COUNT(*) OVER (ORDER BY trip_start_date ASC ROWS BETWEEN 7 PRECEDING AND 1 PRECEDING) = 7,
    AVG(trip_total_revenue) OVER (ORDER BY trip_start_date ASC ROWS BETWEEN 7 PRECEDING AND 1 PRECEDING),
    NULL 
  ) AS moving_average
```

我们引入了第二个窗口函数，该函数计算窗口框架中存在的行数，如果等于 7，将提供移动平均结果。

![](img/d080603ba7ce983011e11f514131d911.png)

**值得知道：**

+   如果 `ORDER BY` 表达式未在窗口函数中提及，默认规范是 `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING.` 

+   如果指定了 `ORDER BY` 表达式并且使用了聚合函数，则默认窗口框架是 `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`。

[有关窗口框架规范的 BigQuery 文档在这里。](https://cloud.google.com/bigquery/docs/reference/standard-sql/window-function-calls#def_window_frame)

## ROWS 和 RANGE 的区别

在 SQL 中，ROWS 和 RANGE 子句都帮助控制窗口函数在一个组内使用哪些行。

ROWS 子句处理固定数量的行。它计算当前行之前或之后的特定数量的行，无论这些行的值如何。这些行被包含在窗口函数中。

RANGE 子句基于行的值来工作。它考虑与当前行相对的特定值范围内的行。实际值决定哪些行被包含在窗口函数计算中。

因此，虽然 ROWS 子句关注于行的物理位置，但 RANGE 子句考虑行的逻辑值来确定它们是否包含在窗口函数中。

试试这个示例，看看它的实际效果。

```py
with sales_data as (
SELECT
'2023-01-01' AS DATE, 100 AS SALES
UNION ALL
SELECT
'2023-01-02' AS DATE, 50 AS SALES
UNION ALL
SELECT
'2023-01-03' AS DATE, 250 AS SALES
UNION ALL
SELECT
'2023-01-03' AS DATE, 200 AS SALES
UNION ALL
SELECT
'2023-01-04' AS DATE, 300 AS SALES
UNION ALL
SELECT
'2023-01-05' AS DATE, 150 AS SALES
)

SELECT
  *,
  SUM(SALES) OVER (ORDER BY DATE ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total_rows,
  SUM(SALES) OVER (ORDER BY DATE RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total_range
FROM sales_data
```

![](img/3558a26debfc5e2eed168d20cd763863.png)

查看第 3 行和第 4 行以比较这两个子句。ROWS 子句将每一行都加入到总数中，即使存在重复的销售日期。而 RANGE 子句则将具有相同销售日期的行作为一个范围进行分组。例如，在这种情况下，所有日期为 2023-01-03 的行将被视为一个范围。

![](img/c44ef6cece2b28def1a3c1cdfeb2c7f7.png)

照片由 [Christopher Gower](https://unsplash.com/@cgower?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上提供

## **窗口函数是什么？**

有很多可以与窗口函数一起使用的函数。

对于 **聚合函数，** 你可以尝试：

+   **SUM**: 计算数值列的总和。

+   **AVG**: 计算数值列的平均值。

+   **MIN**: 检索列中的最小值。

+   **MAX**: 检索列中的最大值。

+   **COUNT**: 计算列中行的数量。

+   **COUNT** DISTINCT: 计算列中不同值的数量。

然后你会有一系列仅适用于窗口函数的新函数，这些被称为分析函数：

+   [**ROW_NUMBER**](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#row_number): 在窗口帧内为每一行分配一个唯一的编号。

+   [**RANK**](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#rank): 根据窗口帧中指定的顺序为每一行分配一个排名。

+   [**DENSE_RANK**](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#dense_rank): 根据窗口帧中指定的顺序为每一行分配一个排名，排名中没有间隙。

+   [**LAG**](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#lag): 从窗口帧中的前一行中检索值。

+   [**LEAD**](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#lead): 从窗口帧中的后一行中检索值。

+   [**FIRST_VALUE**](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#first_value): 从窗口帧中的第一行中检索值。

+   [**LAST_VALUE**](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#last_value): 从窗口帧中的最后一行中检索值。

> 上述函数均链接到 BigQuery 文档。

![](img/81b39427fbba2b01ce00ce8b96780541.png)

照片由 [Susan Holt Simpson](https://unsplash.com/@shs521?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上提供

# 实际示例

## **每日累计总数**

窗口函数的一个简单用例是累计总数。对于芝加哥出租车数据集，我们可以按月记录收入，但需要一个新列来跟踪迄今为止的总收入。

![](img/0fadd5f53320d91584bed79bff4246a3.png)

```py
with daily_data as (
SELECT
  date(timestamp_trunc(trip_start_timestamp,month)) as trip_month,
  round(sum(trip_total),2) as trip_total_revenue
FROM
  `spreadsheep-case-studies-2023.chicago_taxies_2023.trip_data`
WHERE
  date(trip_start_timestamp) between "2023-01-01" and "2023-06-30"
GROUP BY
  trip_month
ORDER BY
  trip_month DESC
)

SELECT
  trip_month,
  trip_total_revenue,
  round(sum(trip_total_revenue) over (order by trip_month asc),2) AS running_total_revenue,
FROM
  daily_data
ORDER BY
  trip_month DESC
```

# **12 周移动平均**

本文教程强调了在处理时间序列数据时移动平均是常见的。

```py
with daily_data as (
SELECT
  date(timestamp_trunc(trip_start_timestamp,week(monday))) as trip_week,
  round(sum(trip_total),2) as trip_total_revenue
FROM
  `spreadsheep-case-studies-2023.chicago_taxies_2023.trip_data`
WHERE
  date(trip_start_timestamp) between "2023-01-01" and "2023-06-30"
GROUP BY
  trip_week
ORDER BY
  trip_week DESC
)

SELECT
  trip_week,
  trip_total_revenue,
  if
  (
    COUNT(*) OVER (ORDER BY trip_week ASC ROWS BETWEEN 12 PRECEDING AND 1 PRECEDING) = 12,
    AVG(trip_total_revenue) OVER (ORDER BY trip_week ASC ROWS BETWEEN 12 PRECEDING AND 1 PRECEDING),
    NULL 
  ) AS moving_average
FROM
  daily_data
ORDER BY
  trip_week DESC
```

![](img/9c14c861ce0dfbbc629321552e47ef0d.png)![](img/6a58b00bf94ee3628f102d3ba5a6dcf8.png)

将收入与移动平均线一起绘制表明了一个积极的趋势，因为自四月以来，移动平均线每周都在持续上升。如果没有移动平均线，我们的视线可能会被表现较差的周所吸引，而不是看到整体趋势。

![](img/07ec677747f7459b63e7a8788e6d49ad.png)

[Randy Fath](https://unsplash.com/@randyfath?utm_source=medium&utm_medium=referral) 提供的照片，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# **计算异常检测的 Z 分数**

> Z-Score 计算 = (x — 平均值) / 标准差

Z 分数是一种衡量一个数字相对于其他数字组的异常程度或典型程度的方法。它告诉您一个特定的数字距离该组的平均值有多少个标准差。

```py
 if
  (
    COUNT(*) OVER (ORDER BY trip_start_date ASC ROWS BETWEEN 30 PRECEDING AND 1 PRECEDING) = 30,
    round
    (
      (
        trip_total_revenue - 
        AVG(trip_total_revenue) OVER (ORDER BY trip_start_date ASC ROWS BETWEEN 30 PRECEDING AND 1 PRECEDING)
      ) / stddev(trip_total_revenue) OVER (ORDER BY trip_start_date ASC ROWS BETWEEN 30 PRECEDING AND 1 PRECEDING)
    ,1),
    NULL 
  ) AS z_score_30_day
```

在这个例子中，我们取了 trip_total_revenue 的实际值，并减去了我们在过去 30 天里看到的平均每日收入。

然后我们将该数字除以这 30 天的标准差。这告诉我们特定一天的收入离平均值有多近，或该值距离平均值有多少个标准差。

这是一个很方便的指标，可以绘制在图表上，如下所示，因为它为您的数据提供了背景。虽然我们仅查看了过去 30 天的数据，但 Z 分数与之前的 30 天进行比较是毫不费力的，我们可以看到峰值和波动在 Z 分数突显出该天与常态的不同之前似乎微不足道。

![](img/226555676a27e43f41f37cd2c4e54293.png)

对于这类报告，您应该设置一个值来表明存在异常事件。我不会说上面的图表中的任何日期都是异常的，但一个典型的值是 3（即三个标准差）。然而，这完全取决于数据的波动性。

> 完整查询

```py
with daily_data as (
SELECT
  (trip_start_timestamp) as trip_start_date,
  round(sum(trip_total),2) as trip_total_revenue
FROM
  `spreadsheep-case-studies-2023.chicago_taxies_2023.trip_data`
WHERE
  date(trip_start_timestamp) between "2023-01-01" and "2023-06-30"
GROUP BY
  trip_start_date
ORDER BY
  trip_start_date DESC
)

SELECT
  trip_start_date,
  trip_total_revenue,
  if
  (
    COUNT(*) OVER (ORDER BY trip_start_date ASC ROWS BETWEEN 30 PRECEDING AND 1 PRECEDING) = 30,
    round
    (
      (
        trip_total_revenue - 
        AVG(trip_total_revenue) OVER (ORDER BY trip_start_date ASC ROWS BETWEEN 30 PRECEDING AND 1 PRECEDING)
      ) / stddev(trip_total_revenue) OVER (ORDER BY trip_start_date ASC ROWS BETWEEN 30 PRECEDING AND 1 PRECEDING)
    ,1),
    NULL 
  ) AS z_score_30_day
FROM
  daily_data
ORDER BY
  trip_start_date DESC
```

![](img/10ec55cd9d4142cbf0839c38595bc667.png)

[Giorgio Trovato](https://unsplash.com/@giorgiotrovato?utm_source=medium&utm_medium=referral) 提供的照片，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**每月排名前列的表现者**

在芝加哥出租车数据集中有很多出租车公司，我们可能会问每个月表现最好的前三家公司是谁。

为了实现这一点，我们可以使用按 trip_month 分区并按 trip_total_revenue 降序排列的排名分析函数。

```py
 rank() over (partition by trip_month order by trip_total_revenue desc) AS ranking
```

然而，这仍然会为数据集中每个月的所有公司提供结果，而不仅仅是前三名。因此，我们可以利用 **QUALIFY** 子句，它的功能类似于 **WHERE** 子句，以便您可以过滤数据。

qualify 子句只能与窗口函数一起使用，并且可以引用您在选择语句中创建的窗口函数。更多细节请见 [这里](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#qualify_clause)。

下面的结果清楚地表明三家公司主导了出租车市场。

![](img/3e9df968915093c7ccc8fb596f0743dc.png)

```py
with daily_data as (
SELECT
  date(timestamp_trunc(trip_start_timestamp,month)) as trip_month,
  company,
  round(sum(trip_total),2) as trip_total_revenue
FROM
  `spreadsheep-case-studies-2023.chicago_taxies_2023.trip_data`
WHERE
  date(trip_start_timestamp) between "2023-01-01" and "2023-06-30"
GROUP BY
  trip_month,
  company
ORDER BY
  trip_month DESC
)

SELECT
  trip_month,
  company,
  trip_total_revenue,
  rank() over (partition by trip_month order by trip_total_revenue desc) AS ranking
FROM
  daily_data
QUALIFY
  ranking <= 3
ORDER BY
  trip_month DESC
```

![](img/6c1ac4aa546426c43823986a818c0c36.png)

图片由 [Towfiqu barbhuiya](https://unsplash.com/de/@towfiqu999999?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**月度/季度比较**

月度和季度报告对于跟踪 KPI 以及帮助评估业务发展方向至关重要。然而，创建一个在 BigQuery 中提供月度变化的报告，一旦你掌握了方法，可能会变得棘手。

一旦你有了你想要的数据层级，例如下例中的按月数据，你可以使用 LAG 或 LEAD 函数返回上一个月的收入，这样你就可以计算百分比差异。

你可以使用 LAG 或 LEAD，两者的效果相同，具体取决于你如何排序数据。由于我们要提取上个月的收入，因此在这里使用 LAG 更为合适。

![](img/6c9c87ae76d4f7673bb5a72ffbabbc1c.png)

```py
with daily_data as (
SELECT
  date(timestamp_trunc(trip_start_timestamp,month)) as trip_month,
  round(sum(trip_total),2) as trip_total_revenue
FROM
  `spreadsheep-case-studies-2023.chicago_taxies_2023.trip_data`
WHERE
  date(trip_start_timestamp) between "2023-01-01" and "2023-06-30"
GROUP BY
  trip_month
ORDER BY
  trip_month DESC
)

SELECT
  trip_month,
  trip_total_revenue,
  lead(trip_total_revenue) over (order by trip_total_revenue asc) AS previous_month_revenue,
  round
  (
    (
      (
        trip_total_revenue - lag(trip_total_revenue) over (order by trip_total_revenue asc)
      ) / lag(trip_total_revenue) over (order by trip_total_revenue asc)
    ) * 100
  , 1) || "%" AS perc_change
FROM
  daily_data
ORDER BY
  trip_month DESC
```

这篇文章到此结束。如果你有任何问题或挑战，请随时评论，我会尽快回答。

我经常为 BigQuery 和 Looker Studio 撰写文章。如果你感兴趣，可以考虑在 Medium 上关注我获取更多内容！

> *所有图片，除非另有说明，均由作者提供。*

***保持优雅，各位！

Tom***
