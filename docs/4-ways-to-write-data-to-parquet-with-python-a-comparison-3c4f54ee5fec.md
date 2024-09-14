# 使用 Python 将数据写入 Parquet 的 4 种方法：比较

> 原文：[`towardsdatascience.com/4-ways-to-write-data-to-parquet-with-python-a-comparison-3c4f54ee5fec`](https://towardsdatascience.com/4-ways-to-write-data-to-parquet-with-python-a-comparison-3c4f54ee5fec)

## 学习如何高效地使用 Pandas、FastParquet、PyArrow 或 PySpark 将数据写入 Parquet 格式。

[](https://anbento4.medium.com/?source=post_page-----3c4f54ee5fec--------------------------------)![Antonello Benedetto](https://anbento4.medium.com/?source=post_page-----3c4f54ee5fec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3c4f54ee5fec--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3c4f54ee5fec--------------------------------) [Antonello Benedetto](https://anbento4.medium.com/?source=post_page-----3c4f54ee5fec--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3c4f54ee5fec--------------------------------) ·7 分钟阅读·2023 年 3 月 13 日

--

![](img/39041e5a101311da2fe0002dd298f589.png)

[照片来源：Dominika Roseclay](https://www.pexels.com/photo/background-of-pile-of-firewood-with-rough-surface-4318196/)

## 按需课程 | 推荐

*一些读者联系我，询问关于* ***使用 Python 和 PySpark 进行数据工程*** *的按需课程。这些是我推荐的 3 个很好的资源：*

+   [**使用 Apache Kafka 和 Apache Spark 的数据流处理 Nano-Degree（UDACITY）**](https://imp.i115008.net/zaX10r)

+   [**数据工程 Nano-Degree（UDACITY）**](https://imp.i115008.net/zaX10r)

+   [**使用 PySpark 进行大数据处理的 Spark 和 Python（UDEMY）**](https://click.linksynergy.com/deeplink?id=533LxfDBSaM&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fspark-and-python-for-big-data-with-pyspark%2F)

*还不是 Medium 会员？考虑通过我的* [*推荐链接*](https://anbento4.medium.com/membership) *注册，以仅需每月 $5 即可访问 Medium 提供的所有内容！*

# 介绍

在今天的数据驱动世界中，高效存储和处理大规模数据集是许多企业和组织的关键需求。这就是 Parquet 文件格式发挥作用的地方。

[Parquet 是一种**列式存储格式**，旨在优化数据处理和查询性能，同时最小化存储空间。](https://www.databricks.com/glossary/what-is-parquet)

它特别适合需要快速高效分析数据的用例，如数据仓库、大数据分析和机器学习应用。

在本文中，我将演示如何使用四个不同的库将数据写入 Parquet 文件：[Pandas](https://pandas.pydata.org/docs/)、[FastParquet](https://fastparquet.readthedocs.io/en/latest/)、[PyArrow](https://arrow.apache.org/docs/python/index.html) 和 [PySpark](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/index.html)。

特别是，你将学习如何：

+   **从数据库中检索数据，将其转换为 DataFrame，并使用这些库中的每一个将记录写入 Parquet 文件。**

+   **以批量形式将数据写入 Parquet 文件，以优化性能和内存使用。**

到本文结束时，你将对如何使用 Python 写入 Parquet 文件有一个全面的理解，并解锁这种高效存储格式的全部潜力。

# 数据集

作为本教程的一部分使用的数据集包括关于不同货币和不同公司每日账户余额的模拟数据。

数据是通过 Python 递归函数生成的，然后插入到 SnowFlake 数据库表中。

然后，使用 Python 的 `snowflake.connector` 或本地 PySpark 连接工具（*配合 jars*）建立与数据库的连接，以检索数据集并将其转换为 DF 格式。

实现上述步骤的代码可在 [这里](https://gist.github.com/anbento0490/918b9319109b1860b5b7c3878ba08896) 获得，而 DF 的前几行如下所示：

![](img/d5074981abd7092017a3dccf3ffeb6ed.png)

作者生成的模拟数据，已获取并转换为 DF。

现在，让我们描述四种使用 Python 将数据集写入 Parquet 格式的不同策略。

# 探索 4 种写入 Parquet 文件的方法

如你所见，所有方法的共同起点是将数据已转换为 Pandas 或 PySpark DF。

这将使代码更加简洁、可读，并让你的生活更轻松。

# 方法 # 1：使用普通 Pandas

将数据集写入 Parquet 文件的最简单方法可能是使用 `pandas` 模块中的 `to_parquet()` 方法：

```py
# METHOD 1 - USING PLAIN PANDAS
import pandas as pd

parquet_file = 'example_pd.parquet'

df.to_parquet(parquet_file, engine = 'pyarrow', compression = 'gzip')

logging.info('Parquet file named "%s" has been written to disk', parquet_file)
```

在这种情况下，使用了 `engine = 'pyarrow'`，因为 [PyArrow](https://arrow.apache.org/docs/python/index.html) 是一个高性能的 Python 库，用于处理 Apache Arrow 数据。它提供了 Parquet 文件格式的快速和内存高效的实现，可以提高 Parquet 文件的写入性能。

此外，PyArrow 支持多种压缩算法，包括 `gzip`、`snappy` 和 `LZ4`。

在本教程中，让我们假装目标是实现高压缩比（*即生成更小的 Parquet 文件*），即使这意味着压缩和解压速度较慢。这就是为什么使用了 `compression = 'gzip'`。

# **方法 # 2：使用 Pandas 和 FastParquet**

另一种流行的将数据集写入`parquet`文件的方法是使用`fastparquet`包。在最简单的形式下，它的`write()`方法接受一个`pandas`数据框作为输入数据集，并可以使用多种算法进行压缩：

```py
# METHOD 2.1 - USING PANDAS & FASTPARQUET (WRITE IN FULL)
import pandas as pd
import fastparquet as fp

parquet_file = 'example_pd_fp_1.parquet'

fp.write(parquet_file, df, compression = 'GZIP')

logging.info('Parquet file named "%s" has been written to disk', parquet_file)
```

但如果`fastparquet`可以处理`pandas`数据框，那么为什么不使用**方法 #1**呢？

那么，使用`fastparquet`包至少有几个理由：

+   *它被设计成一个轻量级包，以其快速的写入性能和高效的内存使用而闻名。*

+   *它提供了`parquet`格式的纯 Python 实现，并为开发人员提供了更多的灵活性和选项。*

例如，通过使用`fp.write()`方法，你可以指定选项`append = True`，这是`to_parquet()`方法尚不支持的。

当源数据集太大，无法一次写入内存时，这个选项特别方便，因此为了避免`OOM`错误，你决定将其分批写入`parquet`文件中。

下面的代码是一个有效的示例，展示了如何利用`pandas`和`fastparquet`的强大功能，将大型数据集分批写入`parquet`文件：

```py
# METHOD 2.2 - USING PANDAS & FASTPARQUET (WRITE IN BATCHES)
import pandas as pd
import fastparquet as fp

# SETTING BATCH SIZE
batch_size = 250

data = db_cursor_sf.fetchmany(batch_size)
columns = [desc[0] for desc in db_cursor_sf.description]

# CREATES INITIAL DF INCLUDING 1ST BATCH
df = pd.DataFrame(data, columns = columns)
df = df.astype(schema)

# WRITES TO PARQUET FILE
parquet_file = 'example_pd_fp_2.parquet'
fp.write(parquet_file, df, compression = 'GZIP')

total_rows = df.shape[0]
total_cols = df.shape[1]

# SEQUENTIALLY APPEND TO PARQUET FILE
while True:
    data = db_cursor_sf.fetchmany(batch_size)
    if not data:
        break
    df = pd.DataFrame(data, columns = columns)
    df = df.astype(schema)

    total_rows += df.shape[0]

    # Write the rows to the Parquet file
    fp.write(parquet_file, df, append=True, compression = 'GZIP')

logging.info('Full parquet file named "%s" has been written to disk \
              with %s total rows', parquet_file, total_rows)
```

上述代码使用的策略是：

+   定义一个`batch_size`：在本教程中，它设置为仅`250`行，但在生产中可以根据执行作业的工作机器的内存，轻松增加到*数百万行*。

+   从数据库中提取第一批行，使用`fetchmany()`而不是`fetchall()`，并使用此数据集创建一个`pandas`数据框。将此数据框写入`parquet`文件。

+   实例化一个`WHILE loop`，它将不断从数据库中分批提取数据，将其转换为`pandas`数据框，最终附加到初始的`parquet`文件中。

`WHILE loop`将持续运行，直到最后一行被写入`parquet`文件。由于`batch_size`可以根据使用情况进行更新，因此这应该被视为一种更加*“可控”*和内存高效的方法来用 Python 写入文件。

# 方法 # 3：使用 Pandas & PyArrow

在教程早期提到过，`pyarrow`是一个高性能的 Python 库，也提供了`parquet`格式的快速和内存高效实现。

它的强大功能可以通过间接使用（设置`engine = 'pyarrow'`，如**方法 #1**所示）或直接使用其一些本地方法。

例如，你可以很容易地复制**方法 # 2.2**中写入`parquet`文件的代码，使用`ParquetWriter()`方法：

```py
# EXAMPLE 3 - USING PANDAS & PYARROW
import pandas as pd
import pyarrow as pa
import pyarrow.parquet as pq

# SETTING BATCH SIZE
batch_size = 250

parquet_schema = pa.schema([('as_of_date', pa.timestamp('ns')),
                            ('company_code', pa.string()),
                            ('fc_balance', pa.float32()),
                            ('fc_currency_code', pa.string()),
                            ('gl_account_number', pa.string()),
                            ('gl_account_name', pa.string())
                           ])

parquet_file = 'example_pa.parquet'

total_rows = 0

logging.info('Writing to file %s in batches...', parquet_file)

with pq.ParquetWriter(parquet_file, parquet_schema, compression='gzip') as writer:
    while True:
        data = db_cursor_pg.fetchmany(batch_size)
        if not data:
            break
        df = pd.DataFrame(data, columns=list(parquet_schema.names))
        df = df.astype(schema)

        table = pa.Table.from_pandas(df)
        total_rows += table.num_rows

        writer.write_table(table)

logging.info('Full parquet file named "%s" has been written to disk \
              with %s total rows', parquet_file, total_rows)
```

请注意，由于`pyarrow`在幕后利用了 Apache Arrow 格式，因此`ParquetWriter`需要一个`pyarrow`模式作为参数（*这些数据类型相当直观，并且与其* `pandas` *对应类型有些类似*）。

此外，在使用此包时，你不能直接写入 `pandas` 数据框，但需要先将其转换为 `pyarrow.Table`（使用 `from_pandas()` 方法），然后可以使用 `write_table()` 方法将所需数据集写入文件。

尽管**方法 #3**相比其他方法更为详细，但如果在声明模式和列统计信息的可用性对你的用例至关重要，Apache Arrow 格式尤其推荐。

# 方法 #4：使用 PySpark

写入 `parquet` 文件的最后一种且可能是最灵活的方法是使用 `pyspark` 原生的 `df.write.parquet()` 方法。

当然，以下脚本假设你已连接到数据库并成功加载数据到数据框中，如 [这里](https://gist.github.com/anbento0490/918b9319109b1860b5b7c3878ba08896) 所示。

注意，在这种情况下使用了 `mode('overwrite')`，但你可以轻松切换到 `mode('append')`，如果你希望批量写入数据。此外，`pyspark` 允许你指定大量选项，其中包括首选的 `'compression'` 算法：

```py
# EXAMPLE 4 - USING PYSPARK
from pyspark.sql.types import *
from pyspark.sql import SparkSession
from pyspark import SparkConf

# CONNECT TO DB + LOAD DF

# WRITING TO PARQUET
df.write.mode('overwrite')\ # or append
        .option('compression', 'gzip')\
        .parquet("example_pyspark_final.parquet")

df.show()
```

# **结论**

在这篇文章中，我们探索了四种不同的 Python 库，允许你将数据写入 Parquet 文件，包括 *Pandas*、*FastParquet*、*PyArrow* 和 *PySpark*。

实际上，Parquet 文件格式是需要快速高效处理和分析大型数据集的企业和组织的关键工具。

你还学习了如何批量写入数据，这可以进一步优化性能和内存使用。通过掌握这些技能，你将能够充分利用 Parquet 的强大功能，将数据处理和分析提升到一个新的水平。

# **来源**

+   [什么是 Parquet？| Databricks 官方文档](https://www.databricks.com/glossary/what-is-parquet)

+   [Apache Arrow 官方文档](https://arrow.apache.org/#:~:text=Format,data%20access%20without%20serialization%20overhead.)

+   [文件太大？分块处理 — 由 BlueBirz 提供](https://www.bluebirz.net/en/make-it-chunks/)
