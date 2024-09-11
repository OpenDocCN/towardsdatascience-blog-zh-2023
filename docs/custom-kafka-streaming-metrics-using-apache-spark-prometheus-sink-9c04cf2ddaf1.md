# 使用 Apache Spark PrometheusServlet 的自定义 Kafka 指标

> 原文：[https://towardsdatascience.com/custom-kafka-streaming-metrics-using-apache-spark-prometheus-sink-9c04cf2ddaf1?source=collection_archive---------6-----------------------#2023-02-02](https://towardsdatascience.com/custom-kafka-streaming-metrics-using-apache-spark-prometheus-sink-9c04cf2ddaf1?source=collection_archive---------6-----------------------#2023-02-02)

## 在 Apache Spark 中使用 PrometheusServlet 创建和暴露自定义 Kafka Consumer Streaming 指标

[](https://medium.com/@vitorf24?source=post_page-----9c04cf2ddaf1--------------------------------)[![Vitor Teixeira](../Images/db450ae1e572a49357c02e9ba3eb4f9d.png)](https://medium.com/@vitorf24?source=post_page-----9c04cf2ddaf1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9c04cf2ddaf1--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9c04cf2ddaf1--------------------------------) [Vitor Teixeira](https://medium.com/@vitorf24?source=post_page-----9c04cf2ddaf1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b05068b69d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcustom-kafka-streaming-metrics-using-apache-spark-prometheus-sink-9c04cf2ddaf1&user=Vitor+Teixeira&userId=6b05068b69d8&source=post_page-6b05068b69d8----9c04cf2ddaf1---------------------post_header-----------) 发布在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----9c04cf2ddaf1--------------------------------) ·6分钟阅读·2023年2月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9c04cf2ddaf1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcustom-kafka-streaming-metrics-using-apache-spark-prometheus-sink-9c04cf2ddaf1&user=Vitor+Teixeira&userId=6b05068b69d8&source=-----9c04cf2ddaf1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9c04cf2ddaf1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcustom-kafka-streaming-metrics-using-apache-spark-prometheus-sink-9c04cf2ddaf1&source=-----9c04cf2ddaf1---------------------bookmark_footer-----------)![](../Images/6bd8f187e7f1a8a29cca8e8787e9ce8f.png)

图片来源：[Christin Hume](https://unsplash.com/@christinhumephoto?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇博客文章中，我将描述如何使用 Kafka 消费者指标创建和增强当前的 Spark Structured Streaming 指标，并通过 Spark 3 的 PrometheusServlet 将其暴露出来，Prometheus 可以直接访问。之前的 Spark 版本中，必须设置 JmxSink/JmxExporter、GraphiteSink/GraphiteExporter 或自定义 sink 将指标部署到 PushGateway 服务器。尽管如此，我们确实无法避免解决方案复杂性的增加，因为我们必须设置与我们的应用程序交互的外部组件，以便它们可以被 Prometheus 抓取。

## 动机

现如今，软件的可观测性比以往任何时候都更为重要。它使我们能够洞察软件内部发生的情况，而无需直接与系统互动。建立在这一可观测性支柱上的一种方式是暴露应用程序指标。当建立在可观测性栈上时，它们允许我们通过警报或仅仅查看仪表板来检测问题，并通过分析指标找到问题的根本原因。
