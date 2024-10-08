# 4 种易于实现的高影响力调整方法，用于超级提升你的 Python 代码性能

> 原文：[`towardsdatascience.com/4-easy-to-implement-high-impact-tweaks-for-supercharging-your-python-codes-performance-eb0652d942b7`](https://towardsdatascience.com/4-easy-to-implement-high-impact-tweaks-for-supercharging-your-python-codes-performance-eb0652d942b7)

## 如何检测、理解和消除 Python 中的瓶颈，以实现 1500 倍的速度提升

[](https://mikehuls.medium.com/?source=post_page-----eb0652d942b7--------------------------------)![Mike Huls](https://mikehuls.medium.com/?source=post_page-----eb0652d942b7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eb0652d942b7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb0652d942b7--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----eb0652d942b7--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb0652d942b7--------------------------------) ·12 分钟阅读·2023 年 7 月 12 日

--

![](img/63a41aba468ec040a3d1aa42518c8082.png)

你在本文后的 Python 代码（图片来自 [SpaceX](https://unsplash.com/@spacex) 在 [Unsplash](https://unsplash.com/photos/-p-KCm6xB9I)）

我的理念是尝试简单的解决方案，然后再考虑复杂的方案。通过探索本文中的简单方法，你可能会找到所需的性能提升，从而避免实现和调试多进程、线程或其他语言编写的包所需的复杂性和无数小时。

在这篇文章中，我们将**深入探讨**使用**最小化**、**易于实现的技巧**来加速任何 Python 代码的工具和 4 种方法。我们将分析代码，检测瓶颈并以结构化的方式解决这些问题。我们将通过**减少 Python 的工作量**来完成这一过程。

> 如果你必须尽可能快地跨越一定距离，你可以选择加速行驶或缩短路径。同样，与其让 Python 执行更多的操作，不如减少操作的数量。

最终，你将获得对代码性能的更深入理解，掌握代码分析的宝贵技能，以避免在开发过程中遇到瓶颈，并成为一个更好的开发者。让我们开始编码吧！

# 目录

我们将从三个方面分析我们的问题：

在**A 部分**我们定义性能的含义并讨论分析器，接下来我们将使用它来测量我们的代码。

**B 部分**围绕使用工具**定位瓶颈**展开。我们**测量**我们的代码并发现性能瓶颈。我们使用实际例子来理解为何我们的函数性能不佳。

在**C 部分**中，我们讨论了消除瓶颈的方法。在前面的部分中，我们学会了如何检测问题代码并分析它，以便了解减速的原因。在这一部分，我们**讨论性能提升策略**：

+   选择合适的数据结构

+   消除运行缓慢的代码（例如嵌套循环）

+   使用内置函数

最终你将能够将这些通用策略应用于任何代码。

# A. 关于性能

让我们首先处理一些定义和准备工作。

## 1\. 什么是性能？

由于我们正在尝试优化代码的性能，我们首先需要对性能的定义有一个清晰的认识：

> [性能：以所需时间、使用的资源等来估计完成的有用工作量](https://en.wiktionary.org/wiki/performance)

为了简洁起见，我选择了监控函数的执行速度。内存使用和大小也很重要，但我选择省略它以使文章稍短。

[](/using-multi-stage-builds-to-make-your-docker-image-almost-10x-smaller-239068cb6fb0?source=post_page-----eb0652d942b7--------------------------------) ## 使用多阶段构建使你的 Docker 镜像缩小 10 倍

### 通过留下不必要的工具来清理你的 Docker 镜像

[towardsdatascience.com

## 2\. 如何测量性能？

Python 有一个非常方便的内置工具叫做 `cProfile`。这个包提供了*确定性分析*Python 程序的功能。这意味着对*所有*函数调用、函数返回和异常事件之间的时间间隔进行了精确计时。这与*统计分析*不同，后者使用随机样本来推断时间花费的相对情况。

使用 `cProfile` 非常简单：

```py
import cProfile
from typing import List

def uppercase_list(a_list:List) -> List:
  return [str(item).upper() for item in a_list]

cProfile.run(statement="uppercase_list(['hello', 'world', '!'])")
```

这会产生以下结果：

```py
8 function calls in 0.000 seconds
Ordered by: standard name

ncalls  tottime  percall  cumtime  percall  filename:lineno(function)
    1    0.000    0.000    0.000    0.000   <string>:1(<module>)
    1    0.000    0.000    0.000    0.000   cprofile_example.py:6(uppercase_list)
    1    0.000    0.000    0.000    0.000   cprofile_example.py:8(<listcomp>)
    1    0.000    0.000    0.000    0.000   {built-in method builtins.exec}
    1    0.000    0.000    0.000    0.000   {method 'disable' of '_lsprof.Profiler' objects}
    3    0.000    0.000    0.000    0.000   {method 'upper' of 'str' objects}
```

对列的快速说明：

+   **ncalls**: 调用次数

+   **tottime**: 在给定函数中花费的总时间（不包括子函数）

+   **percall**: tottime / ncalls

+   **cumtime**: 在该函数及所有子函数中花费的累计时间

+   **percall**: cumtime / primitive calls

+   **最后一列**: 位置 -> 文件名中的函数名和行号

如你所见，`cProfile` 为我们提供了一个易于使用的工具来检查我们代码的性能。现在让我们专注于如何实际提升我们的 Python 代码。

[](/create-a-python-package-with-super-fast-rust-code-in-3-steps-a27389629beb?source=post_page-----eb0652d942b7--------------------------------) ## 使用超级快的 Rust 代码在 3 个步骤中创建 Python 包

### 使用包含 Rust 代码的包来扩展你的 Python 代码，以实现 >150 倍的性能提升！

[towardsdatascience.com

# B. 性能分析：发现和理解瓶颈

让我们从需要优化的代码开始。作为示例，我们将使用一个非常简单的函数：它接受两个列表，并返回*两个列表中存在多少个不同的项（不区分大小写）*。

让我们看一下这个（故意设计得超不优化的）函数：

```py
def duplicate_count_dumb(list_1:List, list_2:List) -> int:
  """ Returns the distinct number of items that are present in both provided lists (case-insensitive) """
  duplicates: List[str] = []
  for item1 in list_1:
    for item2 in list_2:
      if str(item1).upper() == str(item2).upper():
        if (str(item1).upper() not in duplicates):
          duplicates.append(str(item1).upper())
  return len(duplicates)
```

这个函数显然不是解决我们问题的智能方法，但它确实很好地演示了我们如何使用`cProfile`来发现瓶颈。

[](/python-args-kwargs-and-all-other-ways-to-pass-arguments-to-your-function-bd2acdce72b5?source=post_page-----eb0652d942b7--------------------------------) ## Python 参数、关键字参数及所有其他传递参数的方式

### 熟练设计你的函数参数，提供 6 个示例

[towardsdatascience.com

## 1\. 性能分析我们的函数

当我们在我们的函数上使用`cProfile`时，这里是当我们传递两个都包含 1000 个五字单词的列表时的结果：

```py
2000008 function calls in 0.523 seconds
Ordered by: standard name

 ncalls  tottime  percall  cumtime  percall filename:lineno(function)
      1    0.000    0.000    0.520    0.520 <string>:1(<module>)
      1    0.381    0.381    0.520    0.520 list_matching.py:43(duplicate_count_dumb)
      1    0.002    0.002    0.523    0.523 {built-in method builtins.exec}
      1    0.000    0.000    0.000    0.000 {built-in method builtins.len}
      1    0.000    0.000    0.000    0.000 {method 'append' of 'list' objects}
      1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
2000002    0.139    0.000    0.139    0.000 {method 'upper' of 'str' objects}
```

注意我们在半秒多一点的时间内执行了略多于 200 万次的函数调用。让我们先将这段时间添加到我们的基准中，以便后续进行比较：

```py
┌─Benchmark─────────┬───────────────┬───────────────┬───────────────┐
│ dupcount 1k items │      Min (ms) │      Max (ms) │      Avg (ms) │
├───────────────────┼───────────────┼───────────────┼───────────────┤
│ default           │ 520.405100000 │ 529.250500000 │ 523.529233333 │
└───────────────────┴───────────────┴───────────────┴───────────────┘
```

## 2\. 发现瓶颈

最重要的一行是底部那一行；在这里我们看到我们调用了`upper`方法大约**200 万次**。这是因为我们将 list1 中的每个项与 list2 中的每个项进行比较。

两个项都需要*字符串化*和*转大写*以便进行比较。由于两个列表各有 1000 项，这意味着我们必须进行 100 万次比较（1000x1000）。问题在于我们`upper`函数对 item1 和 item2 进行了 100 万次。

这意味着在最佳情况下，至少`upper`会被**调用 200 万次**。如果项匹配，还会有额外的项检查它们在`duplicates`列表中的存在性并添加它们（这就是为什么会有 2,000,002 次而不是 2,000,000 次）。

## 3\. 为什么这么慢？

Python 因其易用性而受到喜爱，但这也带来了缺点。在底层，Python 的内存分配比其他编程语言如 C（Python 是用 C 编写的）要慢得多。

[](/why-is-python-so-slow-and-how-to-speed-it-up-485b5a84154e?source=post_page-----eb0652d942b7--------------------------------) ## 为什么 Python 这么慢以及如何加快速度

### 查看 Python 的瓶颈在哪里

[towardsdatascience.com

由于 [**Python 中的字符串是不可变的**](https://mikehuls.medium.com/python-quirks-understand-how-a-variable-can-be-modified-by-a-function-that-doesnt-return-anything-343a40cc6923)，在底层每次你`uppercase` 时都会创建**一个新变量**。所有这些变量都需要**分配内存**，而 Python 的内存分配**相对较慢**。当你这样做 200 万次时，你会开始注意到这种性能下降。了解更多关于 Python 设计的信息，可以阅读上面的文章或观看下面的演示：

# C. 消除瓶颈

既然我们已经找到了瓶颈，现在是时候加速我们的代码了。记住我们的**目标**是**让 Python 做更少的工作**。

[](/python-quirks-understand-how-a-variable-can-be-modified-by-a-function-that-doesnt-return-anything-343a40cc6923?source=post_page-----eb0652d942b7--------------------------------) ## Python 奇特之处：理解一个函数如何修改一个不返回任何东西的变量

### 深入探讨 Python 如何传递参数和可变性，以防止意外错误。

towardsdatascience.com

## 1\. 让 Python 做更少的工作 — 理解你的代码

一个简单的减少操作的方法是**在循环之前将每个项目大写**，因为我们不在乎大小写：

```py
def duplicate_count_smarter(list_1:List, list_2:List) -> int:
  """ Returns the distinct number of items that are present in both provided lists (case-insensitive) """
  duplicates: {str} = set()
  list_1 = [str(w).upper() for w in list_1]
  list_2 = [str(w).upper() for w in list_2]
  for item1 in list_1:
    for item2 in list_2:
      if item1 == item2:
        duplicates.add(item1)
  return len(duplicates)
```

正如你所见，我们首先对每个列表中的每个项目进行大写处理。当我们再次对代码进行性能分析时，我们发现这个简单的更改将函数调用的次数**从大约 200 万减少到大约 2000**。这是因为现在我们需要对 list1 **和** list2 中的每个项目进行大写处理（1k + 1k = 2k）。

```py
2007 function calls in 0.022 seconds

Ordered by: standard name

ncalls  tottime  percall  cumtime  percall filename:lineno(function)
    1    0.000    0.000    0.019    0.019 <string>:1(<module>)
    1    0.019    0.019    0.019    0.019 list_matching.py:74(duplicate_count_smarter)
    1    0.000    0.000    0.000    0.000 list_matching.py:77(<listcomp>)
    1    0.000    0.000    0.000    0.000 list_matching.py:78(<listcomp>)
    1    0.003    0.003    0.022    0.022 {built-in method builtins.exec}
    1    0.000    0.000    0.000    0.000 {built-in method builtins.len}
    1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
 2000    0.000    0.000    0.000    0.000 {method 'upper' of 'str' objects}
```

我们调用 `upper` 和 `str` 函数的次数减少了 1,000 倍，从而显著提升了性能；**执行时间减少了 28 倍**，从 523ms 降至 18ms：

```py
┌─Benchmark─────────┬───────────────┬───────────────┬───────────────┐
│ dupcount 1k items │      Min (ms) │      Max (ms) │      Avg (ms) │
├───────────────────┼───────────────┼───────────────┼───────────────┤
│ default           │ 520.405100000 │ 529.250500000 │ 523.529233333 │
│ first_upper       │  18.440300000 │  18.665600000 │  18.548466667 │ (x28)
└───────────────────┴───────────────┴───────────────┴───────────────┘
```

另请注意，我将重复项列表替换为集合，这样我们就不需要检查项目是否已包含。使用集合将进一步减轻 Python 的负担，因为集合默认只包含唯一值。

[](/args-vs-kwargs-which-is-the-fastest-way-to-call-a-function-in-python-afb2e817120?source=post_page-----eb0652d942b7--------------------------------) ## 参数与关键字参数：在 Python 中调用函数的最快方式是什么？

### `timeit` 模块的清晰演示

towardsdatascience.com

## 2\. 避免嵌套循环

另一个容易发现的瓶颈是我们对列表 1 中的每一项都遍历列表 2。如果两个列表都包含 10 项，这就需要进行 (10 x 10=) 100 次比较。如果两个列表都包含 1000 项，则需要进行 100 万次比较。你可以看到，比较次数相对于输入数量呈指数级增长，这会迅速加剧我们的性能问题。*了解更多关于* [***大 O 符号***](https://en.wikipedia.org/wiki/Big_O_notation)*的信息。*

我们可以通过实现下面的代码来避免进行两次循环和比较项：

```py
def duplicate_count_with_in(list_1:List, list_2:List) -> int:
  """ Returns the number of times a word exists in a list (case-insensitive) xxxx"""
  duplicates: {str} = set()
  list_1 = [str(w).upper() for w in list_1]
  list_2 = [str(w).upper() for w in list_2]
  for item1 in list_1:
    if item1 in list_2:
      duplicates.add(item1)
  return len(duplicates)
```

在这个示例中，我们只遍历 `list_1` 并检查 `list_2` 是否包含该值。让我们查看基准测试：

```py
┌─Benchmark─────────┬───────────────┬───────────────┬───────────────┐
│ dupcount 1k items │      Min (ms) │      Max (ms) │      Avg (ms) │
├───────────────────┼───────────────┼───────────────┼───────────────┤
│ default           │ 520.405100000 │ 529.250500000 │ 523.529233333 │
│ first_upper       │  18.440300000 │  18.665600000 │  18.548466667 │ (x28)
│ with_in           │  11.774500000 │  12.401600000 │  12.183367000 │ (x43)     │
└───────────────────┴───────────────┴───────────────┴───────────────┘
```

通过两个简单的改动，我们显著提高了性能，但还有一种方法可以进一步提升性能。

## 3\. 使用合适的数据结构——去重列表

为了节省更多工作，我们可以对两个列表进行去重，因为这可以减少需要执行的比较次数。我们将暂时绕道思考一个去重列表的函数。然后，我们将把它集成到我们的列表比较函数中。你可能想到的第一个函数可能如下所示：

```py
def dedupper(the_list: List) -> List:
  """ Removes duplicates from list (case-insensitive) """
  return_list = []
  for entry in the_list:
    _entry = str(entry).upper()
    if (_entry not in return_list):
      return_list.append(_entry)
  return return_list
```

没有什么特别的；它只会遍历列表中的每一项，并将其放入另一个列表中，如果该项还不在其中。这个函数的大部分工作是在检查一个值是否存在于列表中以进行去重。如果我们使用一个**100,000 个单词**的列表，这个函数需要**稍超过一分钟**才能完成：

```py
┌─Benchmark────────────────────┬──────────────┐
│ duplicate count (100,000x)   │     Avg (ms) │
├──────────────────────────────┼──────────────┤
│ dedup_dumb                   │ 61386.198600 │
└──────────────────────────────┴──────────────┘
```

## 使用 Python 多线程的 2 行代码

### 何时以及如何使用多个核心以更快的速度执行

towardsdatascience.com

让我们**优化**这个函数以使用**集合**：数据集只能包含唯一值。这样我们就不需要让 Python 检查一个值是否已经存在于集合中；现在由集合来处理这个问题。重写后，我们得到了一个更易读且性能更好的函数：

```py
def dedupper_smart(the_list: List) -> List:
    """ Removes duplicates from list (case-insensitive) """
    return list({str(entry).upper() for entry in the_list})
```

如你所见，我们使用集合推导式遍历列表中的每一项。在完成后，我们得到一个仅包含列表中唯一大写值的集合。在返回之前，我们将其转换为列表。当我们将这个新函数与旧函数进行比较时，我们发现它**快了 2,800 多倍！**

```py
┌─Benchmark────────────────────┬──────────────┐
│ duplicate count (100,000x)   │     Avg (ms) │
├──────────────────────────────┼──────────────┤
│ dedup_dumb                   │ 61386.19860  │
│ dedup_smart                  │    21.85930  │(x2,808)
└──────────────────────────────┴──────────────┘
```

集合之所以速度更快，是因为它们是用 C 语言实现的（Python 自身也是用 C 语言编写的）。由于内存也由 C 管理，这样更高效。此外，集合是无序的，并且只能包含唯一值，从而节省了大量的检查和排序操作。这再次将**更多工作从 Python 手中移开**，**提高了性能**。

让我们在列表匹配函数中实现集合并进行基准测试：

```py
def duplicate_count_dedup_lists(list_1:List, list_2:List) -> int:
  """ Returns the number of times a word exists in a list (case-insensitive) xxxx"""
  duplicates: {str} = set()
  set_1= {str(entry).upper() for entry in list_1}
  set_2 = {str(entry).upper() for entry in list_2}
  for item1 in list_1:
    if item1 in list_2:
      duplicates.add(item1)
  return len(duplicates)
```

如你所见，现在列表首先会被去重。当我们进行基准测试时，看到 **巨大的性能提升**：

```py
┌─Benchmark─────────┬───────────────┬───────────────┬───────────────┐
│ dupcount 1k items │      Min (ms) │      Max (ms) │      Avg (ms) │
├───────────────────┼───────────────┼───────────────┼───────────────┤
│ default           │ 520.405100000 │ 529.250500000 │ 523.529233333 │
│ first_upper       │  18.440300000 │  18.665600000 │  18.548466667 │ (x28)
│ with_in           │  11.774500000 │  12.401600000 │  12.183367000 │ (x43)     │
│ dedup_lists_set   │   0.351700000 │   0.593000000 │   0.496767000 │ (x1,053)
└───────────────────┴───────────────┴───────────────┴───────────────┘
```

我们的函数在 **半毫秒** 内完成，而不是半秒钟。我们可能还有一个小窍门可以在下一部分使用。

[](/5-real-handy-python-decorators-for-analyzing-debugging-your-code-c22067318d47?source=post_page-----eb0652d942b7--------------------------------) ## 5 个实际有用的 Python 装饰器用于分析/调试代码

### 将这些实用的通用装饰器直接应用到你的代码中

[towardsdatascience.com

## 4. 使用数据结构函数 — 集合交集

我们可以添加的最后一个优化是跳过循环，使用集合的 `intersection` 方法：

```py
def duplicate_count_smartest(list_1:List, list_2:List) -> int:
  """ Returns the number of times a word exists in a list (case-insensitive) """
  set_1 = {str(entry).upper() for entry in list_1}
  set_2 = {str(entry).upper() for entry in list_2}
  return len(set_1.intersection(set_2))
```

如你所见，我们最终得到一个非常易读、简洁的函数。`intersection` 方法返回一个集合，该集合包含在 `set_1` 和 `set_2` 中都存在的项。最后，我们返回这个集合的长度。让我们将其添加到基准测试中，看看最终结果如何：

```py
┌─Benchmark─────────┬───────────────┬───────────────┬───────────────┐
│ dupcount 1k items │      Min (ms) │      Max (ms) │      Avg (ms) │
├───────────────────┼───────────────┼───────────────┼───────────────┤
│ default           │ 520.405100000 │ 529.250500000 │ 523.529233333 │
│ first_upper       │  18.440300000 │  18.665600000 │  18.548466667 │ (x28)
│ with_in           │  11.774500000 │  12.401600000 │  12.183367000 │ (x43)     │
│ dedup_lists_set   │   0.351700000 │   0.593000000 │   0.496767000 │ (x1,053)
│ sets              │   0.340600000 │   0.369600000 │   0.351167000 │ (x1,490)
└───────────────────┴───────────────┴───────────────┴───────────────┘
```

我们的最终函数不仅可读性大大提高，而且比起最初的版本快了几乎 1500 倍。

[](/cython-for-data-science-6-steps-to-make-this-pandas-dataframe-operation-over-100x-faster-1dadd905a00b?source=post_page-----eb0652d942b7--------------------------------) ## 6 步骤使 Pandas Dataframe 操作速度提升 100 倍

### Cython 数据科学：将 Pandas 与 Cython 结合，实现惊人的速度提升

[towardsdatascience.com

## 进一步优化？

也许你已经经历了这些步骤，但代码性能仍然不够理想。那么，你可以选择应用[**多进程**](https://mikehuls.medium.com/applying-python-multiprocessing-in-2-lines-of-code-3ced521bac8f)、[**线程**](https://mikehuls.medium.com/thread-your-python-program-with-two-lines-of-code-3b474407dbb8)或甚至用像[**C**](https://mikehuls.medium.com/write-your-own-c-extension-to-speed-up-python-x100-626bb9d166e7)、[**Rust**](https://mikehuls.medium.com/create-a-python-package-with-super-fast-rust-code-in-3-steps-a27389629beb)或[**Cython**](https://mikehuls.medium.com/cython-for-absolute-beginners-30x-faster-code-in-two-simple-steps-bbb6c10d06ad)这样的更优化语言重新编写（部分）函数。本文的目的是不立即使用大招，而是首先挤出 Python 能提供的每一滴性能。如果需要进一步优化，本文中的方法将使其更快。

[链接到文章](https://towardsdatascience.com/advanced-multi-tasking-in-python-applying-and-benchmarking-threadpools-and-processpools-90452e0f7d40?source=post_page-----eb0652d942b7--------------------------------) [## Python 中的高级多任务处理：在 6 种情况下应用和基准测试线程池和进程池

### 安全且轻松地将多任务处理应用到你的代码中

[链接到 Data Science](https://towardsdatascience.com/advanced-multi-tasking-in-python-applying-and-benchmarking-threadpools-and-processpools-90452e0f7d40?source=post_page-----eb0652d942b7--------------------------------)

# 结论

在这篇文章中，我们通过了一个非常实用的例子来**识别**、**理解**和**消除性能瓶颈**。我们已经看到，你不必立即对代码进行复杂的结构性更改；有时简单的小的、易于实施的更改可以大大优化你的代码。当这些还不够时，总是可以选择使用线程、多进程或在编译语言中编写 Python 包。

我认为最重要的部分是真正理解你的代码，理解 Python 如何工作以及在哪里性能较差，并应用正确的数据结构。为此，请查看我[**其他文章**](https://mikehuls.com/articles?tags=fast)或这个[**演示文稿**](https://www.youtube.com/watch?v=N7cgUnW-tZQ)。

我希望这篇文章像我希望的那样清晰，但如果不是这样，请告诉我我可以做些什么以进一步澄清。同时，请查看我关于各种编程相关主题的[其他文章](https://mikehuls.com/articles)：

+   [针对绝对初学者的 Git：通过视频游戏理解 Git](https://mikehuls.medium.com/git-for-absolute-beginners-understanding-git-with-the-help-of-a-video-game-88826054459a)

+   [创建和发布自己的 Python 包](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc)

+   [用 FastAPI 在 5 行代码中创建一个快速的自动文档、可维护且易于使用的 Python API](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e)

快乐编程！

— Mike

*附注：喜欢我做的事情吗？* [*关注我！*](https://mikehuls.medium.com/membership)

[链接到 Medium 会员页面](https://mikehuls.medium.com/membership?source=post_page-----eb0652d942b7--------------------------------) [## 使用我的推荐链接加入 Medium — Mike Huls

### 阅读 Mike Huls（以及 Medium 上的其他成千上万名作者）的每一个故事。你的会员费直接支持 Mike…

[链接到 Medium 会员页面](https://mikehuls.medium.com/membership?source=post_page-----eb0652d942b7--------------------------------)
