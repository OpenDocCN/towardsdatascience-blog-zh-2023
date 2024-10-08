# 如何提升 Python 函数的性能

> 原文：[`towardsdatascience.com/improve-python-function-performance-587cfd4fd511`](https://towardsdatascience.com/improve-python-function-performance-587cfd4fd511)

## 加速 Python 中频繁调用的函数

[](https://gmyrianthous.medium.com/?source=post_page-----587cfd4fd511--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----587cfd4fd511--------------------------------)[](https://towardsdatascience.com/?source=post_page-----587cfd4fd511--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----587cfd4fd511--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----587cfd4fd511--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----587cfd4fd511--------------------------------) ·阅读时间 9 分钟·2023 年 3 月 10 日

--

![](img/d4ef78a78679f6b7ede3800157e16a73.png)

图片由 [Esteban Lopez](https://unsplash.com/@exxteban?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/6yjAC0-OwkA?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在当今数据处理量以空前速度增长的世界中，拥有高效和优化的代码变得比以往任何时候都重要。作为一种流行的编程语言，Python 提供了多种内置工具来优化代码性能。其中一个工具是 `lru_cache` 装饰器，它可以用来缓存函数的结果，从而提高性能。

在这篇文章中，我们将探讨如何使用 `lru_cache` 装饰器，并且了解它如何帮助你编写更快、更高效的代码来处理频繁调用的函数。

## 缓存简述

缓存是一种广泛采用的方法，用于提升计算机程序的性能。它涉及在指定的时间范围内临时存储计算成本高或经常访问的信息。通过缓存，开发者可以高效地存储先前计算的结果，从而减少重新计算所需的时间和计算资源。这个过程可以显著改善系统的响应时间和整体性能。

缓存可以通过各种数据结构实现，例如数组、哈希表和链表，也可以通过不同的策略进行管理，例如最近最少使用（LRU）和先进先出（FIFO）。

管理缓存大小可以是其实现中的一个关键方面。没有定期缩小缓存的机制，可能会导致内存使用不断增加，最终导致应用程序崩溃。为了克服这个挑战，程序员必须仔细考虑最适合其特定需求的策略或方法。

在某些情况下，适应性策略可能更为合适，这些策略会根据变化的工作负载模式进行调整。此外，缓存大小管理可以通过不同的技术来实现，例如设置最大大小、实施基于时间的过期策略，或使用结合多种策略的混合方法。

最终，选择合适的缓存管理策略需要仔细考虑诸如性能、内存使用和应用程序的具体需求等因素。总体而言，缓存是一种有效的技术，可以让程序员优化代码并改善用户体验。

## 使用 `lru_cache` 装饰器创建最近最少使用缓存

`lru_cache` 装饰器是语言标准库的一部分，可以从 `functools` 模块中导入。

装饰器使用一种称为**最近最少使用（LRU）**的策略来优先处理最近使用过的对象。每当通过使用 LRU 策略的缓存访问一个对象时，缓存会将其放在最上面，同时将其他对象向下移动一个位置。然而，如果缓存达到其最大大小，则会移除最近最少使用的对象，以腾出空间供新对象使用。这确保了经常使用的对象保持在缓存中，而不常使用的对象则会逐渐被挤出。

> .. 经常使用的对象保持在缓存中，而不常使用的对象则会逐渐被挤出

实现 LRU 的缓存对于处理计算密集型或 I/O 绑定的函数非常有用，这些函数经常以相同的参数被调用。

Python 中的 `lru_cache` 装饰器通过维护一个**线程安全的字典形式缓存**来运作。在调用被装饰的函数时，缓存中会创建一个新条目，其中函数调用参数对应字典的键，返回值对应字典的值。后续使用相同参数调用函数时不会计算新结果，而是直接检索缓存值。这一特性避免了冗余计算，从而提高了整体性能。

> 由于使用字典来缓存结果，函数的定位和关键字参数必须是[可哈希的](https://docs.python.org/3/glossary.html#term-hashable)。
> 
> - Python 文档

**`lru_cache`的大小** **可以通过`maxsize`参数进行调整**，默认为`128`。该值指定缓存可以在任何时间保持的最大条目数。一旦缓存达到最大大小并且有新的调用，装饰器将丢弃最少使用的条目，以腾出空间给最新的条目。这确保缓存不会超过指定的限制，从而防止过度的内存消耗并提高性能。如果`maxsize`设置为`None`，则`LRU`功能将被禁用，这意味着缓存可以无限增长。

重要的是要注意，当两个函数调用具有相同的关键字参数但顺序不同时，将创建两个独立的缓存条目。例如，调用`func(x=10, y=5)`和`func(y=5, x=10)`将导致缓存中出现两个不同的条目，即使这两个调用返回相同的值。

此外，`lru_cache`装饰器接受另一个名为`**typed**`的参数，默认为`False`。

> 如果`*typed*`设置为`true`，不同类型的函数参数将被单独缓存。如果`*typed*`为`false`，实现通常会将它们视为等效调用，并只缓存一个结果。
> 
> — [Python 文档](https://docs.python.org/3/library/functools.html#functools.lru_cache)

例如，当`typed=True`时，调用`func(a=4)`（`a`持有整数值）和`func(a=4.)`（`a`现在持有浮点值），将创建两个不同的缓存条目。不过请注意，一些类型如`str`和`int`即使在`typed`设置为`False`时，也可能会在不同的缓存条目中缓存。

现在假设我们有一个非常简单的 Python 函数，接受两个参数并返回它们的和。

```py
def add_numbers(a, b):
    return a + b
```

现在假设函数被`lru_cache`装饰，并被多次调用，如下所述：

```py
from functools import lru_cache

@lru_cache(maxsize=3)
def add_numbers(a, b):
    return a + b

add_numbers(a=10, b=15)
add_numbers(a=10, b=10)
add_numbers(a=3, b=15)
add_numbers(a=20, b=21)
add_numbers(a=3, b=15) 
```

下图展示了具有`maxsize=3`的 LRU 缓存如何随时间维持，以及在缓存命中或未命中时的行为，以及当当前大小达到指定最大值时如何处理的情况。

![](img/0fd75bd5c48f6a5a0689016d786bb922.png)

LRU 缓存将优先处理最近使用的函数调用 — 来源：作者

1.  最初，当函数被调用时，使用参数`a=10`和`b=15`，缓存为空，发生缓存未命中。函数执行后，输入参数和结果值都会存储在缓存中。

1.  在下次调用时，当函数被调用时，使用参数`a=10`和`b=10`，再次发生缓存未命中，因为这个参数组合在缓存中未找到。函数执行后，创建一个新的缓存条目并放在顶部。

1.  随后，当函数被调用时，使用参数`a=3`和`b=15`，发生了另一个缓存未命中。函数执行后，输入参数和结果值被存储在缓存中。

1.  在下一个调用中，当函数使用`a=20`和`b=1`作为参数时，会发生缓存未命中，并且缓存已达到最大容量。这意味着最久未使用的条目被移除，其余条目向下移动一个位置。执行函数后，最新的条目被添加到缓存中，并放在顶部。

1.  最后，当函数再次以`a=3`和`b=15`作为参数被调用时，会发生缓存命中，因为该条目现在已存储在缓存中。返回的值从缓存中获取，而不是执行函数。然后缓存条目被移到顶部。

## 如何以及何时使用 lru_cache 装饰器

在充分理解了缓存和 LRU 缓存策略后，是时候深入探讨 lru_cache，学习如何充分利用它，同时评估它的影响。

对于本教程的目的，我们将研究一个更复杂的函数，这将从缓存中受益匪浅，特别是`lru_cache`装饰器。我们的`fibonacci`函数可以用来计算**斐波那契数**。

> 在数学中，**斐波那契数**，通常表示为*Fn*，形成了一个序列，即**斐波那契序列**，其中每个数字是前两个数字的和。该序列通常从 0 和 1 开始，尽管一些作者从 1 和 1 开始序列，或者有时（如斐波那契所做）从 1 和 2 开始。以 0 和 1 为起点，该序列的前几个值是：
> 
> `0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144.`
> 
> - [维基百科](https://en.wikipedia.org/wiki/Fibonacci_number)

值得注意的是，下面的函数递归地计算斐波那契数。尽管我们可以引入记忆化来进一步优化解决方案，但这超出了本教程的范围。此外，为了简便起见，我将保持简单，让经验较少的读者也能跟上。

实现未完全优化的事实对我们的目的很有帮助，因为它展示了`lru_cache`如何提升计算密集型函数调用的性能。

```py
def fibonacci(n):
    if n < 0:
        raise ValueError('Invalid input')

    # Base case
    if n <= 1:
        return n

    return fibonacci(n - 1) + fibonacci(n - 2)
```

为了评估我们函数处理多个不同输入所需的时间，我们可以使用`timeit`模块。我们将利用 timeit 模块来测量`fibonacci`函数的性能，并从 5 次重复调用相同参数的函数中取最小时间。

> [在测量执行时间时] 使用`min()`而不是时间的平均值。这是我、Tim Peters 和 Guido van Rossum 的建议。最快的时间代表了在缓存加载且系统不忙于其他任务时算法的最佳性能。所有时间都有噪音——最快的时间噪音最小。很容易证明，最快的时间是最可重复的，因此在计时两个不同实现时最有用。
> 
> — 雷蒙德·赫廷格 [在 StackOverflow](https://stackoverflow.com/questions/8220801/how-to-use-timeit-module#comment12782516_8220943)

```py
# module test.py

from timeit import repeat

setup = """
def fibonacci(n):
    if n < 0:
        raise ValueError('Invalid input')

    # Base case
    if n <= 1:
        return n

    return fibonacci(n - 1) + fibonacci(n - 2)
"""

min_timing = min(repeat(setup=setup, stmt='fibonacci(30)', number=5))
print(f'Min Timing: {round(min_timing, 2)}s')
```

从本质上讲，我们的脚本将测量执行第 30 个斐波那契数的时间。经过 5 次不同的调用，我的机器上最少的时间是 1.39 秒。

```py
$ python3 test.py 
Min Timing: 1.39s
```

`fibonacci` 函数是确定性的，这意味着它在给定相同输入的情况下总是产生相同的输出。因此，我们可以利用缓存的概念。通过添加 `@lru_cache` 装饰器，函数对于给定输入的输出现在被缓存，如果函数再次以相同的输入调用，它将返回缓存的结果而不是重新计算。这可以显著加快函数的执行时间，特别是当函数多次以相同输入值调用时。

```py
from functools import lru_cache

@lru_cache
def fibonacci(n):
    if n < 0:
        raise ValueError('Invalid input')

    # Base case
    if n <= 1:
        return n

    return fibonacci(n - 1) + fibonacci(n - 2)
```

现在让我们对新版本的 `fibonacci` 函数进行计时：

```py
# module test.py

from timeit import repeat

setup = """
from functools import lru_cache

@lru_cache
def fibonacci(n):
    if n < 0:
        raise ValueError('Invalid input')

    # Base case
    if n <= 1:
        return n

    return fibonacci(n - 1) + fibonacci(n - 2)
"""

min_timing = min(repeat(setup=setup, stmt='fibonacci(30)', number=5))
print(f'Min Timing: {min_timing}s')
```

在我的机器上，最小的时间接近零。`fibonacci(30)` 仅在第一次迭代时执行。对于后续的迭代，检索了此函数调用的缓存结果，这是一项廉价的操作。

```py
$ python3 test.py 
Min Timing: 7.790978997945786e-06s
```

## 最终想法

总之，在今天的数据驱动世界中，优化代码性能比以往任何时候都更重要。在 Python 中实现这一点的一种方法是使用 `lru_cache` 装饰器，它可以缓存频繁调用函数的结果并提高其性能。

一般来说，缓存涉及暂时存储计算成本高或频繁访问的信息，从而减少重新计算所需的时间和计算资源。该装饰器使用一种称为最近最少使用（LRU）的策略来优先考虑最近使用的对象，并在缓存达到最大大小时丢弃最少使用的对象。

`lru_cache` 装饰器是优化 Python 代码性能的有效工具，通过缓存计算密集型或 I/O 密集型函数的结果。

[**成为会员**](https://gmyrianthous.medium.com/membership) **并阅读 Medium 上的每一篇故事。你的会员费直接支持我和你阅读的其他作者。你还将获得对 Medium 上每个故事的完全访问权限。**

[](https://gmyrianthous.medium.com/membership?source=post_page-----587cfd4fd511--------------------------------) [## 使用我的推荐链接加入 Medium — Giorgos Myrianthous

### 作为 Medium 会员，你的会员费的一部分将转给你阅读的作者，你将获得对每个故事的完全访问权限……

[gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership?source=post_page-----587cfd4fd511--------------------------------)

**你可能还会喜欢的相关文章**

[](/python-iterables-vs-iterators-688907fd755f?source=post_page-----587cfd4fd511--------------------------------) [## Python 中的可迭代对象 vs 迭代器

### 了解 Python 中可迭代对象和迭代器之间的区别

[Python 中的迭代器与可迭代对象](https://towardsdatascience.com/python-iterables-vs-iterators-688907fd755f?source=post_page-----587cfd4fd511--------------------------------) [## requirements.txt 与 setup.py 在 Python 中的区别

### 了解在 Python 开发和分发过程中，`requirements.txt`、`setup.py` 和 `setup.cfg` 的用途

[requirements.txt 与 setup.py 在 Python 中的区别](https://towardsdatascience.com/requirements-vs-setuptools-python-ae3ee66e28af?source=post_page-----587cfd4fd511--------------------------------) [## 如何在 Python 中创建用户自定义可迭代对象

### 展示如何在 Python 中创建用户自定义迭代器，并使用户自定义类成为可迭代对象

[如何在 Python 中创建用户自定义可迭代对象](https://towardsdatascience.com/make-class-iterable-python-4d9ec5db9b7a?source=post_page-----587cfd4fd511--------------------------------)
