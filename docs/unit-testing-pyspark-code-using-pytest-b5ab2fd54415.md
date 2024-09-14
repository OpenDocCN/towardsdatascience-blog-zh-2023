# 使用 Pytest 对 PySpark 代码进行单元测试

> 原文：[`towardsdatascience.com/unit-testing-pyspark-code-using-pytest-b5ab2fd54415`](https://towardsdatascience.com/unit-testing-pyspark-code-using-pytest-b5ab2fd54415)

[](https://julianwest155.medium.com/?source=post_page-----b5ab2fd54415--------------------------------)![Julian West](https://julianwest155.medium.com/?source=post_page-----b5ab2fd54415--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b5ab2fd54415--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b5ab2fd54415--------------------------------) [Julian West](https://julianwest155.medium.com/?source=post_page-----b5ab2fd54415--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----b5ab2fd54415--------------------------------) ·10 分钟阅读·2023 年 1 月 16 日

--

![](img/314b8ebf1cb2596498d48e1f09861a50.png)

图片来源于[Jez Timms](https://unsplash.com/@jeztimms?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/photos/_Ch_onWf38o?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**我非常喜欢单元测试。**

**阅读了两本书——** [**《务实程序员》**](https://engineeringfordatascience.com/book-notes/pragmatic-programmer/) **和** [**《重构》**](https://engineeringfordatascience.com/book-notes/refactoring/) **——彻底改变了我对单元测试的看法。**

> “测试不是为了找出漏洞。
> 
> 我们相信，测试的主要好处发生在你思考和编写测试的时候，而不是运行它们的时候。”
> 
> *— 《务实程序员》，大卫·托马斯和安德鲁·亨特*

我不再把测试视为在完成数据管道后需要完成的繁琐工作，而是将其视为一个强大的工具，用于改善代码设计，减少耦合，更快速地迭代，并与工作中的其他人建立信任。

然而，为数据应用程序编写良好的测试可能很困难。

与传统的软件应用程序具有相对明确的输入不同，生产环境中的数据应用程序依赖于大量且不断变化的输入数据。

在测试环境中准确地表示这些数据，以足够的细节覆盖所有边界情况，可能是非常具有挑战性的。有些人认为对数据管道进行单元测试没有什么意义，而是更关注数据验证技术。

我坚信在数据管道中同时实现单元测试和数据验证。**单元测试不仅仅是为了找出漏洞，更是为了创建设计更好的代码，并与同事和最终用户建立信任。**

如果你能养成编写测试的习惯，你将编写出设计更好的代码，节省长远的时间，并减少管道失败或在生产中给出不正确结果的痛苦。

# 单元测试 PySpark 代码的挑战

一个好的单元测试应具备以下特征：

+   **专注**。每个测试应测试一个单一的行为/功能。

+   **快速**。允许你快速迭代并获得反馈。

+   **孤立**。每个测试应该负责测试特定的功能，并且不依赖于外部因素以便成功运行。

+   **简洁**。创建测试不应包含大量的样板代码来模拟/创建复杂对象以便测试运行。

**当涉及到专门为 PySpark 管道编写单元测试时，编写专注、快速、孤立和简洁的测试可能是一个挑战。**

你可能会遇到一些障碍……

## 首先编写可测试的代码

PySpark 管道往往被编写成一个负责多个转换的巨型函数。

例如：

```py
def main(spark, country):
    """Example function for processing data received from different countries"""
    # fetch input data
    ...
    # preprocess based on individual country requirements
    if country == 'US':
       # preprocessing for US
    elif country == 'GB':
       # preprocessing for UK
    else:
       # more preprocessing etc..
    # join dataframes together
    ...
    # run some calculations/aggregate
    ...
    # save results
    ...
```

从这个角度考虑转换是合理的，并且在很多方面更容易理解和阅读。

但是，当你开始尝试为这个函数编写测试时，你会很快意识到，编写一个覆盖所有功能的测试是非常困难的。

这是因为函数高度耦合，并且函数可能有许多不同的路径。

即使你编写了一个测试来验证输入数据和输出数据是否符合预期。如果测试因任何原因失败，将很难理解长函数的哪个部分出现了问题。

相反，你应该将你的转换拆分为负责单一任务的可重用函数块。然后你可以为每个单独的函数（任务）编写单元测试。当这些单元测试都通过时，你可以更有信心地将所有函数组合在一起，得到最终管道的输出。

编写测试是一种良好的实践，并迫使你考虑设计原则。如果测试你的代码很困难，那么你可能需要重新考虑你的代码设计。

## 速度

Spark 被优化用于处理非常大的数据，计算也被优化以分布在多台机器上。

这在大型集群中表现得很好，但实际上在你可能用于单元测试的单台机器上会严重影响性能。

当你运行 PySpark 管道时，spark 会评估整个管道并计算一个优化的‘计划’以在分布式集群上执行计算。

计划带来了显著的开销。当你在分布式集群上处理数 TB 的数据时，这是合理的。但在单台机器上处理小数据集时，速度可能会出奇地慢。尤其是与 Pandas 的体验相比。

如果不优化你的 SparkSession 配置参数，你的单元测试将会运行得非常缓慢。

## 依赖 Spark Session

要在你的单元测试中运行 PySpark 代码，你需要一个 SparkSession。

如上所述，理想情况下，每个测试应与其他测试隔离，并且不需要复杂的外部对象。不幸的是，不能避免为单元测试启动 Spark 会话的要求。

创建 Spark 会话是编写 PySpark 管道单元测试时要克服的第一个障碍。

你应该如何为测试创建 SparkSession？

为每个测试初始化一个新的 Spark 会话会显著增加运行测试的时间，并给测试引入大量样板代码。

高效地创建和共享 SparkSession 对于保持测试性能在可接受的水平至关重要。

## 数据

你的测试将需要输入数据。

创建大数据管道的示例数据存在两个主要问题。

首先是大小。显然，你不能在生产中使用的完整数据集上运行测试。你必须使用一个更小的子集。

但是，通过使用一个小数据集，你会遇到第二个问题，即提供足够的测试数据来覆盖你想要处理的所有边界情况。

*真正*很难为测试模拟现实数据。虽然对此没有太多办法，但你可以使用更小、目标明确的数据集进行测试。

# 使用 Pytest 对 PySpark 代码进行单元测试的步骤

让我们通过一个使用[PyTest](https://docs.pytest.org/en/7.2.x/)为 PySpark 管道编写单元测试的示例来进行实践。

> *💻 完整代码可在此* [*GitHub 存储库*](https://github.com/julian-west/e4ds-snippets/tree/master/pyspark/pyspark_unit_testing) *中获取*

## 示例代码

这是一个处理银行交易的 PySpark 管道示例。在这个场景中，我们希望将原始交易分类为借记账户或信用账户交易，通过将其与一些参考数据连接起来。

每个交易记录都附带一个账户 ID。我们将使用这个账户 ID 连接到包含该账户 ID 是借记账户还是信用账户的信息的账户信息表。

```py
import pyspark.sql.functions as F
from pyspark.sql import DataFrame

def classify_debit_credit_transactions(
    transactionsDf: DataFrame, accountDf: DataFrame
) -> DataFrame:
    """Join transactions with account information and classify as debit/credit"""
    # normalise strings
    transactionsDf = transactionsDf.withColumn(
        "transaction_information_cleaned",
        F.regexp_replace(F.col("transaction_information"), r"[^A-Z0-9]+", ""),
    )
    # join on customer account using first 9 characters
    transactions_accountsDf = transactionsDf.join(
        accountDf,
        on=F.substring(F.col("transaction_information_cleaned"), 1, 9)
        == F.col("account_number"),
        how="inner",
    )
    # classify transactions as from debit or credit account customers
    credit_account_ids = ["100", "101", "102"]
    debit_account_ids = ["200", "201", "202"]
    transactions_accountsDf = transactions_accountsDf.withColumn(
        "business_line",
        F.when(F.col("business_line_id").isin(credit_account_ids), F.lit("credit"))
        .when(F.col("business_line_id").isin(debit_account_ids), F.lit("debit"))
        .otherwise(F.lit("other")),
    )
    return transactions_accountsDf 
```

这个示例管道存在几个问题：

+   难以阅读。一个地方包含了大量复杂的逻辑。例如，正则表达式替换、按子字符串连接等。

+   难以测试。单个函数负责多个操作

+   难以重用。借记/信用分类是业务逻辑，无法轻松在项目中重用

## 步骤 1：将代码重构为更小的逻辑单元

让我们首先将代码重构为单独的函数，然后将这些函数组合成主`classify_debit_credit_transactions`函数。

然后，我们可以为每个单独的函数编写测试，以确保其按预期行为。

虽然这增加了代码的总体行数，但测试起来更容易，我们现在可以在项目的其他部分重用这些函数。

```py
import pyspark.sql.functions as F
from pyspark.sql import DataFrame

def classify_debit_credit_transactions(
    transactionsDf: DataFrame, accountsDf: DataFrame
) -> DataFrame:
    """Join transactions with account information and classify as debit/credit"""
    transactionsDf = normalise_transaction_information(transactionsDf)
    transactions_accountsDf = join_transactionsDf_to_accountsDf(
        transactionsDf, accountsDf
    )
    transactions_accountsDf = apply_debit_credit_business_classification(
        transactions_accountsDf
    )
    return transactions_accountsDf
```

```py
def normalise_transaction_information(transactionsDf: DataFrame) -> DataFrame:
    """Remove special characters from transaction information"""
    return transactionsDf.withColumn(
        "transaction_information_cleaned",
        F.regexp_replace(F.col("transaction_information"), r"[^A-Z0-9]+", ""),
    )
```

```py
def join_transactionsDf_to_accountsDf(
    transactionsDf: DataFrame, accountsDf: DataFrame
) -> DataFrame:
    """Join transactions to accounts information"""
    return transactionsDf.join(
        accountsDf,
        on=F.substring(F.col("transaction_information_cleaned"), 1, 9)
        == F.col("account_number"),
        how="inner",
    )
```

```py
def apply_debit_credit_business_classification(
    transactions_accountsDf: DataFrame,
) -> DataFrame:
    """Classify transactions as coming from debit or credit account customers"""

    CREDIT_ACCOUNT_IDS = ["101", "102", "103"]
    DEBIT_ACCOUNT_IDS = ["202", "202", "203"]

    return transactions_accountsDf.withColumn(
        "business_line",
        F.when(F.col("business_line_id").isin(CREDIT_ACCOUNT_IDS), F.lit("credit"))
        .when(F.col("business_line_id").isin(DEBIT_ACCOUNT_IDS), F.lit("debit"))
        .otherwise(F.lit("other")),
    )
```

## 3\. 使用 Fixtures 创建可重用的 SparkSession

在编写单元测试之前，我们需要创建一个可以在所有测试中重用的 SparkSession。

为此，我们在 `conftest.py` 文件中创建一个 [PyTest fixture](https://docs.pytest.org/en/6.2.x/fixture.html)。

Pytest fixtures 是一次创建然后在多个测试中重复使用的对象。这对于像 SparkSession 这样的复杂对象特别有用，因为创建它们的开销很大。

```py
# conftest.py
from pyspark.sql import SparkSession
import pytest

@pytest.fixture(scope="session")
def spark():
    spark = (
        SparkSession.builder.master("local[1]")
        .appName("local-tests")
        .config("spark.executor.cores", "1")
        .config("spark.executor.instances", "1")
        .config("spark.sql.shuffle.partitions", "1")
        .config("spark.driver.bindAddress", "127.0.0.1")
        .getOrCreate()
    )
    yield spark
    spark.stop()
```

设置一些配置参数以优化 SparkSession，用于在单台机器上处理小数据进行测试是很重要的：

+   `master = local[1]` – 指定 spark 在一台本地机器上运行，并且使用一个线程

+   `spark.executor.cores = 1` – 设置核心数为一个

+   `spark.executor.instances = 1` - 设置执行器为一个

+   `spark.sql.shuffle.partitions = 1` - 设置最大分区数为 1

+   `spark.driver.bindAddress = 127.0.0.1` – （可选）显式指定驱动程序绑定地址。如果你的机器也与远程集群有活动连接，这将很有用。

这些配置参数本质上告诉 spark 你在单台机器上处理数据，并且 spark 不应该尝试分发计算。这将大大节省管道执行和计算本身的时间。

注意，建议使用 `yield` 而不是 `return` 来返回 spark session。有关更多信息，请阅读 [PyTest 文档](https://docs.pytest.org/en/7.1.x/how-to/fixtures.html#yield-fixtures-recommended)。使用 `yield` 还允许你在测试运行后执行任何清理操作（例如，删除本地临时目录、数据库或表等）。

## 4\. 为代码创建单元测试

现在让我们为我们的代码编写一些测试。

我发现最有效的方式是用以下结构组织我的 PySpark 单元测试：

+   创建输入 DataFrame

+   使用我们要测试的函数创建输出 DataFrame

+   指定预期的输出值

+   比较结果

我还尽量确保测试覆盖正面测试用例和至少一个负面测试用例。

```py
from src.data_processing import (
    apply_debit_credit_business_classification,
    classify_debit_credit_transactions,
    join_transactionsDf_to_accountsDf,
    normalise_transaction_information,
)
```

```py
def test_classify_debit_credit_transactions(spark):
    # create input test dataframes
    transactionsDf = spark.createDataFrame(
        data=[
            ("1", 1000.00, "123-456-789"),
            ("3", 3000.00, "222222222EUR"),
        ],
        schema=["transaction_id", "amount", "transaction_information"],
    )
    accountsDf = spark.createDataFrame(
        data=[
            ("123456789", "101"),
            ("222222222", "202"),
            ("000000000", "302"),
        ],
        schema=["account_number", "business_line_id"],
    )

    # output dataframe after applying function
    output = classify_debit_credit_transactions(transactionsDf, accountsDf)

    # expected outputs in the target column
    expected_classifications = ["credit", "debit"]

    # assert results are as expected
    assert output.count() == 2
    assert [row.business_line for row in output.collect()] == expected_classifications
```

```py
def test_normalise_transaction_information(spark):
    data = ["123-456-789", "123456789", "123456789EUR", "TEXT*?WITH.*CHARACTERS"]
    test_df = spark.createDataFrame(data, "string").toDF("transaction_information")
    expected = ["123456789", "123456789", "123456789EUR", "TEXTWITHCHARACTERS"]
    output = normalise_transaction_information(test_df)
    assert [row.transaction_information_cleaned for row in output.collect()] == expected
```

```py
def test_join_transactionsDf_to_accountsDf(spark):
    data = ["123456789", "222222222EUR"]
    transactionsDf = spark.createDataFrame(data, "string").toDF(
        "transaction_information_cleaned"
    )
    data = [
        "123456789",  # match
        "222222222",  # match
        "000000000",  # no-match
    ]
    accountsDf = spark.createDataFrame(data, "string").toDF("account_number")
    output = join_transactionsDf_to_accountsDf(transactionsDf, accountsDf)
    assert output.count() == 2
```

```py
def test_apply_debit_credit_business_classification(spark):
    data = [
        "101",  # credit
        "202",  # debit
        "000",  # other
    ]
    df = spark.createDataFrame(data, "string").toDF("business_line_id")
    output = apply_debit_credit_business_classification(df)
    expected = ["credit", "debit", "other"]
    assert [row.business_line for row in output.collect()] == expected
```

我们现在为 PySpark 管道中的每个组件都有了单元测试。

由于每个测试都重用相同的 SparkSession，运行多个测试的开销大大减少。

# PySpark 代码单元测试的进一步提示

## 创建包含最少所需信息的测试 DataFrame

在创建测试数据的 DataFrame 时，只创建与转换相关的列。

你只需要创建函数所需的列的数据。你不需要所有可能存在于生产数据中的其他列。

这有助于编写简洁的函数，并且更具可读性，因为可以清楚地知道哪些列是函数所需的以及受函数影响的。如果你发现需要一个包含许多列的大 DataFrame 来执行转换，你可能一次尝试做得太多了。

这只是一个指导方针，你自己的用例可能需要更复杂的测试数据，但如果可能，保持数据小巧、简洁，并局限于测试范围。

## 记得调用一个“操作”以触发 PySpark 计算

PySpark 使用惰性计算。你需要在测试期间调用一个‘action’（例如 `collect`、`count` 等），以计算一个可以与预期输出进行比较的结果。

## 如果不需要的话，不要运行所有的 PySpark 测试

PySpark 测试通常比正常单元测试运行时间更长，因为有计算计划和执行的开销。

在开发过程中，利用 Pytest 的一些功能，例如 `-k` 标志来运行单个测试或仅运行单个文件中的测试。然后，仅在提交代码之前运行完整的测试套件。

## 保持单元测试的隔离性

注意不要在测试期间修改你的 spark 会话（例如，创建一个表，但之后不删除它）。

表格将在所有测试中持续存在，这可能会干扰预期行为。

## 尽量将数据的创建保持在使用它的地方附近。

你可以使用 Pytest fixtures 在多个测试之间共享数据框，甚至从 CSV 文件等中加载测试数据。

然而，根据我的经验，为每个单独的测试创建所需的数据更容易且更具可读性。

## 测试正面和负面结果

例如，在测试连接条件时，你应该包括一些不满足连接条件的数据。这有助于确保你既排除了错误的数据，又包含了正确的数据。

> 本博客最初发布在 [engineeringfordatascience.com](https://engineeringfordatascience.com/posts/pyspark_unit_testing_with_pytest/)

如果你使用 Pytest 进行单元测试，可以查看我另一篇包含 Pytest 使用技巧的文章：

[13 Tips for using PyTest](https://towardsdatascience.com/13-tips-for-using-pytest-5341e3366d2d?source=post_page-----b5ab2fd54415--------------------------------) [## 13 Tips for using PyTest

### 单元测试是软件开发中非常重要的技能。有一些很棒的 Python 库可以帮助我们……

[13 Tips for using PyTest](https://towardsdatascience.com/13-tips-for-using-pytest-5341e3366d2d?source=post_page-----b5ab2fd54415--------------------------------)
