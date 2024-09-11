# 1.5 年的 Spark 知识总结为 8 个技巧

> 原文：[https://towardsdatascience.com/1-5-years-of-spark-knowledge-in-8-tips-f003c4743083?source=collection_archive---------0-----------------------#2023-12-24](https://towardsdatascience.com/1-5-years-of-spark-knowledge-in-8-tips-f003c4743083?source=collection_archive---------0-----------------------#2023-12-24)

## 我在 Databricks 客户合作中的学习经验

[](https://michaelberk.medium.com/?source=post_page-----f003c4743083--------------------------------)[![Michael Berk](../Images/c79c07ed3973cad1305a1d970aaea0b5.png)](https://michaelberk.medium.com/?source=post_page-----f003c4743083--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f003c4743083--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f003c4743083--------------------------------) [Michael Berk](https://michaelberk.medium.com/?source=post_page-----f003c4743083--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe101cd051ea3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F1-5-years-of-spark-knowledge-in-8-tips-f003c4743083&user=Michael+Berk&userId=e101cd051ea3&source=post_page-e101cd051ea3----f003c4743083---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f003c4743083--------------------------------) · 8 分钟阅读 · 2023 年 12 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff003c4743083&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F1-5-years-of-spark-knowledge-in-8-tips-f003c4743083&user=Michael+Berk&userId=e101cd051ea3&source=-----f003c4743083---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff003c4743083&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F1-5-years-of-spark-knowledge-in-8-tips-f003c4743083&source=-----f003c4743083---------------------bookmark_footer-----------)![](../Images/a363f6cea7c86a50b902af7da7137544.png)

图 1：如何编写 Apache Spark 的技术图示。图片由作者提供。

在 Databricks，我帮助大型零售组织部署和扩展数据及机器学习管道。以下是我在实际工作中学到的 8 个最重要的 [spark](https://spark.apache.org/) 技巧/窍门。

在本文中，我们假设读者对 Spark 及其结构有一般的工作知识，但本文应对所有层次的读者都适用。

让我们深入探讨一下吧！

# 0 — 快速回顾

快速回顾一下 Spark 的功能…

Spark 是一个大数据处理引擎。它接受 Python/Java/Scala/R/SQL 并将这些代码转换为一组高度优化的转换操作。

![](../Images/06c30062d226c82299f50052ff6b6abf.png)

图 2：Spark 驱动程序和工作节点配置。图像作者提供。

在最低层次上，Spark 创建任务，这些任务是 **对数据分区的可并行变换**。这些任务从驱动节点分配到工作节点，工作节点负责利用它们的 CPU 核心来完成变换。通过将任务分配给可能许多的工作节点，Spark 允许我们进行水平扩展，从而支持在单台机器上不可能实现的复杂数据管道。

好吧，希望这些信息不是全新的。不管怎样，在接下来的部分，我们会慢下来一点。这些提示应该对 Spark 的初学者和中级用户都有帮助。

# 1 — Spark 就是一个杂货店

Spark 是复杂的。为了帮助你和其他人理解其结构，我们借用 [排队理论](https://en.wikipedia.org/wiki/Queueing_theory) 中一个非常好的类比：**spark 就像是一个杂货店**。

当考虑 Spark 的分布式计算组件时，有三个主要组件……

+   **数据分区：** 我们数据的行子集。在我们的杂货店中，它们是 **杂货**。

+   **Spark 任务：** 在数据分区上执行的低级变换。在我们的杂货店中，它们是 **顾客**。

+   **核心：** 你处理器中并行工作的部分。在我们的杂货店中，它们是 **收银员**。

就这样！

现在，让我们利用这些概念来讨论一些 Spark 的基础知识。

![](../Images/7c684a57bb5357a02622754e4786b018.png)

图 3：收银员类比的插图，特别是针对数据倾斜。图像作者提供。

如图 3 所示，我们的收银员（核心）一次只能处理一个顾客（任务）。此外，一些顾客有很多杂货（分区行数），如图中收银员 2 处的第一个顾客所示。从这些简单的观察……

+   收银员（核心）越多，你可以并行处理的顾客（任务）就越多。这是 [水平/垂直扩展](https://www.geeksforgeeks.org/horizontal-and-vertical-scaling-in-databases/#)。

+   如果你没有足够的顾客（任务）来充分利用你的收银员（核心），你就会支付收银员的空闲时间。这与 [自动扩展](https://www.databricks.com/blog/2018/05/02/introducing-databricks-optimized-auto-scaling.html)、集群大小和分区大小有关。

+   如果顾客（任务）的杂货（分区行数）差异很大，你会看到收银员的利用率不均。这是 [**数据倾斜**](/data-skew-in-pyspark-783d529a9dd7)。

+   你的收银员（核心）越好，他们处理单个顾客（任务）的速度就越快。这涉及到升级你的处理器。

+   等等。

由于这个类比来源于直接与分布式计算相关的排队理论，它的解释能力非常强！

> 使用这个类比来调试、沟通和开发 Spark。

# 2— 将数据一次性收集到内存中

Spark 初学者最常见的错误是误解了延迟计算。

[懒惰求值](https://medium.com/@think-data/mastering-lazy-evaluation-a-must-know-for-pyspark-pros-ac855202495e) 意味着在调用内存集合之前不会执行任何数据转换。调用集合的方法示例包括但不限于……

+   [.collect()](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.collect.html)：将 DataFrame 载入内存作为 Python 列表。

+   [.show()](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.show.html)：打印 DataFrame 的前 `n` 行。

+   [.count()](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.count.html)：获取 DataFrame 的行数。

+   [.first()](https://spark.apache.org/docs/3.1.1/api/python/reference/api/pyspark.sql.functions.first.html)：获取 DataFrame 的第一行。

最常见的错误集合方法是程序中普遍使用 `.count()`。每次你调用集合时，所有上游的转换都会从头开始重新计算，因此如果你有 5 次 `.count()` 调用，你的程序将呈渐近地运行 5 倍时间。

> Spark 是懒惰求值的！管道应该有一个从源头到目标的单一流动。

# 3— 达到服务级别协议后停止

在与大型组织合作时，一个出乎意料的常见问题是他们忽视了大局，从而以低效的方式优化管道。

这是大多数用例中管道优化的方法……

1.  **问问我们是否需要做这个项目。** 简而言之，考虑一下优化管道实际上能带来什么。如果你期望运行时间提高20%，而管道的运行成本是$100，那么你是否应该投入你极其昂贵的数据工程师的薪资以节省每次运行的$20？也许吧，也许不是。

1.  **寻找代码中的低悬果实。** 在同意做这个项目后，检查代码是否有明显的缺陷。示例包括懒惰求值的误用、不必要的转换和转换顺序不正确。

1.  **利用计算来使任务在服务级别协议下运行。** 在检查代码相对高效后，直接将计算资源投入问题中，以便你可以 1) 达到服务级别协议，并且 2) 从 Spark UI 收集统计数据。

1.  **停止。** 如果你已经充分利用了计算资源，并且成本不算过高，做一些最后的计算改进后就停下来。你的时间是宝贵的。不要浪费时间去节省美元，而是在其他地方创造数千美元。

1.  **深入挖掘。** 最后，如果你真的需要深入挖掘，因为成本不可接受，那么就卷起袖子，优化数据、代码和计算。

这个框架的美妙之处在于，1–4只需要对Spark有基本了解，并且执行非常快速；有时你可以在30分钟的电话会议中收集1–4的相关信息。该框架还确保我们会在“足够好”时立即停止。最后，如果需要第5步，我们可以将其委托给团队中最强的Spark专家。

> 通过找出所有避免过度优化管道的方法，你可以节省宝贵的开发时间。

# 4—磁盘溢出

磁盘溢出是Spark作业运行缓慢的最常见原因。

这是一个非常简单的概念。Spark设计用于利用内存处理。如果内存不足，Spark将尝试将额外的数据写入磁盘以防止进程崩溃。这称为磁盘溢出。

![](../Images/8965e74adda45122e1a8357d1745c172.png)

图4：突出显示磁盘溢出的Spark UI屏幕截图。图片由作者提供。

写入和读取磁盘速度较慢，因此应尽量避免。如果你想了解如何识别和减轻溢出，请参考[这个教程](https://selectfrom.dev/spark-performance-tuning-spill-7318363e18cb)。然而，一些非常常见且简单的方法来减轻溢出是…

1.  每个任务处理更少的数据，这可以通过更改分区数量来实现，方法是通过[spark.shuffle.partitions](https://spark.apache.org/docs/latest/sql-performance-tuning.html#other-configuration-options)或[repartition](https://spark.apache.org/docs/3.1.3/api/python/reference/api/pyspark.sql.DataFrame.repartition.html)。

1.  增加计算中的RAM与核心的比率。

> 如果你希望你的作业运行得更佳，请防止溢出。

# 5—使用SQL语法

无论你使用Scala、Java、Python、SQL还是R，Spark在底层总是利用相同的转换。因此，选择适合你任务的语言。

对于许多操作，SQL是支持的Spark语言中最简洁的“语言”！更具体地说：

+   如果你在添加或修改列，请使用[selectExpr](https://spark.apache.org/docs/3.1.3/api/python/reference/api/pyspark.sql.DataFrame.selectExpr.html)或[expr](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.functions.expr.html)，尤其是与Python的[f-strings](https://realpython.com/python-f-strings/)配合使用。

+   如果你需要复杂的SQL，创建临时视图然后使用[spark.sql()](https://spark.apache.org/docs/latest/api/sql/index.html)。

这里有两个快速示例…

```py
# Column rename and cast with SQL
df = df.selectExpr([f"{c}::int as {c}_abc" for c in df.columns])

# Column rename and cast with native spark
for c in df.columns:
    df = df.withColumn(f"{c}_abc", F.col(c).cast("int")).drop(c)
```

```py
# Window functions with SQL
df.withColumn("running_total", expr(
  "sum(value) over (order by id rows between unbounded preceding and current row)"
))

# Window functions with native spark
windowSpec = Window.orderBy("id").rowsBetween(Window.unboundedPreceding, Window.currentRow)
df_with_running_total_native = df.withColumn("running_total", F.sum("value").over(windowSpec))
```

> 使用SQL。

# 6—全局过滤器

你需要读取存储在复杂目录中的一堆数据文件吗？如果是这样，请使用Spark极其强大的[读取选项](https://spark.apache.org/docs/latest/sql-data-sources-generic-options.html#generic-file-source-options)。

我第一次遇到这个问题时，我重写了[os.walk](https://www.tutorialspoint.com/python/os_walk.htm)以与数据存储在的云提供商一起使用。我非常自豪地向我的项目伙伴展示了这个方法，他只是说，“让我分享我的屏幕，”然后向我介绍了全局过滤器。

```py
# Read all parquet files in the directory (and subdirectories)
df = spark.read.load(
  "examples/src/main/resources/dir1",
  format="parquet", 
  pathGlobFilter="*.parquet"
)
```

当我应用上面显示的glob过滤器，而不是我自定义的os.walk时，数据摄取操作的速度提高了超过10倍。

> Spark有强大的参数。在构建定制实现之前，请检查是否存在所需的功能。

# 7 — 使用Reduce与DataFrame.Union

循环几乎总是对spark性能有害。原因如下…

Spark有两个核心阶段 — 规划和执行。在规划阶段，spark创建一个有向无环图（DAG），指示如何执行指定的转换。规划阶段相对昂贵，有时可能需要几秒钟，因此你希望尽可能少地调用它。

让我们讨论一个必须遍历许多DataFrame、执行昂贵转换，然后将它们追加到表中的用例。

首先，几乎所有的迭代使用案例都得到了本地支持，特别是[ pandas UDFs](https://stackoverflow.com/questions/58170261/how-to-use-pandas-udf-in-class)、窗口函数和连接。但如果你确实需要一个循环，下面是如何调用一个单一的规划阶段，从而在一个DAG中完成所有变换。

```py
import functools
from pyspark.sql import DataFrame

paths = get_file_paths()

# BAD: For loop
for path in paths:
  df = spark.read.load(path)
  df = fancy_transformations(df)
  df.write.mode("append").saveAsTable("xyz")

# GOOD: functools.reduce
lazily_evaluated_reads = [spark.read.load(path) for path in paths]
lazily_evaluted_transforms = [fancy_transformations(df) for df in lazily_evaluated_reads]
unioned_df = functools.reduce(DataFrame.union, lazily_evaluted_transforms)
unioned_df.write.mode("append").saveAsTable("xyz")
```

第一个解决方案使用for循环遍历路径，进行复杂转换，然后追加到我们感兴趣的delta表中。在第二种方法中，我们存储一个惰性计算的DataFrame列表，对它们进行转换，然后通过合并进行简化，执行一个spark计划并写入。

我们实际上可以通过Spark UI看到后端架构的差异…

![](../Images/d94967682a2201899b5b7897edb06847.png)

图5：for循环与functools.reduce的spark DAG。图片由作者提供。

在图5中，左侧对应于for循环的DAG将有10个阶段。然而，右侧对应于`functools.reduce`的DAG将只有一个阶段，因此可以更容易地进行并行处理。

对于读取400个唯一delta表并追加到一个delta表的简单用例，这种方法比for循环快6倍。

> 创造性地创建一个单一的spark DAG。

# 8 — 使用ChatGPT

这不是关于炒作。

Spark是一个成熟且有充分文档的软件。LLMs，特别是GPT-4，擅长将复杂信息提炼成易于理解和简明的解释。自从GPT-4发布以来，我没有做过一个复杂的spark项目而没有依赖GPT-4。

![](../Images/ac29f4bd334512d8294617a5a8e6fe9e.png)

图6：GPT-4对影响spark中数据分区大小的示例输出。图片由作者提供。

然而，显而易见的是，使用LLMs时要小心。你发送到封闭源模型的任何内容都可能成为母公司组织的训练数据 — 确保你不发送任何敏感信息。此外，请验证GPT的输出是否可靠。

> 如果使用得当，LLMs在spark学习和开发中具有颠覆性作用。每月$20是值得的。
