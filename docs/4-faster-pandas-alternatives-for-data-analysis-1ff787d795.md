# 4 种更快的 Pandas 数据分析替代方案

> 原文：[`towardsdatascience.com/4-faster-pandas-alternatives-for-data-analysis-1ff787d795`](https://towardsdatascience.com/4-faster-pandas-alternatives-for-data-analysis-1ff787d795)

## **流行数据分析工具的性能基准测试**

[](https://chengzhizhao.medium.com/?source=post_page-----1ff787d795--------------------------------)![Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----1ff787d795--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1ff787d795--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ff787d795--------------------------------) [Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----1ff787d795--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ff787d795--------------------------------) ·阅读时长 9 分钟·2023 年 2 月 9 日

--

![](img/5bcd0c2d4c70afe2ff9a9f29d02c97e8.png)

照片由 [Mateusz Butkiewicz](https://unsplash.com/@puszkins?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上拍摄

Pandas 毋庸置疑是 Python 中最受欢迎的库之一。它的 DataFrame 直观且拥有丰富的数据处理 API。许多 Python 库与 Pandas DataFrame 集成，以提高它们的采用率。

然而，Pandas 在处理大数据集的领域并不出色。它主要用于单台机器上的数据分析，而不是机器集群。在这篇文章中，我将尝试测量 **Polars、DuckDB、Vaex 和 Modin 作为 Pandas 的替代方案的性能**。

[类似数据库操作的基准测试](https://h2oai.github.io/db-benchmark/) 由 [h2oai](https://h2oai.github.io/) 发布，这启发了本文的构思。基准测试实验于 2021 年 5 月进行。本文旨在回顾两年来这个领域的许多新特性和改进。

# **为什么 Pandas 在处理大数据集时速度较慢？**

主要原因在于 Pandas 并未设计为在多个核心上运行。Pandas **一次只使用一个 CPU 核心来执行数据处理任务**，在现代拥有多个核心的 PC 上无法利用并行计算的优势。

如何在数据规模较大（仍然可以放在一台机器上）但 Pandas 执行时间较长的情况下解决这个问题？一种解决方案是利用像 Apache Spark 这样的框架，通过集群来执行数据处理任务。但有时，通过对数据进行采样并在单台机器上分析，数据分析的效率可能会更高。

如果你希望在单台机器上进行操作，让我们在本文中比较**Polars、DuckDB、Vaex 和 Modin**作为与 Pandas 的替代方案。为了衡量处理大量数据所需的时间，我将分享在单台机器上的性能基准。

# 性能评估准备

## **测试机器的规格**

MacBook Pro（13 英寸，2020 年，四个 Thunderbolt 3 接口）

+   CPU：2 GHz 四核 Intel Core i5（4 核）

+   内存：16 GB 3733 MHz LPDDR4X

+   操作系统：MacOS Monterey 12.2

## 测试数据集

在这种情况下，适合处理的中大型数据集足以展示差异。[NYC 停车罚单](https://www.kaggle.com/new-york-city/nyc-parking-tickets)[1]是进行评估的良好数据集。该数据集从 2013 年 8 月到 2017 年 6 月有 42.3M 行数据，共 51 列，包括注册州、车辆品牌和车辆颜色等信息，非常有助于洞察分析。我们将使用 2017 财政年度的数据集，包含 10.8M 行，文件大小约为 2.09G。

## 评估过程

+   由于整个运行时间包括将数据读入内存，因此有必要将数据加载单独考虑。

+   我们将重复调用 5**x 次**以避免边缘情况，并使用中位数值作为最终性能结果报告。

## 辅助函数用于重复和计算中位数

```py
from itertools import repeat
from statistics import median
import functools
import time

durations = []

## repeat a given function multiple times, append the execution duration to a list
def record_timer(func, times = 5):
    for _ in repeat(None, times):
        start_time = time.perf_counter()
        value = func()
        end_time = time.perf_counter()
        run_time = end_time - start_time
        print(f"Finished {func.__name__!r} in {run_time:.10f} secs")
        durations.append(run_time)
    return value

## Decorator and compute the median of the function
def repeat_executor(times=5):
    def timer(func):
        """Print the runtime of the decorated function"""
        @functools.wraps(func)
        def wrapper_timer(*args, **kwargs):
            value = record_timer(func, times=times)
            print(f'{median(list(durations))}')
            return value
        return wrapper_timer
    return timer
```

***警告***：我们将展示大量代码，以便读者了解我所做的，而不是不展示过程或指向 GitHub。如果你不关心过程，请跳过，直接查看底部的结果。

# Pandas：基准

为了建立比较的基准，我们将检视日常分析工作的著名用例：**筛选、聚合、连接和窗口函数**。

+   **筛选**：查找车辆品牌为 BMW

+   **聚合**：按车辆品牌分组并执行计数

+   **连接**：在召唤号码上进行自连接

+   **窗口函数**：根据计数对车辆品牌进行排名

我仅选择了用于测试的字段，分别是`‘Summons Number’，‘Vehicle Make’，‘Issue Date’`。请注意，如果选择所有字段，最后两个查询会显著变慢。

```py
import pandas as pd
from repeat_helper import repeat_executor

df = pd.read_csv("./Parking_Violations_Issued_-_Fiscal_Year_2017.csv")
df = df[['Summons Number', 'Vehicle Make', 'Issue Date']]

# ## Filter on the Vehicle Make for BMW
@repeat_executor(times=5)
def test_filter():
    return df[df['Vehicle Make'] == 'BMW']['Summons Number']

# # ## Group By on the Vehicle Make and Count 
@repeat_executor(times=5)
def test_groupby():
    return df.groupby("Vehicle Make").agg({"Summons Number":'count'})

# # ## SELF join
@repeat_executor(times=5)
def test_self_join():
    return df.set_index("Summons Number").join(df.set_index("Summons Number"), how="inner", rsuffix='_other').reset_index()['Summons Number']

## window function
@repeat_executor(times=5)
def test_window_function():
    df['summon_rank'] = df.sort_values("Issue Date",ascending=False) \
        .groupby("Vehicle Make") \
        .cumcount() + 1
    return df

test_filter()
# # The median time is 0.416s
test_groupby()
# # The median time is 0.600s
test_self_join()
# # The median time is 4.159s
test_window_function()
# # The median time is 17.465s
```

# DuckDb：高效的 OLAP 内存数据库

[DuckDB](https://duckdb.org/)因其列式向量化引擎支持分析类型的查询而越来越受欢迎。它是[SQLite](https://sqlite.org/)的一个分析或 OLAP 版本，SQLite 是一种广泛采用的简单嵌入式内存数据库管理系统。

尽管它是一个数据库管理系统，但与 Microsoft SQL Server 或 Postgres 相比，安装并不复杂；此外，运行查询不需要外部依赖。我惊讶于使用[DuckDb CLI](https://duckdb.org/docs/api/cli.html)执行 SQL 查询是如此简单。

如果你偏好 SQL 接口，DuckDb 可能是你直接在 CSV 或 Parquet 文件上进行数据分析的最佳替代方案。让我们继续一些代码示例，并同时展示使用 DuckDb 进行 SQL 操作是多么简单。

DuckDb 具有一个神奇的 `read_csv_auto` 函数来推断 CSV 文件并将数据加载到内存中。在运行时，我发现我必须将 `SAMPLE_SIZE=-1` 更改为跳过采样，因为我的数据集中有些字段未被正确推断，默认的采样大小为 1,000 行。

```py
import duckdb
from repeat_helper import repeat_executor

con = duckdb.connect(database=':memory:')
con.execute("""CREATE TABLE parking_violations AS SELECT "Summons Number", "Vehicle Make", "Issue Date" FROM read_csv_auto('/Users/chengzhizhao/projects/pandas_alternatives/Parking_Violations_Issued_-_Fiscal_Year_2017.csv', delim=',', SAMPLE_SIZE=-1);""")
con.execute("""SELECT COUNT(1) FROM parking_violations""")
print(con.fetchall())
# ## Filter on the Vehicle Make for BMW
@repeat_executor(times=5)
def test_filter():
    con.execute("""
        SELECT * FROM parking_violations WHERE "Vehicle Make" = 'BMW'
        """)
    return con.fetchall()

# # ## Group By on the Vehicle Make and Count 
@repeat_executor(times=5)
def test_groupby():
    con.execute("""
        SELECT COUNT("Summons Number") FROM parking_violations GROUP BY "Vehicle Make"
        """)
    return con.fetchall()

# # # ## SELF join
@repeat_executor(times=5)
def test_self_join():
    con.execute("""
        SELECT a."Summons Number"
        FROM parking_violations a
        INNER JOIN parking_violations b on a."Summons Number" = b."Summons Number"
        """)
    return con.fetchall()

# ## window function
@repeat_executor(times=5)
def test_window_function():
    con.execute("""
        SELECT *, ROW_NUMBER() OVER (PARTITION BY "Vehicle Make" ORDER BY "Issue Date")
        FROM parking_violations 
        """)
    return con.fetchall()

test_filter()
# The median time is 0.410s
test_groupby()
# # The median time is 0.122s
test_self_join()
# # The median time is 3.364s
test_window_function()
# # The median time is 6.466s
```

DuckDb 的结果令人印象深刻。我们在过滤测试上表现相当，但在其余三项测试中相较于 pandas 性能更佳。

如果你不习惯编写 Python，你可以使用 DuckDb CLI 的 SQL 接口在命令行中进行操作，或者轻松使用 [TAD](https://duckdb.org/docs/guides/data_viewers/tad)。

![](img/be0704db6ce878cf11e77afb37e64846.png)

作者展示了如何通过 CLI 使用 SQL 查询 DuckDB | 图片由作者提供

# Polars：惊人的快速构建基于 Rust + Arrow

[Ritchie Vink](https://github.com/ritchie46) 创建了 [Polars](https://github.com/pola-rs/polars)。Ritchie 还发布了一篇博客文章，“[我编写了一个最快的 DataFrame 库](https://www.ritchievink.com/blog/2021/02/28/i-wrote-one-of-the-fastest-dataframe-libraries/)”，受到了广泛好评。Polars 的令人印象深刻之处在于，在 h2oai 的 [数据库类操作基准测试](https://h2oai.github.io/db-benchmark/) 中，它在分组和连接操作中排名第一。

这里有几个 Polars 可以替代 Pandas 的理由：

+   Polars 从一开始就支持 DataFrame 的并行化。它不局限于单核操作。

+   PyPolars 基于 Rust 并具有 Python 绑定，性能出色，可与 C 进行比较，而“Arrow 列式格式”是分析 OLAP 类型查询的绝佳选择。

+   延迟评估：计划（而非执行）查询直到触发。这可以用来进一步优化查询，例如额外的下推。

```py
import polars as pl
from repeat_helper import repeat_executor

df = pl.read_csv("./Parking_Violations_Issued_-_Fiscal_Year_2017.csv")
df = df.select(['Summons Number', 'Vehicle Make', 'Issue Date'])

# ## Filter on the Vehicle Make for BMW
@repeat_executor(times=5)
def test_filter():
    return df.filter(pl.col('Vehicle Make') == 'BMW').select('Summons Number')

# # ## Group By on the Vehicle Make and Count 
@repeat_executor(times=5)
def test_groupby():
    return df.groupby("Vehicle Make").agg(pl.col("Summons Number").count())

# # # ## SELF join
@repeat_executor(times=5)
def test_self_join():
    return df.join(df, on="Summons Number", how="inner").select('Summons Number')

# ## window function
@repeat_executor(times=5)
def test_window_function():
    return df.select(
        [
            'Summons Number',
            'Vehicle Make',
            'Issue Date',
            pl.col(['Issue Date']).sort(reverse=True).cumcount().over("Vehicle Make").alias("summon_rank")
        ]
    )   

test_filter()
# # The median time is 0.0523s

test_groupby()
# # # The median time is 0.0808s

test_self_join()
# # # The median time is 1.343s

test_window_function()
# # The median time is 2.705s
```

哇，Polars 的速度真是惊人！在 Polars 中编程让你感觉像是 pySpark 和 Pandas 的混合体，但接口非常熟悉，我用不到 15 分钟就写出了上面的查询，尽管我对 Polars API 没有经验。你可以参考[Polars 文档（Python 版）](https://pola-rs.github.io/polars/py-polars/html/reference/index.html)来快速理解。

# Vaex：离线数据框

Vaex 是另一种采用延迟评估的替代方案，避免了额外的内存浪费和性能损失。它使用内存映射，并且只有在明确要求时才会执行。Vaex 具有一系列方便的数据可视化功能，使得探索数据集变得更加容易。

Vaex 实现了并行化分组操作，并且在连接操作中表现高效。

```py
import vaex
from repeat_helper import repeat_executor

vaex.settings.main.thread_count = 4 # cores fit my macbook

df = vaex.open('./Parking_Violations_Issued_-_Fiscal_Year_2017.csv')
df = df[['Summons Number', 'Vehicle Make', 'Issue Date']]

# ## Filter on the Vehicle Make for BMW
@repeat_executor(times=5)
def test_filter():
    return df[df['Vehicle Make'] == 'BMW']['Summons Number']

# # ## Group By on the Vehicle Make and Count 
@repeat_executor(times=5)
def test_groupby():
    return df.groupby("Vehicle Make").agg({"Summons Number":'count'})

# # ## SELF join
@repeat_executor(times=5)
def test_self_join():
    return df.join(df, how="inner", rsuffix='_other', left_on='Summons Number', right_on='Summons Number')['Summons Number']

test_filter()
# # The median time is 0.006s

test_groupby()
# # The median time is 2.987s

test_self_join()
# # The median time is 4.224s

# window function https://github.com/vaexio/vaex/issues/804
```

然而，我发现窗口函数尚未实现，[在这里跟踪的开放问题](https://github.com/vaexio/vaex/issues/804)。我们可以通过每个组进行迭代，并使用在此[问题](https://github.com/vaexio/vaex/issues/250#issuecomment-491027460)中提到的建议为每行分配一个值。然而，我没有发现 Vaex 自带实现的窗口函数。

```py
vf['rownr`] = vaex.vrange(0, len(vf))
```

# Modin：通过更改一行代码来扩展 pandas

只需更改一行代码，Modin 是否能为用户提供比 Pandas 更好的性能？在 Modin 中，需要做以下更改，将 Pandas 库替换为 Modin。

```py
## import pandas as pd
import modin.pandas as pd
```

然而，仍然有[一个实现列表](https://modin.readthedocs.io/en/stable/supported_apis/dataframe_supported.html)需要在 Modin 中完成。除了代码更改，我们还需要设置其调度的后台。我在这个例子中尝试使用 *Ray*。

```py
import os
os.environ["MODIN_ENGINE"] = "ray"  # Modin will use Ray

#########################
#######Same AS Pandas#######
#########################

test_filter()
# # The median time is 0.828s

test_groupby()
# # The median time is 1.211s

test_self_join()
# # The median time is 1.389s

test_window_function()
# # The median time is 15.635s, 
# `DataFrame.groupby_on_multiple_columns` is not currently supported by PandasOnRay, defaulting to pandas implementation.
```

Modin 上的窗口函数尚未在 Ray 上得到支持，因此它仍然使用 Pandas 的实现。时间花费与 Pandas 的窗口函数更为接近。

# (py)datatable

如果你来自 R 社区，`data.table` 应该不是一个陌生的包。随着任何包的流行，它的核心思想会被引入到其他语言中。（py）datatable 是试图模仿 R 的 `data.table` 核心算法和 API 的一个尝试。

然而，在测试过程中，这并未比 pandas 更快，鉴于语法类似于 R 的 `data.table`，我认为在此作为 Pandas 替代方案提及是不错的。

# 结果

![](img/cccce809f6e7d8d2d2e45640379fec04.png)

最终比较 | 作者提供的图片

# 最终想法

这些是我测试过的 Pandas 替代品，它们在性能上优于 Pandas。同时，API 的变化对 Pandas 并不显著。如果你考虑使用这些库之一，过渡应该是平滑的。另一方面，Pandas 仍然在 API 功能覆盖上最为全面。替代方案在高级 API 支持如窗口函数方面有所不足。

在单台机器上运行 Pandas 仍然是数据分析或临时查询的最佳选择。虽然替代库在某些情况下可能提升性能，但在单台机器上并不总是如此。

**请告诉我你认为哪个是你会选择的 Pandas 最佳替代方案，留下评论。**

[1] 纽约市停车罚单数据集，纽约市，[CC0: 公共领域](https://creativecommons.org/publicdomain/zero/1.0/)，[`www.kaggle.com/datasets/new-york-city/nyc-parking-tickets`](https://www.kaggle.com/datasets/new-york-city/nyc-parking-tickets)

我希望这个故事对你有所帮助。本文是我工程与数据科学故事的**系列文章**之一，目前包括以下内容：

![Chengzhi Zhao](img/51b8d26809e870b4733e4e5b6d982a9f.png)

[Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----1ff787d795--------------------------------)

## 数据工程与数据科学故事

[查看列表](https://chengzhizhao.medium.com/list/data-engineering-data-science-stories-ddab37f718e7?source=post_page-----1ff787d795--------------------------------)53 个故事![](img/8b5085966553259eef85cc643e6907fa.png)![](img/9dcdca1fc00a5694849b2c6f36f038d4.png)![](img/2a6b2af56aa4d87fa1c30407e49c78f7.png)

你还可以[**订阅我的新文章**](https://chengzhizhao.medium.com/subscribe)或成为[**推荐的 Medium 会员**](https://chengzhizhao.medium.com/membership)，享受对 Medium 上所有故事的无限访问权限。

如果有任何问题/评论，**请随时在故事的评论区留言**，或**通过[Linkedin](https://www.linkedin.com/in/chengzhizhao/)或[Twitter](https://twitter.com/ChengzhiZhao)直接联系我**。
