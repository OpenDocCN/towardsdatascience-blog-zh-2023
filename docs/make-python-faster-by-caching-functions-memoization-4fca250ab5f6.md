# 通过缓存函数提升 Python 速度：记忆化

> 原文：[`towardsdatascience.com/make-python-faster-by-caching-functions-memoization-4fca250ab5f6`](https://towardsdatascience.com/make-python-faster-by-caching-functions-memoization-4fca250ab5f6)

## PYTHON 编程

## 这篇文章讨论了使用 Python 标准库进行记忆化。functools.lru_cache 装饰器让这一切变得如此简单！

[](https://medium.com/@nyggus?source=post_page-----4fca250ab5f6--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----4fca250ab5f6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4fca250ab5f6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4fca250ab5f6--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----4fca250ab5f6--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4fca250ab5f6--------------------------------) ·11 分钟阅读·2023 年 11 月 11 日

--

![](img/e3774b4417d263b6337423d06fdb6a85.png)

你可以让 Python 记住函数已经返回的结果，并加以利用。照片由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

我们都知道 Python 可能会很慢：

[](https://medium.com/pythoniq/the-speed-of-python-it-aint-that-bad-9f703dd2924e?source=post_page-----4fca250ab5f6--------------------------------) [## Python 的速度：没那么糟糕！

### 我经常听到 Python 太慢了。真的如此吗？

medium.com](https://medium.com/pythoniq/the-speed-of-python-it-aint-that-bad-9f703dd2924e?source=post_page-----4fca250ab5f6--------------------------------)

在 Python 中，通常最耗时间的操作是调用执行复杂过程的函数和类方法。设想一下，如果你需要对相同的参数运行这样的函数两次，它将需要两倍的时间，即使这两个调用的输出完全相同。是否可以仅仅记住这个输出，并在需要时再次使用它？

是的，你可以！这叫做 [记忆化](https://en.wikipedia.org/wiki/Memoization)，这是编程中的一个常见术语。你可以实现自己的记忆化技术，但实际上你并不需要这样做。Python 提供了一个强大的记忆化工具，而且它在标准库中：`functools.lru_cache` 装饰器。

尽管记忆化通常非常高效，但在 Python 教科书中经常被忽略，即使是那些描述代码分析和内存节省的书籍也不例外。提到 Python 中记忆化的书籍包括 Julien Danjou 的 [*Serious Python*](https://nostarch.com/seriouspython)，Luciano Ramalho 的 [*流畅的 Python*, *第 2 版*](https://www.oreilly.com/library/view/fluent-python-2nd/9781492056348/)，以及 Steven F. Lott 的 [*函数式 Python 编程, 第 3 版*](https://www.packtpub.com/product/functional-python-programming-third-edition/9781803232577)。

本文展示了两件事：如何在 Python 标准库中简单地使用记忆化（即使用`functools.lru_cache`），以及这种技术的强大之处。然而，这并非全是美好的一面。因此，我们也会讨论使用`functools.lru_cache`缓存工具时可能遇到的问题。

# `使用 functools 模块进行缓存`

## `functools.lru_cache`

Python 提供了各种记忆化工具，但今天，我们讨论的是 Python 标准库的一部分：`functools.lru_cache`。

LRU 缓存策略代表*最近最少使用*。在这种策略中，当缓存的大小超过限制时，最少使用的项会被从缓存中移除。

要将其用于函数，请使用`@functools.lru_cache`进行装饰：

```py
from functools import lru_cache

@lru_cache
def foo():
    print("I am running the foo() function")
    return 10
```

注意`foo()`不接受任何参数，这基本上意味着它在一个会话期间只会运行一次：

```py
>>> x = foo()
I am running the foo() function
>>> x
10
>>> y = foo()
>>> y
10
>>> z = foo()
>>> z
10
```

实际上，在被调用三次之后，函数只运行了一次——但我们得到了相同的输出（`10`）三次。

当然，函数记忆化的真正威力体现在接受参数的函数上。这样的函数会对每一组新参数进行运行，但每当之前已经使用过相同的参数时，函数不会重新运行，而是使用记住的输出作为函数的输出。考虑以下函数：

```py
from collections.abc import Sequence

@lru_cache
def sum_of_powers(x: Sequence[float], pow: float) -> float:
    output = sum(xi**pow for xi in x)
    print(f"Call: sum_of_powers({x=}, {pow=})")
    return output
```

注意`sum_of_powers`的类型：

```py
>>> sum_of_powers
<functools._lru_cache_wrapper object at 0x7f...>
```

所以它的类型不是`function`而是`lru_cache_wrapper`。让我们看看函数的实际效果：

```py
>>> x = (1, 1, 2)
>>> sum_of_powers(x, 2)
Call: sum_of_powers(x=(1, 1, 2), pow=2)
6
>>> sum_of_powers(x, 3)
Call: sum_of_powers(x=(1, 1, 2), pow=3)
10
>>> sum_of_powers(x, 2)
6
>>> sum_of_powers(x, 3)
10
```

所以，函数的最后两次调用实际上并没有运行它——但由于记忆化，函数仍然返回了输出。

你可以使用`lru_cache`的`maxsize`参数来指示缓存中保持的项的数量。默认值为`128`。正如 Luciano Ramalho 在他的 [*流畅的 Python. 第 2 版*](https://www.oreilly.com/library/view/fluent-python-2nd/9781492056348/)*.* 书中解释的那样，为了获得最佳性能，你应该使用`maxsize`值为`2`的幂。当`maxsize`为`None`时，缓存可以保存任何数量的对象，更准确地说，是内存允许的数量。请谨慎，因为这可能会导致内存耗尽。

你可以使用一个额外的参数，即`typed`，它是一个布尔对象，默认值为`False`。当它为`True`时，不同类型的相同值会被分别保存。因此，整数值`1`和浮点值`1.0`会作为不同的项保存。当`typed=False`（默认值）时，它们会被作为一个项保存。

有一种方法可以了解装饰了`functools.lru_cache`的函数是如何工作的，感谢函数中添加的`cache_info`属性。更准确地说，这是一个方法：

```py
>>> sum_of_powers.cache_info
<built-in method cache_info of functools._lru_cache_wrapper object at 0x7f...>
```

这就是`cache_info()`的工作原理：

```py
>>> sum_of_powers.cache_info()
CacheInfo(hits=2, misses=2, maxsize=128, currsize=2)
```

这意味着以下几点：

+   `hits=2` → 缓存被使用了两次；实际上，我们对`x=(1, 1, 2)`和`pow=2`使用了函数两次，对`x=(1, 1, 2)`和`pow=3`也使用了两次，但缓存对于每个`x`的值只使用了一次。

+   `misses=2` → 这个输出表示两个具有新参数值的函数调用，因此缓存未被使用。未命中的次数与命中的次数比率越高，缓存的效果越差，因为函数经常使用新的参数值，而很少使用已经使用过的参数值。

+   `maxsize=128` → 缓存的大小；我们将在下文中讨论。

+   `currsize=2` → 缓存中已保留的元素数量。

在一个实际项目中，如果你想使用缓存，最好先进行一些实验，而`cache_info()`方法对此非常有帮助。记住，缓存不应该自动用于任何函数；这可能导致时间损失而不是收益，因为缓存本身也需要一些时间。

## `functools.cache`

从 Python 3.9 开始，`functools`模块还提供了[the](https://docs.python.org/3.9/library/functools.html#functools.cache) `[cache](https://docs.python.org/3.9/library/functools.html#functools.cache)` [decorator](https://docs.python.org/3.9/library/functools.html#functools.cache)。这只是一个包装器，围绕着`functools.lru_cache`，其中`maxsize=None`。因此，使用`functools.cache`装饰器等同于使用`functools.lru_cache(maxsize=None)`。说实话，我认为这不是一个必要的修正——在我看来，标准库不需要这样过于简化的包装器。使用`functools.lru_cache(maxsize=None)`有什么问题吗？

# 注释

## 用于类方法

函数缓存被认为是一种函数式编程工具，但它也可以用于类方法。与 Python 函数不同，Python 类可以有状态，这就是为什么在类中实现单独的缓存很简单：

```py
>>> class JustOneLetterCachedInside:
...     def __init__(self, letter):
...         self.letter = letter
...         self.cache = {}
...     def make_dict(self, n):
...         if n not in self.cache:
...             print(f"The output for n of {n} is being cached.")
...             self.cache[n] = [self.letter] * n
...         return self.cache[n]
>>> instance = JustOneLetterCachedInside("a")
>>> _ = instance.make_dict(10)
The output for n of 10 is being cached.
>>> _ = instance.make_dict(10)
>>> _ = instance.make_dict(10)
>>> _ = instance.make_dict(20)
The output for n of 20 is being cached.
>>> _ = instance.make_dict(20)
>>> instance.cache.keys()
dict_keys([10, 20])
```

这是一种非常简化的缓存方法，而且在更改类实例中的`self.letter`属性后将无法工作。不过，这个问题很容易解决，例如，可以通过将`self.cache`设为一个以字母为键的字典的字典来解决。我将实现留给你作为练习。

这是一个手动实现的示例，但我们也可以使用`functools.lru_cache`装饰器。与函数的用法一样简单：只需在类定义中装饰方法即可：

```py
>>> class JustOneLetter:
...     def __init__(self, letter):
...         self.letter = letter
...     @lru_cache
...     def make_dict(self, n):
...         return [self.letter] * n
>>> instance = JustOneLetter("a")
>>> _ = instance.make_dict(10)
>>> _ = instance.make_dict(10)
>>> _ = instance.make_dict(10)
>>> _ = instance.make_dict(20)
>>> _ = instance.make_dict(20)
>>> instance.make_dict.cache_info()
CacheInfo(hits=3, misses=2, maxsize=128, currsize=2)
```

正如你所看到的，这与函数的使用方式完全相同，所以当你看到这样的需求时，不要犹豫，也为类方法使用`functools.lru_cache`装饰器。

## 仅支持可哈希参数

标准缓存使用哈希表进行映射。这意味着对于具有不可哈希参数的函数，它将不起作用。因此，例如，你不能对以下类型的对象使用缓存：列表、集合和字典。

让我们深入分析上述函数`sum_of_powers()`。它的`x`参数的通用类型是`Sequence[float]`。`collections.Sequence`并未说明其是否可哈希；实际上，遵循该协议的一些类型是可哈希的，例如`tuple`，但其他一些则不是，例如`list`。注意：

```py
>>> sum_of_powers((1, 2, 3), 2)
Call: sum_of_powers(x=(1, 2, 3), pow=2)
14
>>> sum_of_powers([1, 1, 2], 2)
Traceback (most recent call last):
    ...
TypeError: unhashable type: 'list'

>>> from typing import Dict
>>> class Dicti(Dict[str, int]): ...
>>> @ lru_cache
... def foo(x: Dicti) -> int: return len(x)
>>> foo(Dict({"a": 1}))
Traceback (most recent call last):
    ...
TypeError: Type Dict cannot be instantiated; use dict() instead
```

仅供参考，静态类型检查器可以指出使用不可哈希参数调用的缓存装饰函数。这种调用是正常的：

![](img/869711fbc572a9dc49e05152110f663d.png)

来自 Visual Studio Code 的截图。函数调用时使用了一个可哈希的元组，没有静态错误。图像由作者提供

但这个例外：

![](img/4db0874895e429e27c5bec92f4f38973.png)

来自 Visual Studio Code 的截图。[Mypy](https://mypy.readthedocs.io/en/stable/) 指出静态错误，表明尝试缓存一个使用列表（不可哈希）的函数。图像由作者提供

## 纯函数和非纯函数

你可能听说过，记忆化应该仅用于所谓的纯函数，即没有副作用的函数。副作用是指在后台进行的操作；例如，改变全局变量，从数据库中读取数据，记录信息。

通常，非纯函数确实不应缓存，但也有一些例外。例如，想象一个生成报告并将其保存到文件中的函数，基于提供的函数参数值。缓存该函数意味着只生成一次这些参数值的报告，而不是每次函数调用时都生成一次。一般来说，通过缓存可以节省 I/O 操作的成本。然而，在这种情况下，我们必须确保该函数的副作用在后续调用中不会发生变化。例如，如果一个函数会用完全相同内容的新文件覆盖旧文件，但操作时间很重要。

实际上，我们已经观察到具有副作用的函数的缓存。我们在上面做过，当我们缓存打印消息到控制台的函数时。正如你可以回忆的，这些函数只在第一次调用时打印消息，而在使用缓存时不再打印。这正是我们所说的：当副作用不重要时，你可以使用缓存；当副作用重要时，你应该避免，因为你可能会面临副作用只在函数第一次调用时被观察到，而在后续调用中没有观察到的风险。

# 性能

我不会展示通常用于缓存的示例，即计算阶乘的函数。相反，我将使用一个非常简单的函数，它使用 dict comprehension 创建一个简单的字典。

我将使用以下代码：

```py
import perftester
from functools import lru_cache
from rounder import signif

def make_dict(n):
    return {str(i): i**2 for i in range(n)}

@lru_cache
def make_dict_cached(n):
    return {str(i): i**2 for i in range(n)}

n = 100

perftester.config.set_defaults(
    which="time",
    Repeat=1,
    Number=int(1_000_000 / n)
)
t1 = perftester.time_benchmark(make_dict, n=n)
t2 = perftester.time_benchmark(make_dict_cached, n=n)

print(
    "Benchmark for {n}",
    f"Regular: {signif(t1['min'], 4)}",
    f"Cached: {signif(t2['min'], 4)}",
    f"Ratio: {signif(t1['min'] / t2['min'], 4)}"
)
```

你可以对不同的`n`值运行这个脚本，`n`作为两个基准函数的参数。很容易猜到实验背后的假设：`n`值越大，即两个函数创建的字典越大，缓存效果应该越明显。

我们很快就会看到是否如此。我们将对以下`n`值运行脚本：`1`、`10`、`100`、`1000`、`10_000` 和 `100_000`。运行次数需要调整，以避免测试时间过长——但你可以自己用更多的运行次数进行实验；为此，请更改以下片段：`Number=int(1_000_000 / n)`。

你可能会想知道为什么我没有创建一个循环来在脚本的一次运行中运行所有基准测试。这是因为每个实验应该是独立的；否则，我们可能会冒险使后续实验的结果受到偏见。最好是单独运行基准测试，不要在脚本的同一次运行中将它们结合起来。

脚本打印每个函数的最佳结果以及一个额外的指标：缓存函数比未缓存函数快了多少次。下面，我将只展示这个比率：

```py
 1:   4.6
    10:  26.6
   100: 222.7
  1000: 551.7
 10000: 100.0
100000:   9.2
```

如你所见，缓存的性能非常出色，但在某个点上（这里是`10_000`的`n`值）性能显著下降；对于更大的`n`，性能则急剧下降。我们能解决这个问题吗？

不幸的是，我们不能。我们可以在这个函数中做的唯一更改是选择`maxsize`，但在这里没有用，因为缓存只保留一个元素：给定`n`值的字典。

因此，我担心在处理如此大项的缓存时，我们无法提高`functools.lru_cache`的性能。如你所见，这不是缓存技术的问题，而是缓存本身的问题。

我们的性能实验教会了我们两件重要的事情：

+   使用`functools.lru_cache`进行缓存可以显著提高性能。

+   缓存非常大的对象可能会降低性能，而相比之下缓存较小的对象性能更佳。

让我们实现一个简单的自定义缓存，看看它是否会出现相同的情况。这将是一个过于简化的缓存工具，是我在实际项目中不会决定使用的。

这就是代码：

```py
import perftester
from rounder import signif

MEMORY: dict = {}

def make_dict(n):
    return {str(i): i**2 for i in range(n)}

def make_dict_cached(n):
    if n not in MEMORY:
        MEMORY[n] = {str(i): i**2 for i in range(n)}
    return MEMORY[n]

n = 1000000

perftester.config.set_defaults(
    which="time",
    Repeat=1,
    Number=int(1_000_000 / n)
)
t1 = perftester.time_benchmark(make_dict, n=n)
t2 = perftester.time_benchmark(make_dict_cached, n=n)

print(
    f"Benchmark for {n}",
    f"Regular: {signif(t1['min'], 4)}",
    f"Cached: {signif(t2['min'], 4)}",
    f"Ratio: {signif(t1['min'] / t2['min'], 4)}"
)
```

结果如下：

```py
 1:   3.4
    10:  18.3
   100: 184.2
  1000: 726.0
 10000:  82.4
100000:   6.7
```

它们与之前的结果有些不同，但趋势是一样的。这表明，`functools.lru_cache`使用的策略并不是导致性能显著下降的原因。最可能的原因是输出的大小（这里是字典）。

# 结论

缓存是一个极好的工具，可以显著提高性能。好的一点是，它学习使用的时间不长，而且使用起来也很简单。

Python 标准库通过 `functools.lru_cache` 装饰器提供缓存功能。它通常能完全满足你的需求，但有时其局限性可能使其无法使用。最显著的限制似乎是无法使用不可哈希的参数。另一个限制是缓存应仅用于纯函数；然而，正如我们上面讨论的，这并不总是如此。

我们还没有完成缓存的讨论。在这篇文章中，我们探讨了 Python 标准库工具，但在未来的文章中，我们将讨论 PyPi 缓存工具，如 [cache3](https://github.com/StKali/cache3) 和 [cachetools](https://github.com/tkem/cachetools/)。我们将比较它们与 `functools.lru_cache` 的性能，并考虑它们的优缺点。

感谢阅读。如果你喜欢这篇文章，你也可能会喜欢我写的其他文章；你可以在 [这里](https://medium.com/@nyggus) 查看。如果你想加入 Medium，请使用下面的推荐链接：

[](https://medium.com/@nyggus/membership?source=post_page-----4fca250ab5f6--------------------------------) [## 使用我的推荐链接加入 Medium - Marcin Kozak

### 作为 Medium 会员，你的一部分会费将分配给你阅读的作者，你可以完全访问每个故事……

medium.com](https://medium.com/@nyggus/membership?source=post_page-----4fca250ab5f6--------------------------------)
