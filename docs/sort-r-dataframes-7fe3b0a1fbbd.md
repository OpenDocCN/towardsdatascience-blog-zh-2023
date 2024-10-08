# 如何按多个列在 R 中对数据框进行排序

> 原文：[`towardsdatascience.com/sort-r-dataframes-7fe3b0a1fbbd`](https://towardsdatascience.com/sort-r-dataframes-7fe3b0a1fbbd)

## 在 R 编程语言中对数据框进行排序

[](https://gmyrianthous.medium.com/?source=post_page-----7fe3b0a1fbbd--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----7fe3b0a1fbbd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7fe3b0a1fbbd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7fe3b0a1fbbd--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----7fe3b0a1fbbd--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7fe3b0a1fbbd--------------------------------) ·4 分钟阅读·2023 年 4 月 27 日

--

![](img/4ee5e1a89e83d4f4e809ddc061df614a.png)

[Andre Taissin](https://unsplash.com/fr/@andretaissin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片，来源于 [Unsplash](https://unsplash.com/photos/hOwcob_3dpc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

R 是一种被统计学家和机器学习科学家广泛使用的编程语言。数据框是强大的 R 构造体，能够有效且强大地进行数据处理、操作和分析。

在今天的简短教程中，我们将演示如何在一个或多个列上对 R 数据框进行排序，无论是降序还是升序。

首先，让我们在 R 中创建一个数据框，我们将在本教程中引用它，以演示一些有关排序的有用概念。

```py
df <- data.frame(
  id = c(1:8),
  value = c(123, 86, 234, 235, 212, 121, 123, 899),
  colC = c("A", "C", "B", "B", "A", "B", "C", "C"),
  colD = c(TRUE, TRUE, FALSE, FALSE, FALSE, FALSE, TRUE, FALSE)
)

df
  id value colC  colD
1  1   123    A  TRUE
2  2    86    C  TRUE
3  3   234    B FALSE
4  4   235    B FALSE
5  5   212    A FALSE
6  6   121    B FALSE
7  7   123    C  TRUE
8  8   899    C FALSE
```

# 按单列对 R 数据框进行排序（升序或降序）

现在假设我们想按照特定列对数据框进行排序。为此，我们可以利用 `[order()](https://stat.ethz.ch/R-manual/R-devel/library/base/html/order.html)` 函数，并相应地切片数据框。在 `order` 函数中，我们可以指定在排序数据框时 R 需要考虑的列。

## **升序**

现在假设我们想按照 `value` 列的升序对数据框进行排序。默认情况下，`order()` 函数将以升序对数据框进行排序。

```py
df[with(df, order(value)), ]

  id value colC  colD
2  2    86    C  TRUE
6  6   121    B FALSE
1  1   123    A  TRUE
7  7   123    C  TRUE
5  5   212    A FALSE
3  3   234    B FALSE
4  4   235    B FALSE
8  8   899    C FALSE
```

## 降序

如果你想指定降序，只需在列值前加上负号，这样就会表示我们想要反转升序。

```py
df[with(df, order(-value)), ]

  id value colC  colD
8  8   899    C FALSE
4  4   235    B FALSE
3  3   234    B FALSE
5  5   212    A FALSE
1  1   123    A  TRUE
7  7   123    C  TRUE
6  6   121    B FALSE
2  2    86    C  TRUE
```

# 按多个列对数据框进行排序

`order()` 函数可以接受任意数量的列名。这意味着我们可以指示语言对数据框进行多列排序。同时，也可以对不同的列使用不同的排序策略（即降序与升序）。

我们在 `order()` 函数中提供的列名顺序将决定该函数对输入数据框进行排序的实际顺序。

现在假设我们想要根据 `value` 和 `id` 列来排序我们的数据框，分别按降序和升序排序。我们可以使用以下代码片段来实现。

```py
df[with(df, order(-value, id)), ]

  id value colC  colD
8  8   899    C FALSE
4  4   235    B FALSE
3  3   234    B FALSE
5  5   212    A FALSE
1  1   123    A  TRUE
7  7   123    C  TRUE
6  6   121    B FALSE
2  2    86    C  TRUE
```

注意 `id=1` 和 `id=7` 的条目是如何按照升序排序的，这些条目是基于 `value` 列的降序排序的。

现在假设我们想要按照相同的列对数据框进行排序，不过这次我们将对两个列都使用降序排序。

```py
df[with(df, order(-value, -id)), ]

  id value colC  colD
8  8   899    C FALSE
4  4   235    B FALSE
3  3   234    B FALSE
5  5   212    A FALSE
7  7   123    C  TRUE
1  1   123    A  TRUE
6  6   121    B FALSE
2  2    86    C  TRUE
```

如前所述，请确保提供列的顺序，以便函数在排序输入数据框时考虑这些列。注意，当 `order()` 函数的输入列发生变化时，输出结果将如何改变：

```py
df[with(df, order(-id, -value)), ]

 id value colC  colD
8  8   899    C FALSE
7  7   123    C  TRUE
6  6   121    B FALSE
5  5   212    A FALSE
4  4   235    B FALSE
3  3   234    B FALSE
2  2    86    C  TRUE
1  1   123    A  TRUE
```

# 最终思考

在今天的简短教程中，我们演示了如何根据某些列对 R 中的数据框进行排序，无论是升序还是降序。数据框是强大的结构，让 R 程序员和统计学家能够分析数据并提取有意义的见解。

数据框排序是分析数据时最基本的方面之一。我希望这篇文章能帮助您了解如何根据特定要求快速排序数据框。

👉 [**成为会员**](https://gmyrianthous.medium.com/membership) **并阅读 Medium 上的每一个故事。您的会员费直接支持我和其他您阅读的作者。您还将获得对 Medium 上每个故事的完全访问权限。**

[`gmyrianthous.medium.com/membership?source=post_page-----7fe3b0a1fbbd--------------------------------`](https://gmyrianthous.medium.com/membership?source=post_page-----7fe3b0a1fbbd--------------------------------) [## 通过我的推荐链接加入 Medium — Giorgos Myrianthous

### 作为 Medium 会员，您的会员费的一部分将分配给您阅读的作者，并且您可以完全访问每一个故事……

[gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership?source=post_page-----7fe3b0a1fbbd--------------------------------)

👇**相关的文章你可能也喜欢** 👇

[`gmyrianthous.medium.com/membership?source=post_page-----7fe3b0a1fbbd--------------------------------`](https://gmyrianthous.medium.com/membership?source=post_page-----7fe3b0a1fbbd--------------------------------) ## ETL 与 ELT：有什么区别？

### 关于数据工程中 ETL 和 ELT 的比较

[towardsdatascience.com ## 创建本地 dbt 项目

### 如何使用 Docker 创建一个包含虚拟数据的本地 dbt 项目用于测试

[towardsdatascience.com
