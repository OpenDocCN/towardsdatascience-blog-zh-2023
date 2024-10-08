# 从 Pandas 切换到雷电般快速的 Polars 并永不回头的 7 个简单步骤

> 原文：[`towardsdatascience.com/7-easy-steps-to-switch-from-pandas-to-lightning-fast-polars-and-never-return-b14c66fc85b9`](https://towardsdatascience.com/7-easy-steps-to-switch-from-pandas-to-lightning-fast-polars-and-never-return-b14c66fc85b9)

## 最常见的 Pandas 操作的速查表翻译为 Polars

[](https://ibexorigin.medium.com/?source=post_page-----b14c66fc85b9--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----b14c66fc85b9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b14c66fc85b9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b14c66fc85b9--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----b14c66fc85b9--------------------------------)

·发布于[数据科学前沿](https://towardsdatascience.com/?source=post_page-----b14c66fc85b9--------------------------------) ·阅读时间 9 分钟·2023 年 4 月 3 日

--

![](img/c3c3d6087a84b3a47f4c643d748b0d43.png)

图片由作者通过 Midjourney 提供

## 是时候说再见了！

Pandas 可以做任何事。几乎任何事。但（这真的是我无数次希望它可以有所不同的*但*）它缺乏速度。Pandas 根本无法跟上今天数据集规模和复杂度增长的速度。

Pandas 的作者 Wes McKinney 表示，当他编写 Pandas 时，他有一个针对其库的经验法则：

> 拥有比数据集大小多 5 到 10 倍的 RAM。

也许你曾经对引入 Iris 数据集时的这一规则视而不见，但今天情况有所不同。你根本无法在 RAM*坚决*停留在 64GB 的情况下加载 100GB 的数据集（这在现代社会中已经很常见）。

当然，也有像 Dask 这样的优秀替代方案。但 Dask 并没有实现新的功能。它只是将现有的 Pandas 语法扩展到多个进程（线程）上，忽略了底层的性能和内存问题。

它将 Pandas 视为**黑箱**。请原谅我这样说，但这几乎就像是在猪身上涂口红一样。

Polars，本文的重点，是从头开始用 Rust 编写的，旨在修复 Pandas 的所有不足。在我上一篇文章中，我们已经看到它比即将推出的 Pandas 2.0（配备 PyArrow 后端）更快。

虽然那篇文章重点关注速度优势，这篇文章则更多地关注语法和功能，并展示了如何在七个简单步骤中从 Pandas 切换到 Polars，也许永远不会再切换回来。

## 0. 读写数据

尽管速度极其缓慢，CSV 仍然是存储数据的最流行文件格式之一。所以，让我们从 Polars 的`read_csv`函数开始吧。

除了明显的速度优势外，它只在参数数量（Pandas 的 `read_csv` 有 49 个。是的，我数过）和语法上与 Pandas 有所不同。

![](img/44c230f7575349cfdbb31f100a9d7f71.png)

作者提供的图片

混淆的参数名称不应该是问题，因为大多数现代 IDE 都具有自动补全或弹出文档功能（JupyterLab 上的**Shift + Tab**，谢谢 🙏）。

如果你不知道的话，`dtype` 参数防止 Pandas 设置自动数据类型，并允许用户设置自定义数据类型，如 `cut` 的 `string` 类型或日期类型列的 `datetime`。

你可以在 Polars 中使用相同的行为，使用 `dtypes`（注意是‘s’），虽然它不允许通过字符串设置类型。你必须提供 Python 内建类型或 Polars 的类型，例如 `pl.Boolean`、`pl.Categorical`、`pl.DateTime`、`pl.Int64` 或 `pl.Null`（表示缺失值）。你可以通过调用 `dir(pl)` 查看完整列表。

读取和写入 Parquet 文件，这比 CSV 文件更快、更节省内存，Polars 通过 `read_parquet` 和 `write_parquet` 函数也支持这些操作。

![](img/0b4af4f7dd3227f8a748cb952903ee03.png)

作者提供的图片

## 1\. 创建 Series 和 DataFrames

你并不总是从文件中读取数据。像在 Pandas 中一样，你可以从头创建 DataFrames 和 Series，语法几乎相同：

![](img/de99210af7adb4c1045ec58c3f988b82.png)![](img/ba3f26652bda07fab8aa5303850303a5.png)

作者提供的图片

Polars DataFrames 也有许多与 Pandas 几乎相同名称和行为的方法。打个招呼吧：

+   `[apply](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.apply.html#polars.DataFrame.apply)` - 对 DataFrame 的每一行应用自定义用户定义的函数

+   `[corr](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.corr.html#polars.DataFrame.corr)` - 相关性矩阵

+   `[describe](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.describe.html#polars.DataFrame.describe)` - 汇总统计，五数概括

+   `[drop](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.drop.html#polars.DataFrame.drop)` - 从 DataFrame 中删除列

+   `[explode](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.explode.html#polars.DataFrame.explode)` - 将给定列解包为长格式（当单元格包含多个值如 `[1, 2, 3]` 时）

+   `[head](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.head.html#polars.DataFrame.head)`、`[tail](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.tail.html#polars.DataFrame.tail)` 和 `[sample(n)](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.sample.html#polars.DataFrame.sample)` - 获取 DataFrame 的不同视图（顶部、底部、随机）

+   `[iter_rows](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.iter_rows.html#polars.DataFrame.iter_rows)` - 返回一个 DataFrame 行的迭代器，包含 Python 原生值

+   `max`、`mean`、`median`、`sum`、`std` 和常见的数学和统计函数。

等等。查看 [这个页面](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/index.html) 以获取 Polars 中 DataFrame 方法的完整列表。

## 2\. 理解 Polars 中的表达式

![](img/bf7e75e893d0599c0f5ea425a523bec5.png)

这是一个查询引擎，人类。 — Midjourney

Polars 的核心是其 **查询引擎**，它运行用户定义的 **表达式**。查询引擎和表达式是 Polars 极快性能的两个关键组件，正如 Polars 用户指南所说的，“令人尴尬的并行”。

你可能会惊讶于 Polars 表达式与 SQL 的相似程度，同时保持与熟悉的 Pandas 语法的紧密联系。

像 SQL 查询一样，你可以为以下内容编写表达式：

+   从现有列创建新列

+   在某些转换后获取数据视图

+   汇总统计

+   处理和清理数据

+   Groupby 语句

等等。

```py
df.filter(pl.col('column') == 'some_value')
```

在上面的查询中，表达式是 `pl.col('column)' == 'some_value'`，这正如你所猜测的，*过滤* DataFrame 以获得 `column` 等于 `some_value` 的行。

当你单独运行这个表达式时，你不会得到像在 Pandas 中那样的布尔 Series：

```py
type(pl.col("column") == "some_value")
```

```py
polars.expr.expr.Expr
```

这是因为表达式只在 *上下文* 中进行评估。Polars 中有三种广泛的上下文：

1.  选择数据 — 在 `select` 上下文中，表达式应用于列，并且必须在结果中产生相同长度的列。这种行为应当对你来说很熟悉。`filter` 函数也与这个上下文相关联。

1.  分组数据 — 在 `groupby` 上下文中，表达式在组上工作，结果可能具有任何长度，因为一个组可以有多个成员。

1.  添加新列 — 在这种情况下，表达式用于从头开始或从现有列创建新列。

让我们详细看看每个上下文。

## 3\. 选择数据

Pandas 的括号表示法让位于 Polars 中用于选择列的表达式。

要选择单个列，你可以在 `select` 中使用它们的字面名称，或使用推荐的 `pl.col` 函数来引用列。

要选择多个列，你可以在 `pl.col` 中列出列名，用逗号分隔，或在 `select` 中列出 `pl.col` 引用的列表。我们稍后会看到这些语法之间的区别。

![](img/7cefcd08edf6e03e42c1404b1462778b.png)

图片由作者提供

Polars 包含一些 Pandas 中未完全提供的选择数据功能。例如，你可以使用 `exclude` 排除选择中的列：

```py
df.select(pl.exclude("price")).head()
```

![](img/a346371b15220539a09ec951f8343e33.png)

图片由作者提供

或者使用 `^` 和 `$` 字符之间的正则表达式。下面，我们选择所有以字母 *c* 开头的列：

```py
df.select(pl.col("^c.+$")).head()
```

![](img/b0d429fbda06c39284086ac7415a465b.png)

图片由作者提供

你还可以根据数据类型对子集 DataFrame，这可能让你想起 Pandas 中的 `select_dtypes`（左侧）：

![](img/645c4fc4bd91ba1c12d477ccc2d67c7a.png)

图片由作者提供

要选择所有数值列，我们在 `pl.col` 中使用了 `Int64` 和 `Float64` 类型。

## 4\. 数据过滤

你可以使用 `filter` 函数通过布尔索引对 DataFrames 进行子集化。例如，在列上使用 `is_between` 函数可以创建一个表达式来筛选范围内的数值列。

![](img/d240bddd0cd570e39959051d86bb9221.png)

图片由作者提供

你可以使用熟悉的布尔运算符 `&` (AND) 和 `|` (OR) 结合多个 *条件表达式*。在下面的示例中，我们选择 `color` 列为 'E' 或 'J' 并且钻石的 `price` 低于 500 的行：

![](img/934b5e017bea5f566317496ae9aab707.png)

图片由作者提供

另外，注意我们在右侧如何使用 Polars 中的 `is_in`。

## 5\. 创建新列

你可以在 `with_columns` 上下文中创建新列。在下面的示例中，`new_col` 使用 `pl.col('price') ** 2` 定义，并通过别名给新列命名，就像 SQL 中的 `as` 关键字一样。

![](img/65624baaae02358a66024e15b3d2c7e2.png)

图片由作者提供

在第二个示例中，我们结合了两个列（尽管这没有意义），演示了如何将整数和字符串列与 Polars 结合使用。你可以在使用 `pl.col` 引用的列上使用任何本地 Python 或第三方函数和运算符。

如果你想将新列插入到 DataFrame 中，你必须覆盖原始的 `df` 变量。

> 顺便提一下，Polars 中的字符串列具有熟悉的 `.str` 接口，用于特殊的文本操作函数，如 `contains` 或 `lengths`。完整列表请参见 [这里](https://pola-rs.github.io/polars/py-polars/html/reference/series/string.html)。还有用于专业分类、时间和数组函数的 `[.cat](https://pola-rs.github.io/polars/py-polars/html/reference/series/categories.html)`、`[.dt](https://pola-rs.github.io/polars/py-polars/html/reference/series/temporal.html)` 和 `[.arr](https://pola-rs.github.io/polars/py-polars/html/reference/series/list.html)` 接口。

## 6\. 分组操作

我不认为我们可以不提及 `Groupby` 操作：

![](img/80a82e43f3e9b70d746b962f35409a1c.png)

图片由作者提供

使用 Polars 的 `groupby` 函数时，务必包含 `maintain_order=True` 以确保组不会随机显示。此外，与 Pandas 不同，`groupby(col_name)` 表达式仅作用于给定列。要根据 `col_name` 对所有列进行分组，你必须使用聚合上下文。其语法如下：

```py
df.groupby(
    "cut", maintain_order=True
).agg(pl.col("*").count())
```

在 `groupby` 上下文之后，你将链式聚合上下文，并指定上下文影响哪些列。然后在结果上链式调用任何函数，例如 `count`。

这是另一个示例，按钻石切割质量分组，并返回每组的平均数值：

![](img/8beaae709bd9675c7defc2f7a358d5ce.png)

要了解更多关于 Polars 中高级 `groupby` 表达式的信息，请阅读[这里](https://pola-rs.github.io/polars-book/user-guide/dsl/groupby.html)。

## 7\. Polars 中的懒惰 API

Polars 最棒的功能之一是其懒惰 API。在该 API 中，查询不是逐行执行的，而是由查询引擎端到端处理的。

这是查询优化和*令人尴尬的并行*魔法发生的地方。你可以仅用两个关键字将任何在**急切模式**下编写的表达式转换为懒惰模式：

```py
import polars as pl

df = pl.read_csv("data/diamonds.csv")

query = df.lazy().filter(
    pl.col("cut") == "Ideal"
)

type(query)
```

```py
polars.lazyframe.frame.LazyFrame
```

当你在链式表达式之前添加 `lazy()` 函数时，DataFrame 会变成 `LazyFrame`。此时，查询尚未执行，你可以链式添加更多表达式。一旦准备好，你可以调用 `collect()` 来获取结果：

```py
query.collect().head()
```

![](img/d9993faba6603676332e7d66b21a03ea.png)

图片由作者提供

虽然它在急切模式下已经很快，但懒惰模式为查询引擎提供了额外的（是的，三倍的 x）动力。

如果你希望将懒惰 API 设为默认，可以在读取数据时使用 `scan_*` 函数，而不是 `read_*`：

```py
df = pl.scan_csv("data/diamonds.csv")

q1 = df.filter(
    pl.col("cut") == "Ideal"
)

q1.collect().head()
```

这样，你就能避免每次都编写 `lazy()` 函数。

如果你处理的数据集对你的内存构成威胁，你可以使用流式处理，使 Polars 分批处理数据。这个功能可以通过在`collect`中设置`streaming=True`来启用。更多关于这一优秀功能的信息，请参见[此页面](https://pola-rs.github.io/polars-book/user-guide/lazy-api/streaming.html)。

## 结论

Polars 可能很新（我是说，它刚刚出生不久），但已经非常受欢迎。在开源标准中，它是明星。只需看看它的竞争对手：

1.  Pandas，发布于 2011 年，拥有 37.5k 个 GitHub 星标。

1.  Apache Spark，发布于 2014 年，拥有 26.8k 个星标。

1.  Vaex，发布于 2017 年，拥有 7.9k 个 GitHub 星标。

1.  Dask，发布于 2015 年，拥有 10.9k 个星标。

1.  Apache Arrow，发布于 2016 年，拥有 11.4k 个星标。

相比之下，Polars 于 2020 年发布，已经积累了 15.9k 个星标，接近其长期顶级竞争对手的一半。

这应该能给你一个大概的了解风向如何变化。虽然 Pandas 2.0 发布后情况可能会有所变化，但我认为 Polars 已经在给 Pandas 带来不小的竞争。

喜欢这篇文章吗？说实话，它那奇特的写作风格呢？想象一下，如果你能访问到更多类似的文章，全部由一位才华横溢、迷人而风趣的作者（顺便说一下，就是我 :）。

只需 4.99$的会员费用，你将不仅能访问我的故事，还能获取 Medium 上最优秀的思想者们的宝贵知识。如果你使用[我的推荐链接](https://ibexorigin.medium.com/membership)，你将获得我超级 nova 的感激之情和一记虚拟的击掌，以支持我的工作。

[](https://ibexorigin.medium.com/membership?source=post_page-----b14c66fc85b9--------------------------------) [## 通过我的推荐链接加入 Medium — Bex T.

### 获得对我所有⚡优质⚡内容的独家访问权限，无限制地浏览 Medium。通过购买我提供的服务来支持我的工作……

ibexorigin.medium.com](https://ibexorigin.medium.com/membership?source=post_page-----b14c66fc85b9--------------------------------) ![](img/a01b5e4fb641db5f35b8172a4388e821.png)
