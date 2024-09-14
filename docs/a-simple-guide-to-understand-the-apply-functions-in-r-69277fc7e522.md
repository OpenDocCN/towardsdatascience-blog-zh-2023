# 理解 R 中 apply() 函数的简单指南

> 原文：[`towardsdatascience.com/a-simple-guide-to-understand-the-apply-functions-in-r-69277fc7e522`](https://towardsdatascience.com/a-simple-guide-to-understand-the-apply-functions-in-r-69277fc7e522)

## 一劳永逸地学会如何使用这些有用的函数

[](https://gustavorsantos.medium.com/?source=post_page-----69277fc7e522--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----69277fc7e522--------------------------------)[](https://towardsdatascience.com/?source=post_page-----69277fc7e522--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----69277fc7e522--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----69277fc7e522--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----69277fc7e522--------------------------------) ·阅读时长 8 分钟·2023 年 10 月 6 日

--

![](img/d38e794bd5b719ca31a8e398cfe549a7.png)

照片由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/0iKjge_aOVo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

我在开始这篇文章时要说的是，我每天都使用**R**和**Python**语言。老实说，我觉得 Python 中 `apply` 函数的使用方式更简单、更直观。

反思原因，我认为是因为 Python 中的选项不多。R 语言提供了许多不同的选项，即我喜欢称之为的*apply 函数家族*。

我记得曾经有人说他们总是直接使用循环来解决问题，因为他们总是记不住每个 apply 函数的作用，也不知道哪个函数最适合使用。

我希望这些问题在这篇文章后能得到解决。我的目的是希望读者在阅读本文后，对这些函数家族有一个清晰的理解，以及如何和何时使用它们。

要进行这些练习，我们快速创建一个示例数据框，没有过多的标准。包括一个 ID、产品名称、销售数量和两个不同时间段的金额。

```py
# Create dataset
dtf <- data.frame(
  id = 1:100,
  product= sample(c('product A', 'product B', 'product C', 'product D'), size=100, replace=T),
  qty = as.integer( rnorm(100, 10, 2) ),
  amt = rnorm(100, 1280, 300),
  amt2 = rnorm(100, 1280, 300)
)

[OUT]:
     id   product qty    amt   amt2
1     1 product A   9  954.1 1418.5
2     2 product B  12 1606.9  877.7
3     3 product D   7 1241.6 1433.5
4     4 product A  11 1413.2 1203.8
5     5 product B  10 1623.3 1451.1
```

不再多说，让我们深入探讨。

# 函数

R 中的*apply 家族*有 4 个主要函数：`apply()`、`lapply()`、`sapply()` 和 `tapply()`。

## apply( )

`apply()` 函数被称为*家族*。它可能是最直接的函数。这个函数*应用*相同的操作到行或列的所有元素。这里是语法。

`apply(X, MARGIN, FUN)`

> 对矩阵 X 应用指定的函数，按行（1）或列（2）。

最简单的例子是**均值**的应用。我们有一个包含一堆产品销售的数据集。但销售的平均数量和平均销售金额是多少？

要快速查看这一点，我们可以使用`apply`并选择函数`mean`来应用于数据集中每个数值列。

因此，我们的`X`是一个矩阵，包含数据集中的数值列 3（`qty`）和 4（`amt`）——因为我们不能对*产品 A*和*产品 B*取平均。`MARGIN`可以是`1`以应用于行，或`2`以应用于列。`FUN`是要应用于每个元素的函数。代码如下。

```py
# Apply: Apply a function to all the columns
apply( X= dtf[,c(3,4,5)], MARGIN= 2, FUN= mean)

[OUT]:
qty     amt    amt2 
9.55 1303.42 1267.46 
```

## 常见错误

+   当使用`apply()`时，我们必须使用`X`的切片表示法，因为如果我们尝试使用整个数据集`dtf`（考虑到有字符串列），R 会抛出错误。我们不能对字符串求平均。

```py
apply( X= dtf, MARGIN= 2, FUN= mean)

     id product     qty     amt    amt2 
     NA      NA      NA      NA      NA 
Warning messages:
1: In mean.default(newX[, i], ...) :
  argument is not numeric or logical: returning NA
```

+   即使我们尝试使用整个列，例如`dtf$qty`来获取数量的均值（这是数值型的），也不会很好，因为只有一个变量，所以更简单的方法是直接使用`mean(dtf$qty)`，对吗？

+   当运行`apply()`时，R 会在运行函数之前首先读取对象`X`的维度。如果你尝试`dim(dtf$qty)`，你会看到输出是`NULL`。这就是你下面收到错误的原因。维度需要大于 1。

```py
apply( X= dtf$amt, MARGIN= 2, FUN= mean)

Error in apply(X = dtf$amt, MARGIN = 2, FUN = mean) : 
  dim(X) must have a positive length
```

很好，简单明了。

## 按行应用函数

现在让我们再看看数据。如果我们想按行计算均值，以了解 amount1 和 amount2 之间的平均值，我们可以使用相同的代码，只需将`MARGIN=1`更改为`1`，然后函数将计算行的简单平均值：[amt + amt2] / 2。

```py
 id   product qty    amt   amt2
1     1 product A   9  954.1 1418.5
2     2 product B  12 1606.9  877.7
3     3 product D   7 1241.6 1433.5
4     4 product A  11 1413.2 1203.8
5     5 product B  10 1623.3 1451.1

# Apply: Apply a function to all the rows
apply( X= dtf[, c(4,5)], MARGIN= 1, FUN= mean)

[OUT]:
  [1] 1186.3 1242.3 1337.6 1308.5 1537.2 1007.1 1094.0 1465.9 1602.8 1204.4 1155.4 1190.6  812.3 1565.5 1118.5
 [16] 1346.4 1259.2 1319.0 1293.2 1402.1 1471.2 1491.8 1248.5 1154.7 1693.5 1358.8 1396.8 1262.8 1383.0 1270.0
 [31] 1621.8  933.7  850.7  892.6 1482.3 1191.3 1612.7 1677.2 1496.7 1504.8  947.7  865.5  953.5 1151.8  947.6
 [46] 1763.6 1229.1 1328.0  893.1 1386.8 1004.8  975.0  931.7 1665.4 1417.0 1482.3  974.0 1444.9 1233.7 1548.7
 [61] 1469.9 1612.0 1159.9 1130.9 1617.9 1290.0 1227.9 1072.6 1367.1 1027.3 1472.1 1263.1 1347.1 1463.3 1324.0
 [76] 1361.6 1330.7 1380.8 1699.4 1389.0 1165.8 1146.8 1358.7 1326.3 1213.6  983.1 1385.5  919.4 1212.9 1226.1
 [91] 1003.2 1643.5 1327.8 1566.7  966.4 1270.6 1359.5 1252.3 1216.1 1405.6
```

不过，我们可以将其添加到数据集中。

```py
# Average of the amounts (avg by row)
dtf$avg_amounts <- apply( X= dtf[, c(4,5)], MARGIN= 1, FUN= mean)

# See first 5 rows
dtf |> head(5)

  id product qty    amt   amt2 avg_amounts
1  1       A   9  954.1 1418.5        1186
2  2       B  12 1606.9  877.7        1242
3  3       D   7 1241.6 1433.5        1338
4  4       A  11 1413.2 1203.8        1309
5  5       B  10 1623.3 1451.1        1537
```

如果我们想添加一列来计算`amt/qty`，我们可以通过按行（`MARGIN=1`）使用`apply`中的自定义函数来计算。自定义函数接收一个矩阵**mtrx**，并将**mtrx**第二列的值除以第一列的值。

```py
# Average AMT by product QTY
dtf$amt_by_qty <- apply(
  X= dtf[, c(3,4)],
  MARGIN= 1,
  FUN= function(mtrx){ mtrx[2]/mtrx[1]}
       )

# See first 5 rows
dtf |> head(5)

[OUT]:
  id product qty    amt   amt2 avg_amounts amt_by_qty
1  1       A   9  954.1 1418.5        1186      106.0
2  2       B  12 1606.9  877.7        1242      133.9
3  3       D   7 1241.6 1433.5        1338      177.4
4  4       A  11 1413.2 1203.8        1309      128.5
5  5       B  10 1623.3 1451.1        1537      162.3
```

## lapply( )

在 R 语言中，**列表**的概念与 Python 中的不同。在 R 中，它是一个对象的集合，不一定是相同类型的。当我们需要对列表中的每个元素应用函数时，这就是`lapply()`的用途。

重要的是要注意`lapply` **返回一个与 X 具有相同长度的列表对象**。

`lapply(X, FUN)`

我能想到的最简单的例子是，如果你有一个列表，想要检查列表中每个对象的类型。让我们看一下这个简单例子的代码。

```py
# Creating a list
l <- list(c(1,2), 'l', 1.6, TRUE)

# Check the type of each object in the list
lapply(l, class)

[OUT]:
[[1]]
[1] "numeric"

[[2]]
[1] "character"

[[3]]
[1] "numeric"

[[4]]
[1] "logical"
```

正如预期的那样，长度相同（4 个对象），并且函数`class`已应用于列表中的每个元素。基本上，如果你处理的是列表对象或需要返回列表，那么你应该使用这种方法。

> 如果你处理的是列表对象或需要返回列表，你应该使用这种方法。

## sapply( )

根据文档，`sapply()`是`lapply()`的用户友好版本，**返回相同长度的向量或矩阵**，而不是返回一个列表。如果参数`simplify=TRUE`（默认开启），它将返回一个数组（简化对象）。

> sapply()是 lapply()的用户友好版本，返回一个更简单的对象，如数组。

`sapply(X, FUN, simplify = TRUE)`

```py
# Use sapply for the same list
sapply(l, FUN = class)

[OUT]: an array
[1] "numeric"   "character" "numeric"   "logical" 
```

回到我们创建的数据集`dtf`，列`product`包含了以‘*product*’开头的名称。如果我们想去除这些，`sapply`可以是一个很好的助手。在使用循环之前，甚至在加载 Tidyverse 之前，记住 Base R 中的 apply 函数可以提供帮助。

注意，我们在这里使用了一个自定义函数，将字符串分成两部分，一旦找到空格字符，就取该列表的第二个元素。

```py
# Using sapply to remove the word 'product' from the description
dtf$product <- sapply(dtf$product,
                      FUN= function(x){ strsplit(x, ' ')[[1]][2] })

[OUT]:
  id product qty    amt   amt2
1  1       A   9  954.1 1418.5
2  2       B  12 1606.9  877.7
3  3       D   7 1241.6 1433.5
4  4       A  11 1413.2 1203.8
5  5       B  10 1623.3 1451.1
```

## tapply( )

最后，`tapply()`是一个在处理因子（或组）时使用的函数。

`tapply(X, INDEX, FUN, simplify = TRUE)`

> 使用 tapply 对每组值应用一个函数。

这个函数的明显例子是如果我们想在没有*分组*函数的情况下检查`amt`的均值，`tapply`会很有用。

+   `X`是矩阵或向量。

+   `INDEX`是包含组的列。

+   `FUN`是要应用的函数。

```py
# Using tapply to calculate the mean of amt by product 
tapply(X= dtf$amt, INDEX = dtf$product, FUN = mean)

[OUT]:
   A    B    C    D 
1295 1290 1304 1323 
```

这就是关于 R 中 apply 家族函数的最常用部分了。

# 在你离开之前

好吧，apply 函数是很好的映射函数。在你作为数据科学家的日常工作中，会有很多情况需要映射函数，这比循环要好，无论是为了性能还是为了更好的代码可读性。

通过这个简要介绍，我相信你现在可以*将* apply 函数家族应用到你的数据中。

+   `apply`：用于对数据集中的每一行或列的每个元素应用相同的函数。

+   `lapply`：用于对列表中的每个元素应用一个函数。返回一个长度相同的列表作为结果。

+   `sapply`：lapply 的友好版本，返回一个数组作为结果。

+   `tapply`：用于对每一组值应用一个函数。几乎类似于分组操作。

GitHub 上的代码：

[](https://github.com/gurezende/Studying/tree/master/R/apply?source=post_page-----69277fc7e522--------------------------------) [## Studying/R/apply at master · gurezende/Studying

### 这是一个包含我对新包测试和学习的代码库 - Studying/R/apply at master · gurezende/Studying

github.com](https://github.com/gurezende/Studying/tree/master/R/apply?source=post_page-----69277fc7e522--------------------------------)

# 参考

[](https://www.guru99.com/r-apply-sapply-tapply.html?source=post_page-----69277fc7e522--------------------------------#5) [## apply(), lapply(), sapply(), tapply() Function in R with Examples

### 本教程旨在介绍 apply()函数系列。apply()函数是所有函数中最基本的一个…

www.guru99.com](https://www.guru99.com/r-apply-sapply-tapply.html?source=post_page-----69277fc7e522--------------------------------#5)

[`www.datacamp.com/tutorial/r-tutorial-apply-family`](https://www.datacamp.com/tutorial/r-tutorial-apply-family)

[SANTOS, Gustavo R. 2023\. *使用 R 进行数据整理.* 第 1 版. Packt Publishing.](https://a.co/d/1gij8AW)
