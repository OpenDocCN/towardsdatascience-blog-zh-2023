# 如何在 Python 中有效使用 lambda 函数作为数据科学家

> 原文：[`towardsdatascience.com/how-to-effectively-use-lambda-functions-in-python-as-a-data-scientist-fd6171554053`](https://towardsdatascience.com/how-to-effectively-use-lambda-functions-in-python-as-a-data-scientist-fd6171554053)

## 对其语法、功能和在数据科学中的适用性的介绍

[](https://thomasdorfer.medium.com/?source=post_page-----fd6171554053--------------------------------)![Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----fd6171554053--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fd6171554053--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fd6171554053--------------------------------) [Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----fd6171554053--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fd6171554053--------------------------------) ·阅读时间 7 分钟·2023 年 3 月 1 日

--

![](img/bca7d99bc3361a6b91e7b7b30d88b872.png)

图片由作者提供。

# 介绍

Python 中的 lambda 函数是小型的、**匿名**（或无名）函数，其语法比常规 Python 函数更简洁。

在数据科学领域，lambda 函数常与**高阶函数**一起使用。高阶函数是指接受一个或多个函数作为参数，或者返回一个函数作为结果的函数。常见的例子包括 `map()`、`filter()` 和 `reduce()`。

在深入应用之前，我们先来看看 lambda 函数的语法。

# 语法

要在 Python 中使用 lambda 函数，需要以下**四个组件**：

![](img/8567cc6dccff6764939133684174bd70.png)

图片由作者提供。

+   **lambda：** 定义 lambda 函数的关键字。

+   **参数：** Lambda 函数可以接受一个或多个参数。如果参数数量超过一个，它们需要用逗号分隔。

+   **冒号：** 作为函数参数和表达式之间的分隔符。

+   **表达式：** 一个有效的 Python 表达式，它将被评估并作为函数的结果返回。

应用此语法，我们可以产生一个简单的 lambda 函数示例，该示例接受两个输入参数 `x` 和 `y`，并返回它们的和：

```py
>>> (lambda x, y: x + y)(3, 5)
8
```

通过在 lambda 函数周围使用括号，我们可以利用一种称为[立即调用函数表达式](https://en.wikipedia.org/wiki/Immediately_invoked_function_expression)的原则，该原则允许我们定义并立即执行一个函数。

由于 lambda 函数是一个表达式，我们也可以给它赋予一个名称。以下是与上述相同函数的示例，但这次我们将其命名为 `func`。

```py
>>> func = lambda x, y: x+y
>>> func(3, 5)
8
```

现在我们对 lambda 函数的语法有了更好的理解，让我们看看它们常被应用的一些场景。

# 在数据科学中的适用性

在数据科学领域，lambda 函数最常与**高阶函数**一起使用，高阶函数接受一个或多个函数作为参数，或者用于嵌套函数称为**闭包** —— 更多内容会在后面讲到。

现在让我们看一些具体的例子。在实际操作中，我们将考察 lambda 函数在 Python 列表以及 Pandas 数据框中的适用性。

## 数据转换

想象一下，你有一份极度偏斜的数据，希望对其进行对数转换以减少偏斜。或者，你有一些以公里为单位的数据，希望将其转换为海里。像这样的操作可以通过应用 lambda 函数轻松实现。

这是一个如何使用 lambda 函数将以公里为单位的数据转换为海里单位的简单例子：

```py
# list
>>> data_km = [1, 2, 3, 4, 5]
>>> data_nm = list(map(lambda x: x / 1.852, data_km))
>>> data_nm
[0.5399568034557235,
 1.079913606911447,
 1.6198704103671706,
 2.159827213822894,
 2.6997840172786174]
```

```py
# Pandas DataFrame
>>> import pandas as pd 
>>> df = pd.DataFrame({'data_km': [1, 2, 3, 4, 5]})
>>> df['data_nm'] = df['data_km'].apply(lambda x: x / 1.852)
>>> df
  data_km   data_nm
0       1   0.539957
1       2   1.079914
2       3   1.619870
3       4   2.159827
4       5   2.699784
```

请注意，当应用于列表时，我们使用内置的[**map**](https://docs.python.org/3/library/functions.html#map)函数，该函数接受一个函数（在我们的例子中是 lambda 函数）和一个可迭代对象（在我们的例子中是名为 `data_km` 的列表）作为参数。

## 过滤

过滤在你只对数据的某一特定子集感兴趣时非常有用。假设你有一些包含年龄列的医疗数据，你只对年龄超过 75 岁的老年患者感兴趣。使用内置的[**filter**](https://docs.python.org/3/library/functions.html#filter)函数，它也接受一个函数和一个可迭代对象作为输入参数，可以按如下方式完成：

```py
# list
>>> data_all = [84, 61, 27, 75, 90]
>>> data_elderly = list(filter(lambda x: x > 75, data_all))
>>> data_elderly
[84, 90]
```

```py
# Pandas DataFrame
>>> import pandas as pd 
>>> df = pd.DataFrame({'data_all': [84, 61, 27, 75, 90]})
>>> df_elderly = df[df['data_all'].apply(lambda x: x > 75)]
>>> df_elderly
  data_all
0       84
4       90
```

## 排序

Lambda 函数可以用来根据某些标准对数据进行排序。例如，你可以使用 lambda 函数根据特定列对列表或数据框进行排序。

在下面的例子中，我们有一个名字列表，希望按姓氏进行排序。这可以按如下方式完成：

```py
# list
>>> names = ["James Watt", "Charles Darwin", "Carl Friedrich Gauss"]
>>> names.sort(key=lambda x: x.split()[-1])
>>> names
['Charles Darwin', 'Carl Friedrich Gauss', 'James Watt']
```

```py
# Pandas DataFrame
>>> import pandas as pd
>>> names = ["James Watt", "Charles Darwin", "Carl Friedrich Gauss"]
>>> df = pd.DataFrame({'names': names})
>>> df_sorted = df.sort_values(by="names", key=lambda x: x.str.split().str[-1])
>>> df_sorted
                 names
1       Charles Darwin
2 Carl Friedrich Gauss
0           James Watt
```

## 分组和聚合

在组级别分析数据通常可以帮助我们获取有关组间差异的有意义的见解。Pandas 数据框，加上[**groupby**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.groupby.html)和[**agg**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.agg.html)函数，是一种常用的框架。

假设你有一些临床数据，包含参与了一个实验的信息，该实验分为三组：A、B 和 C，每组包含略有不同的程序。在完成实验并收集相应数据后，你希望查看分配到每个实验组的患者比例：

```py
>>> import pandas as pd
>>> group = ['A', 'B', 'A', 'A', 'B', 'C']
>>> age = [84, 61, 27, 75, 90, 56]
>>> weight = [164, 189, 135, 121, 172, 138]
>>> df = pd.DataFrame({'group': group, 'age': age, 'weight': weight})
>>> df_grouped = df.groupby('group').apply(lambda x: x['group'].count() / df.shape[0])
>>> df_grouped
group
A    0.500000
B    0.333333
C    0.166667
dtype: float64
```

这项分析将告诉你，大多数患者被分配到了实验组 *A*（50%），其次是组 *B*（33.33%）和组 *C*（16.67%）。

此外，**groupby** 可以与 **agg** 一起使用，以便对不同的列应用不同的聚合函数。在下面的示例中，我们对组级的 *平均* 年龄感兴趣，但对患者的 *中位数* 体重感兴趣。

```py
>>> import pandas as pd
>>> group = ['A', 'B', 'A', 'A', 'B', 'C']
>>> age = [84, 61, 27, 75, 90, 56]
>>> weight = [164, 189, 135, 121, 172, 138]
>>> df = pd.DataFrame({'group': group, 'age': age, 'weight': weight})
>>> df_grouped = df.groupby('group').agg({"age": lambda x: x.mean(),
                                          "weight": lambda x: x.median()})
>>> df_grouped
         age   weight
group  
    A   62.0    135.0
    B   75.5    180.5
    C   56.0    138.0
```

## 闭包

由于其简洁的语法，lambda 函数通常用于名为 [**闭包**](https://en.wikipedia.org/wiki/Closure_(computer_programming)) 的嵌套函数中。简而言之，闭包允许访问外部函数的变量，即使外部函数已经结束。

为了说明这一点，让我们使用闭包创建一个自定义定义的乘数：

```py
>>> def multiplier(factor):
        return lambda x: x * factor
>>> double = multiplier(2)
>>> triple = multiplier(3)
>>> double(10)
20
>>> triple(2)
6
```

在这里，函数 `multiplier` 返回一个“闭包”变量 `factor` 的 lambda 函数。具体来说，当 `multiplier` 使用参数 `factor` 被调用时，它会创建一个新函数，该函数在其定义中包含对 `factor` 的引用。这意味着当新函数—例如存储在 `double` 或 `triple` 等变量中—稍后用某个参数 `x` 被调用时，函数仍然可以访问传递给 `multiplier` 的 `factor` 的值。

另一个这个概念有用的应用是解决二次函数问题。这些函数通常用于物理学中如计算抛射物的轨迹，但也经常用于数据分析、优化问题和信号处理。让我们看一个假设的二次函数示例，该函数描述了一个假设的抛射物的运动。`a`、`b` 和 `c` 的任意输入值分别选择为 -50、450 和 0。

```py
>>> def quad_func(a, b, c):
        return lambda x: a * x**2 + b * x+ c
>>> f = quad_func(-50, 450, 0)
>>> f(0)
0
>>> f(2)
700
>>> f(4)
1000
>>> f(6)
900
>>> f(8)
400
```

如果我们将时间值在 0 到 9 之间绘制图形，我们会得到如下结果：

![](img/ad431a76738306db028bf76ffb8ff55d.png)

作者提供的图片。

# 结论

我希望这篇文章让你更好地理解了 lambda 函数如何在数据科学应用中，如数据转换、过滤、排序、分组和聚合，以及闭包等方面有效使用。

# 更多资源

+   [4\. 更多控制流工具 — Python 3.11.2 文档](https://docs.python.org/3/tutorial/controlflow.html)

+   [如何使用 Python Lambda 函数 — Real Python](https://realpython.com/python-lambda/)

## 喜欢这篇文章吗？

让我们保持联系！你可以在 [Twitter](https://twitter.com/ThomasADorfer) 和 [LinkedIn](https://www.linkedin.com/in/thomasdorfer/) 上找到我。

如果你喜欢支持我的写作，你可以通过一个[Medium 会员](https://thomasdorfer.medium.com/membership)来做到这一点，它为你提供了对我所有故事以及 Medium 上成千上万其他作者的故事的访问权限。
