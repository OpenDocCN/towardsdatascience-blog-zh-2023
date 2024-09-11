# Tabyl：现代 R 用户的频率表格

> 原文：[https://towardsdatascience.com/tabyl-a-frequency-table-for-the-modern-r-user-e061cd48baef?source=collection_archive---------4-----------------------#2023-05-20](https://towardsdatascience.com/tabyl-a-frequency-table-for-the-modern-r-user-e061cd48baef?source=collection_archive---------4-----------------------#2023-05-20)

## 旧的已过时，新的正在兴起！

[](https://medium.com/@zvonimir.boban.mef?source=post_page-----e061cd48baef--------------------------------)[![兹沃尼米尔·博班](../Images/cbad06b7e1f5d021ce9b2dc31b8a6a65.png)](https://medium.com/@zvonimir.boban.mef?source=post_page-----e061cd48baef--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e061cd48baef--------------------------------)[![向数据科学迈进](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e061cd48baef--------------------------------) [兹沃尼米尔·博班](https://medium.com/@zvonimir.boban.mef?source=post_page-----e061cd48baef--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe31978768a4e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftabyl-a-frequency-table-for-the-modern-r-user-e061cd48baef&user=Zvonimir+Boban&userId=e31978768a4e&source=post_page-e31978768a4e----e061cd48baef---------------------post_header-----------) 发表在 [向数据科学迈进](https://towardsdatascience.com/?source=post_page-----e061cd48baef--------------------------------) ·6 分钟阅读·2023 年 5 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe061cd48baef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftabyl-a-frequency-table-for-the-modern-r-user-e061cd48baef&user=Zvonimir+Boban&userId=e31978768a4e&source=-----e061cd48baef---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe061cd48baef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftabyl-a-frequency-table-for-the-modern-r-user-e061cd48baef&source=-----e061cd48baef---------------------bookmark_footer-----------)![](../Images/d48ea5e85a16b163c6b27c381d3e8e90.png)

使用 Canva 图像生成器创建的图像

任何处理分类数据的人最终都会遇到计算某个类别的绝对数和比例的需要。本文介绍了通过一系列实际示例使用 `tabyl` 函数创建频率表格。

## `tabyl` 为表格带来了什么（没有刻意捣乱的意思 :D）？

`tabyl` 函数是 R 语言中 `janitor` 包的一个特性。它是创建列联表的非常方便的工具，也被称为频率表或交叉制表表格。以下是使用 `tabyl` 的一些好处：

1\. 简单的语法：`tabyl` 具有易于使用的语法。它可以接受一个、两个或三个变量，并自动返回一个包含计数和比例的数据框。

2\. 灵活性：`tabyl` 可以生成单向（单变量）、双向（双变量）和三向（三变量）列联表。这种灵活性使其适用于各种应用场景。

3\. 自动计算比例：`tabyl` 自动计算单向列联表的比例（百分比）。对于双向和三向表，可以结合使用同一包中的 `adorn_percentages` 函数来实现相同的结果。

4\. 与 `dplyr` 的兼容性：`tabyl` 的输出是数据框（或 tibble），这使其完全兼容 `dplyr` 函数和 tidyverse 生态系统。这意味着你可以轻松地将 `%>%` 管道操作符应用于进一步的数据处理或可视化功能。

5\. 整洁且信息丰富的输出：`tabyl` 提供整洁且信息丰富的输出，包括将变量名作为行名和列名，这使得结果更易于解释。

基于以上所有原因，当你想在 R 中创建频率表时，`tabyl` 是一个很好的选择。它简化了许多步骤，并且与 tidyverse 数据分析方法很好地集成。

## 数据集

![](../Images/52c15143fee6d189481988be0cd4dfdb.png)

由 [Hans Veth](https://unsplash.com/@hans_veth?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/mushroom?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

本文将使用关于不同类型蘑菇的气味的可食性数据来演示 `tabyl` 函数的优点。在这里，我将使用一个名为 mushrooms 的整理数据集，但你可以访问 Kaggle 上的[原始数据](https://www.kaggle.com/datasets/uciml/mushroom-classification?resource=download)。以下是用于清理数据的代码。

```py
library(tidyverse)
library(janitor)

mushrooms <- read_csv("mushrooms.csv") %>%
  select(class, odor) %>%
  mutate(
    class = case_when(
      class == "p" ~ "poisonous",
      class == "e" ~ "edible"
    ),
    odor = case_when(
      odor == "a" ~ "almond",
      odor == "l" ~ "anise",
      odor == "c" ~ "creosote",
      odor == "y" ~ "fishy",
      odor == "f" ~ "foul",
      odor == "m" ~ "musty",
      odor == "n" ~ "none",
      odor == "p" ~ "pungent",
      odor == "s" ~ "spicy"
    )
  )
```

如果你对上述语法不熟悉，请查看我早期文章中的 tidyverse 实用指南。

[](/diving-into-the-tidyverse-using-the-titanic-data-83f54295d5df?source=post_page-----e061cd48baef--------------------------------) [## 使用泰坦尼克号数据深入了解 tidyverse

[towardsdatascience.com](/diving-into-the-tidyverse-using-the-titanic-data-83f54295d5df?source=post_page-----e061cd48baef--------------------------------)

## 旧的

为了更好地理解 `tabyl` 提供了哪些优势，让我们首先使用基础 R 中的 `table` 函数创建一个频率表。

```py
table(mushrooms$class)

   edible poisonous 
     4208      3916 
```

```py
table(mushrooms$odor, mushrooms$class)

            edible poisonous
  almond      400         0
  anise       400         0
  creosote      0       192
  fishy         0       576
  foul          0      2160
  musty         0        36
  none       3408       120
  pungent       0       256
  spicy         0       576
```

毫不奇怪，气味竟然是预测蘑菇可食性的一个重要指标，任何“有怪味”的蘑菇可能都是有毒的。谢谢进化！此外，似乎还有很多更多的有毒蘑菇，因此在自己采摘蘑菇时始终要小心谨慎。

如果我们希望直接使用变量名而不指定`$`运算符，我们需要使用`with`命令使数据集对`table`函数可用。

```py
mush_table <- with(mushrooms, table(odor, class))
```

不幸的是，如果我们想升级到比例而不是绝对数，我们不能使用相同的函数，而是要使用另一个函数——`prop.table`。

```py
prop.table(mush_table)

class
odor            edible   poisonous
  almond   0.049236829 0.000000000
  anise    0.049236829 0.000000000
  creosote 0.000000000 0.023633678
  fishy    0.000000000 0.070901034
  foul     0.000000000 0.265878877
  musty    0.000000000 0.004431315
  none     0.419497784 0.014771049
  pungent  0.000000000 0.031511571
  spicy    0.000000000 0.070901034
```

默认情况下，这会给我们一个按列的比例表。如果我们想要按行的比例，我们可以指定`margin`参数（1表示按行，2表示按列）。

```py
prop.table(mush_table, margin = 1)

class
odor           edible  poisonous
  almond   1.00000000 0.00000000
  anise    1.00000000 0.00000000
  creosote 0.00000000 1.00000000
  fishy    0.00000000 1.00000000
  foul     0.00000000 1.00000000
  musty    0.00000000 1.00000000
  none     0.96598639 0.03401361
  pungent  0.00000000 1.00000000
  spicy    0.00000000 1.00000000
```

所有这些特殊函数可能会感觉繁琐且难以记住，因此拥有一个包含所有上述功能的单一函数将是很好的。

此外，如果我们使用`class(mush_table)`命令检查创建的对象类型，我们会发现它属于`table`类。

这创建了一个兼容性问题，因为如今R用户大多使用tidyverse生态系统，该系统以将函数应用于`data.frame`类型对象并通过管道（`%>%`）运算符串联结果为中心。

## 新的

让我们用`tabyl`函数做同样的事情。

```py
tabyl(mushrooms, class)

     class    n   percent
    edible 4208 0.5179714
 poisonous 3916 0.4820286
```

```py
mush_tabyl <- tabyl(mushrooms, odor, class)
mush_tabyl

     odor edible poisonous
   almond    400         0
    anise    400         0
 creosote      0       192
    fishy      0       576
     foul      0      2160
    musty      0        36
     none   3408       120
  pungent      0       256
    spicy      0       576
```

与相应的`table`输出相比，使用`tabyl`函数生成的表格更整洁，变量名（类别）被明确指出。此外，对于一维表格，除了数字，百分比也会自动生成。

我们还可以注意到，我们不需要使用which函数就可以直接指定变量名。此外，运行`class(mush_tabyl)`告诉我们生成的对象是`data.frame`类，这确保了与tidyverse的兼容性！

## 装饰过的janitor

![](../Images/2cc78ca4b68b0faf12ec63349517fdd7.png)

使用 Canva 图像生成器创建的图像

为了额外的`tabyl`功能，`janitor`包还包含了一系列`adorn`函数。要获取百分比，我们只需将生成的频率表传递给`adorn_percentages`函数。

```py
mush_tabyl %>% adorn_percentages()

odor    edible  poisonous
   almond 1.0000000 0.00000000
    anise 1.0000000 0.00000000
 creosote 0.0000000 1.00000000
    fishy 0.0000000 1.00000000
     foul 0.0000000 1.00000000
    musty 0.0000000 1.00000000
     none 0.9659864 0.03401361
  pungent 0.0000000 1.00000000
    spicy 0.0000000 1.00000000
```

如果我们想要按列的百分比，我们可以将`denominator`参数指定为“col”。

```py
mush_tabyl %>% adorn_percentages(denominator = "col")

     odor     edible   poisonous
   almond 0.09505703 0.000000000
    anise 0.09505703 0.000000000
 creosote 0.00000000 0.049029622
    fishy 0.00000000 0.147088866
     foul 0.00000000 0.551583248
    musty 0.00000000 0.009193054
     none 0.80988593 0.030643514
  pungent 0.00000000 0.065372829
    spicy 0.00000000 0.147088866
```

`tabyl` — `adorn`组合甚至使我们能够轻松地将数量和百分比结合在同一表格单元格中…

```py
mush_tabyl %>% adorn_percentages %>% adorn_ns

     odor           edible         poisonous
   almond 1.0000000  (400) 0.00000000    (0)
    anise 1.0000000  (400) 0.00000000    (0)
 creosote 0.0000000    (0) 1.00000000  (192)
    fishy 0.0000000    (0) 1.00000000  (576)
     foul 0.0000000    (0) 1.00000000 (2160)
    musty 0.0000000    (0) 1.00000000   (36)
     none 0.9659864 (3408) 0.03401361  (120)
  pungent 0.0000000    (0) 1.00000000  (256)
    spicy 0.0000000    (0) 1.00000000  (576)
```

… 或将总计添加到行和列。

```py
mush_tabyl %>% adorn_totals(c("row", "col"))

odor edible poisonous Total
   almond    400         0   400
    anise    400         0   400
 creosote      0       192   192
    fishy      0       576   576
     foul      0      2160  2160
    musty      0        36    36
     none   3408       120  3528
  pungent      0       256   256
    spicy      0       576   576
    Total   4208      3916  8124
```

## 结论

R中`janitor`包的`tabyl()`函数提供了一个用户友好且灵活的解决方案，用于创建一维、二维或三维列联表。它在自动计算比例和生成整洁的数据框方面表现出色，能够与tidyverse生态系统，特别是`dplyr`，无缝集成。它的输出结构良好且易于解释，并且可以通过adorn函数进一步增强，从而简化生成信息频率表的整体过程。这使得`tabyl()`在R的数据分析中成为一个非常有用的工具。
