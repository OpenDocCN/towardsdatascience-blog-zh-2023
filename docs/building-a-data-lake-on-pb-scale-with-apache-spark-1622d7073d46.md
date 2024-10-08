# 使用 Apache Spark 在 PB 规模上构建数据湖

> 原文：[`towardsdatascience.com/building-a-data-lake-on-pb-scale-with-apache-spark-1622d7073d46`](https://towardsdatascience.com/building-a-data-lake-on-pb-scale-with-apache-spark-1622d7073d46)

## 我们在 Emplifi 如何处理大数据

[](https://medium.com/@vrba.dave?source=post_page-----1622d7073d46--------------------------------)![David Vrba](https://medium.com/@vrba.dave?source=post_page-----1622d7073d46--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1622d7073d46--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1622d7073d46--------------------------------) [David Vrba](https://medium.com/@vrba.dave?source=post_page-----1622d7073d46--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1622d7073d46--------------------------------) ·15 分钟阅读·2023 年 1 月 26 日

--

![](img/ec428d788058b2d1569bf4768b4938db.png)

图片由[Victor Hanacek](https://picjumbo.com/author/viktorhanacek/)在[picjumbo](https://picjumbo.com/big-data/)提供

在职业上，我在 Emplifi（前身为 Socialbakers）公司的数据工程团队中度过了过去四年，其中一个我参与的最大项目是构建一个分布式数据存储系统，目前该系统存储了近一 PB 的数据，目的是为数据分析师和研究人员提供可以高效分析和研究的数据表。正如你可以想象的那样——构建和维护这样一个数据湖并非易事，因为数据不仅频繁变化，而且其架构随着时间的推移而演变，拥有几十个甚至几百个具有不同嵌套层级的字段。

在这篇文章中，我想分享我在这段旅程中的经验和亮点，主要集中在技术层面。

我们每天处理的数据来源于社交网络，如 Facebook、Twitter、Instagram、YouTube、LinkedIn 或 TikTok。处理的数据集主要是这些网络上的公共档案和发布的帖子。一小部分数据也来自内部系统。我们的存储系统建立在 S3 AWS 上，我们称之为数据湖，因为我们在这里以原始格式以及预处理、处理和聚合后的形式存储数据。原始数据主要以压缩的 JSON 文件形式存在，而处理和聚合的数据集则以 Apache Parquet 格式存储，并以（Hive metastore）表格形式向用户公开。

# 数据流

数据经过公司基础设施的多个点，首先，它通过公共和在某些情况下也通过私有 API 从社交网络下载。接下来，它到达 DynamoDB，这是 AWS 上的一个键值分布式数据库服务，我们称之为主数据库。DynamoDB 擅长处理记录的频繁更新，这对我们的用例非常有用，因为数据来自社交网络，其中每条记录（如 post）随着时间的推移而演变——它收集互动并频繁变化。每次在 DynamoDB 中的更新也会在我们数据湖的*stage*层保存到 S3。这是 Apache Spark 中一系列步骤的起点，这些步骤将生成一致的表，数据分析师和研究人员可以使用标准分析工具高效访问。接下来的章节，我将更详细地描述这些步骤。

# 数据的结构

传入到*stage*的数据是以压缩 JSON 文件保存的连续记录流。每条记录有两个子结构——*oldImage*和*newImage*，其中第一个是记录更新前的状态，而后者包含更新后的数据。每条记录（post）可能在一天内被多次下载自社交网络，因此这个流中包含重复项，即每个*id*唯一标识的 post 可能出现多次。我们需要只考虑每个*id*的最新*version*来正确更新最终表。这里可以看到数据结构的一个简单示例：

```py
{
  'newImage': {
    'id': 1, 
    'version': 100, 
    'created_time': '2023-01-01: 10:00:00',
    'interactions': 50,
    MANY OTHER FIELDS...
  }
  'oldImage': {
    'id': 1, 
    'version': 101, 
    'created_time': '2023-01-01: 10:00:00',
    'interactions': 51,
    MANY OTHER FIELDS...
  }
}
```

# 数据湖——三层抽象

我们的数据湖构建在 S3（AWS 上的分布式对象存储）上，并有三层抽象，我们称之为*stage*、*target*和*mart*。在*stage*层，我们保存主要来自 DynamoDB 的原始数据，但也来自其他数据库，如 Elasticsearch、Postgres、MongoDB，以及各种内部 API。这些数据主要以压缩 JSON 格式到达，有时也以 CSV 格式到达。这些数据集每天进行预处理，并以 Apache Parquet 格式保存于*stage*中。

这些 Parquet 文件接下来会被处理，并用于更新*target*中的表，更新可以是每日一次，也有些情况下——数据新鲜度不那么关键——每周一次。与*stage*数据不同，*target*表由我们的数据用户直接访问。

对于一些数据结构不太友好的表，我们创建新的表来转换数据，以便于查询，并将其保存到*mart*层——我们不在*target*层这样做，因为这里我们希望保持表作为数据来源系统的镜像，以避免用户混淆，例如，DynamoDB 中的数据与*target*中的数据不同。

![](img/5495216f31b7fb9c093199870c20c697.png)

数据湖的结构（图片由作者制作）

对我们来说，使表格在查询时高效是一个重要优先事项。数据分析师不得不等待几分钟才能完成查询是非常痛苦的。我们在两个层面上以不同的成功程度实现了这一点：

+   在 *target* 中：我们的手脚会有些束缚，因为我们保证数据与原始数据库中的结构相同。然而，我们可以通过自定义文件组织来实现相当大的成果，例如使用分区、桶和排序等技术。有关更多细节，请参见下一节。

+   在 *mart* 中：我们可以做得更多，简化嵌套结构，并根据对表执行的典型查询进行预连接或预聚合。

# 阶段

*stage* 是数据最终暴露给用户的入口点。这里传入的原始数据每天都会经过以下 3 个处理步骤：

+   模式演变

+   数据清理

+   数据去重

## 模式演变——阶段层

来自数据的结构随时间发生变化并不罕见——数据集中可能会添加新字段，甚至某些字段的数据类型可能会改变。如果没有一个健全的模式演变步骤，这将是一个相当大的问题。Apache Spark 允许你通过提供模式来读取数据，或者让 Spark 推断它（使用所谓的 schema-on-read）。

通常，你对模式应该是什么有一些期望（理想情况下与已经存在的 *target* 表的模式相同），但是将此模式提供给 Spark 会导致所有新字段可能因为某些原因在数据的新增量中缺失。此外，如果某个字段的数据类型发生变化，Spark 将会将其读取为 *NULL*（如果这两种数据类型不兼容），或者会丢弃这些行，或者根据指定的 [模式](https://spark.apache.org/docs/latest/sql-data-sources-json.html) 完全失败（另见我的另一篇 [文章](https://medium.com/swlh/notes-about-json-schema-handling-in-spark-sql-be1e7f13839d) 关于 JSON 模式演变）。另一方面，让 Spark 推断模式似乎更合理，但也有一些缺点和需要考虑的点：

+   时间戳将被推断为字符串（除了 Spark 3.0 版本，它将被推断为时间戳）

+   map 类型被推断为结构体

+   如果没有其他逻辑，所有新字段可能会传播到最终表中，这可能是不希望的——有人应该确认新字段是否可以暴露给用户（也许新字段只是由于某些错误产生的）

+   如果没有其他逻辑，如果数据类型与 *target* 表中的类型不同，将增量合并到 *target* 表中将会失败

我们解决这个问题的方法是使用我们内部开发的架构比较工具。我们为所有数据集的架构建立了版本控制系统（架构注册表）。在处理过程中，我们首先让 Spark 推断架构，然后将其与版本化的架构进行比较。它会检测所有新字段并在一些通知渠道中报告。如果数据类型发生变化，算法会决定这些新类型是否可以安全地转换为版本化类型而不会丢失数据。基于这种比较，我们创建了一个新的架构，并使用这个新架构重新读取数据，将变化的字段转换为版本化字段。

唯一的问题发生在新数据类型无法安全转换时，此时我们会使用新的数据类型，但在通知渠道中报告错误，因为这意味着在*目标*层的后续作业将会因为这个问题而失败。在这种情况下，我们需要手动调查为什么会发生这种情况，是否只是由于某些错误导致的单次更改，或者是数据结构中的永久性更改，这意味着我们需要相应地调整最终表格。

![](img/1b488011a807daf13490281ba833d4c1.png)

我们的架构注册表提供了 API 和 Web 用户界面。在这里，你可以看到来自 Web UI 的截图，其中比较了一个数据源的两个架构版本。版本 1.0.6 中存在而版本 1.0.5 中不存在的新字段以绿色突出显示。

## 数据清洗

在使用演化后的架构读取数据到 DataFrame 并转换类型后，我们应用一系列过滤器，排除某些关键字段（如*id*、*version*、*created_time*等）中具有*NULL*值的记录。这些字段对于数据的正确去重和在*目标*层的最终表格中的正确合并是必要的。我们将这些记录称为损坏记录，并将其保存在一个单独的文件夹中，以便调查为什么缺少这些值。

## 数据去重

如前所述，*stage*中的数据可能包含多个具有相同*id*的记录版本。这是因为个别记录（主要是来自社交网络的帖子）不断变化，我们一天内从社交网络下载这些记录多次。

*stage*层的处理作业在清晨运行，目标是获取前一天的增量（新数据），并为将数据合并到最终表格的*目标*作业做准备。在架构演化和数据清洗之后，我们还需要进行去重，即对于每个*id*，我们将仅保留最新版本：

```py
increment_df = (
  spark.read
  .format('json')
  .option('path', input_path)
  .schema(evolved_schema)
  .load()
  .select('newImage.*')
)

w = Window().partitionBy('id').orderBy(desc('version'))

result_df = (
  increment_df
  .withColumn('r', row_number().over(w))
  .filter(col('r') == 1)
  .drop('r')
)
```

从代码中可以看出，我们仅从*newImage*中选择数据，并使用窗口函数[*row_number*()](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.functions.row_number.html#pyspark.sql.functions.row_number)进行去重（有关在 Spark 中使用窗口函数的详细信息，请参见我另一篇[文章](https://medium.com/towards-data-science/spark-sql-102-aggregations-and-window-functions-9f829eaa7549)）。

# 目标

*目标*层的目标是创建与 DynamoDB、Elasticsearch 和其他内部数据库镜像的表，这些表可以通过标准分析工具直接访问，如 SQL、Spark 中的 DataFrame API 或 Pandas，以及 Python 数据科学生态系统中的所有库都可以在其上使用。请注意，通过直接访问原始数据库实现这一点将非常困难。

从技术上讲，在*目标*中，我们从*阶段*获取 parquet 文件，并将其插入到现有表中。我们还希望为分析师提供准备好的分析查询表，因此在创建表的布局时我们投入了一些精力。表本身只是对文件的某种抽象，我们再次使用 Apache Parquet 格式。

## 表布局

我们使用*写时复制*的概念，这意味着在每次更新时，整个表和所有文件都会被覆盖。这是相当昂贵的，特别是对于大型表，然而，它允许我们保持文件的紧凑和组织，以满足高效读取的要求。

表示社交网络帖子的数据表按时间维度进行分区，典型的分区列是*created_year*和*created_month*，这两个列都是从帖子的*created_time*派生出来的。这种分区加快了数据的读取，因为分析师通常对一些近期数据感兴趣。因此，处理引擎如 Spark 或 Presto 确保只扫描查询的分区，而跳过其他分区。这些表还按列*profile_id*进行分桶，因为它可以与表示个人资料的表在此列上连接。对该列进行聚合和连接的查询不会引起 Spark 中的数据洗牌，这样可以提高此类查询的总体效率。此外，如果查询包含对*profile_id*列的过滤条件，则可以进行分桶修剪（有关分桶的更多信息，请参见我另一篇[文章](https://medium.com/towards-data-science/best-practices-for-bucketing-in-spark-sql-ea9f23f7dd53)）。

## 合并增量

*target*层中的处理作业在*stage*作业后清晨运行，其目标是将前一天的增量合并到*target*表中。我们将增量和表都读取到 DataFrames 中，将它们联合，并使用窗口函数*row_number* — 类似于我们在*stage*中所做的 — 选择将保存到新快照中的最新记录版本。因此，如果特定的*id*已经在表中，它将被来自增量的具有更大*version*的记录所替代：

```py
stage_df = (
  spark.read
  .format('parquet')
  .schema(input_schema)
  .option(input_path)
  .load()
)

target_df = spark.table(table_name)

w = Window().partitionBy('id').orderBy(desc('version'))

result_df = (
  stage_df.unionByName(target_df)
  .withColumn('r', row_number().over(w))
  .filter(col('r') == 1)
  .drop('r')
)
```

然而，如果*stage_df*和*target_df*的模式不同，这种方法将不起作用，因此我们需要再次应用模式演变步骤。

## 模式演变 — 目标层

同样地，在*stage*中我们对模式进行了演变，我们也需要在*target*中进行相同的操作。要成功将增量合并到*target*表中，两个表的模式必须相同。然而，如果增量中添加了新字段，这种情况可能不会成立。

我们使用一种称为*input_schema*的模式来控制这些新字段是否已添加到表中。这个*input_schema*也有版本，如果我们想将新字段推广到表中，我们会创建一个包含这些新字段的新版本的*input_schema*。这是一个手动步骤，考虑到应该有人确认这些新字段可以暴露给数据用户，这似乎是合理的。

接下来，我们还需要将表的模式更改为与增量的模式相同。Spark SQL 提供了一种使用*ALTER TABLE table_name ADD COLUMNS*向表中添加新列的方法，但它没有提供添加嵌套字段的方法。

因此，我们将这一功能自行实现到我们的框架中。在这种情况下，*target*作业会在一个临时位置创建一个具有新模式的空表。之后，我们删除原始表，并将空表指向原始表的数据位置。这样，我们将获得一个具有相同数据但模式已修改的新表：

```py
(
  spark.createDataFrame([], new_schema)
  .write
  .mode('overwrite')
  .option('path', table_location_temp)
  .saveAsTable(table_name_temp)
)

spark.sql('ALTER TABLE table_name_temp SET LOCATION table_location')
spark.sql('MSCK REPAIR TABLE table_name_temp')
spark.sql('DROP TABLE table_name')
spark.sql('ALTER TABLE table_name_temp RENAME TO table_name')
```

## 原子写入

从我们开发数据湖的初期开始，最大的挑战之一就是确保原子写入到位。问题是，当简单地覆盖一个表时...

```py
(
  df.write
  .mode('overwrite')
  .format('parquet')
  .option('path', output_path)
  .saveAsTable(table_name)
)
```

…不是原子的。如果 Spark 作业因任何原因在写入过程中失败，表可能会停止存在，或者 S3 前缀可能会开始包含部分写入的数据。在我们的框架中，我们通过始终将表保存在不同的位置并以新名称*table_name_temp*保存来实现原子性，在写入成功后，我们交换表名称，因此新的快照只有在成功写入后才会对用户可用。如果作业失败，我们不会交换名称，而是从头开始在新位置重新启动过程：

```py
 output_path = posixpath.join(output_path, str(int(time.time())))

(
  df.write
  .mode('overwrite')
  .format('parquet')
  .option('path', output_path)
  .saveAsTable(table_name_tmp)
)
spark.sql('DROP TABLE IF EXISTS table_name')
spark.sql('ALTER TABLE table_name_tmp RENAME TO table_name')
```

我们保存数据的路径包含写入时的时间戳，并且我们始终保留最近的几个快照，这使得所谓的*时间旅行*成为可能——如果我们发现新创建的表因某些原因（由于某些错误）而损坏，我们可以将其指向任何之前的快照。

## 数据质量

*目标* 表中的数据的一致性和质量至关重要，因为这些表直接暴露给数据用户。这可以通过在每次更新后检查表格来实现，以确保数据没有由于代码中的某些错误、原始数据库的错误导出或数据在经过转换步骤后到达最终表时可能发生的其他问题而被破坏。

我们使用的数据质量框架是[Great Expectations](https://greatexpectations.io/)，它可以与 Spark 集成。对于每个数据集，我们可以定义一组期望，这些期望会在创建表的新快照后进行验证。如果一些关键期望未能满足，我们可以将表“时间旅行”到之前的快照，并详细调查失败的期望。

# Mart

尽管从*目标* 表中访问数据相当高效，但对于某些表来说，这并不是很用户友好。这是自然的，因为，如前所述，我们保持*目标* 表中的数据结构与数据在原始数据库中存储的结构相同，而这些数据库不一定是为分析目的存储的。这主要涉及嵌套数据结构中的列，例如数组或结构体。有时，使用 SQL 的人们在查询中转换这些字段可能太繁琐，因此我们创建了从*目标* 表派生的附加表，以便在这些表中转换数据结构，从而更容易进行查询。有时这些表也可以与其他表连接，并且它们是为某些特定用户群体量身定制的。这些派生表被保存在湖泊的*mart* 层中。*mart* 的另一个目的是保留由分析师计算的一些汇总和结果表。

# 环境、作业、协调

在*stage*、*target* 和*mart* 层中的所有处理都是用 PySpark 实现的，并且在 Python 中实现了处理模式演变的自定义逻辑。代码在[Databricks](https://www.databricks.com/)平台上运行，对于每个数据源，我们在*stage* 层和*target* 层都有一个作业，对于某些数据源，还有*mart* 层的作业。所有作业都使用 Apache Airflow 技术进行协调，这使我们能够定义作业之间的依赖关系。

去重、模式演变、原子数据保存等核心逻辑都在类和模块中实现，并编译成一个轮子（wheel），这个轮子被导入到任务中，以便基于软件工程的最佳实践复用代码。

湖中的所有步骤基本上都是自动化的，仅有少数几个与引入新数据源相关的手动步骤。在这种情况下，我们只需在配置文件中添加一些配置参数，并将新数据源的模式添加到模式注册表中。Git 管道会在 Databricks 中创建作业，并通过 Airflow 按照预定的调度启动它们。其他手动步骤与监控和审批新字段以及对无法安全转换为版本化类型的更改数据类型进行调试相关。

如前所述，数据用户可以使用 SQL 或 Spark 中的 DataFrame API 访问*目标*和*数据集市*表，这些 API 可以方便地与 Pandas 集成，从而实现与整个 Python 数据科学和机器学习库生态系统的集成。喜欢使用 SQL 的分析师可以使用[Querybook](https://www.querybook.org/)——一个用户友好的笔记本界面——它连接到 Presto 引擎，而喜欢使用 Spark 的科学家则可以在 Databricks 的笔记本环境中访问表格。

![](img/55e7a234cc00623df4475ec964f357b3.png)

我们的数据湖用户（分析师和研究人员）可以在 Spark 上使用数据科学和机器学习库，或者使用 Presto 引擎用 SQL 查询数据。他们可以选择在 Databricks 和 Querybook 中工作，这两个平台都提供了具有用户友好界面的笔记本环境，适用于各种使用场景。

# 致谢

构建和维护数据湖是整个数据工程团队的协作工作。我想感谢所有同事，不仅感谢在数据湖项目中进行的有益且富有成效的合作，感谢我们在过去四年中提出的大量有趣的想法和意见，还要感谢对本文的审阅和有益的评论。

# 结论

在处理大数据的公司中，拥有一个强大且可靠的数据湖似乎是必需的。传统的数据仓库概念已不再适用，因为数据量庞大，并且来自具有复杂结构的不同数据库和系统。然而，在实现这样的存储系统时，仍然有几个挑战需要应对。

我们在关系数据库中认为理所当然的 ACID 事务在此已不再那么明显，必须处理存储系统中的低级概念，如文件组织。通过一些额外的努力，可以恢复 ACID——我们在原子性方面达到了这一点。另一种方法是使用更先进的表格格式，如 Delta 或 Iceberg，它们提供 ACID。

数据模式的变化，无论是由于导出过程中出现错误还是实体真的发生了变化，都是一个相当棘手的问题，如果不加以谨慎处理，可能会导致数据丢失。我们在内部开发了一个工具，我们考虑将来将其开源。这个工具允许我们对所有数据源进行模式版本控制，以便查看其模式如何演变的历史。它还允许我们比较模式，执行各种操作，并轻松处理模式演变。

维护数据湖需要与数据用户（通常是分析师、研究人员和数据科学家）进行持续讨论。了解对表执行了什么样的查询，使我们能够通过在分区、桶化甚至排序方面使用自定义布局来进行高度优化。
