# 如何优化多维 Numpy 数组操作的 Numexpr

> 原文：[`towardsdatascience.com/how-to-optimize-multidimensional-numpy-array-operations-with-numexpr-32ba9ea8e9a6`](https://towardsdatascience.com/how-to-optimize-multidimensional-numpy-array-operations-with-numexpr-32ba9ea8e9a6)

## [快速计算](https://medium.com/@qtalen/list/fast-computing-2a37a7e82be5)

## Numpy 性能优化的实际案例研究

[](https://qtalen.medium.com/?source=post_page-----32ba9ea8e9a6--------------------------------)![Peng Qian](https://qtalen.medium.com/?source=post_page-----32ba9ea8e9a6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----32ba9ea8e9a6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----32ba9ea8e9a6--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----32ba9ea8e9a6--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----32ba9ea8e9a6--------------------------------) ·阅读时间 5 分钟·2023 年 10 月 22 日

--

![](img/96fef5f72810913ebac709c8f00de6a6.png)

如何使用 Numexpr 优化多维 Numpy 数组操作。图片来源：作者创建，[Canva](https://www.canva.com/)。

这是一篇相对简短的文章。在其中，我将以一个实际场景为例，解释如何在多维 Numpy 数组中使用[Numexpr 表达式](https://numexpr.readthedocs.io/en/latest/user_guide.html?ref=dataleadsfuture.com#supported-functions)以实现显著的性能提升。

目前关于如何在多维 Numpy 数组中使用 Numexpr 及其表达式的文章不多，因此希望这篇文章能对你有所帮助。

# 介绍

最近，在回顾一些旧工作时，我偶然发现了这段代码：

```py
def predict(X, w, b):
    z = np.dot(X, w)
    y_hat = sigmoid(z)
    y_pred = np.zeros((y_hat.shape[0], 1))

    for i in range(y_hat.shape[0]):
        if y_hat[i, 0] < 0.5:
            y_pred[i, 0] = 0
        else:
            y_pred[i, 0] = 1
    return y_pred
```

这段代码将预测结果从概率转换为机器学习中逻辑回归模型的 0 或 1 分类结果。

但天哪，谁会用`for loop`来遍历 Numpy ndarray？

你可以预见，当数据量达到一定程度时，它不仅会占用大量内存，而且性能也会变差。

是的，这段代码是我年轻时写的。

带着责任感，我计划今天使用 Numexpr 库重写这段代码。

在这个过程中，我将展示如何在多维 Numpy 数组中使用 Numexpr 及其`where`表达式，以实现显著的性能提升。

# 代码实现

如果你不熟悉 Numexpr 的基本用法，可以参考这篇文章：

[](/exploring-numexpr-a-powerful-engine-behind-pandas-cdb94965ca3a?source=post_page-----32ba9ea8e9a6--------------------------------) ## 探索 Numexpr：Pandas 背后的强大引擎

### 使用 Python 的 Numexpr 和 Pandas 的 eval/query 函数提升数据分析性能

[towardsdatascience.com

本文使用实际案例演示了 Numexpr 在 Numpy 和 Pandas 中的 API 和表达式的具体用法。

> `where(bool, number1, number2): number` - 如果布尔条件为真，则为 number1，否则为 number2。

上述是 Numexpr 中 where 表达式的用法。

在处理矩阵数据时，你可能习惯使用 Pandas 的 `DataFrame`。但由于 Pandas 的 `eval` 方法不支持 `where` 表达式，你只能选择在多维 Numpy ndarray 中使用 Numexpr。

不用担心，我会马上为你解释。

在开始之前，我们需要导入必要的包并实现一个 `generate_ndarray` 方法，用于生成特定大小的 ndarray 进行测试：

```py
from typing import Callable
import time

import numpy as np
import numexpr as ne
import matplotlib.pyplot as plt

rng = np.random.default_rng(seed=4000)

def generate_ndarray(rows: int) -> np.ndarray:
    result_array = rng.random((rows, 1))
    return result_array
```

首先，我们生成一个 200 行的矩阵，以查看它是否是我们想要的测试数据：

```py
In:  arr = generate_ndarray(200)
     print(f"The dimension of this array: {arr.ndim}")
     print(f"The shape of this array: {arr.shape}")

Out: The dimension of this array: 2
     The shape of this array: (200, 1)
```

为了接近实际的逻辑回归模型情况，我们生成一个形状为 `(200, 1)` 的 ndarray。当然，你也可以根据需要测试其他形状的 ndarray。

接着，我们开始编写 Numexpr 在 `numexpr_to_binary method` 中的具体用法：

+   首先，我们使用索引来分隔需要处理的列。

+   然后，使用 Numexpr 的 where 表达式来处理这些值。

+   最后，将处理过的列与其他列合并，以生成所需结果。

由于 ndarray 的形状是 `(200, 1)`，只有一列，因此我添加了一个新维度。

代码如下：

```py
def numexpr_to_binary(np_array: np.ndarray) -> np.ndarray:
    temp = np_array[:, 0]
    temp = ne.evaluate("where(temp<0.5, 0, 1)")
    return temp[:, np.newaxis]
```

我们可以用一个 10 行的数组测试结果，以查看是否符合预期：

```py
arr = generate_ndarray(10)
result = numexpr_to_binary(arr)

mapping = np.column_stack((arr, result))
mapping
```

![](img/13311bcec6696d413197750909d02feb.png)

我测试了一个 10 行的数组，结果是我想要的。图片由作者提供

看，匹配是正确的。我们的任务已完成。

整个过程可以通过以下图示演示：

![](img/7b69c20ea53ba88dd7a3408f48aa0219.png)

Numexpr 如何转换多维 ndarray 的整个过程。图片由作者提供

# 性能比较

在代码实现后，我们需要将 Numexpr 实现版本与之前的 `for each` 实现版本进行比较，以确认性能是否有所提升。

首先，我们实现一个 `numexpr_example` 方法。该方法基于 Numexpr 的实现：

```py
def numexpr_example(rows: int) -> np.ndarray:
    orig_arr = generate_ndarray(rows)
    the_result = numexpr_to_binary(orig_arr)
    return the_result
```

接着，我们需要补充一个 `for_loop_example` 方法。该方法参考了我需要重写的原始代码，并用作性能基准：

```py
def for_loop_example(rows: int) -> np.ndarray:
    the_arr = generate_ndarray(rows)
    for i in range(the_arr.shape[0]):
        if the_arr[i][0] < 0.5:
            the_arr[i][0] = 0
        else:
            the_arr[i][0] = 1
    return the_arr
```

然后，我编写了一个测试方法 `time_method`。该方法将分别生成从 10 到 10 的 9 次方行的数据，调用相应的方法，并最终保存不同数据量所需的时间：

```py
def time_method(method: Callable):
    time_dict = dict()
    for i in range(9):
        begin = time.perf_counter()
        rows = 10 ** i
        method(rows)
        end = time.perf_counter()
        time_dict[i] = end - begin
    return time_dict
```

我们分别测试 numexpr 版本和 `for_loop` 版本，并使用 `matplotlib` 绘制不同数据量所需的时间：

```py
t_m = time_method(for_loop_example)
t_m_2 = time_method(numexpr_example)
plt.plot(t_m.keys(), t_m.values(), c="red", linestyle="solid")
plt.plot(t_m_2.keys(), t_m_2.values(), c="green", linestyle="dashed")
plt.legend(["for loop", "numexpr"])
plt.xlabel("exponent")
plt.ylabel("time")
plt.show()
```

![](img/2f0838e067b94b2ae7c340f88d0055db.png)

Numexpr 版本的实现具有显著的性能提升。图片来源：作者

可以看出，当数据行数大于 10 的 6 次方时，Numexpr 版本的实现具有显著的性能提升。

# 结论

在前一篇文章中解释了 Numexpr 的基本用法，本篇文章通过实际工作中的具体示例，说明如何使用 Numexpr 重写现有代码以获得性能提升。

本文主要使用了 Numexpr 的两个特性：

1.  Numexpr 允许以矢量化的方式进行计算。

1.  在 Numexpr 的计算过程中，不会生成新的数组，从而显著减少内存使用。

感谢阅读。如果你有其他解决方案，请随时留言与我讨论。

感谢阅读我的故事。

你可以[**订阅**](https://www.dataleadsfuture.com/#/portal)以获取我最新的数据科学故事。

如果你有任何问题，可以在[LinkedIn](https://www.linkedin.com/in/qtalen/)或[Twitter(X)](https://twitter.com/qtalen)找到我。

本文最初发布在[Data Leads Future](https://www.dataleadsfuture.com/how-to-optimize-multidimensional-numpy-array-operations-with-numexpr/#/portal)。
