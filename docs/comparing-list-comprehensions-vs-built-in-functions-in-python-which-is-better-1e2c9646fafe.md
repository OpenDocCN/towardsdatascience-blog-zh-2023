# 比较 Python 中的列表推导式与内置函数：哪种更好？

> 原文：[`towardsdatascience.com/comparing-list-comprehensions-vs-built-in-functions-in-python-which-is-better-1e2c9646fafe`](https://towardsdatascience.com/comparing-list-comprehensions-vs-built-in-functions-in-python-which-is-better-1e2c9646fafe)

## 对语法、可读性和性能的深入分析

[](https://thomasdorfer.medium.com/?source=post_page-----1e2c9646fafe--------------------------------)![Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----1e2c9646fafe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1e2c9646fafe--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1e2c9646fafe--------------------------------) [Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----1e2c9646fafe--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1e2c9646fafe--------------------------------) ·阅读时长 9 分钟·2023 年 3 月 21 日

--

![](img/24a86e8397f03f62c53d4cf68e844fb2.png)

作者提供的图片。

你是否曾在雨天通过 Netflix 滚动，感到被无尽的电影和节目选择所压倒？

在编程中，选择的悖论可能同样令人不知所措。面对如此众多的库和框架，提供了无数种实现相同目标的方法，容易在选择的海洋中迷失方向。

在 Python 中，这种情况通常出现在程序员需要在**函数式编程**方法（如内置函数 `map()`、`filter()` 和 `reduce()`）与更具 Python 风格的**列表推导式**之间做选择时。

在这篇文章中，我们将通过语法、可读性和性能的视角探讨这两种不同方法的优缺点。

# 列表推导式

在 Python 中，列表推导式是一种简洁的方法，它基于已存在的列表生成一个新列表。简单来说，它本质上是一个**for 循环**的一行代码，并可以在末尾包含一个**if 条件**。语法可以分解为如下：

![](img/1580b25f72c556932fc1111478f01d23.png)

作者提供的图片。

假设我们有一个名为 `numbers` 的数字列表，我们希望从中选取偶数并对其平方。现在，老旧的方式是这样的：

```py
squared_numbers = []
for number in numbers:
    if number % 2 == 0:
        squared = number ** 2
        squared_numbers.append(squared)
```

然而，使用列表推导式，我们可以在一行代码中完成这一操作：

```py
squared_numbers = [i**2 for i in numbers if i % 2 == 0]
```

无论哪种方式都能得到相同的结果，但列表推导式提供了一个更清晰、更可读的解决方案，因为其语法字面上是：*“****做这个*** *对于* ***每个值*** *在* ***这个列表*** *如果* ***这个条件*** *满足”*。

一般来说，列表推导式通常比常规的 `for` 循环更快，因为它们不需要在每次迭代时查找列表并调用其 `append` 方法。

现在我们对列表推导式有了比较好的理解，接下来我们来看看它们与一些常用的内置函数（如 `map()`、`filter()` 和 `reduce()`）相比如何。这就是我之前提到的选择悖论。程序员往往知道这些方法的存在，但该选择哪一个呢？

让我们逐一了解每个内置函数，并将它们与 Pythonic 对应的列表推导式进行比较。

# Map

如果你的目标是对可迭代对象（如列表）中的每一项应用转换函数，那么 `map()` 函数是一个很好的起点。其语法相当简单，只需要两个输入参数：(1) 一个转换函数，以及 (2) 一个可迭代对象（即你的输入列表）。

假设我们有一个与欧元对应的数字列表，我们希望将它们转换为美元。这可以通过以下方式完成：

```py
>>> eur = [1, 2, 3, 4, 5]
>>> usd = list(map(lambda x: x / 0.939276, eur))
>>> usd
[1.0646497940967299,
 2.1292995881934598,
 3.1939493822901897,
 4.2585991763869195,
 5.323248970483649]
```

注意，我们必须在这里明确指定 `list()` 函数，因为 `map()` 本地返回的是一个迭代器——一个 map 对象。

还要注意，`map()` 允许你使用这些匿名的、即兴的 lambda 函数，这些函数允许你即时定义一个函数。如果你想了解更多关于 lambda 函数、它们的语法以及如何使用它们的内容，可以查看以下文章：

[](/how-to-effectively-use-lambda-functions-in-python-as-a-data-scientist-fd6171554053?source=post_page-----1e2c9646fafe--------------------------------) ## 如何在数据科学中有效使用 Python 的 Lambda 函数

### 对其语法、功能以及在数据科学中的适用性的介绍

towardsdatascience.com

## 与列表推导式的比较

你可能已经注意到，相同的任务也可以通过列表推导式来完成。那么让我们看看它们在可读性和性能方面的比较。

具体来说，我们将讨论三种场景：(1) 列表推导式，(2) 使用预定义输入函数的 `map()`，以及 (3) 使用即兴的 lambda 函数的 `map()`。

```py
# predefined conversion function
def eur_to_usd(x):
    return x / 0.939276
```

```py
>>> lst = list(range(1000000))

# list comprehension
>>> %timeit -r 10 -n 10 [i / 0.939276 for i in lst]
163 ms ± 4.96 ms per loop (mean ± std. dev. of 10 runs, 10 loops each)

# map with predefined input function
>>> %timeit -r 10 -n 10 list(map(eur_to_usd, lst))
197 ms ± 4.33 ms per loop (mean ± std. dev. of 10 runs, 10 loops each)

# map with lambda function
>>> %timeit -r 10 -n 10 list(map(lambda x: x / 0.939276, lst))
204 ms ± 4.28 ms per loop (mean ± std. dev. of 10 runs, 10 loops each)
```

就简洁性和可读性而言，列表推导式在这里似乎赢得了比赛。程序员的意图立即显现出来，不需要额外的关键字或定义额外的函数。然而，值得注意的是，对于更复杂的操作，可能需要定义单独的转换函数，这将削弱列表推导式通常因其可读性而获得的一些优势。

就性能而言，上述示例清楚地表明，列表推导式是最快的，其次是使用预定义输入函数的`map()`，最后是使用 lambda 函数的`map()`。

关于使用临时 lambda 函数的问题是：它会为输入列表中的每个项目调用，导致计算开销，因为 lambda 函数对象的创建和销毁，最终导致性能下降。相比之下，预定义函数经过优化并存储在内存中，这使得执行更为高效。

## 底线

在性能方面，列表推导式明显优于`map()`。此外，它们的语法易于阅读，通常被认为更直观，并且被认为比源自函数式编程的`map()`更具 Python 风格。

# 过滤

`filter()`函数允许你根据给定条件选择可迭代对象的一个子集。与`map()`类似，它需要两个输入参数：（1）过滤函数，通常是[lambda 函数](https://how-to-effectively-use-lambda-functions-in-python-as-a-data-scientist-fd6171554053)，以及（2）一个可迭代对象。

以下是一个示例，我们过滤掉所有奇数，只保留偶数：

```py
>>> numbers = [1, 2, 3, 4, 5]
>>> filtered = list(filter(lambda x: x % 2 == 0, numbers))
>>> filtered
[2, 4]
```

类似于`map()`，我们必须明确声明我们希望返回一个列表，因为`filter()`原生返回一个迭代器对象。

## 与列表推导式的比较

让我们看看内置`filter()`函数的性能差异，再次使用预定义输入函数和 lambda 函数，并与列表推导式进行比较。

```py
# predefined filter function
def fil(x):
    if x % 2 == 0:
        return True
    else:
        return False
```

```py
>>> lst = list(range(1000000))

# list comprehension
>>> %timeit -r 10 -n 10 [i for i in lst if i % 2 == 0]
84.6 ms ± 2.24 ms per loop (mean ± std. dev. of 10 runs, 10 loops each)

# filter with predefined filter function
>>> %timeit -r 10 -n 10 list(filter(fil, lst))
134 ms ± 6.39 ms per loop (mean ± std. dev. of 10 runs, 10 loops each)

# filter with lambda function
>>> %timeit -r 10 -n 10 list(filter(lambda x: x % 2 == 0, lst))
159 ms ± 6.67 ms per loop (mean ± std. dev. of 3 runs, 10 loops each)
```

就可读性而言，对于`map()`的说法同样适用于`filter()`：列表推导式相当易于阅读，不需要任何预定义或临时函数或额外的关键字。然而，有人认为使用`filter()`函数会立即展示程序员的意图，即过滤某物，可能比列表推导式更直接。当然，这是一项高度主观的事项，取决于个人的偏好和品味。

就性能而言，我们看到的结果与`map()`获得的类似。列表推导式是最快的，其次是使用预定义过滤函数的`filter()`，最后是使用临时 lambda 函数的`filter()`。这再次是由于 lambda 函数需要在运行时创建新函数对象所带来的开销。

## 底线

列表推导式的性能超过其函数式`filter()`对应物——几乎是 2 倍，并且通常被认为更具 Python 风格。然而，易读性在这方面略显主观。有些人喜欢列表推导式直观和 Pythonic 的方式，而另一些人则偏爱使用`filter()`函数，因为它清晰地传达了其功能和程序员的意图。

# 减少

最后，让我们看一下`reduce()`。这个内置函数通常用于需要在多个步骤中累积单一结果的情况。它还接受两个输入参数：（1）一个归约函数，和（2）一个可迭代对象。

让我们通过一个示例来使其功能更清晰。在这个例子中，我们希望计算一个整数列表的乘积：

```py
>>> from functools import reduce
>>> integers = [1, 2, 3, 4, 5]
>>> reduce(lambda x, y: x * y, integers)
120
```

再次，我们使用一个 lambda 来定义我们的归约函数，这里是对整数列表进行简单的滚动乘法。这会执行以下计算：1 x 2 x 3 x 4 x 5 = 120。

## 与列表推导式的比较

使用列表推导式达到相同的目标这次有点棘手，需要一些额外的步骤，例如初始化变量和使用[海象运算符](https://docs.python.org/3/whatsnew/3.8.html)：

```py
>>> integers = [1, 2, 3, 4, 5]
>>> product = 1
>>> [product := product * num for num in numbers]
>>> product
120
```

虽然仍然可以通过列表推导式获得相同的结果，但这些额外的步骤显著降低了代码的可读性。

此外，现在还有多种低代码替代方法，例如`math.prod()`：

```py
>>> from math import prod
>>> integers = [1, 2, 3, 4, 5]
>>> prod(integers)
120
```

然而，在性能方面，这两者之间似乎没有重大区别：

```py
>>> integers = list(range(1, 10001))

# using reduce
>>> %timeit -r 10 -n 100 reduce(lambda x, y: x * y, integers)
24.5 ms ± 299 µs per loop (mean ± std. dev. of 10 runs, 100 loops each)

# using math.prod
>>> from math import prod
>>> %timeit -r 10 -n 100 prod(integers)
23.8 ms ± 707 µs per loop (mean ± std. dev. of 10 runs, 100 loops each)
```

## 关键点

在 Python 中，`reduce()`用于对列表中的值对进行滚动计算的使用在逐年减少，主要是因为有更高效和直观的替代方法，如`math.prod()`。`reduce()`和列表推导式在这里并没有提供一个清晰的语法，这使得读者很难快速理解代码。

PS：如果你仍然是`reduce()`的频繁用户，我很想在评论中了解你的使用案例！

# 结论

尽管在其他语言中不如其他语言那样普遍，`map()`、`filter()`以及偶尔使用的`reduce()`仍然在基于 Python 的应用程序中使用。然而，列表推导式由于其更直观的语法被视为更具 Python 风格，并且在大多数情况下，可以替代`map()`和`filter()`函数，同时还带来明显的性能提升。

相比之下，`reduce()`函数的特性使其不容易被列表推导式替代。然而，如上所述，它们可以被低代码替代方法如`math.prod()`函数替代。

## 喜欢这篇文章吗？

让我们联系一下！你可以在[Twitter](https://twitter.com/ThomasADorfer)和[LinkedIn](https://www.linkedin.com/in/thomasdorfer/)找到我。

如果你喜欢支持我的写作，你可以通过[Medium 会员](https://thomasdorfer.medium.com/membership)来做到这一点，这将为你提供访问我所有故事以及 Medium 上其他成千上万作家的权限。

[](https://medium.com/@thomasdorfer/membership?source=post_page-----1e2c9646fafe--------------------------------) [## 通过我的推荐链接加入 Medium — Thomas A Dorfer

### 阅读 Thomas A Dorfer 的每一篇故事（以及 Medium 上其他成千上万的作家的故事）。你的会员费直接支持…

medium.com](https://medium.com/@thomasdorfer/membership?source=post_page-----1e2c9646fafe--------------------------------)
