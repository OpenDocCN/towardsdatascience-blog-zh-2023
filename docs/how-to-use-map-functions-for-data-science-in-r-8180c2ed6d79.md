# 如何在 R 中使用映射函数进行数据科学

> 原文：[`towardsdatascience.com/how-to-use-map-functions-for-data-science-in-r-8180c2ed6d79`](https://towardsdatascience.com/how-to-use-map-functions-for-data-science-in-r-8180c2ed6d79)

## 从 tidyverse 中学习强大的函数式编程工具

[](https://roryspanton.medium.com/?source=post_page-----8180c2ed6d79--------------------------------)![Rory Spanton](https://roryspanton.medium.com/?source=post_page-----8180c2ed6d79--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8180c2ed6d79--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8180c2ed6d79--------------------------------) [Rory Spanton](https://roryspanton.medium.com/?source=post_page-----8180c2ed6d79--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8180c2ed6d79--------------------------------) ·7 分钟阅读·2023 年 2 月 2 日

--

![](img/7814864921e31d3a070fa03387a45857.png)

图片由 [Z](https://unsplash.com/@dead____artist?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

所有数据科学家都需要重复代码。无论是为多个数据集拟合模型，还是一次更改多个值，多次运行相同的代码都是必不可少的。

重复代码有很多方法。然而，尽管大多数程序员使用循环，还有更简洁、可读性更高且效率更高的替代方案。引入`map`家族的函数，来自 purrr 包。

在本文中，我将解释什么是映射，以及如何使用`map`、`map2`和`pmap`函数，这些函数来自[R 的 purrr 包](https://purrr.tidyverse.org/)。

# 什么是“映射”，以及如何在 R 中实现？

purrr 包所做的映射并不是大多数人熟悉的地理类型。R 拥有优秀的地理空间分析工具，但这里讨论的不是这些。

“[映射](https://en.wikipedia.org/wiki/Map_(higher-order_function))”是编程中的一个专业术语，指的是在一组参数上反复应用一个函数。

我们可以通过一个简单的例子来理解这一点。假设我们有一个列表，每个元素包含 100 个数字。如果我们想计算每组数字的均值，我们可以使用`map`以直接的方式来完成。

```py
library(tidyverse)

# Set seed for reproducibility
set.seed(1234)

# Generate list of numeric values for this example
values_list <- list(rnorm(100, 100, 15), 
                    rnorm(100, 110, 20), 
                    rnorm(100, 75, 15))

# Use map to get the mean of each set of numbers
map(values_list, mean)
```

在这里，我们首先加载 tidyverse，它包含提供映射函数的 purrr 包。然后，我们使用`map`来获取每个列表元素的均值。

基本的`map`函数接受两个参数。首先，我们指定一个要应用函数的列表或向量；在本例中是`values_list`。其次，我们命名要应用的函数；`mean`。这使得`map`成为一个“[高阶函数](https://en.wikipedia.org/wiki/Higher-order_function)”，因为它将另一个函数作为参数。运行此代码会给出`values_list`中每个元素的均值，如下所示。

![](img/668e596f283a5129731a5ab8c50e5028.png)

除了命名我们想应用的函数外，我们还可以使用公式来定义函数。当定义带有附加参数或更复杂表达式的函数时，这一点尤其有用。在下面的代码中，我们使用这样的公式来计算每个列表元素的均值，即使数据中存在缺失值。

```py
# Add some missing values to the second element of the list
values_list[[2]][c(2, 24, 93)] <- NA

# Get the means with map again, this time discounting NAs
map(values_list, ~ mean(., na.rm = T))
```

在这段代码中，我们将函数定义为一个带有波浪号（~）符号的公式。在`mean`内部，我们使用点（.）符号来引用作为参数的列表元素。用公式定义`mean`函数使我们可以指定额外的参数，例如`na.rm = TRUE`，用于处理`values_list`中的 NA 值。这会生成一个均值列表，如之前的例子所示。

# 为什么使用 `map` 函数而不是循环？

如果你熟悉 for 循环，你可能会发现`map`的操作和你如何使用循环解决上述问题之间的相似之处。这里是将相同操作用循环表达的方式：

```py
# Expressing the operation above as a loop
for (i in values_list) {
  print(mean(i, na.rm = TRUE))
}
```

这个循环非常简单。每次迭代时，它从`values_list`中获取一个新元素，并打印该元素中值的均值，就像`map`示例一样。甚至有一个[R 包](https://timteafan.github.io/loopurrr/)可以将`map`语句转换为循环！

所以，如果你可以将 `map` 语句表达为循环，你为什么还要学习 `map` 呢？

与循环相比，映射有一些优势：

+   `map` 函数通常更简洁，占用一行，而不是至少三行。这意味着它们可以更容易地内联使用和嵌入其他函数中，从而开启了强大的新使用可能性。

+   与循环不同，映射迫使你定义函数。这会导致更整洁、更具模块化的代码，便于重用。

+   相关于上一点，定义函数通常会加速代码的执行。这意味着映射操作可能比循环更快（尽管这并不一定保证）。

循环仍然有其用武之地，有时在它们是最快选项或在重复非常复杂或不寻常的操作时使用它们更为合适。也就是说，`map`系列也能处理更复杂的函数。

# 如何使用 `map` 的变体：`map2` 和 `pmap`

`map2` 和 `pmap` 函数是 `map` 的直接扩展。

## `map2`

`map2` 允许你应用一个接受两个参数的函数。例如，你可以使用它来拟合几个线性模型，这些模型以 x 和 y 变量作为参数。

```py
# Define combinations of variables to model
x <- c("mpg", "hp", "wt")
y <- c("hp", "wt", "mpg")

# Apply the lm function to these variables using map2
map2(x, y, ~ lm(get(.x) ~ get(.y), data = mtcars))
```

在这里，我们创建了两个向量 `x` 和 `y`，包含了 `mtcars` 数据集中的变量组合。在我们的 `map2` 函数表达式中，我们将这些变量称为 `.x` 和 `.y`。`get` 函数只是帮助 `lm` 使用名称定位 `mtcars` 中感兴趣的列。这导致三个线性模型被拟合；每个指定的变量组合对应一个。

## pmap

`pmap` 允许你对具有两个以上参数的函数表达式进行操作。例如，可以将三个不同列表中的值连接起来进行进一步分析。

```py
# Define some example values from a clinical trial
baseline <- c(101, 92.3, 98.2)
treatment <- c(103.3, 92.1, 99.8)
followup <- c(112.1, 95.4, 104.2)

# Concatenate those values together, rowwise
pmap(list(baseline, treatment, followup), ~ c(..1, ..2, ..3))
```

调用 `pmap` 时，我们将输入数据封装在一个列表中。每个输入被称为 `..1, ..2, ..3` 等。结果是一个连接值的列表，如下所示。

![](img/876e3197a99124c24e6d112463f48fca.png)

# 如何控制 map 函数的输出

你可能已经注意到，到目前为止所有的示例都返回了列表作为输出。然而，也可以让 map 函数返回不同类型的向量和数据框。

回到本文的第一个示例，我们可以使用 `map` 获取一个均值列表。但我们也可以通过使用 `map_dbl` 函数来获取一个平坦的均值向量。

```py
map_dbl(values_list, ~ mean(., na.rm = TRUE))
```

![](img/e7c2e309bedee543e0d546e72e1743ef.png)

上述代码的结果：一个均值向量，而不是列表。

向我们的 map 函数名称添加 “_dbl” 实现了这一点。还有一些其他变体适用于 `map`、`map2` 和 `pmap`。以下是它们的列表及其功能：

+   “_dbl” 返回一个双精度向量

+   “_lgl” 返回一个逻辑（TRUE/FALSE）向量

+   “_int” 返回一个整数向量

+   “_chr” 返回一个字符向量

+   “_raw” 返回一个原始向量

+   “_dfr” 返回一个 tibble，每行是一次 map 操作的结果

+   “_dfc” 返回一个 tibble，每列是一次 map 操作的结果

使用这些函数变体返回向量和 tibble 可以解锁许多可能性。我经常在其他 tidyverse 函数中使用 `map_dbl` 和 `map_chr` 来基于自定义函数创建新列。我也使用 `map_dfr` 和 `map_dfc` 来用一两行代码替代长循环。这些函数很棒，毫无疑问值得学习和记住。

# 是否应该使用 map 函数代替 base-R 的 “apply” 函数？

正如我们所建立的，purrr 包提供了一整套 `map` 函数，可以执行各种操作。

然而，R 也配备了一套内置的映射函数；`apply` 函数。这些函数的功能与 purrr 的基本 map 函数相同，但在语法和处理数据类型的方式上有所不同。

尽管 base-R 的 `apply` 函数被广泛使用，但我更喜欢 purrr 的 `map` 函数，原因有几个：

+   这样更容易确切了解 `map` 函数将输出何种类型的数据。这在 base-R 函数中并不总是如此，后者有时会根据输入表现出不可预测的行为。

+   它们被设计为与其他 tidyverse 包兼容，这对于已经在你的工作流程中使用它们来说非常有用。

+   Purrr 还包含了更多高级的映射函数，这是 `apply` 系列无法匹敌的。学习 `map` 可以让你更轻松地掌握这些函数。

有时你可能会更倾向于使用 `apply`。如果你正在开发一个包，并且不想引入额外的依赖，内置函数是最佳选择。`apply` 函数的运行速度也可能比 `map` 函数更快，因此在重复计算密集型操作时，它们可能更值得使用。

尽管如此，`map` 函数对于新手来说更容易学习，这也是我在这里重点讲解它们的原因。此外，一旦你掌握了 `map` 的使用，`apply` 函数也会变得容易理解。

学习 R 中的 `map` 函数是扩展你的数据科学工具集的好方法。与循环相比，映射通常节省空间，而不牺牲可读性。它还适用于许多任务，是高效重复代码的一个极佳标准方法。因此，既然你知道了如何使用 `map`，就试试看，享受 tidyverse 中最强大的工具之一吧。
