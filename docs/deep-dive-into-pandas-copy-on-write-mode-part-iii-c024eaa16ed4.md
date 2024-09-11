# 深入探讨 Pandas 的 Copy-on-Write 模式 — 第三部分

> 原文：[https://towardsdatascience.com/deep-dive-into-pandas-copy-on-write-mode-part-iii-c024eaa16ed4?source=collection_archive---------10-----------------------#2023-09-29](https://towardsdatascience.com/deep-dive-into-pandas-copy-on-write-mode-part-iii-c024eaa16ed4?source=collection_archive---------10-----------------------#2023-09-29)

## *解释 Copy-on-Write 的迁移路径*

[](https://medium.com/@patrick_hoefler?source=post_page-----c024eaa16ed4--------------------------------)[![Patrick Hoefler](../Images/35ca9ef1100d8c93dbadd374f0569fe1.png)](https://medium.com/@patrick_hoefler?source=post_page-----c024eaa16ed4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c024eaa16ed4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c024eaa16ed4--------------------------------) [Patrick Hoefler](https://medium.com/@patrick_hoefler?source=post_page-----c024eaa16ed4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F103b3417e0f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-dive-into-pandas-copy-on-write-mode-part-iii-c024eaa16ed4&user=Patrick+Hoefler&userId=103b3417e0f5&source=post_page-103b3417e0f5----c024eaa16ed4---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c024eaa16ed4--------------------------------) ·4分钟阅读·2023年9月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc024eaa16ed4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-dive-into-pandas-copy-on-write-mode-part-iii-c024eaa16ed4&user=Patrick+Hoefler&userId=103b3417e0f5&source=-----c024eaa16ed4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc024eaa16ed4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-dive-into-pandas-copy-on-write-mode-part-iii-c024eaa16ed4&source=-----c024eaa16ed4---------------------bookmark_footer-----------)![](../Images/4c3dc59cff33a2049e06fcb3cf63d410.png)

图片由 [Zoe Nicolaou](https://unsplash.com/@lekneuro?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/44g_jwn3JzY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 介绍

引入写时复制（CoW）是一个重大变更，将对现有的 pandas 代码产生一定影响。我们将研究如何调整我们的代码以避免在 CoW 默认启用时出现错误。这目前计划在 2024 年 4 月发布的 pandas 3.0 版本中实现。[本系列的第一篇帖子](https://medium.com/towards-data-science/deep-dive-into-pandas-copy-on-write-mode-part-i-26982e7408c6)解释了写时复制的行为，而[第二篇帖子](https://medium.com/towards-data-science/deep-dive-into-pandas-copy-on-write-mode-part-ii-b023432a5334)则深入探讨了与写时复制相关的性能优化。

我们计划添加一个警告模式，该模式将对所有使用 CoW（写时复制）可能改变行为的操作发出警告。由于警告可能会对用户产生很大的干扰，因此必须谨慎处理。本文解释了常见情况以及如何调整代码以避免行为变化。

## 链式赋值

链式赋值是一种通过两个连续操作更新一个对象的技术。

```py
import pandas as pd

df = pd.DataFrame({"x": [1, 2, 3]})

df["x"][df["x"] > 1] = 100
```

第一个操作选择了列`"x"`，而第二个操作限制了行数。这些操作有许多不同的组合（例如，与`loc`或`iloc`结合使用）。在 CoW 下，这些组合都不会起作用。相反，它们会引发`ChainedAssignmentError`警告，以便移除这些模式，而不是默默无闻地什么也不做。

通常，你可以使用`loc`来代替：

```py
df.loc[df["x"] > 1, "x"] = 100
```

`loc`的第一个维度总是对应于`row-indexer`。这意味着你可以选择一个行的子集。第二个维度对应于`column-indexer`，这使你能够选择一个列的子集。

当你想要在行的子集上设置值时，使用`loc`通常会更快，因此这将清理你的代码并提供性能提升。

这是 CoW 将产生影响的明显情况。它也会影响链式的就地操作：

```py
df["x"].replace(1, 100)
```

模式与上述相同。列选择是第一个操作。`replace`方法尝试在临时对象上操作，这将无法更新初始对象。你也可以通过指定要操作的列来轻松移除这些模式。

```py
df = df.replace({"x": 1}, {"x": 100})
```

## 避免的模式

[我之前的帖子](https://medium.com/towards-data-science/deep-dive-into-pandas-copy-on-write-mode-part-i-26982e7408c6)解释了 CoW 机制如何工作以及 DataFrames 如何共享底层数据。如果两个对象共享相同的数据，而你在就地修改一个对象时，将会执行防御性复制。

```py
df2 = df.reset_index()
df2.iloc[0, 0] = 100
```

`reset_index` 操作将创建底层数据的视图。结果分配给一个新变量 `df2`，这意味着两个对象共享相同的数据。这在 `df` 被垃圾回收之前一直有效。因此，`setitem` 操作会触发复制。如果你不再需要初始对象 `df`，这完全没有必要。只需重新分配到相同的变量将使对象所持有的引用失效。

```py
df = df.reset_index()
df.iloc[0, 0] = 100
```

总结来说，在同一方法中创建多个引用会保持不必要的引用活跃。

链接不同方法时创建的临时引用是可以的。

```py
df = df.reset_index().drop(...)
```

这只会保持一个引用活跃。

## 访问底层 NumPy 数组

pandas 目前通过 `to_numpy` 或 `.values` 让我们访问底层的 NumPy 数组。如果你的 DataFrame 由不同的数据类型组成，例如：

```py
df = pd.DataFrame({"a": [1, 2], "b": [1.5, 2.5]})
df.to_numpy()

[[1\.  1.5]
 [2\.  2.5]]
```

DataFrame 由两个数组支持，这两个数组必须合并为一个。这会触发复制。

另一种情况是 DataFrame 仅由一个 NumPy 数组支持，例如：

```py
df = pd.DataFrame({"a": [1, 2], "b": [3, 4]})
df.to_numpy()

[[1 3]
 [2 4]]
```

我们可以直接访问数组并获取视图，而不是复制。这比复制所有数据要快得多。我们现在可以对 NumPy 数组进行操作，并可能就地修改它，这也会更新 DataFrame，并可能更新所有共享数据的其他 DataFrame。由于我们移除了许多防御性复制，这在写时复制的情况下变得更加复杂。现在，更多的 DataFrame 将相互共享内存。

`to_numpy` 和 `.values` 将因而返回只读数组。这意味着结果数组是不可写的。

```py
df = pd.DataFrame({"a": [1, 2], "b": [3, 4]})
arr = df.to_numpy()

arr[0, 0] = 1
```

这将触发一个 `ValueError`：

```py
ValueError: assignment destination is read-only
```

你可以通过两种不同的方式避免这种情况：

+   如果你想避免更新与数组共享内存的 DataFrame，请手动触发复制。

+   使数组可写。这是一种更高效的解决方案，但会绕过写时复制（Copy-on-Write）规则，因此应谨慎使用。

```py
arr.flags.writeable = True
```

在某些情况下，这不可能实现。一个常见的情况是，当你访问由 PyArrow 支持的单列时：

```py
ser = pd.Series([1, 2], dtype="int64[pyarrow]")
arr = ser.to_numpy()
arr.flags.writeable = True
```

这将返回一个 `ValueError`：

```py
ValueError: cannot set WRITEABLE flag to True of this array
```

Arrow 数组是不可变的，因此无法使 NumPy 数组可写。在这种情况下，Arrow 到 NumPy 的转换是零复制的。

## 结论

我们已经看到了最具侵入性的写时复制相关更改。这些更改将成为 pandas 3.0 的默认行为。我们还研究了如何调整代码，以避免在启用写时复制时破坏代码。如果你能避免这些模式，升级过程应该会非常顺利。
