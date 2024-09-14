# 5 个函数是管理数据所需的全部工具

> 原文：[`towardsdatascience.com/5-functions-is-all-you-need-to-manage-your-data-with-dplyr-1630825c47b0`](https://towardsdatascience.com/5-functions-is-all-you-need-to-manage-your-data-with-dplyr-1630825c47b0)

## 如何高效地使你的数据准备就绪

[](https://sonery.medium.com/?source=post_page-----1630825c47b0--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----1630825c47b0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1630825c47b0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1630825c47b0--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----1630825c47b0--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1630825c47b0--------------------------------) ·8 分钟阅读·2023 年 3 月 10 日

--

![](img/37b7af736531d661d732e23977fce5dd.png)

照片由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/-1_RZL8BGBM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

那些处理过真实数据的任务的人知道，大部分的工作都集中在数据整理上。

我所说的数据整理包括将数据准备好以供其他利益相关者或下游过程使用的操作。

无论你是数据分析师、数据科学家，还是数据工程师，你都需要在日常工作中执行以下一种或多种操作：

+   过滤（例如，给我德克萨斯州的销售数据）

+   排序（例如，我想查看上周的前 10 名畅销产品）

+   更新（例如，更改这些产品的类别）

+   总结（例如，我想查看每个类别的平均收入）

数据分析和操作工具存在于数据科学生态系统中，以提供高效的操作方式，从而在生态系统中保持其存在。

在这篇文章中，我们将学习如何使用数据科学中主要工具之一：dplyr 来处理这些任务。

这是一个针对 R 编程语言的包，被描述为“数据操作的语法”。

我们可以单独安装 dplyr，也可以使用 [tidyverse](https://www.tidyverse.org/)，这是一个用于数据科学的 R 包集合。我更喜欢后者，因为它允许我使用 tidyverse 中其他包的一些函数（例如 read_csv 来自 readr）。

让我们从导入库和读取数据集开始。我们将使用我准备的模拟数据的样本数据集。你可以从我的 [datasets](https://github.com/SonerYldrm/datasets) 仓库下载它。

```py
library(tidyverse)

sales <- read_csv("sales_data_with_stores.csv")

# display the first 6 rows
head(sales)

# A tibble: 6 x 8
  store  product_group product_code stock_qty   cost   price last_week_sales last_month_sales
  <chr>  <chr>                <dbl>     <dbl>  <dbl>   <dbl>           <dbl>            <dbl>
1 Violet PG2                   4187       498 421\.    570\.                13               58
2 Rose   PG2                   4195       473 546\.    712\.                16               58
3 Violet PG2                   4204       968 640\.    855\.                22               88
4 Daisy  PG2                   4219       241 870\.   1035\.                14               45
5 Daisy  PG2                   4718      1401  12.5    26.6               50              285
6 Violet PG4                   5630       287   5.85    7.59              24              116
```

结果对象是一个 tibble，类似于 DataFrame 或表格。

所有数据处理挑战都可以归结为使用以下 5 个函数中的一个或多个：

+   select

+   mutate

+   filter

+   arrange

+   summarise

# **1\. 选择**

select 函数可用于选择列。它允许我们通过保留或删除列来进行选择，依据列的名称和类型。

让我们通过几个示例来了解 select 的工作方式。以下示例展示了如何选择特定的列。

```py
# select product_code and price columns
select(sales, product_code, price)

# A tibble: 1,000 x 2
   product_code   price
          <dbl>   <dbl>
 1         4187  570\.  
 2         4195  712\.  
 3         4204  855\.  
 4         4219 1035\.  
 5         4718   26.6 

# the following does the same
select(sales, c(product_code, price))
```

我们也可以通过排除（删除）一个或多个列来进行选择，方法是使用“！”：

```py
# select all but product_group column
select(sales, !product_group)

# A tibble: 1,000 x 7
   store  product_code stock_qty   cost   price last_week_sales last_month_sales
   <chr>         <dbl>     <dbl>  <dbl>   <dbl>           <dbl>            <dbl>
 1 Violet         4187       498 421\.    570\.                13               58
 2 Rose           4195       473 546\.    712\.                16               58
 3 Violet         4204       968 640\.    855\.                22               88
 4 Daisy          4219       241 870\.   1035\.                14               45
 5 Daisy          4718      1401  12.5    26.6               50              285
```

如果我们选择多个连续的列，使用“:”定义范围会更方便。

```py
# select all columns from store to cost
select(sales, store:cost)

# A tibble: 1,000 x 5
   store  product_group product_code stock_qty   cost
   <chr>  <chr>                <dbl>     <dbl>  <dbl>
 1 Violet PG2                   4187       498 421\.  
 2 Rose   PG2                   4195       473 546\.  
 3 Violet PG2                   4204       968 640\.  
 4 Daisy  PG2                   4219       241 870\.  
 5 Daisy  PG2                   4718      1401  12.5 
```

我们还可以使用列索引，因此以下操作与上述操作相同：

```py
# select all columns from store to cost
select(sales, 1:5)
```

我们还可以根据列的数据类型选择列。例如，以下示例选择了数值列。

```py
# select all numeric columns
select(sales, where(is.numeric))

# A tibble: 1,000 x 6
   product_code stock_qty   cost   price last_week_sales last_month_sales
          <dbl>     <dbl>  <dbl>   <dbl>           <dbl>            <dbl>
 1         4187       498 421\.    570\.                13               58
 2         4195       473 546\.    712\.                16               58
 3         4204       968 640\.    855\.                22               88
 4         4219       241 870\.   1035\.                14               45
 5         4718      1401  12.5    26.6               50              285
```

通过使用相同的谓词函数（即 where），我们可以选择非数字列，如下所示：

```py
# select all non-numeric columns
select(sales, !where(is.numeric))

# A tibble: 1,000 x 2
   store  product_group
   <chr>  <chr>        
 1 Violet PG2          
 2 Rose   PG2          
 3 Violet PG2          
 4 Daisy  PG2          
 5 Daisy  PG2 
```

# 2\. mutate

mutate 函数顾名思义，通过更新现有列或创建新列来修改 tibble。

例如，我们可以将价格值增加 10%：

```py
# increase price by 10 percent
mutate(sales, price = price * 1.1)

# A tibble: 1,000 x 8
   store  product_group product_code stock_qty   cost   price last_week_sales last_month_sales
   <chr>  <chr>                <dbl>     <dbl>  <dbl>   <dbl>           <dbl>            <dbl>
 1 Violet PG2                   4187       498 421\.    627\.                13               58
 2 Rose   PG2                   4195       473 546\.    784\.                16               58
 3 Violet PG2                   4204       968 640\.    940\.                22               88
 4 Daisy  PG2                   4219       241 870\.   1138\.                14               45
 5 Daisy  PG2                   4718      1401  12.5    29.2               50              285
```

我们还可以创建新列：

```py
# create price_updated column by increasing price by 10 percent
mutate(sales, price_updated = price * 1.1)

# A tibble: 1,000 x 9
   store  product_group product_code stock_qty   cost   price last_week_sales last_month_sales price_updated
   <chr>  <chr>                <dbl>     <dbl>  <dbl>   <dbl>           <dbl>            <dbl>         <dbl>
 1 Violet PG2                   4187       498 421\.    570\.                13               58        627\.  
 2 Rose   PG2                   4195       473 546\.    712\.                16               58        784\.  
 3 Violet PG2                   4204       968 640\.    855\.                22               88        940\.  
 4 Daisy  PG2                   4219       241 870\.   1035\.                14               45       1138\.  
 5 Daisy  PG2                   4718      1401  12.5    26.6               50              285         29.2
```

多次更新可以在一次操作中完成。在做一个示例演示这种情况之前，我们先提到两个非常有用的功能：

+   我们可以使用“%>%”运算符将不同类型的操作组合在一起，以创建管道并解决涉及多个步骤的复杂任务。

+   新列会立即可用，因此我们可以在同一个 mutate 函数中使用它们。

这里是一个示例，展示了上述两个功能：

```py
> sales %>%
+     select(cost, price) %>%
+     mutate(
+         price_updated = price * 1.1,
+         profit_updated = price_updated - cost)

# A tibble: 1,000 x 4
     cost   price price_updated profit_updated
    <dbl>   <dbl>         <dbl>          <dbl>
 1 421\.    570\.          627\.           206\.  
 2 546\.    712\.          784\.           238\.  
 3 640\.    855\.          940\.           300\.  
 4 870\.   1035\.         1138\.           268\.  
 5  12.5    26.6          29.2           16.7 
```

在上述示例中，我们首先从 sales 中选择价格和成本列，并从价格列创建 price_updated 列，然后使用这个新列创建 profit_updated 列。

# 3\. 筛选

filter 函数允许我们根据条件或条件集筛选观察值（即行）。

以下示例筛选出价格超过 1000 的行。

```py
filter(sales, price > 1000)

# A tibble: 5 x 8
  store  product_group product_code stock_qty  cost price last_week_sales last_month_sales
  <chr>  <chr>                <dbl>     <dbl> <dbl> <dbl>           <dbl>            <dbl>
1 Daisy  PG2                   4219       241  870\. 1035\.              14               45
2 Violet PG1                   9692        68 1243  1500\.              26               94
3 Violet PG1                   7773       602  976\. 1325\.              19               60
4 Daisy  PG1                   1941       213  847  1177\.              18               72
5 Daisy  PG1                   4140        92  803  1202\.              12               24
```

让我们使用一个更复杂的筛选条件：

```py
# rows with a price of more than 1000 and store is Daisy
filter(sales, price > 1000 & store == "Daisy")

# A tibble: 3 x 8
  store product_group product_code stock_qty  cost price last_week_sales last_month_sales
  <chr> <chr>                <dbl>     <dbl> <dbl> <dbl>           <dbl>            <dbl>
1 Daisy PG2                   4219       241  870\. 1035\.              14               45
2 Daisy PG1                   1941       213  847  1177\.              18               72
3 Daisy PG1                   4140        92  803  1202\.              12               24
```

如果我们有一个包含多个值的条件，可以使用“%in%”运算符。

```py
# rows with product group of PG3, PG4, or PG5 and store is Daisy
filter(sales, product_group %in% c("PG3", "PG4", "PG5") & store == "Daisy")

# A tibble: 302 x 8
   store product_group product_code stock_qty  cost price last_week_sales last_month_sales
   <chr> <chr>                <dbl>     <dbl> <dbl> <dbl>           <dbl>            <dbl>
 1 Daisy PG4                   5634       205 14.2   18.0              14               53
 2 Daisy PG4                   2650       239 59.4  111\.               15               38
 3 Daisy PG4                   5647       352  5.85  13.3              37              108
 4 Daisy PG4                   5693       260  7.62  13.3              19               74
 5 Daisy PG4                   5696       260  7.62  13.3              29               98
```

# 4\. 排序

arrange 函数根据列中的值对行进行排序。它类似于 SQL 的 order by 和 Pandas 的 sort_values 函数。

```py
# order rows by price
arrange(sales, price)

# A tibble: 1,000 x 8
   store  product_group product_code stock_qty  cost price last_week_sales last_month_sales
   <chr>  <chr>                <dbl>     <dbl> <dbl> <dbl>           <dbl>            <dbl>
 1 Violet PG4                   5454       -22 0.570  0.66              31               28
 2 Violet PG5                   5621      -123 1.32   0.76              49              100
 3 Daisy  PG4                   2279       525 1.34   1.23              14               18
 4 Rose   PG2                   9372       350 1.41   1.42              16               40
 5 Rose   PG4                   2138       482 1.63   1.61              13               23
```

默认情况下，排序是按升序进行的。我们可以通过在列名之前添加“ -”来将其更改为降序。

```py
# order rows by price descending
arrange(sales, -price)

# A tibble: 1,000 x 8
   store  product_group product_code stock_qty  cost price last_week_sales last_month_sales
   <chr>  <chr>                <dbl>     <dbl> <dbl> <dbl>           <dbl>            <dbl>
 1 Violet PG1                   9692        68 1243  1500\.              26               94
 2 Violet PG1                   7773       602  976\. 1325\.              19               60
 3 Daisy  PG1                   4140        92  803  1202\.              12               24
 4 Daisy  PG1                   1941       213  847  1177\.              18               72
 5 Daisy  PG2                   4219       241  870\. 1035\.              14               45
```

要按多个列排序，我们可以用逗号分隔列名。以下示例按商店名称排序，然后按 last_week_sales（降序）排序。

```py
arrange(sales, store, -last_week_sales)

# A tibble: 1,000 x 8
   store product_group product_code stock_qty  cost price last_week_sales last_month_sales
   <chr> <chr>                <dbl>     <dbl> <dbl> <dbl>           <dbl>            <dbl>
 1 Daisy PG6                    856     52748 31.4  38.0             1883             6880
 2 Daisy PG3                   2481     10543  8.25 14.2              947             4100
 3 Daisy PG6                   3957     10090 26.9  31.3              867             2355
 4 Daisy PG6                    889     21569 13.0  16.1              808             2990
 5 Daisy PG6                   9635     26576  8.53 11.4              673             2484
```

# 5\. 总结

它将组汇总为一行。结合 group_by 函数，我们可以使用它来计算组的聚合值。这一操作类似于 SQL 的 group by 和 Pandas 的 groupby 函数。

```py
# group by rows in sales by store
by_store <- group_by(sales, store)

# calculate the avg price for each store
summarise(by_store, avg_price = mean(price))

# A tibble: 3 x 2
  store  avg_price
* <chr>      <dbl>
1 Daisy       69.3
2 Rose        60.5
3 Violet      67.8
```

有几个聚合函数可以从数据中提取见解并进行深入分析。

这是来自官方文档的列表：

+   中心: `[mean()](https://rdrr.io/r/base/mean.html)`、`[median()](https://rdrr.io/r/stats/median.html)`

+   分布: `[sd()](https://rdrr.io/r/stats/sd.html)`、`[IQR()](https://rdrr.io/r/stats/IQR.html)`、`[mad()](https://rdrr.io/r/stats/mad.html)`

+   范围: `[min()](https://rdrr.io/r/base/Extremes.html)`、`[max()](https://rdrr.io/r/base/Extremes.html)`，

+   位置: `[first()](https://dplyr.tidyverse.org/reference/nth.html)`、`[last()](https://dplyr.tidyverse.org/reference/nth.html)`、`[nth()](https://dplyr.tidyverse.org/reference/nth.html)`，

+   计数: `[n()](https://dplyr.tidyverse.org/reference/context.html)`、`[n_distinct()](https://dplyr.tidyverse.org/reference/n_distinct.html)`

+   逻辑: `[any()](https://rdrr.io/r/base/any.html)`、`[all()](https://rdrr.io/r/base/all.html)`

本文中涵盖的函数可以帮助你完成几乎所有的数据处理和分析工作。将它们一起使用在管道中提供了更大的灵活性。

*你可以成为* [*Medium 会员*](https://sonery.medium.com/membership) *来解锁我写作的全部内容，以及 Medium 的其他内容。如果你已经是会员，请不要忘记* [*订阅*](https://sonery.medium.com/subscribe) *，这样你会在我发布新文章时收到电子邮件。*

感谢你的阅读。请告诉我你的反馈意见。
