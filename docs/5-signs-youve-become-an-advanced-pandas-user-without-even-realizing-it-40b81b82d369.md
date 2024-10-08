# 你已经成为高级 Pandas 用户的 5 个迹象

> 原文：[`towardsdatascience.com/5-signs-youve-become-an-advanced-pandas-user-without-even-realizing-it-40b81b82d369`](https://towardsdatascience.com/5-signs-youve-become-an-advanced-pandas-user-without-even-realizing-it-40b81b82d369)

## 时候该获得荣誉了

[](https://ibexorigin.medium.com/?source=post_page-----40b81b82d369--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----40b81b82d369--------------------------------)[](https://towardsdatascience.com/?source=post_page-----40b81b82d369--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----40b81b82d369--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----40b81b82d369--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----40b81b82d369--------------------------------) ·阅读时间 10 分钟·2023 年 5 月 12 日

--

![](img/e83e79f70c536ea968b37c4d93615614.png)

图片由 [Barbara A Lane](https://pixabay.com/users/barbaraalane-756613/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2144354) 提供，来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2144354)

## 介绍

你是否常常幻想 Pandas DataFrames 和 Series？你是否花费数小时进行复杂的操作和聚合，几乎没有注意到背部疼痛，一边想着“这真是太有趣了”？

你可能已经成为了一个高级 Pandas 用户而未曾意识到。加入这个少数派 Pandas 爱好者的俱乐部，接受你已经正式成为数据魔法师的事实。

那么，让我们来看看你是否属于这个俱乐部的五个迹象。

## 0\. 了解何时抛弃 Pandas

当你刚开始学习数据分析时，Pandas 可能让你觉得它可以做所有事情。许多在线课程将 Pandas 推销为满足所有数据相关需求的一站式服务。

然而，随着经验的积累，你逐渐意识到 Pandas 有许多不足之处。你知道如何退一步问自己，“Pandas 在这里是最佳选择吗？”

有一些场景下，答案是一个大大的 NO。这些包括实时数据处理、大规模数据集处理、高性能计算和生产级数据管道。

1/ 对于实时数据处理，想象一下一个大炮，它以每小时 100 发的速度从某个过程发射实时数据片段。这些数据片段来得又快又猛，你必须在空中捕捉、处理并保存每一个。

温和地说，Pandas 会被这种级别的数据处理窒息。相反，你应该使用像 Apache Kafka 这样的库。

2/ 当涉及到大规模数据集时，Pandas 的创始人 Wes McKinney 有一个经验法则：

> RAM 必须比数据集大小大 5-10 倍才能使 Pandas 达到最佳性能。

“足够简单”，如果是 2013 年你会这么说，但今天的数据集往往轻松打破这一规则。

3/ 高性能计算就像指挥一场交响乐。就像指挥需要协调许多不同音乐家的动作以创造和谐的表演一样，高性能计算任务需要协调和同步多个处理元素和线程以获得最佳结果。

至于 Pandas，它是单独运行的。

4/ 对于生产级的数据管道，可以把它们看作是供水系统。正如供水系统需要可靠、可扩展和可维护以确保稳定的清洁水源一样，数据管道也需要类似的特质。虽然 Pandas 可能负责清理和转换，但其他库应处理其余部分。

离开 Pandas 的毛茸茸的怀抱可能很困难，但如果它不够用，不要感到内疚去探索其他选项。

就个人而言，我最近对 Polars 产生了兴趣，这是一种用 Rust 编写的库，旨在从零开始解决 Pandas 的所有限制。

[](/7-easy-steps-to-switch-from-pandas-to-lightning-fast-polars-and-never-return-b14c66fc85b9?source=post_page-----40b81b82d369--------------------------------) ## 7 个简单步骤，从 Pandas 切换到闪电般快速的 Polars 并且永远不再回头

### 编辑描述

[towardsdatascience.com

你还可以与 `datatable` 这样的库进行混搭。这里是我经常用来在几分之一秒内加载大型 CSV 文件并在 Pandas 中执行分析的代码片段：

```py
import datatable as dt

df = dt.fread("my_large_file.csv").to_pandas()
```

## 1\. 速度需求

Pandas 是一个庞大的库，有许多不同的方法来执行相同的任务。然而，如果你是一个经验丰富的用户，你会知道在特定情况下哪个方法最有效。

例如，你对 `apply`、`applymap`、`map`、`iterrows` 和 `itertuples` 等迭代函数之间的区别非常熟悉。你还了解在使用较慢的替代方案以获得更好的功能和使用最佳方案以获得最佳速度之间的权衡。

虽然有些人可能会称你为挑剔，但你会仔细使用 `iloc` 和 `loc`，因为你知道 `iloc` 在索引行时更快，而 `loc` 在索引列时更快。

然而，当涉及到索引值时，你会避免使用这些访问器，因为你知道使用`[query](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.query.html)`函数进行条件索引快上好几个数量级。

```py
# DataFrame of stock prices
stocks_df = pd.DataFrame(
    columns=['date', 'company', 'price', 'volume']
)

threshold = 1e5

# Rows where the average volume for a company 
# is greater than some threshold
result = df.query(
    '(volume.groupby(company).transform("mean") > @threshold)'
)
```

你也知道 `replace` 函数是 `query` 的好伙伴，用于替换值。

```py
df.query('category == "electronics"').replace(
    {"category": {"electronics": "electronics_new"}}, inplace=True
)
```

此外，你熟悉不同的文件格式，如 CSV、Parquets、Feathers 和 HDFs，并且会根据需要有意识地选择这些格式，而不是盲目地将所有内容倒入传统的 CSV 文件中。你知道，选择正确的格式可以帮助节省大量时间和内存资源。

除了文件格式，你还有一个强大的技巧——向量化！

与其将 DataFrames 视为普通的 *数据框*，你更倾向于将它们视为矩阵，将列视为向量。每当你发现自己想使用类似 `apply` 或 `itertuples` 的迭代函数时，你首先会看看是否可以利用向量化将一个函数同时应用于一列中的所有元素，而不是逐个应用。

此外，你更喜欢使用具有 `.values` 属性的底层 NumPy 数组，而不是 Pandas Series，因为你亲眼见证了使用 NumPy 数组进行向量化的速度要快得多。

[](/how-to-boost-pandas-speed-and-process-10m-row-datasets-in-milliseconds-48d5468e269?source=post_page-----40b81b82d369--------------------------------) ## 如何提高 Pandas 速度并在毫秒内处理 1000 万行数据集

### 编辑描述

[towardsdatascience.com

当一切都失败时，你不会就此放弃。绝不。

你会转向 [Cython](https://stackoverflow.com/questions/30270117/how-to-speed-up-pandas-with-cython-or-numpy) 或 [Numba](https://numba.pydata.org/) 进行真正的计算密集型任务，因为你是专家。虽然大多数人学习了 Pandas 的基础知识，你却花费了几小时痛苦的时间来学习这两项技术。这就是你与众不同的地方。

好像这些还不够，你还认真阅读了 Pandas 用户指南的 [提升性能](https://pandas.pydata.org/pandas-docs/stable/user_guide/enhancingperf.html) 页面。

## 2\. 如此多的数据类型

Pandas 提供了如此多的数据类型灵活性。你不满足于仅使用 `float`、`int` 和 `object` 数据类型，你已经将以下两张图片作为你的桌面壁纸：

![](img/f7b4f4b040adf6ce0d4c36f25391bf3a.png)

来源：[`pbpython.com/pandas_dtypes.html`](http://pbpython.com/pandas_dtypes.html) BSD-3 许可协议。

![](img/badbe7314f755c3288891f07ef0b8a7c.png)

来源：[`docs.scipy.org/doc/numpy-1.13.0/user/basics.types.html`](https://docs.scipy.org/doc/numpy-1.13.0/user/basics.types.html)。SciPy 文档。

你刻意选择尽可能小的数据类型，因为你知道它对 RAM 很友好。你知道 `int8` 比 `int64` 占用的内存少得多，浮点数也是如此。

你还像避瘟疫一样避免使用 `object` 数据类型，因为它是最糟糕的类型。

在读取数据文件之前，你会用 `cat file_name.extension` 查看其顶部几行，以决定你想为列使用哪些数据类型。然后，当使用 `read_`* 函数时，你会为每列填写 `dtype` 参数，而不是让 Pandas 自行决定。

你还尽可能在 *就地* 执行数据处理。否则，你知道 Pandas 会生成 DataFrames 和 Series 的副本，浪费你的内存。此外，你对 `pd.Categorical` 和 `chunksize` 等类和参数有很好的掌握。

## 3\. Pandas 的朋友们

如果有一件事能让 Pandas 成为数据分析库的霸主，那就是它与数据生态系统其他部分的集成。

例如，现在你一定意识到如何将 Pandas 的绘图后端从 Matplotlib 更改为 Plotly、HVPlot、holoviews、Bokeh 或 Altair。

是的，Matplotlib 是 Pandas 的好朋友，但偶尔你也想要像 Plotly 或 Altair 这样互动性强的工具。

```py
import pandas as pd
import plotly.express as px

# Set the default plotting backend to Plotly
pd.options.plotting.backend = 'plotly'
```

说到后端，你也注意到 Pandas 在全新 2.0.0 版本中添加了一个完全支持的 PyArrow 实现，用于其 `read_*` 函数来加载数据文件。

```py
import pandas as pd

pd.read_csv(file_name, engine='pyarrow')
```

当只有 NumPy 后端时，存在许多限制，比如对非数值数据类型支持不足、几乎完全忽略缺失值或不支持复杂数据结构（日期、时间戳、分类数据）。

在 2.0.0 版本之前，Pandas 一直在开发内部解决方案，但这些方案不如一些重度用户的期望。使用 PyArrow 后端，数据加载速度显著提高，并带来了 Apache Arrow 用户熟悉的一系列数据类型：

```py
import pandas as pd

pd.read_csv(file_name, engine='pyarrow', dtype_engine='pyarrow')
```

另一个我相信你在 JupyterLab 中经常使用的 Pandas 酷炫功能是 DataFrames 的样式设置。

由于 Jupyter 项目如此出色，Pandas 开发者在 `.style` 属性下添加了一些 HTML/CSS 魔法，因此你可以以一种揭示额外洞察的方式装饰普通的 DataFrames。

```py
df.sample(20, axis=1).describe().T.style.bar(
    subset=["mean"], color="#205ff2"
).background_gradient(
    subset=["std"], cmap="Reds"
).background_gradient(
    subset=["50%"], cmap="coolwarm"
)
```

![](img/a56c5db29cb487d67e19c25a4992956b.png)

图片由作者提供。

## 4\. 数据雕刻师

由于 Pandas 是一个数据分析和处理库，你是否能灵活地调整和转换数据集以适应你的目的，才是真正的高手标志。

尽管大多数在线课程提供了现成的、清理过的列格式数据，但野外的数据集有许多不同的形状和形式。例如，一种最令人讨厌的数据格式是基于行的（金融数据中非常常见）：

```py
import pandas as pd

# create example DataFrame
df = pd.DataFrame(
    {
        "Date": [
            "2022-01-01",
            "2022-01-02",
            "2022-01-01",
            "2022-01-02",
        ],
        "Country": ["USA", "USA", "Canada", "Canada"],
        "Value": [10, 15, 5, 8],
    }
)

df
```

![](img/bd76a5c60fb8f67ede3d384070fb402a.png)

图片由作者提供

你必须能够将行格式转换为更有用的格式，如以下示例，使用 `[pivot](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.pivot.html)` 函数：

```py
pivot_df = df.pivot(
    index="Date",
    columns="Country",
    values="Value",
)

pivot_df
```

![](img/2cb5c4b98554dd38571fea9daf3cbb04.png)

你也可能需要执行这个操作的相反操作，称为 melt。

下面是一个使用`[melt](https://pandas.pydata.org/docs/reference/api/pandas.melt.html)`函数的 Pandas 示例，它将列数据转换为行数据格式：

```py
df = pd.DataFrame(
    {
        "Date": ["2022-01-01", "2022-01-02", "2022-01-03"],
        "AAPL": [100.0, 101.0, 99.0],
        "GOOG": [200.0, 205.0, 195.0],
        "MSFT": [50.0, 52.0, 48.0],
    }
)

df
```

![](img/94c66f36d2e14f49d512378c2725015a.png)

图片由作者提供

```py
melted_df = pd.melt(
    df, id_vars=["Date"], var_name="Stock", value_name="Price"
)

melted_df
```

![](img/44e4b0a8b5abeb6d7510780a37b23473.png)

图片由作者提供

这些函数可能很难理解，甚至更难应用。

还有类似的函数，如 `pivot_table`，它创建一个数据透视表，可以对表中的每个值进行不同类型的聚合计算。

另一个函数是`stack/unstack`，它可以压缩/展开 DataFrame 索引。`crosstab` 计算两个或多个因素的交叉表，默认情况下，计算因素的频率表，也可以计算其他汇总统计数据。

然后是`groupby`。尽管这个函数的基本用法很简单，但其更高级的用例非常难以掌握。如果将 Pandas `groupby` 函数的内容制作成一个单独的库，它会比 Python 生态系统中的大多数库都要大。

```py
# Group by a date column, use a monthly frequency 
# and find the total revenue for `category`

grouped = df.groupby(['category', pd.Grouper(key='date', freq='M')])
monthly_revenue = grouped['revenue'].sum()
```

精确选择适合特定情况的函数是你成为真正的数据雕刻师的标志。

## 认识 Pandas 中最难的函数，第一部分

### 编辑描述

towardsdatascience.com

阅读第二部分和第三部分来了解本节提到的函数的详细情况。

## 结论

虽然文章的标题可能看起来像是一种戏谑的方式来识别高级 Pandas 用户，但我的目的是为那些希望提升数据分析技能的初学者提供一些指导。

通过突显高级用户的一些独特习惯，我想揭示这个多功能库的一些鲜为人知但强大的功能。

无论你是经验丰富的数据专家还是刚刚起步，通过识别高级用户的特征并采纳他们的一些技巧和窍门，你都可以将数据分析提升到一个新的水平。

希望这篇文章为你提供了一些娱乐和灵感，激励你探索 Pandas 的深度，成为数据处理的高手。感谢你的阅读！

喜欢这篇文章以及它那奇特的写作风格吗？想象一下能访问到数十篇类似的文章，全部由一位才华横溢、迷人且机智的作者（就是我，顺便说一下 :)）编写。

仅需 4.99 美元的会员资格，你不仅可以访问我的故事，还可以获得 Medium 上来自最优秀、最聪明的头脑的知识宝藏。如果你使用我的推荐链接，你将获得我超级 nova 的感激之情和一个虚拟的击掌以支持我的工作。

[](https://ibexorigin.medium.com/membership?source=post_page-----40b81b82d369--------------------------------) [## 通过我的推荐链接加入 Medium - Bex T.

### 获取对我所有 ⚡优质⚡ 内容以及 Medium 上所有内容的独家访问权限，无限制地支持我的工作，给我买一杯咖啡…

ibexorigin.medium.com](https://ibexorigin.medium.com/membership?source=post_page-----40b81b82d369--------------------------------) ![](img/241e86d1ac029974dc37f42263151bef.png)

图片由作者通过 Midjourney 提供。
