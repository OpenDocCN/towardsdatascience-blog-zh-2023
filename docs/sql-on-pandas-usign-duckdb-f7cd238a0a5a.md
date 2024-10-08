# SQL 在 Pandas 上——我新的最爱，速度提升 10 倍。

> 原文：[`towardsdatascience.com/sql-on-pandas-usign-duckdb-f7cd238a0a5a`](https://towardsdatascience.com/sql-on-pandas-usign-duckdb-f7cd238a0a5a)

## 将两者的最佳特点结合起来

[](https://thuwarakesh.medium.com/?source=post_page-----f7cd238a0a5a--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----f7cd238a0a5a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f7cd238a0a5a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f7cd238a0a5a--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----f7cd238a0a5a--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f7cd238a0a5a--------------------------------) ·阅读时间 5 分钟·2023 年 3 月 23 日

--

![](img/c3fe425569b86a13bbb8b061d74d222e.png)

[照片由 Akira Hojo](https://unsplash.com/@joephotography?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我热衷于寻找加速分析任务的方法。

我以前写过几篇关于内存使用、运行时间和加速数据密集型任务的技巧的文章。由于我们主要每天使用 Pandas，我不断研究改进性能的方法。

[](/fuzzy-string-matching-in-pandas-2c185a24617f?source=post_page-----f7cd238a0a5a--------------------------------) ## 如何在 Pandas 数据框中进行模糊字符串匹配

### 匹配文本中没有完美匹配的部分

towardsdatascience.com

这个令人印象深刻的 Python 库功能丰富且可靠。但速度并不是它的超级能力之一。通过一些优化，我们可以获得一些速度提升。但这些技术也有它们的问题。

[](/3-important-sql-optimization-technique-d6da3e9c8442?source=post_page-----f7cd238a0a5a--------------------------------) ## 3 种 SQL 优化技术可以瞬间提升查询速度

### 在完全更换数据模型之前尝试的一些简单技巧

towardsdatascience.com

像 Polars 这样的新替代品也在获得关注。但我仍然偏爱 Pandas，就像许多其他数据专业人士一样。

这就是为什么我们使用数据库而不是内存中的数据框的原因之一。数据库被设计用来从磁盘检索数据并高效地执行计算。

然而，大多数数据科学项目不需要数据库。这就像用大锤子砸坚果一样。这时，像 SQLite 这样的内存数据库就派上用场了。

在内存中，数据库没有单独的进程运行，也不需要复杂的设置。最新版本的 Python 自带 SQLite。

但是 SQLite 也有其缺点。虽然你可以使用 SQL 查询数据库，但速度上的好处并不显著。

[## 3 种部署机器学习模型的方法](https://towardsdatascience.com/3-ways-to-deploy-machine-learning-models-in-production-cdba15b00e?source=post_page-----f7cd238a0a5a--------------------------------)

### 部署机器学习模型并使其对用户或项目的其他组件可用。

towardsdatascience.com

然而，我们有一个现代化的解决方案——DuckDB。像 SQLite 一样，设置非常简单，你可以将数据库和代码一起移植。而且你可以比 Pandas API 和 SQLite 更快。

## 使用 DuckDB 查询 Pandas dataframe。

DuckDB 是一个独立的数据库。然而，你也可以将任何 dataframe 转换为 DuckDB 表并进行查询。但在做所有这些之前，这里是如何安装它的：

```py
pip install duckdb
```

这可能会让你感到惊讶。但这就是我们安装 DuckDB 的方式。尽管它是一个数据库，但它是一个 Python 包进行安装。

由于这是一个简单的内存数据库，它没有像 Postgres 和其他常规数据库那样复杂的配置。你可以设置一些参数，你可以在[文档](https://duckdb.org/docs/sql/configuration)中找到这些参数。

假设我们有一个出租车行程时长的数据集。我们需要计算那些起点在经度 -73.95 西侧的行程的平均时长。

如果你跟随这个教程，你可以使用下面的代码生成测试数据集。

```py
import pandas as pd
import numpy as np

# Define the number of rows in the dataset
num_rows = 10000000

# Generate a random longitude for each row
pickup_longitude = np.random.uniform(low=-38.0, high=-94.0, size=num_rows)

# Generate a random trip duration for each row
trip_duration = np.random.normal(loc=10, scale=5, size=num_rows)

# Create a DataFrame with the pickup longitude and trip duration columns
df = pd.DataFrame(
    {"pickup_longitude": pickup_longitude, "trip_duration": trip_duration}
)
```

我们使用了计时装饰器来测量运行所需的时间。

[## 我在几乎所有数据科学项目中使用的 5 个 Python 装饰器](https://towardsdatascience.com/python-decorators-for-data-science-6913f717669a?source=post_page-----f7cd238a0a5a--------------------------------)

### 装饰器提供了一种新的便捷方式，用于从缓存到发送通知的各种操作。

towardsdatascience.com

```py
...

@timing_decorator
def find_avg_trip_duration_in_the_west():
    return df[df['pickup_longitude'] < -73.95]['trip_duration'].mean()

find_avg_trip_duration_in_the_west()

>> Function find_avg_trip_duration_in_the_west took 0.49195261001586914 seconds to run.
>> 9.995189356480168
```

上面的代码在我的计算机上运行了 0.2 秒。这里是使用 DuckDB API 而不是 Pandas API 的相同查询：

```py
...
import duckdb

@timing_decorator
def find_avg_trip_duration_in_the_west():
    return duckdb.execute(
        'SELECT AVG(trip_duration) FROM df WHERE pickup_longitude < -73.95'
    ).df()

>> Function find_avg_trip_duration_in_the_west took 0.05432271957397461 seconds to run.

>> |    |   avg(trip_duration) |
>> |---:|---------------------:|
>> |  0 |              9.995189|
```

如你所见，使用 DuckDB API 的速度比原生 Pandas API 快十倍。

## DuckDB 与 SQLite 的性能有何不同？

SQLite 是目前最受欢迎的内存数据库。这就是为什么我们在 Python 中默认提供它。

但是 DuckDB 在两个方面表现出色。首先，如果你不需要持久化，你也不必创建它。你可以像查询数据库表一样直接查询你的 Pandas dataframe。这就是我们在前一个示例中所做的。

[](/5-python-gui-frameworks-to-create-desktop-web-and-even-mobile-apps-c25f1bcfb561?source=post_page-----f7cd238a0a5a--------------------------------) ## 6 个 Python GUI 框架，用于创建桌面、网页和甚至移动应用。

### 你可以纯粹用 Python 构建美丽的应用程序。

towardsdatascience.com

另一个好处是 DuckDB 仍然比 SQLite 更快。实际上，将本文中的示例复制到 SQLite 上运行的速度比 Pandas 慢。这使得 SQLite 在性能优先时成为最后的选择。但它仍然非常擅长持久化和移植数据及代码。

这是我们示例的 SQLite 版本：

```py
...
import sqlite3

conn = sqlite3.connect("taxi.db")

df.to_sql("trips", conn)

@timing_decorator
def find_avg_trip_duration_in_the_west():
    cursor = conn.cursor()
    cursor.execute(
        "SELECT AVG(trip_duration) FROM trips WHERE pickup_longitude < -73.95"
    )
    result = cursor.fetchone()[0]
    cursor.close()
    return result

find_avg_trip_duration_in_the_west()

>> Function find_avg_trip_duration_in_the_west took 0.5150568962097168 seconds to run.
>> 9.995189
```

如你所见，在 Pandas 上运行 SQLite 的 SQL 需要我们在磁盘上创建一个单独的数据库实例，并将数据插入其中。如果我们使用 DuckDB，这些步骤都是不必要的。

此外，它花费了 0.51 秒，而 Pandas API 只需 0.49 秒。

## 结论

Pandas 毫无疑问是让 Python 成为数据科学热门选择的奇迹之一。然而，这个成熟的库使得数据整理任务变得缓慢。

在这篇文章中，我们讨论了使用 DuckDB 的简单查询 API 来提高速度。如果你了解 SQL，可以直接用 SQL 查询你的 Pandas 数据框，而不是使用 Pandas API。

尽管 DuckDB 有优点，但它并不是万灵药。

对于大规模项目，你仍然需要可扩展的数据库，比如 Postgres。而且，使用 Pandas API 可能开发更快，且所有懂 Python 的人都能理解它们。

> 感谢阅读，朋友！如果你喜欢我的文章，让我们在[**LinkedIn**](https://www.linkedin.com/in/thuwarakesh/)、[**Twitter**](https://twitter.com/Thuwarakesh) 和[**Medium**](https://thuwarakesh.medium.com/)保持联系。
> 
> 还不是 Medium 会员？请使用这个链接[**成为会员**](https://thuwarakesh.medium.com/membership)，因为对你没有额外费用，我会因推荐你而获得少量佣金。
