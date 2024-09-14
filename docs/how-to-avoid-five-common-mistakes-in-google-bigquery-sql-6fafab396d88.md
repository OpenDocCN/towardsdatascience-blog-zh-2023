# 如何避免 Google BigQuery / SQL 中的五个常见错误

> 原文：[`towardsdatascience.com/how-to-avoid-five-common-mistakes-in-google-bigquery-sql-6fafab396d88`](https://towardsdatascience.com/how-to-avoid-five-common-mistakes-in-google-bigquery-sql-6fafab396d88)

## 在使用 BigQuery 多年的过程中，我观察到即使是经验丰富的数据科学家也常常犯的 5 个问题

[](https://medium.com/@benjamin.thuerer?source=post_page-----6fafab396d88--------------------------------)![本杰明·图赫尔](https://medium.com/@benjamin.thuerer?source=post_page-----6fafab396d88--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6fafab396d88--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6fafab396d88--------------------------------) [本杰明·图赫尔](https://medium.com/@benjamin.thuerer?source=post_page-----6fafab396d88--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6fafab396d88--------------------------------) ·阅读时间 8 分钟·2023 年 10 月 25 日

--

![](img/6b6e9ac6ded4356c801ecb5e2396fdfb.png)

[詹姆斯·哈里森](https://unsplash.com/@jstrippa?utm_source=medium&utm_medium=referral) 拍摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Google BigQuery 受欢迎有很多原因。它速度极快，易于使用，提供完整的 GCP 套件，照顾好你的数据，并确保早期发现错误。更重要的是，你可以使用标准 SQL 和一些非常不错的内置函数。简而言之，它几乎是一个完整的套餐！

> 始终假设有错误和重复项，始终如此！

然而，与其他网络服务和编程语言类似，在使用 BigQuery 时，有一些事项需要了解，以避免陷入陷阱。多年来，我自己犯了很多错误，并意识到几乎所有我认识的人在某些时候都会遇到相同的问题。我在这里想指出其中的一些问题，因为我在职业生涯中发现这些问题较晚，并且也看到其他经验丰富的数据科学家遇到相同的问题。

因此，我将提供我认为几乎所有人都会在某个时候在 BigQuery 中犯的潜在错误的前 5 名列表，并且有些错误可能甚至不知道。务必避免这些错误，因为每一点都可能导致严重后果，并记住在处理数据时要有正确的态度：始终假设有错误和重复项，始终如此！

# 1\. 使用 “NOT IN” 时要小心

这发生得很快。你很匆忙，想快速检查两个表，看看一个表中提到的某个项目是否也存在于第二个表中。既然 `NOT IN` 语句听起来如此直观，为什么不使用它呢？

问题是当表中有`NULL`值时，`NOT IN`不会按预期工作。如果是这样，你将得不到想要的结果！

看看这个代码示例，我只是尝试找出*input_2*中**不**在*input_1*中的类别：

```py
WITH
  input_1 AS (
  SELECT
    category
  FROM (
    SELECT
      ["a", "b", CAST(NULL AS STRING), "d"] AS category),
    UNNEST(category) category ),

  input_2 AS (
  SELECT
    category
  FROM (
    SELECT
      ["a", "b", "c", "d"] AS category),
    UNNEST(category) category )

SELECT
  *
FROM
  input_2
WHERE
  input_2.category NOT IN (
  SELECT
    category
  FROM
    input_1)
```

注意*input_1*单元格没有*c*类别，但这个查询的结果将是`没有数据可显示`。

为了克服这个问题，请确保使用`LEFT JOIN`或删除所有`NULL`值，如稍微调整的代码所示（注意最后一行）：

```py
WITH
  input_1 AS (
  SELECT
    category
  FROM (
    SELECT
      ["a", "b", CAST(NULL AS STRING), "d"] AS category),
    UNNEST(category) category ),

  input_2 AS (
  SELECT
    category
  FROM (
    SELECT
      ["a", "b", "c", "d"] AS category),
    UNNEST(category) category )

SELECT
  *
FROM
  input_2
WHERE
  input_2.category NOT IN (
  SELECT
    category
  FROM
    input_1
  WHERE category IS NOT NULL)
```

# 2\. 使用“UNNEST”数据时你可能会丢失行

当你处理 BigData 时，嵌套列是必备的。它们将节省费用并保持数据组织有序。此外，有时将数据嵌套是根据分区的特定顺序限制数据的一个方便步骤。

然而，在展开数据时，要注意你可能会丢失数据！当在`CROSS JOIN`中使用`UNNEST`时，你将丢失所有嵌套列中没有数据的行。

查看这个例子，我们定义了 2 个不同的 ID，但只有一个在嵌套列中有数据：

```py
WITH 
input_1 AS (
  SELECT 1 as id, ['phone', 'car', 'paper'] as data 
UNION ALL
  SELECT 2 as id, [] as data 
)

SELECT 
id,
element
FROM input_1,
UNNEST(data) as element
```

运行此代码时，你将看到由于`CROSS JOIN`，ID 号 2 将丢失。这可能对你的数据产生严重影响，因为你实际上是将该 ID 从后续分析中过滤掉了。

一种改进方法是使用`LEFT JOIN`代替下面代码中的`CROSS JOIN`（请注意最后一行）：

```py
WITH 
input_1 AS (
  SELECT 1 as id, ['phone', 'car', 'paper'] as data 
UNION ALL
  SELECT 2 as id, [] as data 
)

SELECT 
id,
element
FROM input_1
LEFT JOIN UNNEST(data) as element
```

# 3\. BigQuery ML 中的成本往往高于估算

估算成本很重要。当使用 BigQuery 时，你可能在处理大数据（因为 BigQuery 的强项就在于此）。BigQuery 的一个直观特点是查询成本会自动估算，并且根据估算的分区修剪提供最大成本。

例如，当成本估算为 1 TB 时，你可以预期查询费用为 6.25 美元或更少（因为 6.25 美元/ TB 是按需定价模式；使用聚类时，你甚至可以进一步减少使用的数据量）。

![](img/ad8a1010305241929e0515296cffb7bb.png)

图 1：基于芝加哥出租车旅行数据集的模型训练估算。当训练模型时，它实际上将计费 51.6 GB。（作者提供的图像）

然而，BigQuery ML 是不同的！我想在这里指出两点：

1.  它有不同的定价模式，例如线性回归相当昂贵（312.5 美元/ TB）。

1.  处理成本取决于底层的 Vertex AI 训练成本。不幸的是，在运行查询之前，BigQuery 无法告诉你透传的 Vertex AI 成本将是多少。所以你可能会支付比估计的多得多的费用。

让我们通过一个例子来突出第 2 点。在这里，我尝试基于天和小时来预测旅行长度，使用的是公开的芝加哥出租车旅行数据集：

```py
CREATE MODEL your_model 
OPTIONS(
    MODEL_TYPE='BOOSTED_TREE_REGRESSOR',
    BOOSTER_TYPE = 'GBTREE',
    NUM_PARALLEL_TREE = 5,
    MAX_ITERATIONS = 5,
    TREE_METHOD = 'HIST',
    EARLY_STOP = FALSE,
    SUBSAMPLE = 0.85,
    INPUT_LABEL_COLS = ['trip_miles']
) AS

SELECT
  EXTRACT(hour
  FROM
    trip_start_timestamp) hour,
  EXTRACT(dayofweek
  FROM
    trip_start_timestamp) weekday,
  trip_miles
FROM
  `bigquery-public-data.chicago_taxi_trips.taxi_trips`
WHERE
  trip_miles IS NOT NULL;
```

在估算处理成本时，它估计为 3.12 GB。但请注意估算中的 `(ML)`（见图 1）。因为这提示你可能会变得更昂贵。事实上，它确实如此。训练此模型的结果是账单为 51.63 GB，因此比原始估算贵了大约 17 倍。

# 4. 在生产环境中并行化作业时避免使用 MERGE 语句

更新现有表是生产流水线中的常见情况。这是因为你不断获取新数据需要追加，但有时你也希望历史变化能够反映在你的表中。

也就是说，拥有一个可扩展的更新表的设置很重要。一个流行的选择是使用 `MERGE` 语句。它们允许你根据特定条件更新或插入数据。它们真正强大的地方在于，因为 `MERGE` 语句允许你根据定义的条件只更新一行匹配的数据，从而防止引入重复数据。

然而，有一个问题。`MERGE` 语句不允许很好的并行化。以下是一个示例查询，尝试将数据合并到现有表中。当你并行运行 `MERGE` 语句时，你会遇到一些问题。

首先，我们创建一个虚拟表，我们希望将数据合并到其中：

```py
CREATE OR REPLACE TABLE
  your_project.your_dataset.merge_test ( 
    local_date date,
    value INT64 )
PARTITION BY
  local_date;
```

然后我们并行运行这个查询，它只向虚拟表中插入或更新新的日期：

```py
MERGE
  your_project.your_dataset.merge_test o
USING
  (
  SELECT
    date local_date,
    1 value
  FROM
    UNNEST(GENERATE_DATE_ARRAY("2022-01-01", "2023-12-31", INTERVAL 1 day)) date 
  ) n
ON
  n.local_date = o.local_date
  WHEN MATCHED THEN UPDATE SET value = o.value
  WHEN NOT MATCHED THEN INSERT VALUES (local_date, value)
```

当并行运行时，你会观察到两个问题：

1.  当在同一分区上使用 `UPDATE` 语句时，你会遇到一个错误消息。错误消息如下：*由于并发更新，无法序列化对表 your_project.your_dataset.merge_test 的访问。*

1.  你还会观察到，稍后触发的查询在 *挂起* 状态中停留了很长时间（几秒钟），而不是 *运行* 状态。

尽管 `MERGE` 语句非常有用且强大，但请记住，在一个设计用于将数据并行化到同一表中的生产环境中时，查询要么由于更新中的分区冲突而崩溃，要么当并行运行超过 2 个任务时，查询将处于 *挂起* 状态，从而失去了并行化的好处。

避免这种情况的一种方法是使用 `DELETE / INSERT` 语句或创建临时表，稍后将它们批量合并在一起。

# 5. 分区和集群临时表

临时（或 `TEMP`）表在构建包含多个子查询的复杂查询时非常有用。当你处理大数据并希望一次加载数据然后在查询的后续部分多次使用时，它们也很方便。所以，当你遇到可能的超时问题时，查询运行时间过长，它们可能是你所寻找的解决方案。

然而，用户通常只看到创建一个在整个查询运行时会被物化的临时表的好处。但这并不是全部，因为类似于其他所有物化表，`TEMP` 表也可以进行*分区*和*聚类*。这提供了额外的优势，可以减少查询成本，并改善运行时（特别是在处理非常大的数据时）。

附带的查询并不是非常有用，但它展示了这个概念。该查询基于公开的芝加哥出租车行程数据集创建了两个`TEMP` 表，其中一个使用完整的数据集，另一个仅使用该数据集的一部分。在最后的查询中，这两个表被连接在一起，以筛选出子集。由于`TEMP` 表是分区和聚类的，实际将这两个临时表连接在一起的查询只会计费 1GB，而不进行分区时则会计费 36GB：

```py
CREATE TEMP TABLE trips
PARTITION BY
  trip_start_date
CLUSTER BY
  unique_key AS
SELECT
  unique_key,
  taxi_id,
  DATE(trip_start_timestamp) trip_start_date,
  trip_total
FROM
  `bigquery-public-data.chicago_taxi_trips.taxi_trips`; 

CREATE TEMP TABLE trips_filtered
PARTITION BY
  trip_start_date
CLUSTER BY
  unique_key AS
SELECT
  unique_key,
  taxi_id,
  DATE(trip_start_timestamp) trip_start_date,
  trip_total
FROM
  `bigquery-public-data.chicago_taxi_trips.taxi_trips`
WHERE
  EXTRACT(year
  FROM
    trip_start_timestamp) = 2022
  AND SUBSTR(taxi_id, -1, 1) = "b";

SELECT
  DISTINCT taxi_id
FROM
  trips
JOIN
  trips_filtered
USING
  (unique_key,
    taxi_id,
    trip_start_date)
```

此外，在处理大型数据集并进行空间索引时，在聚类和分区的`TEMP` 表上执行连接操作也会改善运行时间。

# 总结

我们都会犯错，这是一件好事，只要我们从中学习。这篇故事中提到的 5 个问题对我来说是最重要的，因为它们并不是很知名，而且初学者遇到的机会很少。很少有人考虑这些问题，但随着经验的增长，这 5 个问题中的某些可能会以负面方式影响数据或生产环境。所以请注意这些问题，并记住在处理数据时的态度：*总是假设存在错误和重复，永远如此！*
