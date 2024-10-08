# 使用 Polars 进行 EDA：针对 Pandas 用户的逐步指南（第一部分）

> 原文：[`towardsdatascience.com/eda-with-polars-step-by-step-guide-for-pandas-users-part-1-b2ec500a1008`](https://towardsdatascience.com/eda-with-polars-step-by-step-guide-for-pandas-users-part-1-b2ec500a1008)

## 用 Polars 提升你的数据分析水平

[](https://medium.com/@antonsruberts?source=post_page-----b2ec500a1008--------------------------------)![Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----b2ec500a1008--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b2ec500a1008--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b2ec500a1008--------------------------------) [Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----b2ec500a1008--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b2ec500a1008--------------------------------) ·阅读时间 12 分钟·2023 年 7 月 6 日

--

![](img/1ec62693f9e625f8eb7847708504fcaa.png)

图片由 [Mitul Grover](https://unsplash.com/@mitulgrover?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

每隔一段时间，就会出现一种工具，显著改变数据分析的方式。我认为 Polars 就是这样一种工具，因此在这一系列文章中，我将深入探讨这个库，将其与更知名且成熟的库 —— Pandas 进行比较，并展示使用示例数据集的分析工作流程。

# 什么是 Polars？

Polars 是一个用 Rust 编写的极其快速的 DataFrame 库。幸运的是，对于我们（数据科学家/分析师）来说，它有一个非常完善的 Python 封装，暴露出一整套功能来处理数据和构建数据管道。以下是我在切换到 Polars 后看到的主要优点：

+   更快的预处理操作

+   处理比 RAM 更大的数据集的能力

+   由于需要正确构建数据管道，代码质量更高

你可以在这个 [用户指南](https://pola-rs.github.io/polars-book/user-guide/#philosophy) 中看到完整的好处，并在这个 H20 [基准测试](https://h2oai.github.io/db-benchmark/) 中查看速度比较。

# 从 Pandas 切换

乍一看，Pandas 和 Polars 似乎非常相似，例如 `.read_csv()` 或 `.head()` 这些方法在它们之间是共享的，因此你可以在不进行任何更改的情况下执行基本的探索性操作。但随着你开始使用这个库，你会发现这两个库之间有许多不同之处。从语法到思维方式，切换到 Polars 并非易事。这就是为什么我希望这些文章能帮助你入门。

# 设置

为了跟随这个项目，请确保从最新的笔记本中拉取这个[GitHub 仓库](https://github.com/aruberts/tutorials/blob/main/polars/basics.ipynb)。本项目使用的数据可以从[Kaggle](https://www.kaggle.com/datasets/datasnaek/youtube-new?resource=download&sort=published)下载（CC0：公共领域）。这是一个关于 YouTube 顶级趋势视频的数据集，应该能为这系列文章提供足够的复杂性。另外，你需要安装 Pandas 和 Polars，可以通过 pip 安装这两个包。

一切准备就绪后，让我们开始项目吧！这里的主要目标是让你更熟悉 Polars，所以请确保跟着步骤操作，或者在自己的数据集上练习这些概念。再一次，在[GitHub 仓库](https://github.com/aruberts/tutorials/blob/main/polars/basics.ipynb)中你可以找到所有代码的笔记本。

# 数据处理

## 读取数据

读取数据对于 Pandas 用户来说会很熟悉，因为它使用了完全相同的方法。让我们读取一下英国视频的统计数据以进行进一步分析，并打印出 DataFrame 的形状。

```py
# Define file path
path  = './youtube/GBvideos.csv'

# Read in using Polars
df_pl = pl.read_csv(path)
print(df_pl.shape)
>>> (38916, 16)

# Read in using Pandas
df_pd = pd.read_csv(path)
print(df_pd.shape)
>>> (38916, 16)
```

如果你对这两个读取操作的时间进行计时，你将会有第一次的“哇”时刻。在我的笔记本电脑上，Polars 读取文件的时间约为 110 毫秒，而 Pandas 约为 270 毫秒。这是 2.5 倍的加速，但你会经常看到读写操作的加速效果远超此值（尤其是对于更大的文件）。

## 常见的探索性方法

你在读取数据时首先做什么？我猜测你会打印出前几行（或样本），检查数据类型、形状等。Polars 与 Pandas 共享很多这些高级方法，所以你可以使用 `.head()` 方法查看前几行，使用 `.tail()` 方法查看最后几行。

```py
print(df_pl.head(2))
print(df_pl.tail(2))
print(df_pl.sample(2))
```

![](img/ee17eb91e56a42cc5a89d49a24fb24ce.png)

前两行。截图由作者提供。

虽然你可以看到输出，但其格式并不理想。你可以使用 `Config` 更改输出的显示方式。例如，为了使打印输出更宽，你可以将每行的最大字符数设置为 200，如 `pl.Config.set_tbl_width_chars(200)`。这样输出会看起来更为美观。

![](img/a82fd24d1e2f072e1b28e9a498f0399b.png)

宽格式。截图由作者提供。

你可以调整许多其他参数（例如，隐藏列数据类型），所以一定要查看一下[配置文档](https://pola-rs.github.io/polars/py-polars/html/reference/config.html)。

# 选择列

## 按数据类型

你可能从打印输出中看到 Polars 的列数据类型有所不同。数值列通常被分配为`Int32`，`Int64`，`Float32`，`Float64`，而分类列通常被分配为`Utf8`。要按数据类型选择列，可以使用所谓的`selectors`与`.select()`方法一起使用。这些`selectors`是 API 的相对较新功能，它们为选择列提供了更直观的方式。例如，下面是选择所有数值列和所有分类列的代码。

```py
import polars.selectors as cs

# Polars feature selectiom
numeric_data_pl = df_pl.select(cs.numeric())
categorical_data_pl = df_pl.select(cs.string())

# Pandas feature selection
numeric_data_pd = df_pd.select_dtypes(include="number")
categorical_data_pd = df_pd.select_dtypes(exclude="number")
```

![](img/2b867381551357ccc49403733ba679e2.png)![](img/4b552e6e511515acc04b5ddf2c85b13e.png)

数值和分类特征。作者截图。

## 按列名

如果你想按名称选择列，可以使用相同的`.select()`方法，但现在需要将列名放在`pl.col()`中。要选择多个列，只需将名称放在列表中，与 Pandas 非常类似。

```py
subset_pl = df_pl.select(
    pl.col("likes"), 
    pl.col("views"),
    pl.col("comment_count")
)
susbet_pd = df_pd[["likes", "views", "comment_count"]]

# This will also work
susbet_pl_2 = df_pl[["likes", "views", "comment_count"]]
```

应该注意的是，`df_pl[["likes", "views", "comment_count"]]`也会由于 Polars 实现的语法糖而有效。但写完整的语句是一种好习惯，因此我建议你同时练习两种写法。

# 选择行

要选择特定的行，你需要使用`.filter()`方法。注意，Polars 没有索引，这意味着像`.iloc`这样的命令不可用。我们来看看数据集中有多少行的观看次数少于 1000。这应该非常少，因为不受欢迎的视频进入“Trending”标签页的可能性很小。

```py
filtered_pl = df_pl.filter(pl.col("views") < 1000)
filtered_pl.shape
>>> (6, 16)

filtered_pd = df_pd[df_pd['views'] < 1000]
filtered_pd.shape
>>> (6, 16)
```

# 数据质量检查

对于更复杂的用例，让我们进行基本的数据质量检查。在进行数据质量检查时，检查每列的缺失行数和静态列数始终是一个好主意。在 Pandas 中，这可以通过内置检查和聚合非常简单地完成。

```py
missing = df_pd.isna().sum()
missing = missing[missing > 0]
static = df_pd.nunique() == 1
static = static[static]

print("Missing rows:")
print(missing)
print("\nStatic Columns:")
print(static)

>>> Missing rows:
>>> description    612
>>> dtype: int64
>>> Static Columns:
>>> Series([], dtype: bool)
```

使用 Polars 时，这部分稍显复杂，需要链式调用几个方法。

```py
missing = (
    df_pl.select(pl.all().is_null().sum())
    .melt(value_name="missing")
    .filter(pl.col("missing") > 0)
)
static = (
    df_pl.select(pl.all().n_unique())
    .melt(value_name="unique")
    .filter(pl.col("unique") == 1)
)
print("Missing columns:")
print(missing)

print("\nStatic columns:")
print(static)
```

```py
Missing columns:
shape: (0, 2)
┌──────────┬─────────┐
│ variable ┆ missing │
│ ---      ┆ ---     │
│ str      ┆ u32     │
╞══════════╪═════════╡
└──────────┴─────────┘

Static columns:
shape: (0, 2)
┌──────────┬────────┐
│ variable ┆ unique │
│ ---      ┆ ---    │
│ str      ┆ u32    │
╞══════════╪════════╡
└──────────┴────────┘
```

让我们详细解析计算缺失行数量的代码。

+   `df_pl.select(pl.all())`对所有列重复指定操作

+   `.is_null().sum()`统计空值的数量（Polars 对 NA 的表示）

+   `.melt()`将宽格式的 DataFrame 转换为长格式

+   `.filter(pl.col("missing") > 0)`筛选出没有缺失行的列

你可以看到，链式操作非常简单，流程与 PySpark 非常相似。虽然代码略显复杂，但在 Polars 中的执行速度比其他工具快约 4 倍。

令人惊讶的是，数据质量检查的结果不匹配。使用 Pandas 时，`description`列有 612 个缺失行，而在 Polars 中我们看不到这种情况。这是因为 Polars 将缺失字符串视为空字符串`""`，因此它们不会出现在空值计数中。如果需要，你可以使用`.replace()`方法轻松将这些字符串替换为空值。

## 数据预处理

准备数据需要完成两个步骤：

+   将日期列转换为 `datetime` 格式

+   用实际类别名称替换类别 ID

为此，我们需要使用 `.with_columns()` 方法，因为它会返回一个包含更改后列的整个 DataFrame。在这种情况下，`.select()` 方法不适用，因为它只会返回处理后的列。此外，我们还需要使用 `.str` 命名空间，它与 Pandas 非常相似。这个命名空间提供了所有字符串操作，例如 `.str.contains()` 或 `.str.to_lowercase()`（所有操作请参见 [这里](https://pola-rs.github.io/polars/py-polars/html/reference/expressions/string.html)）。如果你对在 Polars 中处理字符串感兴趣，完成这篇文章后可以查看这个 [帖子](https://medium.com/me/stats/post/fcf7054a929a)。

```py
# Pandas datetime conversion
df_pd['publish_time'] = pd.to_datetime(df_pd['publish_time'])
df_pd['trending_date'] = pd.to_datetime(
    df_pd['trending_date'], format='%y.%d.%m'
)

# Polars datetime conversion
df_pl = df_pl.with_columns(
    pl.col('trending_date').str.to_date(format='%y.%d.%m'),
    pl.col('publish_time').str.to_datetime()
)
```

要替换类别，我们只需应用 `.map_dict()` 方法，它类似于 Pandas 的 `.map()`。在 Polars 中，`.map()` 仅适用于函数，所以请记住这一点。

```py
import json

# Load ID to Category mapping
with open('./youtube/US_category_id.json', 'r') as f:
    categories = json.load(f)

id_to_category = {}
for c in categories['items']:
    id_to_category[int(c['id'])] = c['snippet']['title']

# Pandas mapping
df_pd['category_id'] = df_pd['category_id'].map(id_to_category)

# Polars mapping
df_pl = df_pl.with_columns(pl.col("category_id").map_dict(id_to_category))
```

现在数据已准备好，让我们终于进行一些分析吧！

# 基本探索性数据分析

本节将涵盖执行 EDA 时一些最重要的技术，即单变量数据分析、汇总和可视化。

## 单变量数据分析

单变量数据分析是最简单的分析方法，但它却至关重要。一次查看一个变量，它可以让你更好地了解数据，并指导你进一步的探索。

## 类别列

由于我们在数据预处理阶段已将类别 ID 映射到实际名称，让我们使用 `.value_counts()` 查看它们的分布。

```py
# Polars value counts
category_counts = df_pl['category_id'].value_counts(sort=True).head()
print(category_counts)
```

```py
┌──────────────────┬────────┐
│ category_id      ┆ counts │
│ ---              ┆ ---    │
│ str              ┆ u32    │
╞══════════════════╪════════╡
│ Music            ┆ 13754  │
│ Entertainment    ┆ 9124   │
│ People & Blogs   ┆ 2926   │
│ Film & Animation ┆ 2577   │
│ Howto & Style    ┆ 1928   │
└──────────────────┴────────┘
```

`value_counts()` 操作在 Polars 中也存在，只需记得将 `sort=True` 设置为 Pandas 的相同行为。接下来，我们可以使用这些信息创建一个基本的条形图。在 Polars 中绘图相对简单，尽管目前没有像 Pandas 那样的内建绘图方法。一些绘图库不接受 Polars 系列作为输入，但幸运的是，将 Series 转换为常见的格式——Python 列表、NumPy 数组和 Pandas Series——相当简单。

```py
# Convert to Python list
list_data = df_pl"category_id"].to_list()
# Convert to NumPy
numpy_data = df_pl["category_id"].to_numpy()
# Convert to Pandas
pandas_data = df_pl["category_id"].to_pandas()
```

```py
# Barplot
sns.barplot(
    y=category_counts["category_id"].to_numpy(),
    x=category_counts["counts"].to_numpy(),
    color="#306e81",
)
plt.title("Category Counts in Trending Data")
plt.xlabel("Counts")
plt.show()
```

![](img/51e023bbe43f9df121990a088376f8ac.png)

图片由作者提供。

看起来音乐是 YouTube 趋势中最频繁的类别，其次是娱乐，这并不令人惊讶。另一方面，制作节目、非营利、旅游和汽车及车辆内容的人进入流行趋势的难度会更大。

## 数值列

数值特征的单变量分析可以使用 `.describe()` 方法，该方法的行为与 Pandas 非常相似。此外，我们还可以绘制对数视图的直方图。对数变换用于处理像 4.24 亿次观看的重度异常值。有趣的是，正在流行的影片的最少观看次数仅为 851。

```py
views_stats = df_pl.select(pl.col("views")).describe()
print(views_stats)

sns.histplot(df_pl['views'].log())
plt.title("Log Views Distribution in Trending")
plt.show()
```

![](img/14f4049400f866322afdb7dac420d298.png)![](img/0002d2dce0ea79fed6e6e0a756cf0493.png)

截图和图片由作者提供。

还有很多列需要探索，所以我建议你自己去探索，因为你现在拥有工具了。完成后，我们可以进入更复杂的分析形式。

## 多变量数据分析

首先，哪些频道在趋势页面上出现得最频繁？我们可以再次使用 `.value_counts()`，但让我们改用 `.groupby().agg()` 方法，因为它更灵活，并且对接下来的工作会很有用。我将按 *频道标题* 分组，并使用 `.count()` 方法计算行数。

```py
channel_popularity = (
    df_pl.groupby(pl.col("channel_title"))
    .agg(pl.count().alias("trending_count"))
    .sort(pl.col("trending_count"), descending=True)
)

print(channel_popularity.head())
```

```py
shape: (5, 2)
┌───────────────────────────────────┬────────────────┐
│ channel_title                     ┆ trending_count │
│ ---                               ┆ ---            │
│ str                               ┆ u32            │
╞═══════════════════════════════════╪════════════════╡
│ The Tonight Show Starring Jimmy … ┆ 208            │
│ Jimmy Kimmel Live                 ┆ 207            │
│ TheEllenShow                      ┆ 207            │
│ Saturday Night Live               ┆ 206            │
│ WWE                               ┆ 205            │
└───────────────────────────────────┴────────────────┘
```

与 Pandas 类似，在 `.groupby()` 中你需要指定要创建聚合的列名。注意对于 Polars，你需要用 `pl.col()` 包装列名，但由于实现了语法糖，它也可以在没有它的情况下工作。在 `.agg()` 内部，你通常需要提供要聚合的列名，但在这种情况下，我使用了 `pl.count()` 方法，因为我想计算行数。注意，你可以使用 `.alias()` 方法重新命名你创建的任何聚合/列。

让我们创建一些其他统计数据，即：

+   独特的流行视频数量

+   总浏览量、点赞数和评论数

+   平均浏览量、点赞数和评论数

```py
channel_stats_pl = df_pl.groupby("channel_title").agg(
    pl.count().alias("trending_count"), # number of occurences in the dataset
    pl.col("title").n_unique().alias("number_of_trending_videos"), # number of unique trending videos
    pl.col("views").sum().alias("total_views"), # total number of views
    pl.col("likes").sum().alias("total_likes"), # total number of likes
    pl.col("comment_count").sum().alias("total_comments"), # total number of comments
    pl.col("views").mean().alias("average_views"), # average number of views
    pl.col("likes").mean().alias("average_likes"), # average number of likes
    pl.col("comment_count").mean().alias("average_comments"), # average number of comments
)
print(channel_stats_pl.sample(5))
```

![](img/12f6c0e7de3d9bfe87f3a2638e522352.png)

图片由作者提供。

看起来一切都按预期工作。相同的聚合可以在 Pandas 中使用 `.agg()` 方法实现。

```py
channel_stats_pd = df_pd.groupby("channel_title").agg(
    trending_count=pd.NamedAgg(column="title", aggfunc="count"),
    number_of_trending_videos=pd.NamedAgg(column="title", aggfunc="nunique"),
    total_views=pd.NamedAgg(column="views", aggfunc="sum"),
    average_views=pd.NamedAgg(column="views", aggfunc="mean"),
    total_likes=pd.NamedAgg(column="likes", aggfunc="sum"),
    average_likes=pd.NamedAgg(column="likes", aggfunc="mean"),
    total_comments=pd.NamedAgg(column="comment_count", aggfunc="sum"),
    average_comments=pd.NamedAgg(column="comment_count", aggfunc="mean"),
)
```

结果应该是相同的，但在 Polars 中**执行时间快了 ~10 倍**。

这些聚合通常是分析的良好第一步，它们在后续可能会有用（例如在仪表盘中），因此将聚合代码重构为一个函数是很有必要的。

```py
def make_aggregates(df: pl.DataFrame, groupby: str, agg_features: list[str]) -> pl.DataFrame:
    # Aggregates that measure popularity using video counts
    popularity_aggs = [
        pl.count().alias("trending_count"),
        pl.col("title").n_unique().alias("number_of_trending_videos"),
    ]
    # Aggregates that measure popularity using metrics of the videos
    metrics_agg = []
    for agg in agg_features:
        if agg not in df.columns:
            print(f"{agg} not in the dataframe. Skipping...")
        else:
            metrics_agg.append(pl.col(agg).sum().alias(f"total_{agg}"))
            metrics_agg.append(pl.col(agg).mean().alias(f"average_{agg}"))

    stats = df.groupby(groupby).agg(popularity_aggs + metrics_agg)
    stats = stats.sort("trending_count", descending=True)
    return stats
```

上面你可以看到这个聚合函数的样子。注意我们可以在将所需的聚合操作传递给 Polars `.agg()` 方法之前，将它们存储在一个列表中，这个方法非常强大。现在，这个函数不仅可以应用于 `channel_title` 列，还可以应用于例如 `category_id`。

```py
channel_aggs = make_aggregates(
    df = df_pl, 
    groupby = "channel_title", 
    agg_features = ["views", "likes", "comment_count"]
)
category_aggs = make_aggregates(
    df = df_pl, 
    groupby = "category_id", 
    agg_features = ["views", "likes", "comment_count"]
)

print("Top Channels")
print(channel_aggs.head())

print("\nTop Categories")
print(category_aggs.head())
```

![](img/6f7e6a37ff50a5afd03a4b6dfb825c8b.png)

图片由作者提供。

从上面的截图中，你可以看到顶级流行频道和顶级流行类别。这些聚合可以进一步放入仪表盘或用于进一步分析。

# 写入数据

将 DataFrames 保存到磁盘实际上非常简单。你只需要确保你写入的文件夹存在。否则，过程与 Pandas 非常相似，只是你使用 `.write_parquet()` 方法而不是 `.to_parquet()`（我每天至少犯一次这个错误）。

```py
channel_aggs.write_parquet("./data/channel_aggs.parquet")
category_aggs.write_parquet("./data/category_aggs.parquet")
```

# 结论

做得好，能做到这一步！总的来说，你已经了解了如何在 Polars 中完成以下操作：

+   读取数据

+   调查 DataFrames

+   执行基本的数据质量检查

+   选择所需的列/行

+   执行基本清理

+   执行基本的单变量分析

+   执行基本的多变量分析

+   将 DataFrames 写入 Parquet 文件

这是一个很好的开始，所以让我们结束本系列的第一部分。确保在你自己的数据集上进行练习，因为我坚信最好的学习方式就是通过实践、实践、再实践。感谢阅读，我们在下一部分见！

## 下一步是什么？

如果你已经掌握了 Polars 的基础知识，是时候进入更高级的内容了。本系列的第二部分涵盖了更复杂的聚合和分析函数，这些对于任何数据专业人士都是必不可少的。

[](/eda-with-polars-step-by-step-guide-to-aggregate-and-analytic-functions-part-2-a22d986315aa?source=post_page-----b2ec500a1008--------------------------------) ## 使用 Polars 进行 EDA：聚合和分析函数的逐步指南（第二部分）

### 使用 Polars 以闪电般的速度进行高级聚合和滚动平均

towardsdatascience.com

## 还不是 Medium 会员？

[](https://medium.com/@antonsruberts/membership?source=post_page-----b2ec500a1008--------------------------------) [## 使用我的推荐链接加入 Medium — Antons Tocilins-Ruberts

### 阅读 Antons Tocilins-Ruberts（以及 Medium 上其他成千上万位作者）的每一个故事。你的会员费直接…

medium.com](https://medium.com/@antonsruberts/membership?source=post_page-----b2ec500a1008--------------------------------)
