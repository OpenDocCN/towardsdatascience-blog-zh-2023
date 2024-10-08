# 使用 Apache Spark PrometheusServlet 自定义 Kafka 指标

> 原文：[`towardsdatascience.com/custom-kafka-streaming-metrics-using-apache-spark-prometheus-sink-9c04cf2ddaf1`](https://towardsdatascience.com/custom-kafka-streaming-metrics-using-apache-spark-prometheus-sink-9c04cf2ddaf1)

## 使用 PrometheusServlet 在 Apache Spark 中创建和暴露自定义 Kafka 消费者流指标

[](https://medium.com/@vitorf24?source=post_page-----9c04cf2ddaf1--------------------------------)![Vitor Teixeira](https://medium.com/@vitorf24?source=post_page-----9c04cf2ddaf1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9c04cf2ddaf1--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----9c04cf2ddaf1--------------------------------) [Vitor Teixeira](https://medium.com/@vitorf24?source=post_page-----9c04cf2ddaf1--------------------------------)

·发表于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----9c04cf2ddaf1--------------------------------) ·阅读时间 6 分钟·2023 年 2 月 2 日

--

![](img/6bd8f187e7f1a8a29cca8e8787e9ce8f.png)

图片由 [Christin Hume](https://unsplash.com/@christinhumephoto?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇博客文章中，我将描述如何创建和增强当前 Spark Structured Streaming 指标，结合 Kafka 消费者指标，并通过 Spark 3 的 PrometheusServlet 暴露这些指标，使 Prometheus 可以直接访问。在之前的 Spark 版本中，必须设置 JmxSink/JmxExporter、GraphiteSink/GraphiteExporter 或自定义的 sink，将指标部署到 PushGateway 服务器。也就是说，我们无法避免解决方案复杂性的增加，因为我们必须设置与应用程序交互的外部组件，以便 Prometheus 可以抓取这些数据。

## 动机

比以往任何时候都更加重要的是，软件的可观察性变得必不可少。它使我们能够在不直接与系统交互的情况下，洞察软件内部发生了什么。建立在这一可观察性支柱上的一种方法是暴露应用程序指标。当这些指标建立在可观察性堆栈上时，它们可以帮助我们通过警报或查看仪表板来检测问题，并通过分析指标找到其根本原因。

Apache Spark 应用程序也不例外。确实可以访问[Spark Web UI](https://spark.apache.org/docs/latest/web-ui.html)并了解我们的应用程序如何运行，但当应用程序数量增加到十倍或一百倍时，排查问题变得困难。这时像[Grafana](https://grafana.com/)这样的可观察性工具就派上用场了。Grafana 能够连接到 Prometheus 数据库，Prometheus 通过目标 PrometheusServlet 与我们的应用程序无缝集成。

配置后，Apache Spark 原生暴露了几个度量，详细信息见[这里](https://spark.apache.org/docs/latest/monitoring.html#list-of-available-metrics-providers)。在 Structured Streaming 中，默认情况下不暴露任何度量，除非我们设置 `"spark.sql.streaming.metricsEnabled" -> "true"`。以下是 Kafka 流作业暴露的度量示例：

![](img/c6e2a17b134c1fc4bcfee302c98f8781.png)

默认的 Spark Structured Streaming 度量

如我们所见，这些度量非常通用，并未提供关于我们源的详细信息。

目标是能够暴露 Kafka 消费者度量，帮助我们监控事件消费的情况。

# 定义自定义度量

度量应该是可以量化的值，提供关于应用程序状态或性能的实时洞察。在本文的范围内，我们将涵盖以下度量：

+   **起始偏移量：** 流查询首次启动的偏移量。

+   **结束偏移量：** 流查询处理的最后偏移量。跟踪查询的消费者进度。

+   **领先偏移量：** 流查询正在消费的主题的最新偏移量。跟踪被消费主题的演变。

+   **滞后偏移量：** 最后处理的偏移量与主题的领先偏移量之间的差异。跟踪流查询与实时之间的差距。

+   **消耗速率：** 流查询主题的消耗速率。它是所有在流查询中订阅的主题的总和。

+   **最后记录时间戳：** 从每个 TopicPartition 消费的最后消息时间戳。跟踪生产者和消费者之间的延迟。

# 创建度量源

下一步，在定义度量之后，是创建度量源，负责将度量暴露给 Spark 的 MetricsSystem。

为了暴露度量，我们需要创建一个扩展了 `Source` 的类。这将创建一个执行器连接到驱动程序，将度量作为心跳过程的一部分传递。

KafkaMetricsSource 实现

确保将仪表定义为 `SettableGauge`，以便在执行过程中能够更新它们。

在定义源之后，我们只需实例化源并在 Spark 的 MetricsSystem 中注册它。

源注册的简化版本

完整的源实现代码可以查看：

[## spark-prometheus-kafka-metrics/KafkaMetricsSource.scala at master ·…](https://github.com/Orpheuz/spark-prometheus-kafka-metrics/blob/master/src/main/scala/org/apache/spark/metrics/KafkaMetricsSource.scala?source=post_page-----9c04cf2ddaf1--------------------------------)

### 使用 Spark 3 PrometheusServlet 增强的 Kafka Streaming 消费者指标的 Spark Streaming 应用程序…

[github.com](https://github.com/Orpheuz/spark-prometheus-kafka-metrics/blob/master/src/main/scala/org/apache/spark/metrics/KafkaMetricsSource.scala?source=post_page-----9c04cf2ddaf1--------------------------------)

# 将 Streaming Query Progress 作为信息来源

现在我们已经有了我们的源，接下来需要做的是使用它。为此，我们需要这些指标来填充我们最近创建的仪表。

如果你之前运行过结构化 Streaming 作业，你可能会注意到，当 Streaming Query 进展时，会有类似下面的输出：

通过分析输出，我们可以看到，通过解析 JSON 已经可以获得大部分指标。开始偏移量指标使用`startOffsets`，结束偏移量使用`endOffset`，最新偏移量使用`latestOffset`，`source.inputRowsPerSecond`作为源的消费速率，延迟偏移量则不会使用`metrics`值，因为我们可以通过使用`endOffset`和`latestOffset`值来计算每个 TopicPartition 的延迟，得到更细粒度的指标。

为了利用这些信息，我们可以使用 [StreamingQueryListener](https://spark.apache.org/docs/latest/api/java/org/apache/spark/sql/streaming/StreamingQueryListener.html) 的 `onProgress` 事件。

第一步是创建一个扩展`StreamingQueryListener`的类，以便我们能够接收并处理进度事件。它接收我们之前创建的`KafkaMetricsSource`，负责发出这些指标。

KafkaOffsetsQueryListener 代码片段

最后一步只是将监听器注册到我们希望接收更新的流中。

如果你希望查看完整的监听器代码，可以在这里查看：

[## spark-prometheus-kafka-metrics/KafkaOffsetsQueryListener.scala at master ·…](https://github.com/Orpheuz/spark-prometheus-kafka-metrics/blob/master/src/main/scala/listeners/KafkaOffsetsQueryListener.scala?source=post_page-----9c04cf2ddaf1--------------------------------)

### 使用 Spark 3 PrometheusServlet 增强的 Kafka Streaming 消费者指标的 Spark Streaming 应用程序…

[github.com](https://github.com/Orpheuz/spark-prometheus-kafka-metrics/blob/master/src/main/scala/listeners/KafkaOffsetsQueryListener.scala?source=post_page-----9c04cf2ddaf1--------------------------------)

# 执行器指标 — 记录时间戳

最后一个指标是每个 `TopicPartition` 的最后处理记录时间戳。这个指标比之前的更复杂，因为信息存在于 Spark 执行器的计算时间中。

我们唯一能访问这些信息的方法是创建一个作为副作用的虚拟 Spark 表达式。我们将使用来自[Spark 的 Kafka 模式](https://spark.apache.org/docs/latest/structured-streaming-kafka-integration.html#creating-a-kafka-source-for-streaming-queries)的`value`列来欺骗 Spark 执行此转换。

KafkaTimestampMetrics 表达式

这个表达式接收整个 Kafka 行，提取必要的值，并发出指标。它将返回`value`列，正如我们上面所说的，它将用于欺骗 Spark 执行此表达式。

表达式使用

如果我们例如使用了`timestamp`列，并且在查询的后续部分选择了`value`列，Spark 会将此表达式从最终计划中截断，我们的指标将被忽略。如果你不打算使用`value`列（这比较少见），请确保使用适当的列。

# 配置 PrometheusServlet

设置好一切后，我们只需通过将`spark.ui.prometheus.enabled=true`设置为启用 UI 上的 Prometheus（它创建了一个包含所有驱动程序和执行程序指标的单一端点）并配置`spark.metrics.conf`以满足所需的配置。

运行应用程序并访问[`localhost:4040/metrics/prometheus`](http://localhost:4040/metrics/prometheus)将显示我们之前创建的所有指标以及原生指标。

![](img/d701a801bb6a8f14be5d254d90ea717a.png)

/metrics/prometheus/ 端点上暴露的指标

# 原生 Prometheus 集成的局限性

这个新功能有一些局限性。其中之一是这个端点只暴露以`metrics_`或`spark_info`开头的指标。此外，Spark 不遵循 Prometheus 命名约定，并且当前不支持标签（如果你知道某种方法，请告诉我！）。这意味着我们将在 Prometheus 中拥有大量不同的指标，这可能会有些令人不知所措。这个问题可以通过重新标记指标来解决，但可能会比较麻烦。

# 结论

就这些！你现在可以使用 Spark 的 Prometheus 原生集成创建和暴露 Kafka 自定义指标。现在，只剩下让 Prometheus 抓取所有端点，并使用这些指标创建漂亮的仪表板和报警，以帮助你更好地了解应用程序内部发生的情况。

尽管 Prometheus 指标接收器早期的局限性是可以理解的，因为它是一个*实验性*功能，但我相信它将很快通过更多自定义进行增强。

如果你想查看本文中使用的完整项目，请检查：

[](https://github.com/Orpheuz/spark-prometheus-kafka-metrics?source=post_page-----9c04cf2ddaf1--------------------------------) [## GitHub - Orpheuz/spark-prometheus-kafka-metrics: 提升了 Kafka 的 Spark Streaming 应用程序…

### 目前无法执行该操作。您在另一个标签页或窗口中登录了。您在另一个标签页或窗口中注销了…

[github.com](https://github.com/Orpheuz/spark-prometheus-kafka-metrics?source=post_page-----9c04cf2ddaf1--------------------------------)
