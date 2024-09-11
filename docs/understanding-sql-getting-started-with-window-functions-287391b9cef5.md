# 理解 SQL：入门窗口函数

> 原文：[https://towardsdatascience.com/understanding-sql-getting-started-with-window-functions-287391b9cef5?source=collection_archive---------5-----------------------#2023-09-17](https://towardsdatascience.com/understanding-sql-getting-started-with-window-functions-287391b9cef5?source=collection_archive---------5-----------------------#2023-09-17)

## 通过使用 SQL 窗口函数，获取更多的聚合信息

[](https://medium.com/@dataforyou?source=post_page-----287391b9cef5--------------------------------)[![Rob Taylor, PhD](../Images/5e4e86da7b77404ed42d00a60ea5eacf.png)](https://medium.com/@dataforyou?source=post_page-----287391b9cef5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----287391b9cef5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----287391b9cef5--------------------------------) [Rob Taylor, PhD](https://medium.com/@dataforyou?source=post_page-----287391b9cef5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F98de080592fc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-sql-getting-started-with-window-functions-287391b9cef5&user=Rob+Taylor%2C+PhD&userId=98de080592fc&source=post_page-98de080592fc----287391b9cef5---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----287391b9cef5--------------------------------) ·15 分钟阅读·2023年9月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F287391b9cef5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-sql-getting-started-with-window-functions-287391b9cef5&user=Rob+Taylor%2C+PhD&userId=98de080592fc&source=-----287391b9cef5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F287391b9cef5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-sql-getting-started-with-window-functions-287391b9cef5&source=-----287391b9cef5---------------------bookmark_footer-----------)![](../Images/7d9d02bd38237ca6dc7bedb3e5383324.png)

图片由 [Components AI](https://unsplash.com/@components_ai?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 简介

在 SQL 中聚合数据时，*窗口函数*比与`GROUP BY`子句结合使用的聚合方法提供了更大的灵活性。虽然这两种方法确实执行类似的功能，但窗口函数的不同在于输出的结构方式。具体来说，窗口函数对一组相关的行进行操作，这些行的关系由表行的某些分组或*分区*决定。而且，与将行合并为单一输出行的非窗口函数不同，*所有*行都保持其独立身份，并出现在输出表中。

这种行为与普通的聚合操作大相径庭，能够大大扩展你的分析工具箱，超越简单的汇总统计。例如，窗口函数允许我们计算累计总和、移动平均，甚至像*z*-得分这样的统计指标。

在这篇文章中，我们将查看 SQL 窗口函数的结构和基本功能。这里的重点有些基础，因此如果你还没有接触过窗口函数，或者对它们的使用经验有限，希望这对你会有一些帮助。

本文将使用关于1930年至2022年FIFA世界杯比赛的一些高级汇总数据。这些排名和统计数据来自维基百科，并在知识共享署名-相同方式共享（CC-BY-SA）许可下提供。数据和相关信息可以在[这里](https://en.wikipedia.org/wiki/FIFA_World_Cup)找到。为了本博客的目的，我将表格导入到我自己的PostgresSQL数据库中，但如果你想跟随，你可以从我的[Git 仓库](https://github.com/dataforyounz/fifa-world-cup)中获取表格的副本。在我的数据库中，这个表格被称为`world_cup_placings`，下面显示了一个输出示例：

```py
|year|start_date|end_date|host_country |first_place |second_place  |third_place  |fourth_place|total_teams|matches_played|total_goals|total_attendance|
|----|----------|--------|-------------|------------|--------------|-------------|------------|-----------|--------------|-----------|----------------|
|1930|13/07/30  |30/07/30|Uruguay      |Uruguay     |Argentina     |United States|Yugoslavia  |13         |18            |70         |590,549         |
|1934|27/05/34  |10/06/34|Italy        |Italy       |Czechoslovakia|Germany      |Austria     |16         |17            |70         |363,000         |
|1938|4/06/38   |19/06/38|France       |Italy       |Hungary       |Brazil       |Sweden      |15         |18            |84         |374,835         |
|1950|24/06/50  |16/07/50|Brazil       |Uruguay     |Brazil        |Sweden       |Spain       |13         |22            |88         |1,045,246       |
|1954|16/06/54  |4/07/54 |Switerland   |West Germany|Hungary       |Austria      |Uruguay     |16         |26            |140        |768,607         |
|1958|8/06/58   |29/06/58|Sweden       |Brazil      |Sweden        |France       |West Germany|16         |35            |126        |819,810         |
|1962|30/05/62  |17/06/62|Chile        |Brazil      |Czechoslovakia|Chile        |Yugoslavia  |16         |32            |89         |893,172         |
|1966|11/07/66  |30/07/66|England      |England     |West Germany  |Portugal     |Soviet Union|16         |32            |89         |1,563,135       |
|1970|31/05/70  |21/06/70|Mexico       |Brazil      |Italy         |West Germany |Uruguay     |16         |32            |95         |1,604,065       |
|1974|13/06/74  |7/07/74 |West Germany |West Germany|Netherlands   |Poland       |Brazil      |16         |38            |97         |1,865,762       |
|1978|1/06/78   |25/06/78|Argentina    |Argentina   |Netherlands   |Brazil       |Italy       |16         |38            |102        |1,545,791       |
|1982|13/06/82  |11/07/82|Spain        |Italy       |West Germany  |Poland       |France      |24         |52            |146        |2,109,723       |
|1986|31/05/86  |29/06/86|Mexico       |Argentina   |West Germany  |France       |Belgium     |24         |52            |132        |2,394,031       |
|1990|8/06/90   |8/07/90 |Italy        |West Germany|Argentina     |Italy        |England     |24         |52            |115        |2,516,215       |
|1994|17/06/94  |17/07/94|United States|Brazil      |Italy         |Sweden       |Bulgaria    |24         |52            |141        |3,597,042       |
|1998|10/06/98  |12/07/98|France       |France      |Brazil        |Croatia      |Netherlands |32         |64            |171        |2,785,100       |
|2002|31/05/02  |30/06/02|Korea / Japan|Brazil      |Germany       |Turkey       |South Korea |32         |64            |161        |2,705,198       |
|2006|9/06/06   |9/07/06 |Germany      |Italy       |France        |Germany      |Portugal    |32         |64            |147        |3,359,439       |
|2010|11/06/10  |11/07/10|South Africa |Spain       |Netherlands   |Germany      |Uruguay     |32         |64            |145        |3,178,856       |
|2014|12/06/14  |13/07/14|Brazil       |Germany     |Argentina     |Netherlands  |Brazil      |32         |64            |171        |3,429,873       |
|2018|14/06/18  |15/07/18|Russia       |France      |Croatia       |Belgium      |England     |32         |64            |169        |3,031,768       |
|2022|20/11/22  |18/12/22|Qatar        |Argentina   |France        |Croatia      |Morocco     |32         |64            |172        |3,404,252       |
```

## 关于执行顺序的简要说明

理解 SQL 执行每个子句的顺序是很重要的，所以我们花几分钟时间来检查窗口函数在[执行顺序](https://understanding-sql-order-of-execution-ba2b4e558828)中的位置。

窗口函数只能在 `SELECT` 列表和 `ORDER BY` 子句中使用。它们**不能**与 `GROUP BY`、`HAVING` 或 `WHERE` 子句一起使用。原因在于窗口函数是在这些子句处理*之后*执行的。另一个需要注意的事项是，窗口函数是在非窗口聚合函数（即 `SUM`、`MAX`、`AVG` 等）处理*之后*执行的。正如我们稍后将看到的，这很有用，因为这意味着我们实际上可以在窗口函数中使用这些函数。

## OVER 子句

首先，让我们来看看窗口函数的最简单版本：

```py
FUNCTION_NAME() OVER()
```

`FUNCTION_NAME()`只是一个占位符，用于你希望使用的任何函数；然而，窗口函数必须*总是*包含`OVER`子句。这个子句区别了窗口函数和非窗口函数，它的作用是确定*如何*将行分割以进行处理。在上面的例子中，虽然没有传递任何参数给`OVER`。这完全合法，所以让我们看看实际效果如何。

窗口函数的一个常见用例是为表中的每一行分配一个数值。这可以通过内置的`ROW_NUMBER`函数实现。例如，考虑下面的示例查询：

```py
/* Assigning numbers to each row in the table */
SELECT 
   "year"
   ,host_country
   ,first_place
   ,total_goals
   ,ROW_NUMBER() OVER() AS row_num
FROM world_cup_placings;
;
```

```py
|year|host_country |first_place |total_goals|row_num|
|----|-------------|------------|-----------|-------|
|1930|Uruguay      |Uruguay     |70         |1      |
|1934|Italy        |Italy       |70         |2      |
|1938|France       |Italy       |84         |3      |
|1950|Brazil       |Uruguay     |88         |4      |
|1954|Switerland   |West Germany|140        |5      |
|1958|Sweden       |Brazil      |126        |6      |
|1962|Chile        |Brazil      |89         |7      |
|1966|England      |England     |89         |8      |
|1970|Mexico       |Brazil      |95         |9      |
|1974|West Germany |West Germany|97         |10     |
|1978|Argentina    |Argentina   |102        |11     |
|1982|Spain        |Italy       |146        |12     |
|1986|Mexico       |Argentina   |132        |13     |
|1990|Italy        |West Germany|115        |14     |
|1994|United States|Brazil      |141        |15     |
|1998|France       |France      |171        |16     |
|2002|Korea / Japan|Brazil      |161        |17     |
|2006|Germany      |Italy       |147        |18     |
|2010|South Africa |Spain       |145        |19     |
|2014|Brazil       |Germany     |171        |20     |
|2018|Russia       |France      |169        |21     |
|2022|Qatar        |Argentina   |172        |22     |
```

我们现在有一个新创建的列叫做`row_num`，它包含一个顺序编号列表；每个表行一个。仅使用普通的`OVER`子句，窗口函数将*整个*表视为一个单一分区。这是因为我们没有告诉它其他信息。

让我们看看如果用聚合函数，如`SUM`，替代`ROW_NUMBER`并应用于`total_goals`列（即整个比赛期间的总进球数）会发生什么。下面提供了这个查询：

```py
/* Using SUM() within our window function */
SELECT 
   "year"
   ,host_country
   ,first_place
   ,total_goals
   ,SUM(total_goals) OVER() AS all_goals
FROM world_cup_placings
;
```

```py
|year|host_country |first_place |total_goals|all_goals|
|----|-------------|------------|-----------|---------|
|1930|Uruguay      |Uruguay     |70         |2,720    |
|1934|Italy        |Italy       |70         |2,720    |
|1938|France       |Italy       |84         |2,720    |
|1950|Brazil       |Uruguay     |88         |2,720    |
|1954|Switerland   |West Germany|140        |2,720    |
|1958|Sweden       |Brazil      |126        |2,720    |
|1962|Chile        |Brazil      |89         |2,720    |
|1966|England      |England     |89         |2,720    |
|1970|Mexico       |Brazil      |95         |2,720    |
|1974|West Germany |West Germany|97         |2,720    |
|1978|Argentina    |Argentina   |102        |2,720    |
|1982|Spain        |Italy       |146        |2,720    |
|1986|Mexico       |Argentina   |132        |2,720    |
|1990|Italy        |West Germany|115        |2,720    |
|1994|United States|Brazil      |141        |2,720    |
|1998|France       |France      |171        |2,720    |
|2002|Korea / Japan|Brazil      |161        |2,720    |
|2006|Germany      |Italy       |147        |2,720    |
|2010|South Africa |Spain       |145        |2,720    |
|2014|Brazil       |Germany     |171        |2,720    |
|2018|Russia       |France      |169        |2,720    |
|2022|Qatar        |Argentina   |172        |2,720    |
```

好的，我们为每一行只得到一个值——这正是我们应该预期的。记住，窗口函数是在一个分区内应用到所有行的，而这里（如上所述），窗口函数将整个表视为一个分区。因此，它将只对`total_goals`列中的所有值进行求和。此外，窗口函数保留了行的身份，因此这个输出值会为每一行重复。还要注意，我们在窗口函数中使用了一个聚合函数——这是可能的，因为窗口函数是在聚合函数*之后*处理的。

好的，让我们看看通过计算所有比赛中的平均进球数（注意输出值的四舍五入）我们可以推进到什么程度：

```py
/* Computing the average number of goals */
SELECT 
   "year"
   ,host_country
   ,first_place
   ,total_goals
   ,ROUND(AVG(total_goals) OVER(), 0) AS mean_goals
FROM world_cup_placings
;
```

```py
|year|host_country |first_place |total_goals|mean_goals|
|----|-------------|------------|-----------|----------|
|1930|Uruguay      |Uruguay     |70         |124       |
|1934|Italy        |Italy       |70         |124       |
|1938|France       |Italy       |84         |124       |
|1950|Brazil       |Uruguay     |88         |124       |
|1954|Switerland   |West Germany|140        |124       |
|1958|Sweden       |Brazil      |126        |124       |
|1962|Chile        |Brazil      |89         |124       |
|1966|England      |England     |89         |124       |
|1970|Mexico       |Brazil      |95         |124       |
|1974|West Germany |West Germany|97         |124       |
|1978|Argentina    |Argentina   |102        |124       |
|1982|Spain        |Italy       |146        |124       |
|1986|Mexico       |Argentina   |132        |124       |
|1990|Italy        |West Germany|115        |124       |
|1994|United States|Brazil      |141        |124       |
|1998|France       |France      |171        |124       |
|2002|Korea / Japan|Brazil      |161        |124       |
|2006|Germany      |Italy       |147        |124       |
|2010|South Africa |Spain       |145        |124       |
|2014|Brazil       |Germany     |171        |124       |
|2018|Russia       |France      |169        |124       |
|2022|Qatar        |Argentina   |172        |124       |
```

现在，这非常有用。将平均进球数与比赛总数一起列出，让我们可以直接比较这些值。我们可以很容易地看到个别总数与所有比赛中的平均数相比如何。

让我们进一步推展，通过计算每一行的*z*-分数来实现。为此，我们还需要在另一个窗口函数中使用`STDDEV`函数。下面的查询展示了如何做到这一点：

```py
/* Compute z-score for total goals scored */
SELECT 
   "year"
   ,host_country
   ,first_place
   ,total_goals
   ,ROUND(AVG(total_goals) OVER(), 0) AS mean_goals
   ,ROUND((total_goals - AVG(total_goals) OVER()) / 
          STDDEV(total_goals) OVER(), 2) AS z_score
FROM world_cup_placings
;
```

```py
|year|host_country |first_place |total_goals|mean_goals|z_score|
|----|-------------|------------|-----------|----------|-------|
|1930|Uruguay      |Uruguay     |70         |124       |-1.54  |
|1934|Italy        |Italy       |70         |124       |-1.54  |
|1938|France       |Italy       |84         |124       |-1.14  |
|1950|Brazil       |Uruguay     |88         |124       |-1.02  |
|1954|Switerland   |West Germany|140        |124       |0.47   |
|1958|Sweden       |Brazil      |126        |124       |0.07   |
|1962|Chile        |Brazil      |89         |124       |-0.99  |
|1966|England      |England     |89         |124       |-0.99  |
|1970|Mexico       |Brazil      |95         |124       |-0.82  |
|1974|West Germany |West Germany|97         |124       |-0.76  |
|1978|Argentina    |Argentina   |102        |124       |-0.62  |
|1982|Spain        |Italy       |146        |124       |0.64   |
|1986|Mexico       |Argentina   |132        |124       |0.24   |
|1990|Italy        |West Germany|115        |124       |-0.25  |
|1994|United States|Brazil      |141        |124       |0.5    |
|1998|France       |France      |171        |124       |1.36   |
|2002|Korea / Japan|Brazil      |161        |124       |1.07   |
|2006|Germany      |Italy       |147        |124       |0.67   |
|2010|South Africa |Spain       |145        |124       |0.61   |
|2014|Brazil       |Germany     |171        |124       |1.36   |
|2018|Russia       |France      |169        |124       |1.3    |
|2022|Qatar        |Argentina   |172        |124       |1.39   |
```

看起来相当不错！

## PARTITION BY 子句

到目前为止，我一直在使用*分区*这个术语，所以让我们看看这实际上是什么意思。回顾之前的例子，我们没有明确说明希望如何对表进行分区，所以操作是在整个表的所有行上进行的。另一方面，`PARTITION BY`子句被称为*在*`OVER`子句内，它规定了行应该如何分成组或*分区*。包含这个子句后，窗口函数的结构现在看起来像这样：

```py
FUNCTION_NAME() OVER( PARTITION BY [var] )
```

占位符`[var]`指的是用于分组行的列。为了演示，我们再试一次使用`ROW_NUMBER`函数对行进行编号，这次我们将使用`first_place`列来对行进行分区。查看下面的查询：

```py
/* Add row numbers to each partition */
SELECT 
   "year"
   ,host_country
   ,first_place
   ,total_goals
   ,ROW_NUMBER() OVER( PARTITION BY first_place ) AS row_num
FROM world_cup_placings
;
```

```py
|year|host_country |first_place |total_goals|row_num|
|----|-------------|------------|-----------|-------|
|1978|Argentina    |Argentina   |102        |1      |
|1986|Mexico       |Argentina   |132        |2      |
|2022|Qatar        |Argentina   |172        |3      |
|1962|Chile        |Brazil      |89         |1      |
|2002|Korea / Japan|Brazil      |161        |2      |
|1994|United States|Brazil      |141        |3      |
|1958|Sweden       |Brazil      |126        |4      |
|1970|Mexico       |Brazil      |95         |5      |
|1966|England      |England     |89         |1      |
|1998|France       |France      |171        |1      |
|2018|Russia       |France      |169        |2      |
|2014|Brazil       |Germany     |171        |1      |
|1982|Spain        |Italy       |146        |1      |
|1934|Italy        |Italy       |70         |2      |
|1938|France       |Italy       |84         |3      |
|2006|Germany      |Italy       |147        |4      |
|2010|South Africa |Spain       |145        |1      |
|1930|Uruguay      |Uruguay     |70         |1      |
|1950|Brazil       |Uruguay     |88         |2      |
|1974|West Germany |West Germany|97         |1      |
|1954|Switerland   |West Germany|140        |2      |
|1990|Italy        |West Germany|115        |3      |
```

好的，情况与上次看起来非常不同。首先，我们可以看到输出已使用`first_place`列对表进行了排序，但这并不特别有趣。*有趣*的是`row_num`列的变化。从顶部开始向下查看，我们可以看到编号序列在`first_place`的每个不同值上都会重置，只计数与每个国家的分区相关的行。

让我们在查询基础上再做些扩展。除了`row_num`列，我们还计算总进球数和最大总出席人数。我们将再次使用`first_place`列来对表进行分区，因此这些聚合将仅适用于与每个分区相关的行。下面的查询展示了如何做到这一点：

```py
/* Adding more window functions to the SELECT list */
SELECT 
   "year"
   ,host_country 
   ,first_place
   ,ROW_NUMBER() OVER( PARTITION BY first_place ) AS row_num
   ,total_goals
   ,SUM(total_goals) OVER( PARTITION BY first_place ) AS all_goals
   ,total_attendance
   ,MAX(total_attendance) OVER( PARTITION BY first_place ) AS max_attendance
FROM world_cup_placings
;
```

```py
|year|host_country |first_place |row_num|total_goals|all_goals|total_attendance|max_attednance|
|----|-------------|------------|-------|-----------|---------|----------------|--------------|
|1978|Argentina    |Argentina   |1      |102        |406      |1,545,791       |3,404,252     |
|1986|Mexico       |Argentina   |2      |132        |406      |2,394,031       |3,404,252     |
|2022|Qatar        |Argentina   |3      |172        |406      |3,404,252       |3,404,252     |
|1962|Chile        |Brazil      |1      |89         |612      |893,172         |3,597,042     |
|2002|Korea / Japan|Brazil      |2      |161        |612      |2,705,198       |3,597,042     |
|1994|United States|Brazil      |3      |141        |612      |3,597,042       |3,597,042     |
|1958|Sweden       |Brazil      |4      |126        |612      |819,810         |3,597,042     |
|1970|Mexico       |Brazil      |5      |95         |612      |1,604,065       |3,597,042     |
|1966|England      |England     |1      |89         |89       |1,563,135       |1,563,135     |
|1998|France       |France      |1      |171        |340      |2,785,100       |3,031,768     |
|2018|Russia       |France      |2      |169        |340      |3,031,768       |3,031,768     |
|2014|Brazil       |Germany     |1      |171        |171      |3,429,873       |3,429,873     |
|1982|Spain        |Italy       |1      |146        |447      |2,109,723       |3,359,439     |
|1934|Italy        |Italy       |2      |70         |447      |363,000         |3,359,439     |
|1938|France       |Italy       |3      |84         |447      |374,835         |3,359,439     |
|2006|Germany      |Italy       |4      |147        |447      |3,359,439       |3,359,439     |
|2010|South Africa |Spain       |1      |145        |145      |3,178,856       |3,178,856     |
|1930|Uruguay      |Uruguay     |1      |70         |158      |590,549         |1,045,246     |
|1950|Brazil       |Uruguay     |2      |88         |158      |1,045,246       |1,045,246     |
|1974|West Germany |West Germany|1      |97         |352      |1,865,762       |2,516,215     |
|1954|Switerland   |West Germany|2      |140        |352      |768,607         |2,516,215     |
|1990|Italy        |West Germany|3      |115        |352      |2,516,215       |2,516,215     |
```

现在，这些聚合本身并不是特别有用，它们只是为了演示目的。但让我们看看从这个查询中实际得到的结果。

首先，`all_goals`列提供了每个获胜国家在所有比赛中进球的总数（回忆一下，我们已使用`first_place`列对行进行了分区）。例如，在阿根廷获胜的比赛中，我们可以看到在1978年阿根廷主办时进了102球，1986年墨西哥主办时进了132球，最近在卡塔尔进了172球。`all_goals`值就是这两个值的总和，即406。

其次，`max_attendance`值返回每个获胜国家的最高比赛出席人数。例如，在表中列出的世界杯中，意大利赢得了其中四场，其中2006年在德国举办的世界杯的最高出席人数（3,359,439）。因此，对于所有意大利获胜的行，这就是`max_attendance`返回的值。

回顾查询，你可能会注意到我们需要写`OVER( PARTITION BY first_place )`三次；每个窗口函数在`SELECT`列表中出现一次。如果你的查询需要多个窗口函数，并且使用相同的列进行分区，这可能会变得有些乏味。那么有没有更好的方法来实现相同的目标？有的。确实有。在这些情况下——所有函数的窗口设置相同——我们可以使用一个单独的`WINDOW`子句来定义分区，并为其指定一个可以通过`OVER`调用的名称。查看下面的查询：

```py
/* Same as above, but now using the WINDOW clause */
SELECT 
   "year"
   ,host_country 
   ,first_place
   ,ROW_NUMBER() OVER w AS row_num
   ,total_goals
   ,SUM(total_goals) OVER w AS all_goals
   ,total_attendance
   ,MAX(total_attendance) OVER w AS max_attendance
FROM world_cup_placings
WINDOW w AS ( PARTITION BY first_place )
;
```

现在，这并不会消除在`OVER`子句后提供参数的需要——这是无法避免的——但这种方法出错的可能性要小得多，并且确实有助于清理查询。你可以检查结果是否与上面的输出一致。

## `ORDER BY`子句

另一个可以添加到`OVER`中的子句是`ORDER BY`子句。这个子句肯定很熟悉，实际上它的工作方式完全符合你的预期。唯一的不同是，当我们在`OVER`子句中使用它时，它会影响每个分区内的排序。包含这个子句后，我们的窗口函数现在看起来是这样的：

```py
FUNCTION_NAME() OVER( PARTITION BY [var] ORDER BY [val] )
```

我们可以使用`ORDER BY`子句在执行操作之前对值进行排序。如果我们想使用`RANK`函数对值进行排名，这非常有用，因为`RANK`函数会为每个*不同*的`ORDER BY`值分配一个数值。这意味着它的行为与`ROW_NUMBER`函数略有不同，因为`RANK`函数会为重复值分配相同的排名。查看下面的查询并比较行输出：

```py
/* Comparing ROW_NUMBER and RANK using ORDER BY clause */
SELECT 
   "year"
   ,host_country
   ,first_place
   ,total_goals
   ,ROW_NUMBER() OVER( ORDER BY total_goals DESC ) AS row_num
   ,RANK() OVER( ORDER BY total_goals DESC ) AS row_rank
FROM world_cup_placings
;
```

```py
|year|host_country |first_place |total_goals|row_num|row_rank|
|----|-------------|------------|-----------|-------|--------|
|2022|Qatar        |Argentina   |172        |1      |1       |
|1998|France       |France      |171        |2      |2       |
|2014|Brazil       |Germany     |171        |3      |2       |
|2018|Russia       |France      |169        |4      |4       |
|2002|Korea / Japan|Brazil      |161        |5      |5       |
|2006|Germany      |Italy       |147        |6      |6       |
|1982|Spain        |Italy       |146        |7      |7       |
|2010|South Africa |Spain       |145        |8      |8       |
|1994|United States|Brazil      |141        |9      |9       |
|1954|Switerland   |West Germany|140        |10     |10      |
|1986|Mexico       |Argentina   |132        |11     |11      |
|1958|Sweden       |Brazil      |126        |12     |12      |
|1990|Italy        |West Germany|115        |13     |13      |
|1978|Argentina    |Argentina   |102        |14     |14      |
|1974|West Germany |West Germany|97         |15     |15      |
|1970|Mexico       |Brazil      |95         |16     |16      |
|1966|England      |England     |89         |17     |17      |
|1962|Chile        |Brazil      |89         |18     |17      |
|1950|Brazil       |Uruguay     |88         |19     |19      |
|1938|France       |Italy       |84         |20     |20      |
|1930|Uruguay      |Uruguay     |70         |21     |21      |
|1934|Italy        |Italy       |70         |22     |21      |
```

看到了区别吗？如果我们查看第二行和第三行，我们会看到巴西2014和法国1998都得到了171个进球。虽然`row_num`列为这些行分配了不同的值，但`row_rank`列则分配了相同的排名。

需要注意的是，对于`ROW_NUMBER`窗口函数，我实际上不需要指定`ORDER BY total_goals DESC`，但我这样做是为了强调`ROW_NUMBER`窗口函数不关心重复值；它只是为每一行编号，不管是否按`total_goals`排序。

好的，现在让我们尝试将`ORDER BY`和`PARTITION BY`子句一起使用，以找出每个团队最后一次赢得世界杯的年份。首先，我们将使用`first_place`列来对表进行分区，然后使用`year`列按降序对行进行排序。以下查询展示了如何实现这一点：

```py
/* Rank winning teams by competition year to find most recent win */
SELECT 
   "year"
   ,host_country
   ,first_place
   ,RANK() OVER( PARTITION BY first_place ORDER BY "year" DESC ) AS row_rank
FROM world_cup_placings
;
```

```py
|year|host_country |first_place |row_rank|
|----|-------------|------------|--------|
|2022|Qatar        |Argentina   |1       |
|1986|Mexico       |Argentina   |2       |
|1978|Argentina    |Argentina   |3       |
|2002|Korea / Japan|Brazil      |1       |
|1994|United States|Brazil      |2       |
|1970|Mexico       |Brazil      |3       |
|1962|Chile        |Brazil      |4       |
|1958|Sweden       |Brazil      |5       |
|1966|England      |England     |1       |
|2018|Russia       |France      |1       |
|1998|France       |France      |2       |
|2014|Brazil       |Germany     |1       |
|2006|Germany      |Italy       |1       |
|1982|Spain        |Italy       |2       |
|1938|France       |Italy       |3       |
|1934|Italy        |Italy       |4       |
|2010|South Africa |Spain       |1       |
|1950|Brazil       |Uruguay     |1       |
|1930|Uruguay      |Uruguay     |2       |
|1990|Italy        |West Germany|1       |
|1974|West Germany |West Germany|2       |
|1954|Switerland   |West Germany|3       |
```

通过按降序排列比赛年份，我们确保每个分区的第一行是最新的年份。现在，由于每年只能有一个获胜者，因此`year`列中没有重复值。因此，当我们为每个分区分配排名时，第一行将始终具有排名1，这对应于每个国家的最新胜利。

然而，这个输出有点杂乱，我们必须阅读每一行以找出每个分区的排名。我们可以通过将上述查询放在子查询中，并过滤出仅具有排名1的行来整理一下。查看下面的查询以了解实际效果：

```py
/* Filter only rows that have a rank of 1 */
SELECT 
   "year"
   ,first_place
FROM (
   SELECT 
     "year"
     ,host_country
     ,first_place
     ,RANK() OVER( PARTITION BY first_place ORDER BY "year" DESC ) AS row_rank
   FROM world_cup_placings
) AS subq
WHERE row_rank = 1
;
```

```py
|year|first_place |
|----|------------|
|2022|Argentina   |
|2002|Brazil      |
|1966|England     |
|2018|France      |
|2014|Germany     |
|2006|Italy       |
|2010|Spain       |
|1950|Uruguay     |
|1990|West Germany|
```

## 最终备注

窗口函数非常灵活，当需要仅使用子集行来计算值时，它们通常更加高效。此帖仅触及窗口函数的表面，并仅演示了它们的基本功能。不管怎样，希望你在此帖中找到了一些有用的内容。在后续的帖子中，我们将基于这些概念，探索如何使用窗口函数计算一些更为复杂的度量。

## 相关文章

+   [理解 SQL：执行顺序](https://medium.com/towards-data-science/understanding-sql-order-of-execution-ba2b4e558828)

感谢阅读！

如果你喜欢这篇文章并希望保持更新，请考虑[关注我的 Medium 账号](https://medium.com/@dataforyou)。这将确保你不会错过任何新内容。

若要无限制访问所有内容，请考虑注册[Medium 订阅](https://medium.com/membership)。

你还可以在[Twitter](https://twitter.com/dataforyounz)、[LinkedIn](https://www.linkedin.com/in/dataforyou/)上关注我，或者查看我的[GitHub](https://github.com/dataforyounz)，如果这更符合你的兴趣。
