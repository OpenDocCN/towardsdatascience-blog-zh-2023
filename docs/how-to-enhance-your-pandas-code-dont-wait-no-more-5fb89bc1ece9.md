# 如何提升你的 Pandas 代码 — 不要再等待了

> 原文：[`towardsdatascience.com/how-to-enhance-your-pandas-code-dont-wait-no-more-5fb89bc1ece9`](https://towardsdatascience.com/how-to-enhance-your-pandas-code-dont-wait-no-more-5fb89bc1ece9)

## 提升你的 pandas 代码性能的 6 个简单改变

[](https://polmarin.medium.com/?source=post_page-----5fb89bc1ece9--------------------------------)![Pol Marin](https://polmarin.medium.com/?source=post_page-----5fb89bc1ece9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5fb89bc1ece9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5fb89bc1ece9--------------------------------) [Pol Marin](https://polmarin.medium.com/?source=post_page-----5fb89bc1ece9--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----5fb89bc1ece9--------------------------------) ·9 分钟阅读·2023 年 4 月 10 日

--

![](img/5316eff4e5a960cbcac4b7ab3908130b.png)

[Pascal Müller](https://unsplash.com/@millerthachiller?utm_source=medium&utm_medium=referral)拍摄于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**Pandas**可以说是任何数据科学家简历中的必备技能。无论你是否使用它，掌握它始终是一种资产。

然而，就像其他任何事情一样，关键不是知道如何使用*pandas*，而是实际编写高效的*pandas*代码。这是大多数人做不到的，但在读完这篇文章后，你不会再是其中的一员。

但首先要做的事情是。

## 什么是 Pandas 以及为什么效率很重要？

根据官方定义，“**pandas**是一个快速、强大、灵活且易于使用的开源数据分析和操作工具，建立在[Python](https://www.python.org/)编程语言之上”[1]。

它真的很强大，许多人在处理数据时默认使用它。

另外，它将数据集加载到我们的 RAM 中，所以关心优化空间使用是个好主意。

但我们不仅想优化它的内存管理，我们可能更关心让代码执行得更快。为什么？显而易见，更快的代码可以节省时间。你知道他们怎么说：时间就是金钱（尤其是当你的代码在 AWS 或 GCP 上运行时）。

硬件可以在一定程度上提高我们程序的性能，但这只是方程式的一半。高效的代码是另一半，和前者一样重要。因此，对于我们这些每天使用它的人来说，掌握 pandas 几乎是必需的。

不过，请不要分心。主要目标是让我们的代码符合要求。在我看来，优化应当留到你的脚本运行过慢或占用过多内存时。并不是说你不应考虑效率，但如果不值得付出额外的努力，就不要将其作为优先事项。

我将尝试超越逻辑效率的说法，但让我们简要突出两个最基本的：

+   **只使用你需要的** — 加载对你的分析有用的列/行，并去掉其余的。

+   **选择正确的变量类型** — 如前所述，pandas 将所有数据加载到系统的 RAM 中，所以明智地选择变量类型是值得的。例如，当所有操作都可以用整数完成时，就不应使用浮点数。

    加载数据框并使用`astype()`函数以确保所有列都具有期望的类型。

在开始之前，让我与大家分享我将使用的数据框，并结合`%timeit`魔法函数来将性能提升量化：

![](img/0e24140bf311cb2938254ad195bb8fbf.png)

本文随机创建的数据 — 作者截图

## 1\. 尽量避免使用循环

这并不总是可能，但去掉循环是你可以为代码添加的最佳优化之一。

Pandas 构建在 NumPy 之上，所以我们最好利用矢量化，而不是使用循环对列/行进行操作。

这个例子是许多人会采用的一种方法，试图从一列中减去另一列的值：

```py
def subtract_open_from_close(df):
    subtracted_price = []
    for i,row in df.iterrows():
        subtracted_price.append(row['Open'] - row['Close'])

    df['SubtractedPrice'] = subtracted_price
    return df

%timeit -n 1 -r 1 subtract_open_from_close(df)
14 s ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)
```

好吧，这并没有花费太多时间，对于一个 ~500k 行的数据框花费了 14 秒。还不错，但如果我告诉你这可以在不到一秒钟内完成呢？

让我们看看做同样事情的更好（甚至是最佳）方法：

```py
def subtract_open_from_close_vectorized(df):
    df['SubtractedPrice'] = df['Open'] - df['Close']
    return df

%timeit subtract_open_from_close_vectorized(df)
2.24 ms ± 57.3 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
```

PAM。

改进效果真是疯狂！我们不仅简化了代码（减少了三行），还节省了**大量**时间。这要归功于矢量化特性，它不是一次操作一个数据元素，而是同时处理所有数据元素。一次操作可以应用于任意数量的数据元素。

仅这一优化就带来了 100 倍的执行时间提升。值得吧？

## 2\. 如果需要循环，避免使用 Iterrows()

有几种替代`iterrows`的方法，比如使用`apply()`或列表推导。然而，我想提供一个与`iterrows()`相当的替代方案，无论如何都需要使用循环。

使用`itertuples()`会返回每一行作为**命名元组**，这是 collections 模块中的一种专门数据类型。它们像元组一样工作，但通过名称访问字段，使用点号（**.**）查找。

让我们重复之前的实验，这次使用`itertuples()`：

```py
df = spread_df.copy()
def subtract_open_from_close(df):
    subtracted_price = []
    for tup in df.itertuples():
        subtracted_price.append(tup.Open - tup.Close)

    df['SubtractedPrice'] = subtracted_price
    return df

%timeit -n 1 -r 1 subtract_open_from_close(df)
821 ms ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)
```

改进效果依然巨大。虽然`iterrows`执行需要大约 14 秒，但使用`itertuples`的完全相同代码则不到一秒钟。

虽然使用矢量化是首选，但我们已经看到循环也可以进行优化。

## 3\. Append 与 Concat

我曾多次需要连接数据框，过去曾一直问自己一个问题：我应该使用 `append()` 还是 `concat()`。最后我默认使用 concat，因为我觉得它更易于使用。

但不要像我一样随意回答这个问题。它的**最终**取决于你愿意连接多少个数据框。当只有两个时，不用太在意，使用其中一个或另一个不会对代码性能产生太大影响。

然而，当尝试连接超过两个数据框时，我建议使用 concat。为什么？因为 `concat()` 是一个库函数，允许我们同时连接多个数据框，而 `append()` 是 DataFrame 类的方法，仅允许一次连接两个数据框。

长话短说：使用 `concat()` 可以同时连接多个数据框，而使用 `append()` 需要一个循环来完成相同的操作。

这不仅在时间上效率较低，而且在内存使用上也不够高效，因为每次调用 append 函数都会返回一个新的数据框，存储在系统内存中。所以，如果我们要连接 N 个数据框，我们会创建 N-1 个数据框，直到得到最终结果，这些数据框都占用宝贵的内存空间。

不好。

所以在追加多个数据框时使用 `concat()`。

## 4\. 不要破坏你的 GroupBy 和合并

`groupby()` 和 `merge()` 都是强大的工具，我敢打赌你每天都在使用它们。因此，妥善处理它们也非常重要。

1.  **首先进行过滤**。GroupBy 和合并是代价高昂的操作，因此请确保在分组/合并之前过滤掉不需要的数据。

1.  **尽量避免在 GroupBy 中使用自定义函数**。当你需要不同于通常的总和、平均值等的分组功能时，你会想创建自己的分组函数。很可能有人已经做过这件事。可能还有内置函数可以处理这些情况。尽可能使用这些内置函数，因为使用自定义函数会降低性能。

1.  **考虑使用 DuckDB**。DuckDB 是一个强大的 OLAP 系统，可以真正帮助你执行像这样分析任务。我最近写了一篇更深入探讨这个话题的文章，如果你对 DuckDB 感兴趣，可以去读读：

[## 忘记 SQLite，改用 DuckDB——稍后感谢我](https://towardsdatascience.com/forget-about-sqlite-use-duckdb-instead-and-thank-me-later-df76ee9bb777?source=post_page-----5fb89bc1ece9--------------------------------)

### DuckDB 及其 Python 集成介绍

towardsdatascience.com

## 5\. 在大型数据集上使用 query()

我必须承认我之前并没有意识到这一点，但在稍微测试了一下后，我认为值得将其添加到这篇文章中。

Pandas 提供了一种名为 `query()` 的数据框方法，目的是用布尔表达式查询数据框的列[2]。它利用了**numexpr** 解析器，这是一个用于 NumPy 的快速数值表达式求值器[3]。

说够理论了，接下来我们展示一下 `query()` 在整个数据框与通常过滤方法的性能对比。由于我们的数据框中包含日期，我们就随机选择一个日期，并尝试从那个日期获取数据：

```py
%timeit df[df['ExtractDate'] == '2022-01-03']
61.7 ms ± 1.67 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)

%timeit df.query("ExtractDate=='2022-01-03'")
32.4 ms ± 1.4 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)
```

很酷吧？在这个 ~500k 行的数据框上，性能提升了将近**50%**。如果我们对一个包含 2,279 行的小数据子集做同样的操作呢？看看这个：

```py
jan22 = df[df['ExtractDate'].str.startswith('2022-01')] # 2279 rows

%timeit jan22[jan22['ExtractDate'] == '2022-01-03']
339 µs ± 13.8 µs per loop (mean ± std. dev. of 7 runs, 1,000 loops each)

%timeit jan22.query("ExtractDate=='2022-01-03'")
831 µs ± 37.5 µs per loop (mean ± std. dev. of 7 runs, 1,000 loops each)
```

结果表明，相反的方向——在小数据框上使用`query()`并不理想，因为与通常的 pandas 过滤方法相比，它会显著降低性能。这就是为什么我在本节标题中指定了“在大型数据集上”。

Pandas 开发者已经深入研究了这一方法，并且他们在线上提供了相关分析，但主要的结论之一是：“只有当你的数据框有大约 100,000 行或更多时，你才会看到使用 `numexpr` 引擎与 `DataFrame.query()` 的性能提升。”[4]

## 6\. 不要默认使用 CSV 文件

我过去总是使用 CSV 文件来加载和保存数据。我现在并不反对 CSV 文件，视任务而定我仍然会使用它们，但当数据更相关和更大时，我已经转向使用**parquet** 文件。

为什么选择 parquet？阅读官方介绍：“Apache Parquet 是一种开源的、面向列的数据文件格式，旨在实现高效的数据存储和检索。它提供了高效的数据压缩和编码方案，增强了处理复杂数据的大数据性能。”[5]

我们可以看到，parquet 文件在内存使用方面比 CSV 文件更高效——它们是“为高效的数据存储设计的，并提供高效的数据压缩和编码方案”——但这如何转化为时间呢？

```py
%timeit df.to_csv("data.csv")
1.82 s ± 43.7 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

%timeit df.to_parquet('data.parquet.gzip', compression='gzip')
1.07 s ± 26.1 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

嗯，这就是**超过 40% 的时间提升**。这里的差异只是几毫秒，但想象一下，这 40% 的提升如何转化为更大的数据框。

说到加载时间，我们来看看它们的差异：

```py
%timeit df = pd.read_csv("data.csv")
212 ms ± 8.1 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

%timeit df = pd.read_parquet('data.parquet.gzip')
73 ms ± 1.89 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)
```

在这里我们再次看到一些改进，不过这次提升更大——**65%**。这意味着在处理更大的数据集时，我们可以节省大量时间。

顺便提一句：还有**Feather 格式 (.ftr)**，虽然我个人没有使用过，但在互联网上看到了很多很好的参考资料，并且有令人印象深刻的分析显示它甚至超过了 parquet，所以也可以试试这个格式。

## 结论

有很多方法可以优化 pandas，我这里只提到了一些，我从中受益最多的方法。

希望这篇文章对你有启发和帮助。一旦你掌握了这些信息，就没有理由不加以使用！

```py
 **Thanks for reading the post!** 
            I really hope you enjoyed it and found it insightful.

          Follow me for more content like this one, it helps a lot!
                                  **@polmarin**
```

如果你想进一步支持我，可以考虑通过下面的链接订阅 Medium 的会员：这不会花费你额外的钱，但会帮助我完成这个过程。非常感谢！

[](https://medium.com/@polmarin/membership?source=post_page-----5fb89bc1ece9--------------------------------) [## 使用我的推荐链接加入 Medium - Pol Marin

### 阅读 Pol Marin 的每一篇故事（以及 Medium 上的成千上万其他作者的故事）。你的会员费用直接支持 Pol…

medium.com](https://medium.com/@polmarin/membership?source=post_page-----5fb89bc1ece9--------------------------------)

## 资源

[1] [pandas — Python 数据分析库](https://pandas.pydata.org/)

[2] [pandas.DataFrame.query — 文档](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.query.html)

[3] [NumExpr — Github](https://github.com/pydata/numexpr)

[4] [数据的索引和选择 — Pandas](https://pandas.pydata.org/docs/user_guide/indexing.html#performance-of-query)

[5] [Apache Parquet](https://parquet.apache.org/)
