# 使用 Polars 进行探索性数据分析：聚合和分析函数的逐步指南（第二部分）

> 原文：[`towardsdatascience.com/eda-with-polars-step-by-step-guide-to-aggregate-and-analytic-functions-part-2-a22d986315aa`](https://towardsdatascience.com/eda-with-polars-step-by-step-guide-to-aggregate-and-analytic-functions-part-2-a22d986315aa)

## 使用 Polars 实现闪电般速度的高级聚合和滚动平均

[](https://medium.com/@antonsruberts?source=post_page-----a22d986315aa--------------------------------)![Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----a22d986315aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a22d986315aa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a22d986315aa--------------------------------) [Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----a22d986315aa--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a22d986315aa--------------------------------) ·9 分钟阅读·2023 年 7 月 10 日

--

![](img/e244e107d7b8e9c628cb4f7312d675a8.png)

由 [Spencer Davis](https://unsplash.com/@spencerdavis?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

在 这一系列的第一部分 中，我们介绍了 Polars 的基础知识，并将其功能和语法与 Pandas 进行了比较。本部分将进一步增加查询的复杂性，因此我们将学习如何执行一些相当复杂的聚合、滚动统计等。如果你对 Polars 不熟悉或需要复习，确保查看 上一部分。否则，我们继续探索 Polars！

# 设置

与上一部分一样，确保克隆/拉取这个 [GitHub 仓库](https://github.com/aruberts/tutorials/tree/main/polars)，因为它包含了你需要的所有代码。特别是，我们将讨论这个 [笔记本](https://github.com/aruberts/tutorials/blob/main/polars/time_analysis.ipynb)，所以如果你想跟随，请务必获取它。

本项目使用的数据可以从 [Kaggle](https://www.kaggle.com/datasets/datasnaek/youtube-new?resource=download&sort=published) 下载（CC0: 公共领域）。我假设你已经安装了 Polars，所以只需确保使用 `pip install -U polars` 将其更新到最新版本即可。

# 数据处理

## 读取数据

类似于上一篇文章，我们来读取 UK trending 数据集和 `category_id` 列的映射。

```py
csv_path  = './youtube/GBvideos.csv'
json_path = './youtube/US_category_id.json'

df = pl.read_csv(csv_path)

with open(json_path, 'r') as f:
    categories = json.load(f)

id_to_category = {}
for c in categories['items']:
    id_to_category[int(c['id'])] = c['snippet']['title']
```

## 清理数据

接下来，让我们解析日期并将类别 ID 映射到类别名称。为了使其更具生产就绪性，我打算将日期解析代码放入一个通用函数中。

```py
def parse_dates(df: pl.DataFrame, date_cols: Dict[str, str]) -> pl.DataFrame:
    expressions = []
    for date_col, format in date_cols.items():
        expressions.append(pl.col(date_col).str.to_date(format=format))

    df = df.with_columns(expressions)
    return df

# Column name with expected date format
date_column_format = {
    "trending_date": '%y.%d.%m',
    "publish_time": '%Y-%m-%dT%H:%M:%S%.fZ'
}

df = parse_dates(df, date_column_format).with_columns(
    pl.col("category_id").map_dict(id_to_category)
)
```

注意，因为函数返回的是一个 DataFrame，我们可以链式调用类别映射来创建一个优雅的（几乎是）单行清理代码。

## 特征工程

通常，你需要创建源自现有特征的新特征。例如，在这个 YouTube 数据集中，我们可以计算一个视频进入“趋势”标签所需的天数，因为我们有 `publish_time` 和 `trending_date`。此外，我们还可以计算点赞、点踩、评论和观看次数的不同比率。

```py
df = df.with_columns(
    time_to_trending=pl.col("trending_date") - pl.col("publish_time").dt.date(),
    likes_to_dislike_ratio=pl.col("likes") / pl.col("dislikes"),
    likes_to_views_ratio=pl.col("likes") / pl.col("views"),
    comments_to_views_ratio=pl.col("comment_count") / pl.col("views"),
)

# Sense check 2 features
print(df[["trending_date", "publish_time", "time_to_trending"]].sample(2))
print(df[["likes", "dislikes", "likes_to_dislike_ratio"]].sample(2))
```

![](img/adb8c51f894a80776296759e7a0ed5dc.png)![](img/29b4b059a1d3e4767894ad9160b04141.png)

派生特征的有效性检查。作者截图。

此外，我们可以从 `trending_date` 列中提取年份、月份和星期几，这些可以用于进一步分析。

```py
df = df.with_columns(
    trending_weekday = pl.col('trending_date').dt.weekday(),
    trending_month = pl.col('trending_date').dt.month(),
    trending_year = pl.col("trending_date").dt.year()
)
```

最终，数据集已预处理完毕，准备进行进一步分析。让我们开始更深入地探索进入趋势所需的时间。

# 进入趋势时间分析

由于一个视频可能多次出现在趋势数据集中（即在不同日期），我们需要首先创建一个每行一个视频的数据框，并标明其进入趋势所需的正确时间。例如，考虑这段出现在数据集中 3 次以上的视频。

![](img/6fdc7e1f966aed4a058d5190232ea85b.png)

作者截图。

实际进入趋势所需的时间是 1 天，然后它在趋势中停留了几天。我们可以使用 `.groupby()` 方法按 `video_id` 分组，并获取一些有用的值：

+   进入趋势所需的时间（`time_to_trending` 的最小值）

+   在“趋势”标签中的停留时间（`trending_date` 的最大值减去最小值）

此外，我们还希望引入有关视频的其他信息，例如 `channel_title` 或 `title` 以进行进一步分析。为此，我们可以使用 `.groupby()` 方法和多个 `by` 参数。

```py
time_to_trending_df = df.groupby(
    ["video_id", "title", "category_id", "channel_title"]
).agg(
    pl.col("time_to_trending").min().dt.days(),
    pl.col("trending_date").min().dt.date().alias("first_day_in_trending"),
    pl.col("trending_date").max().dt.date().alias("last_day_in_trending"),
    (pl.col("trending_date").max() - pl.col("trending_date").min()).dt.days()
    .alias("days_in_trending"),
)

print(f"Average time to trending is {time_to_trending_df['time_to_trending'].mean()} days")
print(f"Median time to trending is {time_to_trending_df['time_to_trending'].median()} days")

>>> Average time to trending is 36.25735294117647 days
>>> Median time to trending is 2.0 days
```

有趣的是，进入趋势的平均时间远大于中位数，表明数据中存在较大的异常值。为了不对进一步分析产生偏差，让我们筛选出所有进入趋势时间超过 60 天的视频。

```py
time_to_trending_df = time_to_trending_df.filter(pl.col("time_to_trending") <= 60)
print(f"Average time to trending is {time_to_trending_df['time_to_trending'].mean()} days")
print(f"Median time to trending is {time_to_trending_df['time_to_trending'].median()} days")

>>> Average time to trending is 3.7225 days
>>> Median time to trending is 2.0 days
```

注意编写和堆叠相当复杂的查询是多么简单！

## 最快进入趋势的类别和频道

首先，让我们仅对出现过 100 次以上的类别进行分析，因为统计不频繁的类别的可靠性较差。有很多方法可以过滤这些类别（例如，使用 `.value_counts()`），但这次我们使用 `pl.count().over()` 来引入一种新的表达方式。使用这种方法，我们可以计算 `category_id` 的行数，这应该会向 `time_to_trending_df` 添加一个新列 `times_in_trending`。你会发现这个语法与 PySpark 和 SQL 非常相似，因此大多数数据专业人员应该很熟悉。

```py
fastest_category_to_trending = (
    time_to_trending_df.with_columns(
        # Count over category ID
        times_in_trending=pl.count().over("category_id")
    # Filter infrequent categories
    ).filter(pl.col("category_times_in_trending") >= 100)
    # Calculate mean time to trending
    .groupby("category_id")
    .agg(pl.col("time_to_trending").mean())
    .sort("time_to_trending")
)
```

在统计每个类别的出现次数之后，过滤不频繁的类别并按组计算平均趋势时间非常简单。虽然这能完成工作，但我们还需要复制粘贴这个查询到 `channel_title` 层级分析中，这样不太优雅。相反，让我们创建一个带有几个可用参数的函数，使其更加通用。

```py
def avg_frequent(
    df: pl.DataFrame,
    by: str,
    frequency_threshold: int,
    metric: str = "time_to_trending",
) -> pl.DataFrame:
    results = (
        df.with_columns(times_in_trending=pl.count().over(by))
        .filter(pl.col("times_in_trending") >= frequency_threshold)
        .groupby(by)
        .agg(pl.col(metric).mean())
        .sort(metric)
    )

    return results

fastest_category_to_trending = avg_frequent(
    time_to_trending_df, by="category_id", frequency_threshold=100
).head(3)
fastest_channel_to_trending = avg_frequent(
    time_to_trending_df, by="channel_title", frequency_threshold=10
).head(3)

print(fastest_category_to_trending)
print(fastest_channel_to_trending) 
```

![](img/5c88df7cb92585510e78e4e91fced00e.png)![](img/0e9be23584907792a6693f2ca429fb7c.png)

进入趋势的最快类别和频道。作者截屏。

有趣但有点意料之中的结果——深夜节目绝对主导了 YouTube 的趋势标签（至少在 2018 年）。吉米·法伦获得了进入趋势的快速通道，其次是 SNL 和《艾伦秀》。

## 保持在趋势中的类别和频道

除了进入趋势标签，谁能在趋势中停留时间最长也很重要。这项分析类似于上一个分析——我们想要按组（例如类别）平均某个指标（趋势中的天数），但只针对频繁的组值（例如出现超过 10 次）。因此，让我们重复使用之前创建的那个很棒的函数。

```py
longest_trending_categories = avg_frequent(
    time_to_trending_df,
    by="category_id",
    frequency_threshold=100,
    metric="days_in_trending",
).tail(3)  # tails because it's sorted in descending

longest_trending_channels = avg_frequent(
    time_to_trending_df,
    by="channel_title",
    frequency_threshold=10,
    metric="days_in_trending",
).tail(3)

print(longest_trending_categories)
print(longest_trending_channels)
```

![](img/a691b6097b058c5936006e3711f228dd.png)![](img/e8f660c5047b58fb9f4cbebe111273c5.png)

有趣的是，各类别之间没有重叠。因此，即使音乐视频进入趋势可能需要一些时间，但它更有可能在趋势中停留更长时间。电影预告片和其他娱乐内容也是如此。

# 随时间推移的趋势类别

所以我们知道，现场喜剧节目进入趋势的速度最快，而音乐和娱乐视频在趋势中停留的时间最长。但情况一直都是这样吗？要回答这个问题，我们需要创建一些滚动汇总。让我们在这一部分回答三个主要问题：

+   每个月每个类别的总趋势视频数量是多少？

+   每个月每个类别的新视频数量是多少？

+   从视图数量来看，各类别的比较如何？

## 每个类别每月的总趋势视频数量

首先，让我们查看每月每个类别的视频总数。为了获取这个统计数据，我们需要使用`.groupby_dynamic()`方法，它允许我们按日期列（指定为`index_column`）和其他任意列（指定为`by`参数）进行分组。分组频率由`every`参数控制。

```py
trending_monthly_stats = df.groupby_dynamic(
    index_column="trending_date",  # date column
    every="1mo",  # can also me 1w, 1d, 1h etc
    closed="both",  # including starting and end date
    by="category_id", # other grouping columns
    include_boundaries=True,  # showcase the boudanries
).agg(
    pl.col("video_id").n_unique().alias("videos_number"),
)

print(trending_monthly_stats.sample(3))
```

![](img/4ea1c6f02f8a6b52416bfb5f93818697.png)

结果重采样数据框。由作者截屏。

你可以在上面看到结果数据框。Polars 的一个很好的特点是我们可以输出边界来检查结果。现在，让我们进行一些绘图以可视化模式。

```py
plotting_df = trending_monthly_stats.filter(pl.col("category_id").is_in(top_categories))

sns.lineplot(
    x=plotting_df["trending_date"],
    y=plotting_df["videos_number"],
    hue=plotting_df["category_id"],
    style=plotting_df["category_id"],
    markers=True,
    dashes=False,
    palette='Set2'
)

plt.title("Total Number of Videos in Trending per Category per Month")
```

![](img/0af2d888e3dbd939bd89f0b8ca136375.png)

视频数量图。由作者生成。

从这个图表中，我们可以看到音乐从 2018 年开始在趋势中占据了最大的份额。这可能表明 YouTube 在战略上有所转变，成为音乐视频的首选平台。娱乐似乎与人物与博客和如何与风格类别一起逐渐下降。

## 每个类别的新月度趋势视频数量

查询是完全一样的，只是现在我们需要提供`index_column`为视频进入趋势的日期。最好在这里创建一个函数，但我将这个作为一个有好奇心的读者的练习。

```py
trending_monthly_stats_unique = (
    time_to_trending_df.sort("first_day_in_trending")
    .groupby_dynamic(
        index_column="first_day_in_trending",
        every="1mo",
        by="category_id",
        include_boundaries=True,
    )
    .agg(pl.col("video_id").n_unique().alias("videos_number"))
)

plotting_df = trending_monthly_stats_unique.filter(pl.col("category_id").is_in(top_categories))
sns.lineplot(
    x=plotting_df["first_day_in_trending"],
    y=plotting_df["videos_number"],
    hue=plotting_df["category_id"],
    style=plotting_df["category_id"],
    markers=True,
    dashes=False,
    palette='Set2'
)

plt.title(" Number of New Trending Videos per Category per Month")
```

![](img/2a87a51ab4e4947c77b2c5fa27aba588.png)

新视频数量图。由作者生成。

在这里我们得到了一些有趣的见解——娱乐和音乐的新视频数量在整个时间段内大致相等。由于音乐视频在趋势标签中停留的时间更长，它们在趋势计数中被过度代表，但当这些视频去重后，这种模式消失了。

## 每个类别的平均观看数

作为此分析的最后一步，让我们比较两个最受欢迎的类别（音乐和娱乐）在不同时间段的观看情况。为了进行这个分析，我们将使用 7 天滚动平均统计来可视化趋势。为了计算这个滚动统计，Polars 提供了一个方便的方法叫做`.groupby_rolling()`。不过，在应用之前，让我们先按`category_id`和`trending_date`汇总所有观看数据，然后相应地排序数据框。这种格式是正确计算滚动统计所必需的。

```py
views_per_category_date = (
    df.groupby(["category_id", "trending_date"])
    .agg(pl.col("views").sum())
    .sort(["category_id", "trending_date"])
)
```

一旦数据框准备好，我们可以使用`.groupby_rolling()`方法，通过在 period 参数中指定`1w`来创建滚动平均统计，并在`.agg()`方法中创建一个平均表达式。

```py
# Calculate rolling average
views_per_category_date_rolling = views_per_category_date.groupby_rolling(
    index_column="trending_date",  # Date column
    by="category_id",  # Grouping column
    period="1w"  # Rolling length
).agg(
    pl.col("views").mean().alias("rolling_weekly_average")
)

# Plotting
plotting_df = views_per_category_date_rolling.filter(pl.col("category_id").is_in(['Music', 'Entertainment']))
sns.lineplot(
    x=plotting_df["trending_date"],
    y=plotting_df["rolling_weekly_average"],
    hue=plotting_df["category_id"],
    style=plotting_df["category_id"],
    markers=True,
    dashes=False,
    palette='Set2'
)

plt.title("7-day Views Average")
```

![](img/641c0cb89292c17def0cef4be44c1d12.png)

图表由作者生成。

根据 7 天滚动平均观看数，音乐完全主导了趋势标签，从 2018 年 2 月开始，这两个类别之间的差距大幅增加。

# 结论

在完成这篇文章并跟随代码后，你应该对 Polars 中的高级汇总和分析函数有更好的理解。特别是，我们已经覆盖了：

+   使用`pl.datetime`的基础知识

+   使用`.groupby()`进行多参数聚合

+   使用`.over()`在特定组中创建汇总

+   使用`.groupby_dynamic()`在时间窗口中生成汇总

+   使用`.groupby_rolling()`在一段时间内生成滚动汇总

掌握这些知识后，你应该能够以闪电般的速度完成几乎所有分析任务。

你可能会觉得一些分析看起来很临时，你的感觉是对的。下一部分将专门讨论这个话题——如何构建和创建数据处理管道。敬请期待！

## 还不是 Medium 会员？

[](https://medium.com/@antonsruberts/membership?source=post_page-----a22d986315aa--------------------------------) [## 使用我的推荐链接加入 Medium — Antons Tocilins-Ruberts

### 阅读 Antons Tocilins-Ruberts（以及 Medium 上成千上万其他作者）的每个故事。你的会员费用直接…

medium.com](https://medium.com/@antonsruberts/membership?source=post_page-----a22d986315aa--------------------------------)
