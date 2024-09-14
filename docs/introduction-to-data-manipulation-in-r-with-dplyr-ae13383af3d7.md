# 使用{dplyr}进行 R 的数据操作入门

> 原文：[`towardsdatascience.com/introduction-to-data-manipulation-in-r-with-dplyr-ae13383af3d7`](https://towardsdatascience.com/introduction-to-data-manipulation-in-r-with-dplyr-ae13383af3d7)

## 学习如何使用 R 中的{dplyr}包，它帮助你解决最常见的数据操作挑战

[](https://antoinesoetewey.medium.com/?source=post_page-----ae13383af3d7--------------------------------)![Antoine Soetewey](https://antoinesoetewey.medium.com/?source=post_page-----ae13383af3d7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ae13383af3d7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ae13383af3d7--------------------------------) [Antoine Soetewey](https://antoinesoetewey.medium.com/?source=post_page-----ae13383af3d7--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ae13383af3d7--------------------------------) ·27 分钟阅读·2023 年 11 月 27 日

--

![](img/34f6144f88eb797315f364b70afbc1de.png)

照片由[Claudio Schwarz](https://unsplash.com/@purzlbaum?utm_source=medium&utm_medium=referral)提供

# 介绍

在之前的帖子中，我们展示了如何[在 R 中操作数据](https://statsandr.com/blog/data-manipulation-in-r/)。特别是，我们介绍了如何创建和操作向量、因子、列表和数据框。这是对 R 的介绍，面向初学者。此外，只要可能，所有操作都是在基础 R 中完成的，即不需要加载任何包。

在这篇帖子中，我们将再次展示如何在 R 中操作数据，但这次使用的是`{dplyr}`包。

`{dplyr}`包由 Hadley Wickham 及 posit 团队开发，提供了一整套功能，帮助你解决最常见的数据操作挑战，例如：

+   基于值过滤观测数据

+   基于值或位置提取观测数据

+   基于特定数量或比例的行进行观测抽样

+   根据一个或多个变量对观测数据进行排序

+   根据变量的名称或位置选择变量

+   重新命名变量

+   基于现有变量添加新变量

+   将观测数据或变量汇总到一个描述性指标

+   按组执行任何操作

+   将观测数据分类为两个或多个组

+   等等

关于该包的更多信息，请访问[dplyr.tidyverse.org](https://dplyr.tidyverse.org/)。

在这篇文章中，我们将介绍使用`{dplyr}`包进行数据操作和数据管理的最常见函数（在数据框上进行说明）。然而，这并不是一个详尽的列表！你可能会需要其他未在此处介绍的函数。对于感兴趣的读者，请参见文章末尾的更多资源。

我经常被问到的问题是，是否最好先学习基础 R 的数据操作，然后再学习`{dplyr}`，还是直接学习`{dplyr}`。

也许每个人不一定同意，但这是我倾向于回答的。`{dplyr}`在效率和代码的清晰度/可读性方面的优势是显而易见的。因此，即使我给统计学家提供[培训](https://datanalyze.be/trainings/)，我也会介绍`{dplyr}`包。然而，我仍然相信，学习基础 R 的数据操作很重要，原因有二：

1.  每个人在某个时候都会遇到用基础 R 编写的代码（无论是与其他 R 用户合作，还是从教科书或在线找到的代码）。

1.  从我收到的学生反馈中，我发现当你对基础 R 有所了解时，学习`{dplyr}`相对容易（这对于那些一开始感到困难的人来说非常有成就感）。

基于这些原因，我倾向于先使用基础 R 教授数据操作，然后逐渐转向`{dplyr}`（以至于在一些高级培训课程中，我几乎完全不使用基础 R）。顺便提一句，这也是我在 R 中进行数据可视化时遵循的方法：我首先教如何使用基础 R 绘制数据，然后逐步教授[如何使用](https://statsandr.com/blog/graphics-in-r-with-ggplot2/)`[{ggplot2}](https://statsandr.com/blog/graphics-in-r-with-ggplot2/)`。

采用这种方法，一些学生可能会觉得他们学习基础 R 的时间被浪费了。至少，在培训期间，他们可能有这种印象。然而，一旦培训结束，他们需要自行学习 R 或从事实际项目时，他们会感激自己学过这两者。

我很想听听其他老师的看法，因此请随时分享你的意见。

# 数据

为了展示不同的函数，我们将使用数据框`penguins`，该数据框可以在`{palmerpenguins}`包中找到（Horst, Hill, 和 Gorman 2020）。数据使用[CC-0](https://creativecommons.org/public-domain/cc0/)许可，并可以从 CRAN 下载：

```py
# install.packages("palmerpenguins")
library(palmerpenguins)
```

在深入之前，我们将数据框重命名为`dat`：

```py
dat <- penguins
```

我喜欢用诸如`dat`这样的通用名称来命名我正在处理的数据框，原因有二：

1.  每次我需要写出数据框的名称时，通常写`dat`比写数据框的完整名称（在此情况下是`penguins`）要简短。

1.  如果我需要对不同的数据框进行类似的分析或绘图，我过去写的代码可以重复使用，只需进行少量修改。通过这个非常简单的技巧，大多数情况下我只需编辑变量的名称，而数据框的名称无需更改（这为我节省了大量时间）。

数据框包含 344 只企鹅的数据和 8 个变量，描述了物种、岛屿、一些喙、鳍和体重的测量、性别和研究年份。有关数据框的更多信息可以通过运行 `?penguins`（在加载 `{palmerpenguins}` 包后）找到。

在这篇文章中，我们将仅关注变量 `species`、`body_mass_g`、`sex` 和 `year`。

在进行不同的数据处理技术之前，让我们首先通过显示数据的结构、前 6 行和总结来检查数据：

```py
str(dat) # structure of the data
```

```py
## tibble [344 × 4] (S3: tbl_df/tbl/data.frame)
##  $ species    : Factor w/ 3 levels "Adelie","Chinstrap",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ body_mass_g: int [1:344] 3750 3800 3250 NA 3450 3650 3625 4675 3475 4250 ...
##  $ sex        : Factor w/ 2 levels "female","male": 2 1 1 NA 1 2 1 2 NA NA ...
##  $ year       : int [1:344] 2007 2007 2007 2007 2007 2007 2007 2007 2007 2007 ...
```

```py
head(dat) # display first 6 rows
```

```py
## # A tibble: 6 × 4
##   species body_mass_g sex     year
##   <fct>         <int> <fct>  <int>
## 1 Adelie         3750 male    2007
## 2 Adelie         3800 female  2007
## 3 Adelie         3250 female  2007
## 4 Adelie           NA <NA>    2007
## 5 Adelie         3450 female  2007
## 6 Adelie         3650 male    2007
```

```py
summary(dat) # summary
```

```py
##       species     body_mass_g       sex           year     
##  Adelie   :152   Min.   :2700   female:165   Min.   :2007  
##  Chinstrap: 68   1st Qu.:3550   male  :168   1st Qu.:2007  
##  Gentoo   :124   Median :4050   NA's  : 11   Median :2008  
##                  Mean   :4202                Mean   :2008  
##                  3rd Qu.:4750                3rd Qu.:2009  
##                  Max.   :6300                Max.   :2009  
##                  NA's   :2
```

# {dplyr} 包

毫无废话，让我们在接下来的部分中说明 `{dplyr}` 包中可用于数据处理的不同函数。

和任何包一样，我们首先需要安装并加载它才能使用：

```py
# install.packages("dplyr")
library(dplyr)
```

请注意，下面介绍的所有函数都需要整洁数据，这意味着：

+   每个变量都有自己的列，

+   每个观测值或案例都有自己的行，并且

+   每个值都有自己的单元格。

# 筛选观测值

可以使用 `filter()` 函数根据观测值进行筛选。这个函数适用于 [定量](https://statsandr.com/blog/variable-types-and-examples/#quantitative) 和 [定性](https://statsandr.com/blog/variable-types-and-examples/#qualitative) 变量：

```py
# filter observations based on a quantitative variable
filter(dat, body_mass_g > 4000)
```

```py
## # A tibble: 172 × 4
##    species body_mass_g sex    year
##    <fct>         <int> <fct> <int>
##  1 Adelie         4675 male   2007
##  2 Adelie         4250 <NA>   2007
##  3 Adelie         4400 male   2007
##  4 Adelie         4500 male   2007
##  5 Adelie         4200 male   2007
##  6 Adelie         4150 male   2007
##  7 Adelie         4650 male   2007
##  8 Adelie         4400 male   2007
##  9 Adelie         4600 male   2007
## 10 Adelie         4150 male   2007
## # ℹ 162 more rows
```

```py
# filter observations based on a qualitative variable
filter(dat, sex == "female")
```

```py
## # A tibble: 165 × 4
##    species body_mass_g sex     year
##    <fct>         <int> <fct>  <int>
##  1 Adelie         3800 female  2007
##  2 Adelie         3250 female  2007
##  3 Adelie         3450 female  2007
##  4 Adelie         3625 female  2007
##  5 Adelie         3200 female  2007
##  6 Adelie         3700 female  2007
##  7 Adelie         3450 female  2007
##  8 Adelie         3325 female  2007
##  9 Adelie         3400 female  2007
## 10 Adelie         3800 female  2007
## # ℹ 155 more rows
```

你可以使用 `&` 组合多个条件（如果条件必须是累积的）或 `|`（如果条件是替代的），例如：

```py
# filter observations based on 2 cumulative conditions
filter(dat, body_mass_g > 4000 & sex == "female")
```

```py
## # A tibble: 58 × 4
##    species body_mass_g sex     year
##    <fct>         <int> <fct>  <int>
##  1 Gentoo         4500 female  2007
##  2 Gentoo         4450 female  2007
##  3 Gentoo         4550 female  2007
##  4 Gentoo         4800 female  2007
##  5 Gentoo         4400 female  2007
##  6 Gentoo         4650 female  2007
##  7 Gentoo         4650 female  2007
##  8 Gentoo         4200 female  2007
##  9 Gentoo         4150 female  2007
## 10 Gentoo         4800 female  2007
## # ℹ 48 more rows
```

请注意，变量名*不*必需要写在单引号或双引号内（`''`或`""`）。下面介绍的所有函数都是如此。

# 管道操作符

从上面的代码可以看出，`filter()` 函数需要将数据框的名称作为第一个参数，然后是条件（使用常见的逻辑操作符 `>`, `<`, `>=`, `<=`, `==`, `!=`, `%in%` 等）作为第二个参数。

所有列表中介绍的函数都需要将数据框的名称作为第一个参数。然而，指定数据框名称有一种变通办法：管道操作符（`|>` 或 `%>%`）。

管道操作符允许执行一系列操作，即将一系列计算连接在一起。当你对数据框执行多个操作时特别有用，你不想在每个中间步骤保存输出。我们将在下面看到如何使用管道操作符进行多个操作，但现在我想先介绍单次操作的使用。

正如你所见，使用`filter()`函数时，管道操作符不是强制性的。然而，我强烈推荐它（即使是对初学者），因为它易于使用、方便、代码可读性高且流行，从现在起，`{dplyr}`中可用的函数将与管道操作符一起展示。

所以使用管道操作符，上述代码变成了：

```py
# filter observations based on a quantitative variable
dat |>
  filter(body_mass_g > 4000)
```

```py
## # A tibble: 172 × 4
##    species body_mass_g sex    year
##    <fct>         <int> <fct> <int>
##  1 Adelie         4675 male   2007
##  2 Adelie         4250 <NA>   2007
##  3 Adelie         4400 male   2007
##  4 Adelie         4500 male   2007
##  5 Adelie         4200 male   2007
##  6 Adelie         4150 male   2007
##  7 Adelie         4650 male   2007
##  8 Adelie         4400 male   2007
##  9 Adelie         4600 male   2007
## 10 Adelie         4150 male   2007
## # ℹ 162 more rows
```

```py
# filter observations based on a qualitative variable
dat |>
  filter(sex == "female")
```

```py
## # A tibble: 165 × 4
##    species body_mass_g sex     year
##    <fct>         <int> <fct>  <int>
##  1 Adelie         3800 female  2007
##  2 Adelie         3250 female  2007
##  3 Adelie         3450 female  2007
##  4 Adelie         3625 female  2007
##  5 Adelie         3200 female  2007
##  6 Adelie         3700 female  2007
##  7 Adelie         3450 female  2007
##  8 Adelie         3325 female  2007
##  9 Adelie         3400 female  2007
## 10 Adelie         3800 female  2007
## # ℹ 155 more rows
```

```py
# filter observations based on 2 cumulative conditions
dat |>
  filter(body_mass_g > 4000 & sex == "female")
```

```py
## # A tibble: 58 × 4
##    species body_mass_g sex     year
##    <fct>         <int> <fct>  <int>
##  1 Gentoo         4500 female  2007
##  2 Gentoo         4450 female  2007
##  3 Gentoo         4550 female  2007
##  4 Gentoo         4800 female  2007
##  5 Gentoo         4400 female  2007
##  6 Gentoo         4650 female  2007
##  7 Gentoo         4650 female  2007
##  8 Gentoo         4200 female  2007
##  9 Gentoo         4150 female  2007
## 10 Gentoo         4800 female  2007
## # ℹ 48 more rows
```

管道操作符只是将一个操作的结果传递到下一个操作，使代码极其易于编写和阅读。

这样一来，我们不再需要在`filter()`函数（或`{dplyr}`包中的任何其他函数）中将数据框的名称作为第一个参数，而是简单地指定数据框的名称，然后结合管道操作符与所需的函数。

# 提取观察值

可以基于以下内容提取观察值：

+   它们的位置，或

+   它们的值。

# 基于它们的位置

基于它们的位置提取观察值可以使用`slice()`函数：

```py
# extract rows 2, 5 and 37
dat |>
  slice(c(2, 5, 37))
```

```py
## # A tibble: 3 × 4
##   species body_mass_g sex     year
##   <fct>         <int> <fct>  <int>
## 1 Adelie         3800 female  2007
## 2 Adelie         3450 female  2007
## 3 Adelie         3950 male    2007
```

此外，提取第一行或最后一行可以使用`slice_head()`和`slice_tail()`：

```py
# extract first 3 rows
dat |>
  slice_head(n = 3)
```

```py
## # A tibble: 3 × 4
##   species body_mass_g sex     year
##   <fct>         <int> <fct>  <int>
## 1 Adelie         3750 male    2007
## 2 Adelie         3800 female  2007
## 3 Adelie         3250 female  2007
```

```py
# extract last 3 rows
dat |>
  slice_tail(n = 3)
```

```py
## # A tibble: 3 × 4
##   species   body_mass_g sex     year
##   <fct>           <int> <fct>  <int>
## 1 Chinstrap        3775 male    2009
## 2 Chinstrap        4100 male    2009
## 3 Chinstrap        3775 female  2009
```

# 基于它们的值

要根据变量的值提取观察值，请使用：

+   `slice_min()`用于选择具有**最低**值的行（按定义的比例），并且

+   `slice_max()`用于选择具有**最高**值的行（按定义的比例）。

```py
# extract observations with 25% lowest body mass
dat |>
  slice_min(body_mass_g, prop = 0.25)
```

```py
## # A tibble: 89 × 4
##    species   body_mass_g sex     year
##    <fct>           <int> <fct>  <int>
##  1 Chinstrap        2700 female  2008
##  2 Adelie           2850 female  2008
##  3 Adelie           2850 female  2008
##  4 Adelie           2900 female  2008
##  5 Adelie           2900 female  2008
##  6 Adelie           2900 female  2009
##  7 Chinstrap        2900 female  2007
##  8 Adelie           2925 female  2009
##  9 Adelie           2975 <NA>    2007
## 10 Adelie           3000 female  2007
## # ℹ 79 more rows
```

```py
# extract observations with 25% highest body mass
dat |>
  slice_max(body_mass_g, prop = 0.25)
```

```py
## # A tibble: 90 × 4
##    species body_mass_g sex    year
##    <fct>         <int> <fct> <int>
##  1 Gentoo         6300 male   2007
##  2 Gentoo         6050 male   2007
##  3 Gentoo         6000 male   2008
##  4 Gentoo         6000 male   2009
##  5 Gentoo         5950 male   2008
##  6 Gentoo         5950 male   2009
##  7 Gentoo         5850 male   2007
##  8 Gentoo         5850 male   2007
##  9 Gentoo         5850 male   2009
## 10 Gentoo         5800 male   2008
## # ℹ 80 more rows
```

# 采样观察值

采样观察值可以通过两种方式完成：

1.  使用`sample_n()`随机抽取**数量**的行

1.  使用`sample_frac()`随机抽取**比例**的行

```py
# random sample of 3 rows
dat |>
  sample_n(size = 3)
```

```py
## # A tibble: 3 × 4
##   species   body_mass_g sex     year
##   <fct>           <int> <fct>  <int>
## 1 Adelie           3450 female  2007
## 2 Chinstrap        3675 female  2009
## 3 Gentoo           4500 female  2007
```

```py
# random sample of half of the rows
dat |>
  sample_frac(size = 1 / 2)
```

```py
## # A tibble: 172 × 4
##    species   body_mass_g sex     year
##    <fct>           <int> <fct>  <int>
##  1 Adelie           4150 male    2008
##  2 Gentoo           5800 male    2008
##  3 Adelie           3650 male    2009
##  4 Adelie           3500 male    2009
##  5 Adelie           3450 female  2007
##  6 Adelie           4300 male    2009
##  7 Chinstrap        3400 female  2008
##  8 Adelie           3950 male    2007
##  9 Chinstrap        3325 female  2009
## 10 Adelie           3950 male    2008
## # ℹ 162 more rows
```

请注意，与基础 R 中的`sample()`函数一样，`size`可以大于数据框的大小。在这种情况下，某些行将会重复，你需要指定参数`replace = TRUE`。

另外，可以使用`slice_sample()`获得随机样本的行数或行的比例。为此，请使用：

+   参数`n`用于选择行的数量，或

+   参数`prop`用于选择行的比例。

```py
# random sample of 3 rows
dat |>
  slice_sample(n = 3)
```

```py
## # A tibble: 3 × 4
##   species body_mass_g sex     year
##   <fct>         <int> <fct>  <int>
## 1 Adelie         3200 female  2007
## 2 Adelie         3800 female  2007
## 3 Gentoo         4800 female  2007
```

```py
# random sample of half of the rows
dat |>
  slice_sample(prop = 1 / 2)
```

```py
## # A tibble: 172 × 4
##    species body_mass_g sex     year
##    <fct>         <int> <fct>  <int>
##  1 Adelie         3900 male    2009
##  2 Adelie         3275 female  2009
##  3 Gentoo         5050 male    2008
##  4 Gentoo         4700 female  2009
##  5 Gentoo         4600 female  2008
##  6 Gentoo         4875 <NA>    2009
##  7 Adelie         3700 <NA>    2007
##  8 Gentoo         3950 female  2008
##  9 Gentoo         4550 female  2007
## 10 Adelie         3500 female  2008
## # ℹ 162 more rows
```

# 排序观察值

可以使用`arrange()`函数对观察值进行排序：

```py
# sort observations based on body mass (ascending order)
dat |>
  arrange(body_mass_g)
```

```py
## # A tibble: 344 × 4
##    species   body_mass_g sex     year
##    <fct>           <int> <fct>  <int>
##  1 Chinstrap        2700 female  2008
##  2 Adelie           2850 female  2008
##  3 Adelie           2850 female  2008
##  4 Adelie           2900 female  2008
##  5 Adelie           2900 female  2008
##  6 Adelie           2900 female  2009
##  7 Chinstrap        2900 female  2007
##  8 Adelie           2925 female  2009
##  9 Adelie           2975 <NA>    2007
## 10 Adelie           3000 female  2007
## # ℹ 334 more rows
```

默认情况下，`arrange()`使用**升序**。要按**降序**排序，请在`arrange()`内部使用`desc()`：

```py
# sort observations based on body mass (descending order)
dat |>
  arrange(desc(body_mass_g))
```

```py
## # A tibble: 344 × 4
##    species body_mass_g sex    year
##    <fct>         <int> <fct> <int>
##  1 Gentoo         6300 male   2007
##  2 Gentoo         6050 male   2007
##  3 Gentoo         6000 male   2008
##  4 Gentoo         6000 male   2009
##  5 Gentoo         5950 male   2008
##  6 Gentoo         5950 male   2009
##  7 Gentoo         5850 male   2007
##  8 Gentoo         5850 male   2007
##  9 Gentoo         5850 male   2009
## 10 Gentoo         5800 male   2008
## # ℹ 334 more rows
```

与`filter()`一样，`arrange()`可以用于多个变量，并且对定量和定性变量都有效：

```py
# sort observations based on two variables
dat |>
  arrange(sex, body_mass_g)
```

```py
## # A tibble: 344 × 4
##    species   body_mass_g sex     year
##    <fct>           <int> <fct>  <int>
##  1 Chinstrap        2700 female  2008
##  2 Adelie           2850 female  2008
##  3 Adelie           2850 female  2008
##  4 Adelie           2900 female  2008
##  5 Adelie           2900 female  2008
##  6 Adelie           2900 female  2009
##  7 Chinstrap        2900 female  2007
##  8 Adelie           2925 female  2009
##  9 Adelie           3000 female  2007
## 10 Adelie           3000 female  2009
## # ℹ 334 more rows
```

上述代码首先根据性别（按字母顺序）排序观察值，然后根据体重（按升序，从最低到最高）排序。

请注意，如果定性变量被定义为有序的[因子](https://statsandr.com/blog/data-types-in-r/#factor)，则排序是基于级别顺序，而不是字母顺序！

# 选择变量

选择变量可以使用`select()`函数，基于：

+   变量的位置，或

+   变量的名称。

```py
# select variables by their positions
dat |>
  select(c(2, 4))
```

```py
## # A tibble: 344 × 2
##    body_mass_g  year
##          <int> <int>
##  1        3750  2007
##  2        3800  2007
##  3        3250  2007
##  4          NA  2007
##  5        3450  2007
##  6        3650  2007
##  7        3625  2007
##  8        4675  2007
##  9        3475  2007
## 10        4250  2007
## # ℹ 334 more rows
```

```py
# select variables by their names
dat |>
  select(body_mass_g, year)
```

```py
## # A tibble: 344 × 2
##    body_mass_g  year
##          <int> <int>
##  1        3750  2007
##  2        3800  2007
##  3        3250  2007
##  4          NA  2007
##  5        3450  2007
##  6        3650  2007
##  7        3625  2007
##  8        4675  2007
##  9        3475  2007
## 10        4250  2007
## # ℹ 334 more rows
```

请注意，也可以删除变量。为此，请在其位置或名称前使用`-`符号：

```py
# remove variables by their positions
dat |>
  select(-c(2, 4))
```

```py
## # A tibble: 344 × 2
##    species sex   
##    <fct>   <fct> 
##  1 Adelie  male  
##  2 Adelie  female
##  3 Adelie  female
##  4 Adelie  <NA>  
##  5 Adelie  female
##  6 Adelie  male  
##  7 Adelie  female
##  8 Adelie  male  
##  9 Adelie  <NA>  
## 10 Adelie  <NA>  
## # ℹ 334 more rows
```

```py
# remove variables by their names
dat |>
  select(-c(body_mass_g, year))
```

```py
## # A tibble: 344 × 2
##    species sex   
##    <fct>   <fct> 
##  1 Adelie  male  
##  2 Adelie  female
##  3 Adelie  female
##  4 Adelie  <NA>  
##  5 Adelie  female
##  6 Adelie  male  
##  7 Adelie  female
##  8 Adelie  male  
##  9 Adelie  <NA>  
## 10 Adelie  <NA>  
## # ℹ 334 more rows
```

也可以使用一系列名称选择变量：

```py
# select all variables from species to sex
dat |>
  select(species:sex)
```

```py
## # A tibble: 344 × 3
##    species body_mass_g sex   
##    <fct>         <int> <fct> 
##  1 Adelie         3750 male  
##  2 Adelie         3800 female
##  3 Adelie         3250 female
##  4 Adelie           NA <NA>  
##  5 Adelie         3450 female
##  6 Adelie         3650 male  
##  7 Adelie         3625 female
##  8 Adelie         4675 male  
##  9 Adelie         3475 <NA>  
## 10 Adelie         4250 <NA>  
## # ℹ 334 more rows
```

最后但同样重要的是，`select()`也可以作为一种简便的方法来重新排列列的顺序：

```py
# put sex as first column, then all the others
dat |>
  select(sex, species:year)
```

```py
## # A tibble: 344 × 4
##    sex    species body_mass_g  year
##    <fct>  <fct>         <int> <int>
##  1 male   Adelie         3750  2007
##  2 female Adelie         3800  2007
##  3 female Adelie         3250  2007
##  4 <NA>   Adelie           NA  2007
##  5 female Adelie         3450  2007
##  6 male   Adelie         3650  2007
##  7 female Adelie         3625  2007
##  8 male   Adelie         4675  2007
##  9 <NA>   Adelie         3475  2007
## 10 <NA>   Adelie         4250  2007
## # ℹ 334 more rows
```

# 重命名变量

要重命名变量，请使用`rename()`函数：

```py
# rename variables
dat |>
  rename(
    body_mass = body_mass_g, # rename body_mass_g into body_mass
    study_year = year # rename year into study_year
  )
```

```py
## # A tibble: 344 × 4
##    species body_mass sex    study_year
##    <fct>       <int> <fct>       <int>
##  1 Adelie       3750 male         2007
##  2 Adelie       3800 female       2007
##  3 Adelie       3250 female       2007
##  4 Adelie         NA <NA>         2007
##  5 Adelie       3450 female       2007
##  6 Adelie       3650 male         2007
##  7 Adelie       3625 female       2007
##  8 Adelie       4675 male         2007
##  9 Adelie       3475 <NA>         2007
## 10 Adelie       4250 <NA>         2007
## # ℹ 334 more rows
```

这可能不是直观的（至少在我学习这个包时不是），所以请记住，您总是需要先写新名称，然后再写旧名称（用`=`符号分隔）。

# 创建或修改变量

您可以使用`mutate()`创建或修改数据框中的某些变量，基于：

+   另一个变量，或

+   选择的向量。

```py
# create a new variable based on an existing one
dat |>
  mutate(
    body_mass_kg = body_mass_g / 1000
  )
```

```py
## # A tibble: 344 × 5
##    species body_mass_g sex     year body_mass_kg
##    <fct>         <int> <fct>  <int>        <dbl>
##  1 Adelie         3750 male    2007         3.75
##  2 Adelie         3800 female  2007         3.8 
##  3 Adelie         3250 female  2007         3.25
##  4 Adelie           NA <NA>    2007        NA   
##  5 Adelie         3450 female  2007         3.45
##  6 Adelie         3650 male    2007         3.65
##  7 Adelie         3625 female  2007         3.62
##  8 Adelie         4675 male    2007         4.68
##  9 Adelie         3475 <NA>    2007         3.48
## 10 Adelie         4250 <NA>    2007         4.25
## # ℹ 334 more rows
```

```py
# create a new variable from a vector of your choice
dat |>
  mutate(
    ID = 1:nrow(dat)
  )
```

```py
## # A tibble: 344 × 5
##    species body_mass_g sex     year    ID
##    <fct>         <int> <fct>  <int> <int>
##  1 Adelie         3750 male    2007     1
##  2 Adelie         3800 female  2007     2
##  3 Adelie         3250 female  2007     3
##  4 Adelie           NA <NA>    2007     4
##  5 Adelie         3450 female  2007     5
##  6 Adelie         3650 male    2007     6
##  7 Adelie         3625 female  2007     7
##  8 Adelie         4675 male    2007     8
##  9 Adelie         3475 <NA>    2007     9
## 10 Adelie         4250 <NA>    2007    10
## # ℹ 334 more rows
```

请注意，如果您创建了一个已在数据框中存在的名称的变量，旧变量将被新变量替换。

与`rename()`类似，`mutate()`要求参数写成`name = expression`的形式，其中`name`是创建或修改的列的名称，`expression`是计算值的公式。

# 总结观察

通常，您会想要用一些[描述性统计量](https://statsandr.com/blog/descriptive-statistics-in-r/)来总结数据。这可以通过`summarize()`函数完成，此外还有大多数用于描述性统计量的函数（`mean()`，`median()`，`min()`，`max()`，`sd()`，`var()`等）：

```py
# compute mean and sd of body mass
dat |>
  summarize(
    body_mass_mean = mean(body_mass_g, na.rm = TRUE),
    body_mass_sd = sd(body_mass_g, na.rm = TRUE)
  )
```

```py
## # A tibble: 1 × 2
##   body_mass_mean body_mass_sd
##            <dbl>        <dbl>
## 1          4202\.         802.
```

请注意：

+   `na.rm = TRUE`参数用于在计算总结统计量时忽略缺失值。

+   `summarize()`和`summarise()`给出完全相同的结果。

# 识别不同的值

可以使用`distinct()`来识别变量的不同值：

```py
# find the distinct species
dat |>
  distinct(species)
```

```py
## # A tibble: 3 × 1
##   species  
##   <fct>    
## 1 Adelie   
## 2 Gentoo   
## 3 Chinstrap
```

识别不同的值主要在定性或定量离散变量上进行，但它可以在任何类型的变量以及多个变量上同时进行。如果指定了多个变量，它会返回变量值的所有组合。

例如，按物种和研究年份：

```py
# combination of distinct species and year
dat |>
  distinct(species, year)
```

```py
## # A tibble: 9 × 2
##   species    year
##   <fct>     <int>
## 1 Adelie     2007
## 2 Adelie     2008
## 3 Adelie     2009
## 4 Gentoo     2007
## 5 Gentoo     2008
## 6 Gentoo     2009
## 7 Chinstrap  2007
## 8 Chinstrap  2008
## 9 Chinstrap  2009
```

# 连接操作

使用`{dplyr}`包的另一个优点是可以一次连接多个操作，使代码的可读性大大提高。这可以轻松通过之前介绍的管道操作符（`|>`或`%>%`）来实现。

到目前为止，我们一直看到相同的结构：我们调用一个数据框，然后对该数据框应用一个操作。从现在开始，我们将看到如何将更多操作组合成一个单一的操作链。

# 按组

`group_by()`允许修改基本函数的执行方式。操作将覆盖由分组命令定义的每个行组，而不是数据框的所有行。通过这种方式，使用`summarize()`的聚合操作将为每个组生成统计数据，而不是所有观察值。

例如，我们可能对计算定量变量在每个定性变量水平（即按组）上的一些描述性统计量感兴趣。

在我们的例子中，假设我们想分别计算每种物种的体重均值和标准差：

```py
# compute mean and sd of body mass by species
dat |>
  group_by(species) |> # group by species
  summarize(
    mean = mean(body_mass_g, na.rm = TRUE), # compute mean
    sd = sd(body_mass_g, na.rm = TRUE) # compute sd
  )
```

```py
## # A tibble: 3 × 3
##   species    mean    sd
##   <fct>     <dbl> <dbl>
## 1 Adelie    3701\.  459.
## 2 Chinstrap 3733\.  384.
## 3 Gentoo    5076\.  504.
```

应用到上述示例，管道操作符的工作方式如下：

1.  第一个操作（`group_by()`）按物种分组观察值。

1.  然后，第一个操作的输出被用作第二个操作（`summarize()`）的输入：计算体重的均值和标准差。

结果是按组计算的体重均值和标准差。正如你所想，所有先前的操作都可以连接以满足你的需求。

同时注意，`group_by()` 可以用于多个分组变量：

```py
# compute mean and sd of body mass by species and sex
dat |>
  group_by(species, sex) |> # group by species and sex
  summarize(
    mean = mean(body_mass_g, na.rm = TRUE), # compute mean
    sd = sd(body_mass_g, na.rm = TRUE) # compute sd
  )
```

```py
## # A tibble: 8 × 4
## # Groups:   species [3]
##   species   sex     mean    sd
##   <fct>     <fct>  <dbl> <dbl>
## 1 Adelie    female 3369\.  269.
## 2 Adelie    male   4043\.  347.
## 3 Adelie    <NA>   3540   477.
## 4 Chinstrap female 3527\.  285.
## 5 Chinstrap male   3939\.  362.
## 6 Gentoo    female 4680\.  282.
## 7 Gentoo    male   5485\.  313.
## 8 Gentoo    <NA>   4588\.  338.
```

（注意，对于所有连接操作，数据框的名称只需在第一次操作中指定。）

# 观察值的数量

一些操作只能在其他操作内部执行。

这是观察值 `n()` 的情况，这只能在 `summarize()` 内部使用：

```py
# number of observations
dat |>
  summarize(n_obs = n())
```

```py
## # A tibble: 1 × 1
##   n_obs
##   <int>
## 1   344
```

为了参考之前的连接操作 `group_by()`，我们可以使用管道操作符分隔的两个操作 `n()` 和 `group_by()` 来计算每组的观察值数量：

```py
# number of observations by species
dat |>
  group_by(species) |>
  summarize(n_obs = n())
```

```py
## # A tibble: 3 × 2
##   species   n_obs
##   <fct>     <int>
## 1 Adelie      152
## 2 Chinstrap    68
## 3 Gentoo      124
```

注意 `n()` 不接受任何参数，因此总是写为空括号。

此外，请注意，`count()` 函数等同于 `summarize(n = n())`：

```py
# number of observations
dat |>
  count()
```

```py
## # A tibble: 1 × 1
##       n
##   <int>
## 1   344
```

```py
# number of observations by species
dat |>
  count(species)
```

```py
## # A tibble: 3 × 2
##   species       n
##   <fct>     <int>
## 1 Adelie      152
## 2 Chinstrap    68
## 3 Gentoo      124
```

# 不同值的数量

`n_distinct()`，它也只能在 `summarize()` 内部使用，计算变量或变量组合的不同值/级别的数量：

```py
# number of distinct species
dat |>
  summarize(n_species = n_distinct(species))
```

```py
## # A tibble: 1 × 1
##   n_species
##       <int>
## 1         3
```

```py
# number of distinct species and year of study
dat |>
  summarize(n_species_year = n_distinct(species, year))
```

```py
## # A tibble: 1 × 1
##   n_species_year
##            <int>
## 1              9
```

注意，你不必为输出指定名称。在这种情况下，将使用操作的名称。例如：

```py
# number of distinct species
dat |>
  summarize(n_distinct(species))
```

```py
## # A tibble: 1 × 1
##   `n_distinct(species)`
##                   <int>
## 1                     3
```

# 首个、最后一个或第 n 个值

也仅在 `summarize()` 内部可用，第一个、最后一个或第 n 个值可以通过以下命令找到：

```py
# first value of the variable sex
dat |>
  summarize(first(sex))
```

```py
## # A tibble: 1 × 1
##   `first(sex)`
##   <fct>       
## 1 male
```

```py
# last value of the variable sex
dat |>
  summarize(last(sex))
```

```py
## # A tibble: 1 × 1
##   `last(sex)`
##   <fct>      
## 1 female
```

```py
# 37th value of the variable sex
dat |>
  summarize(nth(sex, n = 37))
```

```py
## # A tibble: 1 × 1
##   `nth(sex, n = 37)`
##   <fct>             
## 1 male
```

在这个函数中存在一些处理缺失值的有趣参数。感兴趣的读者可以在函数文档中查看更多信息（运行 `?nth()`）。

# 如果 else

一种非常常见的数据转换是众所周知的“if else”技术。这种技术通常用于从现有变量创建另一个可以取 **两个级别** 的变量。

假设我们想创建一个名为 `body_mass_cat` 的新变量，当 `body_mass_g` 等于或大于某个阈值时取值“High”，否则取“Low”。这种转换可以通过 `mutate()` 和 `if_else()` 的组合来完成：

```py
# if else
dat |>
  mutate(
    body_mass_cat = if_else(body_mass_g >= 4000, # condition
      "High", # output if condition is true
      "Low" # output if condition is false
    )
  )
```

```py
## # A tibble: 344 × 5
##    species body_mass_g sex     year body_mass_cat
##    <fct>         <int> <fct>  <int> <chr>        
##  1 Adelie         3750 male    2007 Low          
##  2 Adelie         3800 female  2007 Low          
##  3 Adelie         3250 female  2007 Low          
##  4 Adelie           NA <NA>    2007 <NA>         
##  5 Adelie         3450 female  2007 Low          
##  6 Adelie         3650 male    2007 Low          
##  7 Adelie         3625 female  2007 Low          
##  8 Adelie         4675 male    2007 High         
##  9 Adelie         3475 <NA>    2007 Low          
## 10 Adelie         4250 <NA>    2007 High         
## # ℹ 334 more rows
```

`if_else()` 函数接受 3 个参数：

1.  条件（在我们的例子中：`body_mass_g >= 4000`）

1.  当条件为真时的输出值（在我们的例子中是 `High`）。

1.  当条件为假时的输出值（在我们的例子中是 `Low`）。

如上表所示，当体重缺失时，`if_else()` 也会返回缺失值，这通常是防止观察值被错误分类的好方法。

# 情况

如果你想将变量分类为 **两个以上的级别**，`if_else` 不是最合适的工具。在这种情况下，“情况”更为合适。

供您参考，当我学习 R 时，我曾经编写过嵌套的 if else 函数，即在主要的 if else 中嵌入二级的 if else。大多数时候它有效（虽然经常需要花费大量时间调试代码），但很容易出错。即使你成功让它工作，代码也很难阅读！

所以我强烈建议使用 case when 技术，而不是多个相互嵌套的 if else 函数。

假设我们想将体重分为 3 类：低、中和高。为了说明，我们随意决定体重低于 3500 时为低，超过 4750 时为高，其余为中。

使用嵌套的 if else 函数，我们需要编写如下代码：

```py
# nested if else
dat |>
  mutate(
    body_mass_cat = if_else(body_mass_g < 3500, # first condition
      "Low", # output if first condition is true
      if_else(body_mass_g > 4750, # second condition when first condition is false
        "High", # output when second condition is true
        "Medium" # output when second condition is false
      )
    )
  )
```

```py
## # A tibble: 344 × 5
##    species body_mass_g sex     year body_mass_cat
##    <fct>         <int> <fct>  <int> <chr>        
##  1 Adelie         3750 male    2007 Medium       
##  2 Adelie         3800 female  2007 Medium       
##  3 Adelie         3250 female  2007 Low          
##  4 Adelie           NA <NA>    2007 <NA>         
##  5 Adelie         3450 female  2007 Low          
##  6 Adelie         3650 male    2007 Medium       
##  7 Adelie         3625 female  2007 Medium       
##  8 Adelie         4675 male    2007 Medium       
##  9 Adelie         3475 <NA>    2007 Low          
## 10 Adelie         4250 <NA>    2007 Medium       
## # ℹ 334 more rows
```

这段代码的工作原理如下：

1.  它评估第一个条件 `body_mass_g < 3500`。

1.  如果是真的，`body_mass_cat` 是 `Low`。相反，如果为假，它将评估第二个条件 `body_mass_g > 4750`。

1.  如果第二个条件为真，`body_mass_cat` 是 `High`，否则是 `Medium`。

从上述结果可以看出，它有效。然而，你会承认容易出现编码错误，而且代码既难以编写也难以阅读。

为了改进这个工作流程，我们现在使用 case when 技术：

```py
# case when, without a default option
dat |>
  mutate(
    body_mass_cat = case_when(
      body_mass_g < 3500 ~ "Low",
      body_mass_g >= 3500 & body_mass_g <= 4750 ~ "Medium",
      body_mass_g > 4750 ~ "High"
    )
  )
```

```py
## # A tibble: 344 × 5
##    species body_mass_g sex     year body_mass_cat
##    <fct>         <int> <fct>  <int> <chr>        
##  1 Adelie         3750 male    2007 Medium       
##  2 Adelie         3800 female  2007 Medium       
##  3 Adelie         3250 female  2007 Low          
##  4 Adelie           NA <NA>    2007 <NA>         
##  5 Adelie         3450 female  2007 Low          
##  6 Adelie         3650 male    2007 Medium       
##  7 Adelie         3625 female  2007 Medium       
##  8 Adelie         4675 male    2007 Medium       
##  9 Adelie         3475 <NA>    2007 Low          
## 10 Adelie         4250 <NA>    2007 Medium       
## # ℹ 334 more rows
```

这个工作流程更易于编码和阅读！

如果用于条件的变量中没有缺失值，它甚至可以简化为：

```py
# case when, with a default option
dat |>
  mutate(
    body_mass_cat = case_when(
      body_mass_g < 3500 ~ "Low",
      body_mass_g > 4750 ~ "High",
      .default = "Medium" # default output
    )
  )
```

```py
## # A tibble: 344 × 5
##    species body_mass_g sex     year body_mass_cat
##    <fct>         <int> <fct>  <int> <chr>        
##  1 Adelie         3750 male    2007 Medium       
##  2 Adelie         3800 female  2007 Medium       
##  3 Adelie         3250 female  2007 Low          
##  4 Adelie           NA <NA>    2007 Medium       
##  5 Adelie         3450 female  2007 Low          
##  6 Adelie         3650 male    2007 Medium       
##  7 Adelie         3625 female  2007 Medium       
##  8 Adelie         4675 male    2007 Medium       
##  9 Adelie         3475 <NA>    2007 Low          
## 10 Adelie         4250 <NA>    2007 Medium       
## # ℹ 334 more rows
```

正如你所见，对于不符合任何条件的观察，可以通过 `.default` 指定一个默认输出。

但是，如果存在缺失值，请小心！确实，如果至少有一个缺失值（如我们的案例），上述代码是不正确的，因为缺失 `body_mass_g` 的观察将被误分类为 `Medium`。

因此，如果你真的想指定一个默认输出，我建议使用下面的代码，它将缺失值保留为 `NA`：

```py
# case when, with a default option and missing values
dat |>
  mutate(
    body_mass_cat = case_when(
      body_mass_g < 3500 ~ "Low",
      body_mass_g > 4750 ~ "High",
      is.na(body_mass_g) ~ NA, # keep missing values as NA
      .default = "Medium" # default output
    )
  )
```

```py
## # A tibble: 344 × 5
##    species body_mass_g sex     year body_mass_cat
##    <fct>         <int> <fct>  <int> <chr>        
##  1 Adelie         3750 male    2007 Medium       
##  2 Adelie         3800 female  2007 Medium       
##  3 Adelie         3250 female  2007 Low          
##  4 Adelie           NA <NA>    2007 <NA>         
##  5 Adelie         3450 female  2007 Low          
##  6 Adelie         3650 male    2007 Medium       
##  7 Adelie         3625 female  2007 Medium       
##  8 Adelie         4675 male    2007 Medium       
##  9 Adelie         3475 <NA>    2007 Low          
## 10 Adelie         4250 <NA>    2007 Medium       
## # ℹ 334 more rows
```

我个人更喜欢编写所有类别，而不写默认选项，以提高代码的可读性和健壮性，但这更是一种个人观点。

在所有情况下，无论你使用的是 if else 还是 case when，检查你刚创建的变量以确保获得预期结果是一种良好实践。

# 结论和其他资源

谢谢阅读。

本文介绍并说明了在 R 中最常见的数据操作和数据管理工具，使用了 `{dplyr}` 包。我们还介绍了管道操作符，现代 R 包的用户非常熟悉。

`{dplyr}` 包提供了更多的数据操作函数（特别是使用 `join()` 函数系列合并数据框）。如果你想了解更多关于这个包的内容，我推荐从以下资源开始：

+   [dplyr.tidyverse.org](https://dplyr.tidyverse.org/)

+   [书籍《R for Data Science》中的“数据转换”章节](https://r4ds.hadley.nz/data-transform.html)

+   [备忘单](https://github.com/rstudio/cheatsheets/blob/main/data-transformation.pdf)

+   [说明书](https://dplyr.tidyverse.org/articles/dplyr.html)

+   对于习惯于基础 R 的用户，[说明书](https://cran.r-project.org/web/packages/dplyr/vignettes/base.html) 比较了 `{dplyr}` 函数与其基础 R 等效函数

一如既往，如果你对本文涉及的主题有任何问题或建议，请将其作为评论添加，以便其他读者也能从讨论中受益。

# 参考文献

Horst, Allison Marie, Alison Presmanes Hill, 和 Kristen B Gorman. 2020\. *Palmerpenguins: Palmer Archipelago (Antarctica) Penguin Data*. [`doi.org/10.5281/zenodo.3960218`](https://doi.org/10.5281/zenodo.3960218).

1.  管道操作符的键盘快捷键是 `ctrl + shift + m`（Windows）或 `cmd + shift + m`（Mac）。它将打印 `%>%`，除非你在 RStudio 的设置中指定使用原生管道操作符 `|>`。[↩︎](https://statsandr.com/blog/introduction-to-data-manipulation-in-r-with-dplyr/#fnref1)

# 相关文章

+   [R 中的十大错误及其修复方法](https://statsandr.com/blog/top-10-errors-in-r/)

+   [R 中的单样本 Wilcoxon 检验](https://statsandr.com/blog/one-sample-wilcoxon-test-in-r/)

+   [手动假设检验](https://statsandr.com/blog/hypothesis-test-by-hand/)

+   [R 中的 ANOVA](https://statsandr.com/blog/anova-in-r/)

+   [R 中的相关系数和相关性测试](https://statsandr.com/blog/correlation-coefficient-and-correlation-test-in-r/)

*最初发表于* [*https://statsandr.com*](https://statsandr.com/blog/introduction-to-data-manipulation-in-r-with-dplyr/) *于 2023 年 11 月 27 日。*
