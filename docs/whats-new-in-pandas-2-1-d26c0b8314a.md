# Pandas 2.1 中的新功能

> 原文：[https://towardsdatascience.com/whats-new-in-pandas-2-1-d26c0b8314a?source=collection_archive---------2-----------------------#2023-09-07](https://towardsdatascience.com/whats-new-in-pandas-2-1-d26c0b8314a?source=collection_archive---------2-----------------------#2023-09-07)

## *关于新版本最有趣的功能*

[](https://medium.com/@patrick_hoefler?source=post_page-----d26c0b8314a--------------------------------)[![Patrick Hoefler](../Images/35ca9ef1100d8c93dbadd374f0569fe1.png)](https://medium.com/@patrick_hoefler?source=post_page-----d26c0b8314a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d26c0b8314a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d26c0b8314a--------------------------------) [Patrick Hoefler](https://medium.com/@patrick_hoefler?source=post_page-----d26c0b8314a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F103b3417e0f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhats-new-in-pandas-2-1-d26c0b8314a&user=Patrick+Hoefler&userId=103b3417e0f5&source=post_page-103b3417e0f5----d26c0b8314a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d26c0b8314a--------------------------------) ·5分钟阅读·2023年9月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd26c0b8314a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhats-new-in-pandas-2-1-d26c0b8314a&user=Patrick+Hoefler&userId=103b3417e0f5&source=-----d26c0b8314a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd26c0b8314a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhats-new-in-pandas-2-1-d26c0b8314a&source=-----d26c0b8314a---------------------bookmark_footer-----------)![](../Images/8e89647d8a4a3d7cd6168954effc8280.png)

图片由 [Lukas W.](https://unsplash.com/@theluki?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/e3mu-MTj7Xk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

pandas 2.1 于2023年8月30日发布。让我们来看看这次发布带来了哪些新功能，以及它如何帮助我们改善 pandas 的工作负载。它包含了一系列改进，同时也引入了一些新的弃用功能。

pandas 2.1 在 pandas 2.0 提供的 PyArrow 集成基础上进行了大量构建。我们专注于扩展对预计将成为 pandas 3.0 默认功能的新特性的支持。让我们深入了解这对你意味着什么。我们将详细查看最重要的改进。

我是 pandas 核心团队的一员。我是 [Coiled](https://www.coiled.io) 的开源工程师，在那里我负责 Dask，包括改进 pandas 集成。

## 避免为字符串列使用 NumPy 对象 dtype

pandas 中一个主要的痛点是低效的字符串表示。这是我们工作了很长时间的一个话题。第一个 PyArrow 支持的字符串 dtype 在 pandas 1.3 中可用。它有潜力将内存使用减少约 70% 并提高性能。我在 [我之前的一篇文章](/utilizing-pyarrow-to-improve-pandas-and-dask-workflows-2891d3d96d2b) 中更深入地探讨了这个话题，其中包括内存比较和性能测量（总结：非常令人印象深刻）。

我们决定引入一个新的配置选项，将所有字符串列存储在 PyArrow 数组中。你不再需要担心转换字符串列，这将直接生效。

你可以通过以下方式启用此选项：

```py
pd.options.future.infer_string = True
```

这种行为将在 pandas 3.0 中成为默认，这意味着字符串列将始终由 PyArrow 支持。你需要安装 PyArrow 才能使用此选项。

PyArrow 的行为与 NumPy 对象 dtype 不同，这可能会使详细了解变得棘手。我们实现了用于此选项的字符串 dtype，以便与 NumPy 语义兼容。它的行为将与 NumPy 对象列完全相同。我鼓励大家试一试！

## 改进的 PyArrow 支持

我们在 pandas 2.0 中引入了 PyArrow 支持的 DataFrame。我们的主要目标之一是在过去几个月内改善 pandas 的集成。我们旨在使从 NumPy 支持的 DataFrame 切换尽可能简单。我们关注的一个领域是修复性能瓶颈，因为之前这会导致意外的慢速。

让我们看一个例子：

```py
import pandas as pd
import numpy as np

df = pd.DataFrame(
    {
        "foo": np.random.randint(1, 10, (1_000_000, )),
        "bar": np.random.randint(1, 100, (1_000_000,)),
    }, dtype="int64[pyarrow]"
)
grouped = df.groupby("foo")
```

我们的 DataFrame 有 100 万行和 10 个组。让我们看一下 pandas 2.0.3 与 pandas 2.1 的性能对比：

```py
# pandas 2.0.3
10.6 ms ± 72.7 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)

# pandas 2.1.0
1.91 ms ± 3.16 µs per loop (mean ± std. dev. of 7 runs, 1,000 loops each)
```

在新版本中，这个特定示例的速度快了 5 倍。`merge` 是另一个常用的函数，现在也会更快。我们希望使用 PyArrow 支持的 DataFrame 的体验会更好。

## Copy-on-Write

Copy-on-Write 最初在 pandas 1.5.0 中引入，并预计将在 pandas 3.0 中成为默认行为。Copy-on-Write 在 pandas 2.0.x 上已经提供了良好的体验。我们主要集中在修复已知的错误并提高运行速度。我建议现在在生产中使用此模式。我写了一系列博客文章来解释 [什么是 Copy-on-Write](/a-solution-for-inconsistencies-in-indexing-operations-in-pandas-b76e10719744) 和 [它是如何工作的](/deep-dive-into-pandas-copy-on-write-mode-part-i-26982e7408c6)。这些博客文章详细讲解了 Copy-on-Write 的内部工作原理以及你可以期待什么，包括性能和行为。

我们已经看到 Copy-on-Write 可以将实际工作流的性能提高超过 50%。

## 弃用在 setitem 类操作中的静默上升类型转换

历史上，如果你将不兼容的值设置到列中，pandas 会静默更改其中一个列的数据类型。让我们来看一个例子：

```py
ser = pd.Series([1, 2, 3])

0    1
1    2
2    3
dtype: int64
```

我们有一个包含整数的 Series，这将导致数据类型为整数。让我们将字母 `"a"` 设置到第二行：

```py
ser.iloc[1] = "a"

0    1
1    a
2    3
dtype: object
```

这将把你的 Series 的数据类型更改为 object。Object 是唯一可以同时包含整数和字符串的数据类型。这对很多用户来说是一个重大困扰。Object 列占用大量内存，计算将无法进行，性能下降以及其他许多问题。它还在内部增加了许多特殊处理来适应这些问题。在过去，DataFrame 中的静默数据类型更改曾让我非常烦恼。这个行为现在已被弃用，并将引发 FutureWarning：

```py
FutureWarning: Setting an item of incompatible dtype is deprecated and will raise in a future 
error of pandas. Value 'a' has dtype incompatible with int64, please explicitly cast to a 
compatible dtype first.
  ser.iloc[1] = "a"
```

像我们的例子这样的操作将在 pandas 3.0 中引发错误。DataFrame 的列数据类型将保持一致。你需要在想要更改数据类型时明确指出，这增加了一些代码，但使得未来的开发者更容易理解。

这一变化影响所有的数据类型，例如，将浮点值设置到整数列中也会引发错误。

## 升级到新版本

你可以通过以下方式安装新的 pandas 版本：

```py
pip install -U pandas
```

或：

```py
mamba install -c conda-forge pandas=2.1
```

这将为你的环境提供新的发布版本。

## 结论

我们已经查看了一些改进，这些改进将帮助你编写更高效的代码。这包括性能改进、更容易选择 PyArrow 支持的字符串列以及进一步改进 Copy-on-Write。我们还看到了一项弃用，这将使 pandas 在下一个主要版本中更易于预测其行为。

感谢阅读。欢迎随时联系分享你的想法和反馈。
