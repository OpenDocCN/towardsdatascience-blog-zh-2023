# Python 中的临时变量：可读性与性能

> 原文：[`towardsdatascience.com/temporary-variables-in-python-readability-versus-performance-f6708b5f293c`](https://towardsdatascience.com/temporary-variables-in-python-readability-versus-performance-f6708b5f293c)

## PYTHON 编程

## 临时变量可以使代码更清晰。那么这样的代码性能如何呢？

[](https://medium.com/@nyggus?source=post_page-----f6708b5f293c--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----f6708b5f293c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f6708b5f293c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f6708b5f293c--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----f6708b5f293c--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f6708b5f293c--------------------------------) ·8 分钟阅读·2023 年 6 月 1 日

--

![](img/df7237943e419001b72e9b50f1dae8b8.png)

Python 的快捷方式快吗？图片由[Stefan Steinbauer](https://unsplash.com/@usinglight?utm_source=medium&utm_medium=referral)提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

临时变量是生命周期很短的变量：

[](https://en.wikipedia.org/wiki/Temporary_variable?source=post_page-----f6708b5f293c--------------------------------#:~:text=In%20computer%20programming%2C%20a%20temporary,a%20variable%20with%20local%20scope) [## 临时变量 - 维基百科

### 来自维基百科，免费的百科全书。在计算机编程中，临时变量是生命周期很短的变量……

[en.wikipedia.org](https://en.wikipedia.org/wiki/Temporary_variable?source=post_page-----f6708b5f293c--------------------------------#:~:text=In%20computer%20programming%2C%20a%20temporary,a%20variable%20with%20local%20scope)

临时变量在编程中非常常用，你不需要知道这个术语也可以使用临时变量。一个最常见的用例是使代码更清晰，例如，在管道中：

```py
input → tempvar_1 := func_1(input) →
        tempvar_2 := func_2(tempvar_1) →
        func_3(tempvar_2) → output
```

在这里，我使用了 Python 的[海象运算符](https://docs.python.org/3/whatsnew/3.8.html#assignment-expressions)来直观地表示赋值，就像在 Python 代码中使用的一样。在这个管道中，我们有两个临时变量：`tempvar_1` 和 `tempvar_2`。它们在数据流过代码的时间上生命周期很短，尽管在实际时间上可能很长。`tempvar_1` 仅用于一个目的：将管道第一步的结果传递到下一步。但请注意，从技术上讲，它是没有必要的：

```py
input → func_3(func_2(func_1(input))) → output
```

虽然这两个版本的功能相同，但后者的可读性可能大大降低。因此，前者在编程中被广泛使用，唯一的原因是使代码更清晰。

注意，如果`tempvar_1`或`tempvar_2`在代码后续中使用，它们就不再是临时变量，因为它们的生命周期不会很短。为了简单起见，我们可以假设临时变量是你只使用一次的变量，用于将一个可调用的输出作为输入传递给另一个。

你是否曾经思考过在管道中使用临时变量是否比直接——和最短——的计算方式更好？比如，以下两个代码片段哪个更好？

```py
# snippet 1
third_function(second_function(first_function(x)))

# snippet 2
x1 = first_function(x)
x2 = second_function(x1)
x3 = third_function
```

或者，这次使用简单的算术运算：

```py
# snippet 1
x1 = 2.056 * x
x2 = x1 / (1 + x1)
y = 2.3 / (- x2 - 7.33)

# snippet 2
y = 2.3 / (- 2.056 * x / (1 + 2.056 * x) - 7.33)
```

你会选择哪种方式？这重要吗？

Python 因各种原因而非常受欢迎，其中之一是其代码的*可读性*。同时，Python 也因其*性能差*而闻名——尽管它并不像许多人声称的那样糟糕，正如我在下面的文章中所写的：

[](https://medium.com/pythoniq/the-speed-of-python-it-aint-that-bad-9f703dd2924e?source=post_page-----f6708b5f293c--------------------------------) [## Python 的速度：并没有那么糟糕！

### 我一直听到 Python 太慢了。这是真的吗？

medium.com](https://medium.com/pythoniq/the-speed-of-python-it-aint-that-bad-9f703dd2924e?source=post_page-----f6708b5f293c--------------------------------)

很多时候，你可以——也需要——在可读性和性能之间进行选择。有时你可能需要哪怕是最微小的性能提升，即使这意味着可读性降低。其他时候，小幅度的性能提升意味着没有副作用，并且代码既可读又易懂；为何不选择它呢？

当性能的提升带来一些成本时，你应该小心。你应该问自己——或你所在的开发团队——以下问题：这种微小的性能提升是否值得降低代码的可读性？

在这篇文章中，我想向你展示一个通过避免临时变量实现的改进的例子。去除它们可以稍微提高性能，但通常会以降低可读性为代价。是的，通常是这样，所以*不总是*：如果你运气好，去除临时变量可以帮助你同时提高性能*和*可读性。这是完美的情况，不是吗？

# Python 代码中的临时变量

想象一下你想实现一个计算一系列事物的函数。为了简单起见，我们将进行一些基本的算术计算，使例子变得简单。然而，在现实生活中，这样的管道可能包含多个函数执行各种操作，甚至相当复杂。

```py
def calc_with_tempvar(x):
    y = x**2
    z = y/2
    f = z + 78
    g = f/333.333
    return g
```

因此，我们从 `x` 开始，然后计算 `y`、`z`、`f`，最终得到 `g`，`g` 是最终输出，因此返回。这类似于 [*函数组合*](https://en.wikipedia.org/wiki/Function_composition_(computer_science))，不同之处在于这里我们不是组合函数，而是组合计算。然而，在许多场景中，你将会有实际的函数；例如，代替 `y = x**2`，你可以有 `y = some_function(x)`。在 Python 中类似的一个完美示例就是生成器管道：

[](/building-generator-pipelines-in-python-8931535792ff?source=post_page-----f6708b5f293c--------------------------------) ## 在 Python 中构建生成器管道

### 本文提出了一种优雅的构建生成器管道的方法。

towardsdatascience.com

及其一般版本，理解管道：

[](/building-comprehension-pipelines-in-python-ec68dce53d03?source=post_page-----f6708b5f293c--------------------------------) ## 在 Python 中构建理解管道

### 理解管道是 Python 特有的构建管道的概念。

towardsdatascience.com

在简单的情况下，例如我们的 `calc_with_tempvar()` 函数中，这种方法似乎有些多余。相反，我们可以简单地做如下操作：

```py
def calc_without_tempvar(x):
    return ((x**2)/2 + 78)/333.333
```

这些测试表明，两者产生了完全相同的结果：

```py
>>> for x in (1, 2.3, 0.0000465, 100_000_000.004):
...     assert calc_with_tempvar(x) == calc_without_tempvar(x)
```

没有输出意味着这确实是正确的。

首先，让我们拆解这两个函数，看看它们如何转换为 Python 字节码：

![](img/cc0bd7ca49181294b52740181df7c134.png)

使用 dis.dis() 函数对两个函数进行拆解。图片由作者提供

即使不分析这两个函数的字节码，我们也可以看到，使用临时变量的函数比不使用临时变量的函数在 Python 中需要做更复杂的工作。这不奇怪，对吧？函数的定义方式本身表明 `calc_with_tempvar()` 要达到结果需要做更多工作，而不是 `calc_without_tempvar()`。

## 临时变量：性能

然而，这如何转化为性能呢？为了了解这一点，让我们使用 `[perftester](https://github.com/nyggus/perftester)` Python 包，它专门用于基准测试和测试 Python 函数的时间和内存性能：

[](/benchmarking-python-functions-the-easy-way-perftester-77f75596bc81?source=post_page-----f6708b5f293c--------------------------------) ## 轻松基准测试 Python 函数：perftester

### 你可以使用 perftester 轻松地基准测试 Python 函数。

towardsdatascience.com

对于基准测试，我在 Windows 10 机器上的 WSL 1 中使用了 Python 3.11，配备 32GB 的 RAM 和四个物理（八个逻辑）核心。然而，在我们的案例中，原始时间并不那么重要；我们将重点关注相对比较。

首先，让我更改基准测试的默认设置。我将使用 2000 万次函数调用重复 7 次；从中选择最快的一次作为基准结果。

```py
>>> import perftester
>>> perftester.config.set_defaults(
...     "time",
...     Number=20_000_000,
...     Repeat=7,
... )
```

现在实际的基准测试，对于一个`float`数值：

```py
>>> x = 1.67
>>> t1 = perftester.time_benchmark(calc_with_tempvar, x)
>>> t2 = perftester.time_benchmark(calc_without_tempvar, x)
```

然后让我们看看结果¹：

```py
>>> perftester.pp({
...     "1\. composition": t1["min"],
...     "2\. one-shot": t2["min"],
...     "3\. composition--to--one-shot ratio": t1["min"] / t2["min"]
... })
{'1\. composition': 2.063e-07,
 '2\. one-shot': 1.954e-07,
 '3\. composition--to--one-shot ratio': 1.056}
```

如预期的那样，一次性版本（不使用临时变量）更快——大约快 5%。一方面，这并不多。另一方面，这仅仅是通过如此微小的改变——如此小的变化就达到了 5%！

上述计算是快速的。然而，对于较长的计算，差异可能接近于不可见。

你注意到我们可以稍微改进一下`calc_with_tempvar()`函数吗？我们需要最后一个对象`g`吗？有时候，像这样的对象通过一个好的名字可以提高函数的可读性，但在这种情况下并不需要——所以我们不需要`g`。让我们看看去掉它是否会提高性能：

```py
def calc_with_tempvar_shorter(x):
    y = x**2
    z = y/2
    f = z + 78
    return f/333.333
```

```py
>>> t3 = perftester.time_benchmark(calc_without_tempvar_shorter, x)
>>> t3["min"]
1.998e-07
```

一个微小的改进，因为组合版本比这个版本慢`1.032`倍，而这个版本比一次性版本慢`1.023`倍。但再次强调，这种改进是通过如此微小的变化实现的！既然如此，这个微小的改变值得使用吗？

# 结论

*对我来说——绝对值得，但并非总是。*

关键是，当性能不重要时，优先考虑可读性。如果程序运行多一分钟、10 秒钟或甚至半秒钟都不会改变任何事情——干脆不要考虑通过这些技巧来提高性能。为什么要这样做？为什么要为了微小的改进而降低可读性呢？只需关注可读性。

当然，有时去掉临时变量会提高函数的可读性。在这种情况下，为什么我们还要讨论这个？再次强调，优先考虑可读性，当这也意味着提高性能时——那就完美了。

有时性能确实很重要。即使是秒的分割也可能产生差异。如果是这种情况，你应该分析你的代码并找出瓶颈。其他时候，你可能希望优化代码的每一个部分。一个例子是为他人使用的框架进行工作，对于其中一些人，性能将很重要。在这种情况下，作为框架作者的*你*有责任提供尽可能快的工具。否则，你有可能会冒着一些用户不会使用你的框架的风险。

*总结*：

+   如果性能很重要，避免使用像`calc_with_tempvar()`中的临时变量。如果性能重要性较低（如果有的话），则优先考虑可读性——这意味着是否使用临时变量的决定应该完全基于代码的可读性。

+   临时变量并不总是增加可读性。例如，假设你有一个数学函数 `y(x) = ((x**2)/2 + 78)/333.333`。你认为 `calc_with_tempvar()`，那个包含所有临时变量的函数，会提高可读性吗？我不这么认为。

因此，有时临时变量会提高代码的可读性，有时则不会。如果性能至关重要，请记住，临时变量可能会增加一些轻微的开销。更多时候，这些开销是微不足道的——但在一些项目中，即使是那些秒数的分割也可能很重要。

总之，始终双重检查是否值得在你的代码中去除临时变量——或者是否值得使用它们。

# 注释

¹ 代码使用了 `perftester.pp()` 函数，该函数以标准库函数 `pprint.pprint()` 的方式美观地打印 Python 对象，并将其中的所有数字四舍五入到四位有效数字。它使用了[rounder](https://pypi.org/project/rounder/)包：

[## GitHub - nyggus/rounder: 用于在复杂 Python 对象中对浮点数和复杂数字进行四舍五入的 Python 包](https://github.com/nyggus/rounder?source=post_page-----f6708b5f293c--------------------------------)

### `rounder` 是一个轻量级的包，用于在复杂的 Python 对象（如字典、列表、元组等）中对数字进行四舍五入。

[github.com](https://github.com/nyggus/rounder?source=post_page-----f6708b5f293c--------------------------------)

感谢阅读。如果你喜欢这篇文章，你可能也会喜欢我写的其他文章；你可以在[这里](https://medium.com/@nyggus)查看。如果你想加入 Medium，请使用下面的推荐链接：

[## 使用我的推荐链接加入 Medium - Marcin Kozak](https://medium.com/@nyggus/membership?source=post_page-----f6708b5f293c--------------------------------)

### 阅读 Marcin Kozak 的每一个故事（以及 Medium 上成千上万其他作家的故事）。你的会员费直接支持…

[medium.com](https://medium.com/@nyggus/membership?source=post_page-----f6708b5f293c--------------------------------)
