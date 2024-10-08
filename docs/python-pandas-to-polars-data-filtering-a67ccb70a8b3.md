# Python Pandas 到 Polars：数据过滤

> 原文：[`towardsdatascience.com/python-pandas-to-polars-data-filtering-a67ccb70a8b3`](https://towardsdatascience.com/python-pandas-to-polars-data-filtering-a67ccb70a8b3)

## 你可能需要尽快做出转变

[](https://sonery.medium.com/?source=post_page-----a67ccb70a8b3--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----a67ccb70a8b3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a67ccb70a8b3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a67ccb70a8b3--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----a67ccb70a8b3--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a67ccb70a8b3--------------------------------) ·5 分钟阅读·2023 年 4 月 18 日

--

![](img/e58c7f96ccd6dea3680769f176f614b2.png)

照片由 [Daphné Be Frenchie](https://unsplash.com/@daphne_befrenchie?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，发布在 [Unsplash](https://unsplash.com/s/photos/filtering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我非常欣赏 Pandas。我从开始学习数据科学的第一天起就一直在使用它。Pandas 在数据清洗、预处理和分析的大多数任务中已经绰绰有余。

我对 pandas 唯一的不满是在处理大数据集时。Pandas 进行内存分析，所以当数据量变得非常大时，它的性能开始下降。

另一个与数据大小相关的缺点是某些操作会产生中间副本。因此，为了能够高效地工作，数据集应相对较小于内存。

对于如此大的数据集，存在不同的替代方案。最近获得显著人气的替代方案之一是 Polars。

有大量文章关注 Polars 与 pandas 的速度比较，但很少有从实际角度解释如何使用 Polars 执行常见的数据清洗和处理操作的文章。

在这一系列文章中，我将向你展示一些常用 Pandas 函数的 Polars 版本。第一个主题是数据过滤操作。在开始示例之前，让我们简要提及一下 Polars 的优势。

# Polars 提供了什么？

Polars 是一个用于 Rust 和 Python 的 DataFrame 库。

+   Polars 利用你计算机上的所有可用核心，而 pandas 仅使用单个 CPU 核心来执行操作。

+   Polars 相较于 pandas 更加轻量，并且没有依赖项，这使得导入 polars 的速度非常快。导入 polars 只需 70 毫秒，而导入 pandas 需要 520 毫秒。

+   Polars 进行查询优化，以减少不必要的内存分配。它还能够以流式方式部分或完全处理查询。因此，polars 可以处理比机器上可用 RAM 更大的数据集。

# 使用 pandas 和 polars 进行数据过滤

我们将通过几个示例来学习如何过滤 polars DataFrames。我们还将看到相同操作的 pandas 版本，以便于从 pandas 过渡到 polars。

首先，我们将创建一个 DataFrame 来进行操作。我们将使用我准备的示例数据集。你可以从我的 [数据集](https://github.com/SonerYldrm/datasets) 仓库下载。

```py
# pandas
import pandas as pd

# read csv
df_pd = pd.read_csv("datasets/sales_data_with_stores.csv")

# display the first 5 rows
df_pd.head()
```

![](img/db0cbd6d20f2e2155b686dd61cc2dcef.png)

pandas DataFrame 的前 5 行 (图片由作者提供)

```py
# polars
import polars as pl

# read_csv
df_pl = pl.read_csv("datasets/sales_data_with_stores.csv")

# display the first 5 rows
df_pl.head()
```

![](img/7dc8411e681cdf07bbd1e79847f35c73.png)

polars DataFrame 的前 5 行 (图片由作者提供)

pandas 和 polars 都有相同的函数来读取 csv 文件并显示 DataFrame 的前 5 行。Polars 还显示了列的数据类型和输出的形状，我认为这是一个很有用的附加功能。

**示例 1：按数值过滤**

让我们过滤价格高于 750 的行。

```py
# pandas
df_pd[df_pd["cost"] > 750]

# polars
df_pl.filter(pl.col("cost") > 750)
```

我将仅展示 pandas 或 polars 版本的输出，因为它们是相同的。

![](img/d5bed42a2a0ea92464f2e487648ebf5b.png)

(图片由作者提供)

**示例 2：多个条件**

pandas 和 polars 都支持按多个条件过滤。我们可以使用“and”和“or”逻辑来组合这些条件。

让我们过滤价格大于 750 且商店值为 Violet 的行。

```py
# pandas
df_pd[(df_pd["cost"] > 750) & (df_pd["store"] == "Violet")]

# polars
df_pl.filter((pl.col("cost") > 750) & (pl.col("store") == "Violet"))
```

![](img/0b6b43beba8ac2bf6df2ebc31d1fca8d.png)

(图片由作者提供)

**示例 3：isin 方法**

pandas 的 isin 方法可以用来将行值与一组值进行比较。当条件由多个值组成时，它非常有用。polars 版本的方法是“is_in”。

我们可以按照如下方式选择 PG1、PG2 和 PG3 的行：

```py
# pandas
df_pd[df_pd["product_group"].isin(["PG1", "PG2", "PG5"])]

# polars
df_pl.filter(pl.col("product_group").is_in(["PG1", "PG2", "PG5"]))
```

输出的前 5 行：

![](img/dff9c5264521de9c741f46125b866269.png)

(图片由作者提供)

**示例 4：选择部分列**

要选择一部分列，我们可以将列名列表传递给 pandas 和 polars DataFrames，如下所示：

```py
cols = ["product_code", "cost", "price"]

# pandas (both of the following do the job)
df_pd[cols]
df_pd.loc[:, cols]

# polars
df_pl.select(pl.col(cols))
```

输出的前 5 行：

![](img/65178e93c249dca7852f9f66f00ef6cd.png)

(图片由作者提供)

**示例 5：选择部分行**

我们可以使用 loc 或 iloc 方法来选择 pandas 的部分行。在 polars 中，我们使用非常类似的方法。

这是一个简单的示例，选择第 10 行到第 20 行之间的行：

```py
# pandas
df_pd.iloc[10:20]

# polars
df_pl[10:20]
```

要选择相同的行但仅选择前三列：

```py
# pandas
df_pd.iloc[10:20, :3]

# polars
df_pl[10:20, :3]
```

如果我们想通过名称选择列，可以使用 pandas 中的 loc 方法。

```py
# pandas
df_pd.loc[10:20, ["store", "product_group", "price"]]

# polars
df_pl[10:20, ["store", "product_group", "price"]]
```

**示例 6：按数据类型选择列**

我们还可以选择特定数据类型的列。让我们做一个选择具有 64 位整数（即 int64）数据类型的列的示例。

```py
# pandas
df_pd.select_dtypes(include="int64")

# polars
df_pl.select(pl.col(pl.Int64))
```

输出的前 5 行：

![](img/165e912d2cd854acc76aeb84f2b51f4c.png)

(图片由作者提供)

我们做了几个示例来比较**Pandas**和**Polars**之间的过滤操作。总体而言，**Polars**与**Pandas**非常相似，但在某些情况下采用了类似于**Spark SQL**的方法。如果你对使用**Spark SQL**进行数据清洗和操作很熟悉，你会发现这些相似之处。

话虽如此，考虑到在处理大型数据集时**Polar**的效率，它可能很快成为取代**Pandas**进行数据清洗和操作任务的有力候选者。

*你可以成为* [*Medium 会员*](https://sonery.medium.com/membership) *以解锁我所有的写作内容，以及 Medium 的其他内容。如果你已经是会员了，请不要忘记* [*订阅*](https://sonery.medium.com/subscribe) *，以便在我发布新文章时收到电子邮件。*

感谢阅读。如果你有任何反馈，请告诉我。
