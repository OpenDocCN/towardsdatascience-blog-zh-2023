# 使用 Polars 构建的数据管道：逐步指南

> 原文：[`towardsdatascience.com/data-pipelines-with-polars-step-by-step-guide-f5474accacc4`](https://towardsdatascience.com/data-pipelines-with-polars-step-by-step-guide-f5474accacc4)

## 使用 Polars 构建可扩展且快速的数据管道

[](https://medium.com/@antonsruberts?source=post_page-----f5474accacc4--------------------------------)![Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----f5474accacc4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f5474accacc4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f5474accacc4--------------------------------) [Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----f5474accacc4--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f5474accacc4--------------------------------) ·阅读时间 14 分钟·2023 年 7 月 24 日

--

![](img/bfaae191cbbf33d6ba469a6bffdb83bf.png)

图片由 [Filippo Vicini](https://unsplash.com/@filippo_vicini?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

本文的目的是解释并展示如何使用 Polars 构建数据管道。它整合并使用了你从本系列前两部分获得的所有知识，因此如果你还没有阅读这些内容，我强烈建议你先去阅读，然后再回来这里。

[](/eda-with-polars-step-by-step-guide-for-pandas-users-part-1-b2ec500a1008?source=post_page-----f5474accacc4--------------------------------) ## 使用 Polars 进行 EDA：Pandas 用户的逐步指南（第一部分）

### 提升你在 Polars 上的数据分析技能

towardsdatascience.com [](/eda-with-polars-step-by-step-guide-to-aggregate-and-analytic-functions-part-2-a22d986315aa?source=post_page-----f5474accacc4--------------------------------) ## 使用 Polars 进行 EDA：汇总和分析函数的逐步指南（第二部分）

### 使用 Polars 以闪电般的速度进行高级聚合和滚动平均

towardsdatascience.com

# 设置

你可以在这个 [repository](https://github.com/aruberts/tutorials/tree/main/polars) 中找到所有的代码，所以不要忘记克隆/拉取并给它加星。特别是，我们将探索这个 [file](https://github.com/aruberts/tutorials/blob/main/polars/data_preparation_pipeline.py)，这意味着我们最终将从笔记本走向实际应用！

本项目中使用的数据可以从 [Kaggle](https://www.kaggle.com/datasets/datasnaek/youtube-new?resource=download&sort=published) 下载（CC0: Public Domain）。这与前两部分中使用的 YouTube 趋势数据集相同。我假设你已经安装了 Polars，因此只需确保通过 `pip install -U polars` 更新到最新版本。

# 数据管道

简单来说，数据管道是一个自动化的步骤序列，它从一个或多个位置提取数据，应用处理步骤，并将处理后的数据保存到其他地方，使其可以用于进一步的使用。

## Polars 中的管道

Polars 处理数据的方式非常适合构建可扩展的数据管道。首先，我们可以如此轻松地链式调用方法，这使得一些相当复杂的管道可以优雅地编写。

例如，假设我们想找出 ***2018 年每个月中哪个趋势视频的观看次数最多***。下面你可以看到一个完整的管道，用于计算这个指标并将其保存为 parquet 文件。

```py
import polars as pl
csv_path = "./youtube/GBvideos.csv"

pl.read_csv(csv_path).with_columns(
    # Original date is in string format like 17.01.01
    pl.col("trending_date").str.to_date(format="%y.%d.%m")
).filter(pl.col("trending_date").dt.year() == 2018).with_columns(
    pl.col("views")
    .rank(descending=True)
    # Rank is calculated over a month 
    .over(pl.col("trending_date").dt.month())
    .alias("monthly_rank")
).filter(
    pl.col("monthly_rank") == 1
).select(
    pl.col("trending_date"), pl.col("title"), pl.col("channel_title"), pl.col("views")
).write_parquet(
    "top_monthly_videos.parquet"
)
```

相当不错，对吧？如果你了解 SQL，这很容易阅读和理解。但我们可以做得更好吗？当然可以，使用 Polars 的 `.pipe()` 方法。这种方法为我们提供了一种有结构的方式来将顺序函数应用于 DataFrame。为了使其有效，让我们将上述代码重构成函数。

```py
def process_date(df, date_column, format):
    result = df.with_columns(pl.col(date_column).str.to_date(format))
    return result

def filter_year(df, date_column, year):
    result = df.filter(pl.col(date_column).dt.year() == year)
    return result

def get_first_by_month(df, date_column, metric):
    result = df.with_columns(
        pl.col(metric)
        .rank(method="ordinal", descending=True)
        .over(pl.col(date_column).dt.month())
        .alias("rank")
    ).filter(pl.col("rank") == 1)

    return result

def select_data_to_write(df, columns):
    result = df.select([pl.col(c) for c in columns])
    return result
```

注意这些函数接受一个 Polars DataFrame 作为输入（以及一些其他参数），并输出已经修改的 Polars DataFrame。通过 `.pipe()` 方法将这些方法链在一起是一件轻而易举的事。

```py
(
    pl.read_csv(csv_path)
    .pipe(process_date, date_column="trending_date", format="%y.%d.%m")
    .pipe(filter_year, date_column="trending_date", year=2018)
    .pipe(get_first_by_month, date_column="trending_date", metric="views")
    .pipe(
        select_data_to_write,
        columns=["trending_date", "title", "channel_title", "views"],
    )
).write_parquet("top_monthly_videos.parquet")
```

首先，重格式化后的代码更容易理解。其次，关注点分离通常是一个很好的编程原则，因为它可以更容易地调试和保持代码整洁。对于这个简单的示例，将管道模块化可能有些过于复杂，但你会看到它在下一个部分的更大示例中的有用之处。现在，让我们使用 **懒模式** 来加快整体速度。

懒模式允许我们编写查询和管道，将它们全部组合在一起，然后让后台引擎进行优化。例如，上述代码显然不是最优的。我把列选择作为最后一步，这意味着处理的数据量不必要地大。幸运的是，Polars 足够聪明，能够识别这一点，因此它会优化代码。此外，我们只需在代码中做两个小的更改即可获得速度提升，这点非常不可思议。首先，我们将 `pl.read_csv` 更改为 `pl.scan_csv` 以在懒模式下读取数据。然后，在查询的末尾添加 `.collect()`，以告知 Polars 我们希望执行优化后的查询。

```py
(
    pl.scan_csv(csv_path).pipe(process_date, date_column="trending_date", format="%y.%d.%m")
    .pipe(filter_year, date_column="trending_date", year=2018)
    .pipe(get_first_by_month, date_column="trending_date", metric="views")
    .pipe(
        select_data_to_write,
        columns=["trending_date", "title", "channel_title", "views"],
    )
).collect().write_parquet("top_monthly_videos.parquet")
```

在我的机器上，我得到了约 3 倍的速度提升，考虑到我们仅做了两个非常简单的编辑，这点非常令人印象深刻。现在你已经掌握了管道和延迟评估的概念，让我们进入一个更复杂的示例。

# 机器学习特征的数据管道

> 警告：有很多文本和代码！各部分应该按顺序跟随，因为它们构建了管道。

根据我们手头的数据集（YouTube Trending Videos），我们来构建用于预测**视频在流行中持续时间的特征**。虽然这听起来很简单，但创建这些特征的过程将会相当复杂。数据集的最终格式应该是每个视频 ID 一行，视频进入流行时可用的特征，以及视频在流行中停留的实际天数（目标）。

![](img/6bf7ae837c8dac8debee9bcf95439939.png)

模拟最终数据集格式。作者创建。

在我们的预测任务中可能有用的特征包括：

+   视频特征（例如，类别）

+   进入流行时的观看次数、点赞数、评论数等

+   频道在流行中的过去表现（例如，过去 7 天的流行视频数量）

+   一般流行特征（例如，过去 30 天所有视频的平均流行时间）

下面你可以看到创建该数据集所需的管道的图示表示（确保放大查看）。

![](img/afa2b618abfc31e1c1b02b0c5ba2c9f9.png)

数据管道流程。作者创建。

我知道这信息量很大，所以我们一口一口地消化它。下面你可以找到每个管道步骤的描述和代码。此外，这个管道将使用 YAML 配置文件进行参数化，所以你也会找到每个步骤的配置参数。这是如何读取名为 `pipe_config.yaml` 的 YAML 文件的示例，你可以在仓库中找到。

```py
import yaml

# Read config
with open("pipe_config.yaml", "r") as file:
    pipe_config = yaml.safe_load(file)
```

因此，对于管道的每一步，你会发现：

+   步骤描述

+   相关功能

+   相关配置参数

+   运行到此步骤的管道代码

这样，我们将逐步建立完整的管道，你将对发生的事情有深入了解，并学会如何为自己的数据创建类似的内容。

## 读取数据

这一步的目标不言自明——读取数据集以进行进一步处理。我们有两个输入——一个包含主要数据的 csv 文件和一个包含类别映射数据的 json 文件。此步骤的参数如下：

```py
data_path: "./youtube/GBvideos.csv"
category_map_path: "./youtube/GB_category_id.json"
```

不需要编写读取 csv 数据的函数（因为在 Polars 中已经存在），所以我们只编写了读取类别映射的函数。

```py
def read_category_mappings(path: str) -> Dict[int, str]:
    with open(path, "r") as f:
        categories = json.load(f)

    id_to_category = {}
    for c in categories["items"]:
        id_to_category[int(c["id"])] = c["snippet"]["title"]

    return id_to_category
```

使用此函数，读取所需文件的代码非常简单。

```py
# Create mapping
id_to_category = read_category_mappings(pipe_config["category_map_path"])
col_mappings = {"category_id": id_to_category}

# Pipeline
output_data = pl.scan_csv(pipe_config["data_path"]).collect()
```

现在，让我们进入一个非常不令人兴奋但至关重要的步骤——数据清理。

## 数据清理

这个数据集已经相当干净，但我们需要对日期和类别列做一些额外的预处理。

+   `trending_date` 和 `publish_time` 需要格式化为 `pl.datetime`

+   `category_id` 需要从 ID 映射到实际的类别名称

Polars 需要知道日期将以何种格式提供，因此最好在 `pipe_config.yaml` 文件中对数据格式进行编码，并在相应的日期列中进行说明，以使其清晰且易于更改。

```py
# Pre-processing config
date_column_format:
  trending_date: "%y.%d.%m"
  publish_time: "%Y-%m-%dT%H:%M:%S%.fZ"
```

由于我们希望使用 Polars 管道来模块化代码，我们需要创建两个函数——`parse_dates` 和 `map_dict_columns`，它们将执行两个所需的转换。然而，有个问题——将这些操作分成两个步骤会使代码变得更慢，因为 Polars 无法有效地使用并行化。你可以通过计时这两个 Polars 表达式的执行来自己测试一下。

```py
slow = df.with_columns(
    # Process dates
    pl.col("trending_date").str.to_date("%y.%d.%m"),
    pl.col("publish_time").str.to_date("%Y-%m-%dT%H:%M:%S%.fZ"),
).with_columns(
    # Then process category
    pl.col("category_id").map_dict(id_to_category)
)

fast = df.with_columns(
    # Process all together
    pl.col("trending_date").str.to_date("%y.%d.%m"),
    pl.col("publish_time").str.to_date("%Y-%m-%dT%H:%M:%S%.fZ"),
    pl.col("category_id").map_dict(id_to_category)
)
```

对我来说，第一个表达式慢了 ~2 倍，这非常显著。那么我们该怎么办呢？好吧，这里有个秘密：

> 我们应该在将表达式传递给 `.with_columns` 方法之前构建它们。

因此，函数 `parse_dates` 和 `map_dict_columns` 应该返回表达式列表，而不是转换后的数据帧。这些表达式可以在最终清理函数中组合并应用，我们将称之为 `clean_data`。

```py
def parse_dates(date_cols: Dict[str, str]) -> List[pl.Expr]:
    expressions = []
    for date_col, fmt in date_cols.items():
        expressions.append(pl.col(date_col).str.to_date(format=fmt))

    return expressions

def map_dict_columns(
    mapping_cols: Dict[str, Dict[str | int, str | int]]
) -> List[pl.Expr]:
    expressions = []
    for col, mapping in mapping_cols.items():
        expressions.append(pl.col(col).map_dict(mapping))
    return expressions

def clean_data(
    df: pl.DataFrame,
    date_cols_config: Dict[str, str],
    mapping_cols_config: Dict[str, Dict[str | int, str | int]],
) -> pl.DataFrame:
    parse_dates_expressions = parse_dates(date_cols=date_cols_config)
    mapping_expressions = map_dict_columns(mapping_cols_config)

    df = df.with_columns(parse_dates_expressions + mapping_expressions)
    return df
```

如你所见，我们现在只有一个 `.with_columns` 操作，这使得代码更优化。请注意，所有函数的参数都作为字典提供。这是因为 YAML 被读取为字典。现在，让我们将清理步骤添加到管道中。

```py
# Create mapping
id_to_category = read_category_mappings(pipe_config["category_map_path"])
col_mappings = {"category_id": id_to_category}

# Read in configs
date_column_format = pipe_config["date_column_format"]

# Pipeline
output_data = pl.scan_csv(pipe_config["data_path"]).pipe(
    clean_data, date_column_format, col_mappings
).collect()
```

干净、模块化、快速——还有什么不喜欢的？让我们继续下一步。

## 基础特征工程

这一步在清理的数据上做一些基本的特征工程，即：

+   计算比率特征——点赞与踩的比率、点赞与观看的比率以及评论与观看的比率

+   计算发布和趋势之间的天数差异

+   从 `trending_date` 列中提取工作日

让我们在配置文件中对这些特征的计算进行参数化。我们要指定一个要创建的特征的名称以及数据集中用于计算的相应列。

```py
# Feature engineering config
ratio_features:
  # feature name
  likes_to_dislikes: 
    # features used in calculation 
    - likes           
    - dislikes
  likes_to_views:
    - likes
    - views
  comments_to_views:
    - comment_count
    - views

difference_features:
  days_to_trending:
    - trending_date
    - publish_time

date_features:
  trending_date:
    - weekday
```

函数的逻辑仍然是一样的——构建表达式并将其传递给 `.with_columns` 方法。因此，函数 `ratio_features`、`diff_features` 和 `date_features` 都在名为 `basic_feature_engineering` 的主函数中调用。

```py
def ratio_features(features_config: Dict[str, List[str]]) -> List[pl.Expr]:
    expressions = []
    for name, cols in features_config.items():
        expressions.append((pl.col(cols[0]) / pl.col(cols[1])).alias(name))

    return expressions

def diff_features(features_config: Dict[str, List[str]]) -> List[pl.Expr]:
    expressions = []
    for name, cols in features_config.items():
        expressions.append((pl.col(cols[0]) - pl.col(cols[1])).alias(name))

    return expressions

def date_features(features_config: Dict[str, List[str]]) -> List[pl.Expr]:
    expressions = []
    for col, features in features_config.items():
        if "weekday" in features:
            expressions.append(pl.col(col).dt.weekday().alias(f"{col}_weekday"))
        if "month" in features:
            expressions.append(pl.col(col).dt.month().alias(f"{col}_month"))
        if "year" in features:
            expressions.append(pl.col(col).dt.year().alias(f"{col}_year"))

    return expressions

def basic_feature_engineering(
    data: pl.DataFrame,
    ratios_config: Dict[str, List[str]],
    diffs_config: Dict[str, List[str]],
    dates_config: Dict[str, List[str]],
) -> pl.DataFrame:
    ratio_expressions = ratio_features(ratios_config)
    date_diff_expressions = diff_features(diffs_config)
    date_expressions = date_features(dates_config)

    data = data.with_columns(
        ratio_expressions + date_diff_expressions + date_expressions
    )
    return data
```

类似于前一步，我们只需将主函数传递给 `pipe` 并将所有所需的配置作为参数提供。

```py
# Create mapping
id_to_category = read_category_mappings(pipe_config["category_map_path"])
col_mappings = {"category_id": id_to_category}

# Read in configs
date_column_format = pipe_config["date_column_format"]
ratios_config = pipe_config["ratio_features"]
diffs_config = pipe_config["difference_features"]
dates_config = pipe_config["date_features"]

# Pipeline
output_data = (
    pl.scan_csv(pipe_config["data_path"])
    .pipe(clean_data, date_column_format, col_mappings)
    .pipe(basic_feature_engineering, ratios_config, diffs_config, dates_config)
).collect()
```

很好，我们已经完成了一半的管道！现在，让我们将数据集转换为正确的格式，并最终计算我们的目标——趋势中的天数。

## 数据转换

提醒一下，原始数据集中每个视频有多个条目，因为它详细记录了每一天的趋势。如果一个视频在趋势中停留了五天，这个视频在数据集中会出现五次。我们的目标是得到一个每个视频只有一个条目的数据集（请参见下图）。

![](img/ce3629ab065fa04ef5204cfe10f3af84.png)

数据转换步骤示例。作者创建。

我们可以通过 `.groupby` 和 `.agg` 方法的组合来实现这一点。这里唯一需要配置的参数是过滤那些由于视频花费太长时间才进入流行榜的视频，因为这些视频是前一部分中识别出的离群点。在得到包含 `video_ids` 和相应目标（流行天数）的表后，我们还需要记得从原始数据集中联接特征，因为这些特征不会在 `groupby` 操作中传递。因此，我们还需要指定要联接的特征和作为联接键的列。

```py
# Filter videos
max_time_to_trending: 60

# Features to join to the transformed data
base_columns:
  - views
  - likes
  - dislikes
  - comment_count
  - comments_disabled
  - ratings_disabled
  - video_error_or_removed
  - likes_to_dislikes
  - likes_to_views
  - comments_to_views
  - trending_date_weekday
  - channel_title
  - tags
  - description
  - category_id

# Use these columns to join transformed data with original
join_columns:
  - video_id
  - trending_date
```

为了执行所需的步骤，我们将设计两个函数——`join_original_features` 和 `create_target_df`。

```py
def join_original_features(
    main: pl.DataFrame,
    original: pl.DataFrame,
    main_join_cols: List[str],
    original_join_cols: List[str],
    other_cols: List[str],
) -> pl.DataFrame:
    original_features = original.select(original_join_cols + other_cols).unique(
        original_join_cols
    )  # unique ensures one row per video + date
    main = main.join(
        original_features,
        left_on=main_join_cols,
        right_on=original_join_cols,
        how="left",
    )

    return main

def create_target_df(
    df: pl.DataFrame,
    time_to_trending_thr: int,
    original_join_cols: List[str],
    other_cols: List[str],
) -> pl.DataFrame:
    # Create a DF with video ID per row and corresponding days to trending and days in trending (target)
    target = (
        df.groupby(["video_id"])
        .agg(
            pl.col("days_to_trending").min().dt.days(),
            pl.col("trending_date").min().dt.date().alias("first_day_in_trending"),
            pl.col("trending_date").max().dt.date().alias("last_day_in_trending"),
            # our TARGET
            (pl.col("trending_date").max() - pl.col("trending_date").min()).dt.days().alias("days_in_trending"),
        )
        .filter(pl.col("days_to_trending") <= time_to_trending_thr)
    )

    # Join features to the aggregates
    target = join_original_features(
        main=target,
        original=df,
        main_join_cols=["video_id", "first_day_in_trending"],
        original_join_cols=original_join_cols,
        other_cols=other_cols,
    )

    return target
```

注意，在 `create_target_df` 函数中，`groupby` 操作以创建目标和 `join_original_features` 函数都是运行的，因为它们都使用原始数据集作为输入。这意味着即使我们有一个中间输出（`target` 变量），我们仍然可以在 `pipe` 方法中运行此函数，而不会出现问题。

```py
# Create mapping
id_to_category = read_category_mappings(pipe_config["category_map_path"])
col_mappings = {"category_id": id_to_category}

# Read in configs
date_column_format = pipe_config["date_column_format"]
ratios_config = pipe_config["ratio_features"]
diffs_config = pipe_config["difference_features"]
dates_config = pipe_config["date_features"]

# Pipeline
output_data = (
    pl.scan_csv(pipe_config["data_path"])
    .pipe(clean_data, date_column_format, col_mappings)
    .pipe(basic_feature_engineering, ratios_config, diffs_config, dates_config)
    .pipe(
        create_target_df,
        time_to_trending_thr=pipe_config["max_time_to_trending"],
        original_join_cols=join_cols,
        other_cols=base_features,
    )
).collect()
```

对于最后一步，让我们使用动态和滚动聚合生成更高级的特征（在上一篇文章中详细介绍）。

## 高级聚合

这一步负责生成基于时间的聚合。我们需要提供的唯一配置是聚合的窗口期。

```py
aggregate_windows:
  - 7
  - 30
  - 180
```

## **滚动聚合**

让我们从滚动特征开始。下方是一个示例，展示了一个 `abc` 频道在两天窗口期内的两个滞后滚动特征。

![](img/e9a06003999e420e769cbdf361befe5a.png)

滚动特征示例。图片由作者提供。

在 Polars 中，滚动特征非常简单，你只需要 `.groupby_rolling()` 方法和 `.agg()` 命名空间中的一些聚合。可能有用的聚合包括：

+   先前流行视频的数量

+   先前流行视频的平均流行天数

+   先前流行视频的最大流行天数

鉴于此，让我们构建一个名为 `build_channel_rolling` 的函数，该函数可以将所需的周期作为输入，这样我们就可以轻松创建任何我们想要的滚动特征，并输出这些所需的聚合。`by` 参数应设置为 `channel_title`，因为我们希望按频道创建聚合，而索引列应为 `first_day_in_trending`，因为这是我们的主要日期列。这两列也将用于将这些滚动聚合联接到原始数据框中。

```py
def build_channel_rolling(df: pl.DataFrame, date_col: str, period: int) -> pl.DataFrame:
    channel_aggs = (
        df.sort(date_col)
        .groupby_rolling(
            index_column=date_col,
            period=f"{period}d",
            by="channel_title",
            closed="left",  # only left to not include the actual day
        )
        .agg(
            pl.col("video_id").n_unique().alias(f"channel_num_trending_videos_last_{period}_days"),
            pl.col("days_in_trending").max().alias(f"channel_max_days_in_trending_{period}_days"),
            pl.col("days_in_trending").mean().alias(f"channel_avg_days_in_trending_{period}_days"),
        )
        .fill_null(0)
    )

    return channel_aggs

def add_rolling_features(
    df: pl.DataFrame, date_col: str, periods: List[int]
) -> pl.DataFrame:
    for period in periods:
        rolling_features = build_channel_rolling(df, date_col, period)
        df = df.join(rolling_features, on=["channel_title", "first_day_in_trending"])

    return df
```

`add_rolling_features` 是一个包装函数，可以传递到我们的管道中。它生成并联接配置中指定的周期的聚合。现在，让我们进入最终的特征生成步骤。

## 周期聚合

这些聚合类似于滚动聚合，但它们旨在衡量“流行”标签中的一般行为。

![](img/7c8d82746db65b1ec3b36821024b3ae7.png)

周期特征示例。图片由作者提供。

如果滚动聚合旨在捕捉频道的过去行为，这些聚合将捕捉一般趋势。这可能是有用的，因为决定谁进入流行和停留多长时间的算法不断变化。因此，我们想要创建的聚合是：

+   最近一段时间内流行的视频数量

+   最近一段时间内的平均流行天数

+   最近一段时间内的最大流行天数

函数的逻辑是相同的 — 我们将创建一个函数来构建这些聚合，以及一个包装函数来构建并连接所有时期的聚合。请注意，我们不指定`by`参数，因为我们想要计算每天所有视频的这些特征。还要注意，我们需要在聚合上使用`shift`，因为我们希望使用最后一段时间的特征，而不是当前的。

```py
def build_period_features(df: pl.DataFrame, date_col: str, period: int) -> pl.DataFrame:
    general_aggs = (
        df.sort(date_col)
        .groupby_dynamic(
            index_column=date_col,
            every="1d",
            period=f"{period}d",
            closed="left",
        )
        .agg(
            pl.col("video_id").n_unique().alias(f"general_num_trending_videos_last_{period}_days"),
            pl.col("days_in_trending").max().alias(f"general_max_days_in_trending_{period}_days"),
            pl.col("days_in_trending").mean().alias(f"general_avg_days_in_trending_{period}_days"),
        )
        .with_columns(
            # shift match values with previous period
            pl.col(f"general_num_trending_videos_last_{period}_days").shift(period),
            pl.col(f"general_max_days_in_trending_{period}_days").shift(period),
            pl.col(f"general_avg_days_in_trending_{period}_days").shift(period),
        )
        .fill_null(0)
    )

    return general_aggs

def add_period_features(
    df: pl.DataFrame, date_col: str, periods: List[int]
) -> pl.DataFrame:
    for period in periods:
        rolling_features = build_period_features(df, date_col, period)
        df = df.join(rolling_features, on=["first_day_in_trending"])

    return df
```

最后，让我们将这些都整合到我们的管道中吧！

```py
# Create mapping
id_to_category = read_category_mappings(pipe_config["category_map_path"])
col_mappings = {"category_id": id_to_category}

# Read in configs
date_column_format = pipe_config["date_column_format"]
ratios_config = pipe_config["ratio_features"]
diffs_config = pipe_config["difference_features"]
dates_config = pipe_config["date_features"]

output_data = (
        pl.scan_csv(pipe_config["data_path"])
        .pipe(clean_data, date_column_format, col_mappings)
        .pipe(basic_feature_engineering, ratios_config, diffs_config, dates_config)
        .pipe(
            create_target_df,
            time_to_trending_thr=pipe_config["max_time_to_trending"],
            original_join_cols=pipe_config["join_columns"],
            other_cols=pipe_config["base_columns"],
        )
        .pipe(
            add_rolling_features,
            "first_day_in_trending",
            pipe_config["aggregate_windows"],
        )
        .pipe(
            add_period_features,
            "first_day_in_trending",
            pipe_config["aggregate_windows"],
        )
    ).collect()
```

我希望你和我一样激动，因为我们快到了！最后一步 — 写出数据。

## 写数据

保存转换后的数据非常简单，因为我们可以在`collect()`操作之后直接使用`.save_parquet()`。下面你可以看到文件`data_preparation_pipeline.py`中包含的完整代码。

```py
 def pipeline():
    """Pipeline that reads, cleans, and transofrms data into
    the format we need for modelling
    """
    # Read and unwrap the config
    with open("pipe_config.yaml", "r") as file:
        pipe_config = yaml.safe_load(file)

    date_column_format = pipe_config["date_column_format"]
    ratios_config = pipe_config["ratio_features"]
    diffs_config = pipe_config["difference_features"]
    dates_config = pipe_config["date_features"]

    id_to_category = read_category_mappings(pipe_config["category_map_path"])
    col_mappings = {"category_id": id_to_category}

    output_data = (
        pl.scan_csv(pipe_config["data_path"])
        .pipe(clean_data, date_column_format, col_mappings)
        .pipe(basic_feature_engineering, ratios_config, diffs_config, dates_config)
        .pipe(
            create_target_df,
            time_to_trending_thr=pipe_config["max_time_to_trending"],
            original_join_cols=pipe_config["join_columns"],
            other_cols=pipe_config["base_columns"],
        )
        .pipe(
            add_rolling_features,
            "first_day_in_trending",
            pipe_config["aggregate_windows"],
        )
        .pipe(
            add_period_features,
            "first_day_in_trending",
            pipe_config["aggregate_windows"],
        )
    ).collect()

    return output_data

if __name__ == "__main__":
    t0 = time.time()
    output = pipeline()
    t1 = time.time()
    print("Pipeline took", t1 - t0, "seconds")
    print("Output shape", output.shape)
    print("Output columns:", output.columns)
    output.write_parquet("./data/modelling_data.parquet")
```

我们可以像运行其他 Python 文件一样运行这个管道。

```py
python data_preparation_pipeline.py
```

```py
Pipeline took 0.3374309539794922 seconds
Output shape (3196, 38)
Output columns: [
 'video_id', 'days_to_trending', 'first_day_in_trending',
 'last_day_in_trending', 'days_in_trending', 'views', 'likes', 'dislikes', 
 'comment_count', 'comments_disabled', 'ratings_disabled', 
 'video_error_or_removed', 'likes_to_dislikes', 'likes_to_views',
 'comments_to_views', 'trending_date_weekday', 'channel_title', 
 'tags', 'description', 'category_id', 'channel_num_trending_videos_last_7_days',
 'channel_max_days_in_trending_7_days', 'channel_avg_days_in_trending_7_days',
 'channel_num_trending_videos_last_30_days', 'channel_max_days_in_trending_30_days', 
 'channel_avg_days_in_trending_30_days', 'channel_num_trending_videos_last_180_days',
 'channel_max_days_in_trending_180_days', 'channel_avg_days_in_trending_180_days', 
 'general_num_trending_videos_last_7_days', 'general_max_days_in_trending_7_days', 
 'general_avg_days_in_trending_7_days', 'general_num_trending_videos_last_30_days', 
 'general_max_days_in_trending_30_days', 'general_avg_days_in_trending_30_days',
 'general_num_trending_videos_last_180_days', 'general_max_days_in_trending_180_days',
 'general_avg_days_in_trending_180_days'
]
```

在我的笔记本电脑上，这些步骤不到半秒钟完成，这很令人印象深刻，考虑到我们链在一起的操作数量和生成的特征数量。最重要的是，管道看起来很干净，非常容易调试，并且可以在很短时间内扩展/更改/裁剪。我们做得很棒！

# 结论

如果你按照这些步骤走到了这里 — 做得好！以下是你应该在这篇文章中学到的内容的简要总结：

+   如何将多个操作链在一起形成管道

+   如何使这个管道高效

+   如何结构化你的管道项目并使用 YAML 文件对其进行参数化

确保将这些学习应用到你自己的数据中。我建议从小处着手（2-3 步），然后随着需求的增长扩展管道。确保保持模块化，惰性，并将尽可能多的操作组合到`.with_columns()`中，以确保适当的并行处理。

## 还不是 Medium 会员？

[](https://medium.com/@antonsruberts/membership?source=post_page-----f5474accacc4--------------------------------) [## 使用我的推荐链接加入 Medium — Antons Tocilins-Ruberts

### 阅读 Antons Tocilins-Ruberts 的每个故事（以及 Medium 上成千上万其他作家的故事）。你的会员费直接…

medium.com](https://medium.com/@antonsruberts/membership?source=post_page-----f5474accacc4--------------------------------)
