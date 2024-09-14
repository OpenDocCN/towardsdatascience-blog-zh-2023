# Python 到 SQL — 我现在可以以 20 倍的速度加载数据

> 原文：[`towardsdatascience.com/fast-load-data-to-sql-from-python-2d67aea946c0`](https://towardsdatascience.com/fast-load-data-to-sql-from-python-2d67aea946c0)

## 上传大量数据的好方法、坏方法和丑陋的方法

[](https://thuwarakesh.medium.com/?source=post_page-----2d67aea946c0--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----2d67aea946c0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2d67aea946c0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d67aea946c0--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----2d67aea946c0--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d67aea946c0--------------------------------) ·6 分钟阅读·2023 年 3 月 20 日

--

![](img/8276c9e69e0172eae178b74760c9fee1.png)

[Matheo JBT](https://unsplash.com/@matheo_jbt?utm_source=medium&utm_medium=referral) 摄影，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

速度很重要！在数据管道中，和在其他地方一样。

处理大量数据集是大多数数据专业人员的日常工作。如果它们流入数据库或仓库，这不会成为问题。

但有时我们需要上传足够重的批量数据，以至于使我们的工作站无用数小时。散步一下，喝点水，对你有好处！

但如果您真的想缩短这个任务，我们需要最优的方法将数据加载到数据库中。

如果这是一个预格式化的文件，我会更倾向于使用客户端库来完成。例如，以下 shell 命令可以将您的 CSV 文件上传到远程 Postgres 数据库。

[](https://levelup.gitconnected.com/streamlit-openai-gpt3-example-app-b333da955ceb?source=post_page-----2d67aea946c0--------------------------------) [## 用 Streamlit 在几分钟内创建 GPT3 驱动的应用程序

### 学习构建智能应用程序，而不必过多担心软件开发。

[levelup.gitconnected.com](https://levelup.gitconnected.com/streamlit-openai-gpt3-example-app-b333da955ceb?source=post_page-----2d67aea946c0--------------------------------) [](https://levelup.gitconnected.com/3-ways-of-web-scraping-in-python-e953c4a96ec2?source=post_page-----2d67aea946c0--------------------------------) [## Python 网络抓取的宁静交响曲 — 3 移动篇章

### 在 Python 中进行网络抓取的最简单、最灵活和最全面的方法

[levelup.gitconnected.com](https://levelup.gitconnected.com/3-ways-of-web-scraping-in-python-e953c4a96ec2?source=post_page-----2d67aea946c0--------------------------------)

```py
psql \
  -h $hostname -d $dbname -U $username \
  -c "\copy mytable (column1, column2)  from '/path/to/local/file.csv' with delimiter as ','"
```

但这在我的情况中很少发生。

由于 Python 是我主要的编程语言，我必须通过 Python 上传它们，可能需要进行一些预处理。因此，我做了一个小实验来查看最快的方法。

我找到了最快的方法，但这不是我平常使用的。

[](/etl-github-actions-cron-383f618704b6?source=post_page-----2d67aea946c0--------------------------------) ## 如何使用 GitHub Actions 构建简单的 ETL 管道

### ETL 不必复杂。如果是这样，使用 GitHub Actions。

[towardsdatascience.com

## 丑陋的：如何不上传数据

尽管我现在确信我不应使用这种方法，但我认为这可能最初有帮助。

这是我们所做的。我有一个约 500MB 的磁盘数据集。首先，我尝试使用 psycopg2 模块中的插入命令来加载它。

```py
import psycopg2
import csv
import time

def timing_decorator(func):
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"Function {func.__name__} took {end_time - start_time} seconds to run.")
        return result
    return wrapper

# Establish a connection to the PostgreSQL database
conn = psycopg2.connect(
    host="localhost",
    database="playground",
    user="<your-user-name>",
    password="<your-password>",
)

# Define the path to the CSV file and the name of the table to load it into
csv_file_path = "./data.csv"
table_name = "temp_table"

@timing_decorator
def load_csv_with_insert():

    # Create a cursor object
    cur = conn.cursor()

    # Open the CSV file and read its contents
    with open(csv_file_path, 'r') as f:
        csv_reader = csv.reader(f)
        next(csv_reader) # skip header row

        # Define the query to insert data into the table
        insert_query = f"INSERT INTO {table_name} VALUES ({','.join(['%s']*len(next(csv_reader)))})"

        # Iterate over the CSV rows and execute the insert query for each row
        for row in csv_reader:
            cur.execute(insert_query, row)

        # Commit the changes to the database
        conn.commit()

    # Close the cursor and connection
    cur.close()

load_csv_with_insert()
conn.close()
```

我使用计时装饰器来测量加载所需的时间。这是我五个最喜欢的 Python 装饰器之一。

[](/python-decorators-for-data-science-6913f717669a?source=post_page-----2d67aea946c0--------------------------------) ## 我在几乎所有数据科学项目中使用的 5 个 Python 装饰器

### 装饰器为从缓存到发送通知的一切提供了一种新的便利方式。

[towardsdatascience.com

此外，我故意将数据库保存在本地。因此，带宽不再是考虑因素。

早些时候，我真的认为这可能是加载数据的最快方式。这是因为我们使用游标同时插入数据并提交。但整个过程花费了这么多时间：

```py
Function load_csv_with_insert took 1046.109834432602 seconds to run.
```

好吧，我们如何在没有参考的情况下知道这太慢了？

让我们尝试一下最流行的方法。

[](/5-advanced-sql-techniques-for-real-life-projects-f2db9b6680e2?source=post_page-----2d67aea946c0--------------------------------) ## 这 5 种 SQL 技巧涵盖了约 80%的真实项目

### 加速你的 SQL 学习曲线。

[towardsdatascience.com

## 糟糕的：使用 Pandas 的 to_sql 加载大规模数据集。

如果你经常使用 Pandas 及其`to_sql` API，这可能会让你惊讶。

我一直在使用它，并且继续使用。但这仍然不是处理大规模数据集的最佳方法。

这是我的代码。我使用的是与之前相同的数据库和 CSV。在开始加载数据集之前，我已经截断了表格。

```py
import pandas as pd
from sqlalchemy import create_engine
import time

...

conn = create_engine("postgresql+psycopg2://thuwa:Flora1990@localhost:5432/playground")

@timing_decorator
def load_csv_with_pandas():
    df = pd.read_csv(csv_file_path)

    df.to_sql(table_name, conn, if_exists="append", chunksize=100, index=False)

load_csv_with_pandas()
```

在上述代码中，我没有使用流行的`method`参数。将它们设置为 `[multi](https://pandas.pydata.org/docs/user_guide/io.html#io-sql-method)` [将加速数据加载到分析数据库](https://pandas.pydata.org/docs/user_guide/io.html#io-sql-method) 中，如 Redshift。但在我们的案例中，我们使用的是事务性数据库。

回到重点，这种方法花费了我们 376 秒来加载数据。

```py
Function load_csv_with_pandas took 376.70790338516235 seconds to run.
```

这使得 Pandas 比使用游标加载数据要好得多，效率提升约 3 倍。

那么是什么让它不是最快的呢？毕竟，这是我最喜欢和最常用的方法。

## 好处：使用 COPY 方法

有一种本地方式可以将文本数据上传到 SQL 表中。像 psycopg2 这样的库可以直接提供这种功能。

是的，我们可以直接将文件复制到 Python 中的 SQL 表。

```py
...

@timing_decorator
def load_csv_with_copy():
    # Create a cursor object
    cur = conn.cursor()

    # Use the COPY command to load the CSV file into the table
    with open(csv_file_path, "r") as f:
        next(f)  # skip header row
        cur.copy_from(f, table_name, sep=",")
        conn.commit()

    # Close the cursor and connection
    cur.close()
```

游标中的 `copy_from` 方法使用 SQL 客户端 API 中的 `COPY` 方法，并直接将文件上传到 SQL。我们还传递了额外的参数。

在这种情况下，我们指定用逗号分隔列。我们还使用 `next` 方法跳过第一行，即表头。

结果如下：

```py
Function load_csv_with_copy took 50.60594058036804 seconds to run.
```

惊人的 50 秒 — 比使用游标快 20 倍，比 `to_sql` Pandas 方法快近 8 倍。

但等一下！

我提到我使用 Python，因为通常会进行数据预处理。其他两种方法很直接。但我们如何在 Python 运行时上传现有数据集呢？

[## 初级开发者编写多页 SQL 查询；高级开发者使用窗口函数](https://junior-developers-write-multi-page-sql-queries-seniors-use-window-functions-f82dfeb8e378?source=post_page-----2d67aea946c0--------------------------------)

### 在记录的上下文中执行计算的优雅方式

[towardsdatascience.com](https://junior-developers-write-multi-page-sql-queries-seniors-use-window-functions-f82dfeb8e378?source=post_page-----2d67aea946c0--------------------------------)

## 使用 COPY 写入内存中存在的数据集

在这里我们可以从缓冲区方法中受益。下面的代码可以快速加载现有 Pandas 数据框到 SQL 数据库。

```py
import io

...

@timing_decorator
def load_dataframe_with_copy(df):
    # Create a cursor object
    cur = conn.cursor()

    # Convert the DataFrame to a CSV file-like object
    csv_buffer = io.StringIO()
    df.to_csv(csv_buffer, index=False, header=False)

    # Use the COPY command to load the CSV file into the table
    csv_buffer.seek(0)
    cur.copy_from(csv_buffer, table_name, sep=",")
    conn.commit()

    # Close the cursor and connection
    cur.close()

df = pd.read_csv(csv_file_path)

# Do data processing on df

load_dataframe_with_copy(df)
```

这段代码创建了一个名为 `csv_buffer` 的 `StringIO` 对象，它是一个类似于 CSV 文件的文件类对象。使用 `to_csv()` 方法将 DataFrame 写入该对象，`index=False` 和 `header=False` 以排除 CSV 输出中的索引和标题。

然后在 `csv_buffer` 对象上调用 `seek(0)` 方法，将文件指针移动回文件类对象的开头。

## 结论

处理大数据集与处理普通数据集不同。一个具有挑战性的任务是将这些巨大的数据集加载到数据库中。

除非不需要预处理，我几乎总是使用 Pandas 的 to_sql 方法。然而，我的实验表明，这对大数据集来说不是最好的方法。

COPY 方法是我见过的最快的方法。虽然我建议在受控环境中批量加载时使用这个方法，但对于日常任务，`to_sql` 提供了一个很棒的接口来调整多个上传行为。

> 感谢阅读，朋友！如果你喜欢我的文章，让我们在 [**LinkedIn**](https://www.linkedin.com/in/thuwarakesh/)、[**Twitter**](https://twitter.com/Thuwarakesh) 和 [**Medium**](https://thuwarakesh.medium.com/) 保持联系。
> 
> 还不是 Medium 会员？请使用这个链接来[**成为会员**](https://thuwarakesh.medium.com/membership)，因为这样你无需额外付费，我可以通过推荐你获得少量佣金。
