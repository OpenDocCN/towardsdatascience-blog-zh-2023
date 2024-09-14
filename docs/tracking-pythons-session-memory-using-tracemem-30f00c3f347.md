# 使用 Tracemem 跟踪 Python 会话内存

> 原文：[`towardsdatascience.com/tracking-pythons-session-memory-using-tracemem-30f00c3f347`](https://towardsdatascience.com/tracking-pythons-session-memory-using-tracemem-30f00c3f347)

## PYTHON 编程

## Tracemem 是一个轻量级的库，帮助你跟踪 Python 会话的全部内存。

[](https://medium.com/@nyggus?source=post_page-----30f00c3f347--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----30f00c3f347--------------------------------)[](https://towardsdatascience.com/?source=post_page-----30f00c3f347--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----30f00c3f347--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----30f00c3f347--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----30f00c3f347--------------------------------) ·9 分钟阅读·2023 年 12 月 11 日

--

![](img/6648173ce353664cef3a6ada910ce9e0.png)

Tracemem 是一个用于 Python 的会话内存跟踪器。照片由 [Ronan Furuta](https://unsplash.com/@ronan18?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

`Tracemem` 是一个轻量级的 Python 性能分析工具，可以让你在特定时刻测量 Python 会话的全部内存使用情况，并跟踪随后的变化。这可以用于调试内存问题的代码，或仅仅记录内存使用情况。由于 `Tracemem` 的功能集非常有限，它是一个非常轻量的工具，对会话内存的影响很小。然而，像任何内存分析工具一样，它可能会显著影响程序的执行时间。

在底层，这个包是 `pympler.asizeof.asizeof()` 的一个封装，`[pympler](https://pypi.org/project/Pympler/)` 函数可以测量 Python 会话的内存使用情况。这意味着 `tracemem` 提供了一个简单的 API 来跟踪和评估会话内存。

这种简洁性是有代价的。你不能使用这个工具来测量特定函数、对象或代码片段的内存使用情况。如果你的需求超出了仅仅评估会话内存，你可以使用其他工具，例如：

+   `[pympler](https://pypi.org/project/Pympler/)`

+   `[memory_profiler](https://pypi.org/project/memory-profiler/)`

+   `[perftester](https://pypi.org/project/perftester/)`

+   `[memray](https://pypi.org/project/memray/)`

当然，还有典型的 Python 性能分析器，你可以在这里阅读：

[](https://docs.python.org/3/library/profile.html?source=post_page-----30f00c3f347--------------------------------) [## Python 性能分析器

### 源代码：Lib/profile.py 和 Lib/pstats.py 介绍性能分析器：cProfile 和 profile 提供确定性…

[docs.python.org](https://docs.python.org/3/library/profile.html?source=post_page-----30f00c3f347--------------------------------)

虽然我通常依赖于内置的`[cProfile](https://docs.python.org/3/library/profile.html#module-cProfile)` Python 分析器，但`[line profiler](https://pypi.org/project/line-profiler/)`包提供了一个强大的逐行分析工具。

`Tracemem`的 API 在 Python 中有些不典型，但这是一个刻意的决定，以保持工具尽可能简单和轻量。这种不寻常的 API 也源于`tracemem`是一个分析工具，通常用于调试。因此，语法（或者说，导入）的不同方法不应造成任何重大问题。

本文深入探讨了`tracemem`的基础知识，并演示了如何利用它来监控 Python 会话的整体内存消耗，贯穿程序的执行。对于我们的实验，我们将使用安装在 WSL 1 上的 Python 3.12，在一台（7 年的）Windows 10 机器上，配备 16GB RAM。我们还将进行两个实验：（1）我们将在 Python 3.10、3.11 和 3.12 上复制基于`tracemem`的检查，（2）我们将评估使用此工具是否明显影响程序执行时间。

*免责声明*：我是`tracemem`包及其文档的作者。因此，你可能会注意到这篇文章与包的文档之间有些许相似之处。

# 使用

`Tracemem`是一个分析工具，所以你最有可能将它用于分析和调试。有时，你也可能将它用于生产代码，例如记录应用程序的内存使用情况。然而，最常见的用例是分析，而这个用例严重影响了包的设计。

为了使工具更友好，`tracemem`对象在 Python 会话中可用作真正的全局变量，如下文所述：

[](/are-globals-in-python-really-global-492f1e4faf9b?source=post_page-----30f00c3f347--------------------------------) ## Python 中的全局变量真的全局吗？

### 学习一个技巧，让 Python 对象真正全局化。

[towardsdatascience.com

记住你需要使用`tracemem`的顶级导入，这会导入整个模块：

```py
>>> import tracemem
```

之后，你可以在会话中的任何其他模块中访问其所有对象，而无需再次导入。

不过，请记住，你可以在没有模块名称的情况下使用它的对象，这在*顶级导入*中是不典型的，但在*from-import*中是典型的。因此，例如，使用`tracemem.MEMPOINT()`将不起作用，但`MEMPOINT()`将有效。

下面是所有`tracemem`函数的列表：

+   `MEMPOINT()`，它在你的会话中创建一个内存点

+   `MEMORY()`，它打印内存使用情况，而不创建内存点

+   `MEMPRINT()`，它打印`MEMLOGS`

+   `tracemem()`，一个装饰器函数，在调用被装饰的方法前后创建一个内存点

此外，`tracemem` API 包含以下非常重要的对象：

+   `MEMLOGS`，`MemLogsList` 类的一个实例，一个类似列表的单例容器，保存会话期间创建的所有内存点

让我们逐一分析包的功能。

## `MEMPOINT` 和 MEMLOGS

tracemem 的主要功能是 `MEMPOINT()`。它创建一个所谓的内存点，这是一个测量 Python 会话中使用的内存的点；该函数还将此内存点添加到 `MEMLOGS` 中。

> ***一个内存点***：Python 会话中使用的完整内存的测量点。

第一个内存点在 `tracemem` 被导入时创建：

```py
>>> MEMLOGS
[MemLog(ID='tracemem import', memory=...)]
>>> MEMPOINT("The second MEMPOINT")
>>> len(MEMLOGS)
2
>>> MEMPOINT("The third MEMPOINT")
>>> len(MEMLOGS)
3
>>> MEMLOGS
[MemLog(ID='tracemem import', memory=...),
 MemLog(ID='The second MEMPOINT', memory=...),
 MemLog(ID='The third MEMPOINT', memory=...)]
```

我在上面的 doctests 中使用了省略号，因为内存依赖于许多因素，因此会因机器而异。您可以在以下文章中阅读有关 doctest 的更多信息：

[## Python 文档测试与 doctest：简单方法](https://towardsdatascience.com/python-documentation-testing-with-doctest-the-easy-way-c024556313ca?source=post_page-----30f00c3f347--------------------------------)

### doctest 允许进行文档测试、单元测试、集成测试以及测试驱动开发。

[Python 文档测试与 doctest：简单方法](https://towardsdatascience.com/python-documentation-testing-with-doctest-the-easy-way-c024556313ca?source=post_page-----30f00c3f347--------------------------------)

内存点的默认 ID 是 `None`：

```py
>>> MEMPOINT()
>>> MEMLOGS[-1].ID
'None'
```

当您创建两个具有相同 ID 的内存点时，例如 `"my id"`，第二个内存点将被分配一个新 ID，`"my id-2"`，以此类推：

```py
>>> MEMPOINT("my id")
>>> MEMPOINT("my id")
>>> MEMLOGS[-1].ID
'my id-2'
```

您可以使用任何对象作为 ID，但请记住，在底层使用的是其字符串表示形式。

内存点包含整个会话使用的内存（以字节为单位）。看：

```py
>>> d = {str(i): i for i in range(10_000_000)}
>>> MEMPOINT("After adding a large dict")
>>> del d
>>> MEMPOINT("After removing this dict")
>>> MEMLOGS[-3:] # doctest: +SKIP
[MemLog(ID='my id-2', memory=1929496),
 MemLog(ID='After adding a large dict', memory=1047989344),
 MemLog(ID='After removing this dict', memory=1929848)]
```

这表明如此大的字典会增加会话内存，而使用 `del` 删除它会释放这部分内存。（注意，我跳过了上面的最后一个 `doctest`，因为我想显示内存的值，但它们可能会因会话而异。）

如上所述，`MEMLOGS` 不是一个列表，而是一个类似列表的容器，`tracemem.MemLogsList` 类的一个实例：

```py
>>> type(MEMLOGS)
<class 'tracemem.tracemem.MemLogsList'>
```

这个类的工作方式与常规列表略有不同。它只有一个实例：`MEMLOGS`。大多数典型的列表方法对它不起作用。例如，`MEMPOINT()` 是更新 `MEMLOGS` 的唯一有效方式。您不能向其添加任何内容、进行乘法操作或将其添加到另一个列表中。

`MEMLOGS` 的元素是 `MemLog` 命名元组的实例，因此您可以通过索引和使用属性名 `ID` 和 `memory` 来访问它的两个元素：

```py
>>> lastpoint= MEMLOGS[-1]
>>> type(lastpoint)
<class 'tracemem.tracemem.MemLog'>
>>> lastpoint.ID
'After removing this list'
>>> lastpoint.memory # doctest: +SKIP
1929544
```

`MEMLOGS` 还有以下属性：

+   `.memories`，返回所有报告的内存

+   `.IDs`，返回所有报告的 ID

+   `.filter()`，一个用于过滤`MEMLOGS`的方法，类似于内置的`filter()`函数

+   `.map()`，一个用于对所有 `MEMLOGS` 元素应用函数的方法，类似于内置的 `map()` 函数

这些是 `tracemem` 的重要性较低的属性，因此我在附录中简要描述它们。

## `MEMPRINT`

`MEMPRINT()` 函数打印 `MEMLOGS`，首先将内存转换为 MB。这是一个非常简单的函数，提供了 `MEMLOGS` 的人类可读元素。你应该使用它来打印，但不一定用于记录，你可以选择使用 `MEMLOGS` 中的详细信息进行记录。让我们看看 `MEMPRINT()` 在我们的会话中的工作效果：

```py
>>> MEMPRINT() # doctest: +SKIP
 0   1.59 MB     → tracemem import
 1   1.84 MB     → The second MEMPOINT
 2   1.84 MB     → The third MEMPOINT
 3   1.84 MB     → None
 4   1.84 MB     → my id
 5   1.84 MB     → my id-2
 6   999.45 MB   → After adding a large dict
 7   1.85 MB     → After removing this dict
```

`MEMPRINT()` 不返回任何东西；与 `print()` 类似，它只是将信息打印到标准输出。

## MEMTRACE 装饰器

如果你想跟踪在使用特定函数前后的内存会话，最简单的方法是使用 `MEMTRACE` 装饰器。它为每次调用被装饰的函数创建*之前*和*之后*的内存点：

```py
>>> @MEMTRACE
... def create_huge_list(n):
...     return [i for i in range(n)]
>>> li = create_huge_list(10_000_000)
>>> del li
>>> MEMPOINT("After del li")
>>> MEMLOGS[-3:]
[MemLog(ID='Before create_huge_list()', memory=...),
 MemLog(ID='After create_huge_list()', memory=...),
 MemLog(ID='After del li', memory=...)]
>>> MEMLOGS[-3:] # doctest: +SKIP
```

## MEMORY

当你只想查看会话的当前内存，而不创建 `MEMLOGS` 中的内存点时，请使用 `MEMORY()` 函数：

```py
>>> MEMORY() # doctest: +SKIP
1931664
>>> type(MEMORY())
<class 'int'>
```

这个函数检索（而不是打印）当前会话内存使用情况，以字节为整数值。正如前面提到的，它不会影响 `MEMLOGS`。这个函数用途较为小众，但你可能会在偶尔的任务中发现它有用，例如在交互式分析或调试期间。

# Python 3.10–3.12 会话重量的比较

在这里，我想比较 Python 3.10、3.11 和 3.12 三个 Python 会话的内存重量。`Tracemem` 是一个很好的工具。

我们将使用以下代码：

```py
import tracemem

@MEMTRACE
def create_huge_list(n):
    return [i for i in range(n)]

if __name__ == "__main__":
    li = create_huge_list(10_000_000)
    del li
    MEMPOINT("After del li")
    MEMPRINT()
```

我理解你可能对这一部分有所期待。我甚至可能已经暗示过这一点。然而，实际情况是，三种 Python 版本在会话重量上没有显著差异。忽略细微的差异，我获得了以下结果：

```py
 0   1.12 MB     → tracemem import
 1   1.14 MB     → Before create_huge_list()
 2   391.28 MB   → After create_huge_list()
 3   1.14 MB     → After del li
```

如你所见，新的 Python 3.10–3.12 会话在 Windows 10 机器上的 WSL 1 中使用大约`1.1`–`1.2` MB。

# `Tracemem` 影响执行时间

当`tracemem`测量小型 Python 会话的大小时，它对程序执行时间的影响很小——但仍然可见。当会话占用大量内存时，这种影响可能会很大。我将通过比较我在之前实验中使用的脚本（我们称之为`code_with_tracemem.py`）和其没有`tracemem`的版本（`code_without_tracemem.py`）的性能来演示这一点：

```py
# code_without_tracemem.py

def create_huge_list(n):
    return [i for i in range(n)]

if __name__ == "__main__":
    li = create_huge_list(10_000_000)
    del li
```

我在上面相同的机器上用 Python 3.12 运行了这两个脚本，并使用了内置的 `time` 工具。结果如下：

```py
$ time python code_with_tracemem.py
 0   1.12 MB     → tracemem import
 1   1.14 MB     → Before create_huge_list()
 2   391.28 MB   → After create_huge_list()
 3   1.14 MB     → After del li

real    0m11.490s
user    0m9.938s
sys     0m1.547s
$ time python code_without_tracemem.py

real    0m0.884s
user    0m0.266s
sys     0m0.625s
```

然后我用`n`为`1000`运行了相同的两个脚本：

```py
$ time python code_with_tracemem.py
 0   1.12 MB     → tracemem import
 1   1.14 MB     → Before create_huge_list()
 2   1.18 MB     → After create_huge_list()
 3   1.14 MB     → After del li

real    0m0.268s
user    0m0.203s
sys     0m0.063s
$ time python code_without_tracemem.py

real    0m0.027s
user    0m0.031s
sys     0m0.000s
```

如你所见，`tracemem` 即使在轻量级会话中也能显著影响执行时间。如果你打算将 `tracemem` 用于除分析外的其他目的（例如记录），这点需要考虑。如果你担心这种性能权衡，请记住这是内存分析工具的常见特性——或者至少是我遇到的所有 Python 内存分析工具。内存分析可能是耗时的，所以在使用时要记住这一点。

# 结论

`Tracemem`是一个轻量级且简单的工具，用于基本的会话内存分析。如果你只想跟踪会话的内存消耗，`tracemem`是一个合适的选择。然而，如果你需要更全面的见解，如按特定对象或代码段的内存使用情况细分，你应该探索更高级的分析工具，如`[pympler](https://pypi.org/project/Pympler/)`、`[memory_profiler](https://pypi.org/project/memory-profiler/)`、`[perftester](https://pypi.org/project/perftester/)`、`[memray](https://pypi.org/project/memray/)`、`[cProfile](https://docs.python.org/3/library/profile.html#module-cProfile)`或`[line profiler](https://pypi.org/project/line-profiler/)`。

# 附录

本附录展示了如何使用`MEMLOGS`对象的较不重要属性：`.memories`、`.IDs`、`.filter()`和`.map()`。

你可以以以下方式使用前两个：

```py
>>> MEMLOGS.memories # doctest: +SKIP
[1668032, 1933560, 1933704, 1933776,
 1933960, 1934152, 1047994000, 1934504,
 1935504, 411024672, 1935856]
>>> MEMLOGS.IDs
['tracemem import',
 'The second MEMPOINT',
 'The third MEMPOINT',
 'None',
 'my id',
 'my id-2',
 'After adding a large dict',
 'After removing this dict',
 'Before create_huge_list()',
 'After create_huge_list()',
 'After del li']
```

像内置的`filter()`函数一样，`.filter()`方法接受一个用于过滤的谓词。这个谓词需要与`MemLog`元素配合使用。与内置函数不同，`MEMLOGS.filter()`方法返回一个列表：

```py
>>> def memory_over(memlog: tracemem.MemLog) -> bool:
...     return memlog.memory > 5_000_000
>>> MEMLOGS.filter(memory_over)
[MemLog(ID='After adding a large dict', memory=...),
 MemLog(ID='After removing this dict', memory=...)]
```

你可以将`lambda`函数用作谓词：

```py
>>> MEMLOGS.filter(lambda m: m.memory > 3_750_000)
[MemLog(ID='After adding a list with 10 mln elements', memory=...)]
>>> MEMLOGS.filter(lambda m: m.memory < 1_000_000)
[]
>>> MEMLOGS.filter(lambda m: "after" in m.ID.lower() or "before" in m.ID.lower())
[MemLog(ID='After adding a large dict', memory=...),
 MemLog(ID='After removing this dict', memory=...)]
```

类似于`.filter()`方法，`.map()`方法返回一个列表：

```py
>>> as_MB = MEMLOGS.map(lambda m: round(m.memory / 1024 / 1024))
>>> as_MB
[2, 2, 2, 2, 2, 2, 999, 2, 2, 392, 2]
>>> MEMLOGS.map(lambda m: m.ID.lower())
['tracemem import',
 'the second mempoint',
 'the third mempoint',
 'none',
 'my id',
 'my id-2',
 'after adding a large dict',
 'after removing this dict',
 'before create_huge_list()',
 'after create_huge_list()',
 'after del li']
>>> memlogs = MEMLOGS.map(lambda m: (m.ID.lower(), round(m.memory / 1024 / 1024)))
>>> memlogs[:2]
[('tracemem import', ...), ('the second mempoint', ...)]
```

感谢阅读。如果你喜欢这篇文章，你可能还会喜欢我写的其他文章；你可以在[这里](https://medium.com/@nyggus)查看。并且，如果你想加入 Medium，请使用我下面的推荐链接：

[## 使用我的推荐链接加入 Medium - Marcin Kozak](https://medium.com/@nyggus/membership?source=post_page-----30f00c3f347--------------------------------)

### 作为 Medium 会员，你的一部分会员费将分配给你阅读的作者，并且你可以全面访问每一篇故事……

[medium.com](https://medium.com/@nyggus/membership?source=post_page-----30f00c3f347--------------------------------)
