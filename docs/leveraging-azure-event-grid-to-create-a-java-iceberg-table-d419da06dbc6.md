# 利用 Azure Event Grid 创建 Java Iceberg 表

> 原文：[https://towardsdatascience.com/leveraging-azure-event-grid-to-create-a-java-iceberg-table-d419da06dbc6?source=collection_archive---------29-----------------------#2023-01-10](https://towardsdatascience.com/leveraging-azure-event-grid-to-create-a-java-iceberg-table-d419da06dbc6?source=collection_archive---------29-----------------------#2023-01-10)

## 我们将使用 Azure Event Grid 实现事件驱动架构

[](https://medium.com/@jean-claude.cote?source=post_page-----d419da06dbc6--------------------------------)[![Jean-Claude Cote](../Images/aea2df9c7b95fc85cc336f64d64b0a76.png)](https://medium.com/@jean-claude.cote?source=post_page-----d419da06dbc6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d419da06dbc6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d419da06dbc6--------------------------------) [Jean-Claude Cote](https://medium.com/@jean-claude.cote?source=post_page-----d419da06dbc6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F444ed0089012&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleveraging-azure-event-grid-to-create-a-java-iceberg-table-d419da06dbc6&user=Jean-Claude+Cote&userId=444ed0089012&source=post_page-444ed0089012----d419da06dbc6---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d419da06dbc6--------------------------------) ·6 min read·2023年1月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd419da06dbc6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleveraging-azure-event-grid-to-create-a-java-iceberg-table-d419da06dbc6&user=Jean-Claude+Cote&userId=444ed0089012&source=-----d419da06dbc6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd419da06dbc6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleveraging-azure-event-grid-to-create-a-java-iceberg-table-d419da06dbc6&source=-----d419da06dbc6---------------------bookmark_footer-----------)![](../Images/c4e53807053d374896c74f3522697b71.png)

图片由 Jackson Case 提供于 Unsplash

在 [我们之前的文章](https://medium.com/towards-data-science/streaming-iceberg-table-an-alternative-to-kafka-be54a1624917) 中，我们展示了 Iceberg 表如何作为 Kafka 主题。我们展示了独立的 Java 写入器如何并行生成 parquet 文件，同时单个 Bookkeeper 将这些数据文件附加到 [Iceberg](https://iceberg.apache.org/) 表中。Bookkeeper 通过创建 Iceberg 提交来完成这一操作。

Bookkeeper 需要识别新创建的数据文件是什么。然后，它将这些文件注册到 Iceberg 表中。在我们之前的文章中，我们保持简单，并选择实现基于文件的消息通道。对于每个创建的数据文件，Writers 会在一个众所周知的文件夹中创建一个 Moniker。Bookkeeper 监视这个众所周知的文件夹，并定期读取这些 Monikers。Bookkeeper 使用这些 Monikers 中的信息将新数据文件附加到 Iceberg 表中。

之前的解决方案完全基于文件。它不依赖于任何外部服务。然而，基于文件的解决方案需要大量额外的创建/删除文件操作，并且需要昂贵的数据湖列出文件操作。

这是我们之前基于文件的解决方案的高层次图示。

![](../Images/44376c3ecf621c7a345a28dcc851d3cb.png)

除非另有说明，所有图像均为作者提供

在这篇文章中，我们将尝试通过利用[Azure Event Grid](https://learn.microsoft.com/en-us/azure/event-grid/overview)服务来改进这个解决方案。Event Grid 是一个事件中介，你可以使用它来实现事件驱动架构。在事件驱动架构中，事件用于传输状态变化。

我们感兴趣的状态变化是当 Java Writers 向数据湖添加新文件时的文件创建事件。

这些事件的消费者是 Bookkeeper，它将新数据文件附加到 Iceberg 表中。

这是基于队列的解决方案的高层次图示。

![](../Images/65d221b1569b5ca0a31e73b8d4d23938.png)

除非另有说明，所有图像均为作者提供

## 配置

一个 Azure 存储账户由四项服务组成：容器、文件共享、队列和表。我们感兴趣的两个数据存储服务是容器和[队列](https://learn.microsoft.com/en-us/azure/storage/queues/storage-queues-introduction)。容器是存储 blob 文件的地方。队列是我们将发送文件创建事件的地方。

![](../Images/14e2c989baa48ce027ad6483d8a00701.png)

因此，我们配置 Event Grid 以便每当在特定存储容器内创建文件时发出事件。我们配置 Event Grid 将这些文件创建事件发布到存储队列。

由于我们只对文件创建事件感兴趣，因此我们配置 Event Grid 仅发送“Blob Created”事件。

![](../Images/ccb4340b6bd1b03ec391ebc9314ed384.png)

此外，我们只对在特定路径下创建的文件以及具有特定文件扩展名的文件感兴趣。

![](../Images/567005f9ff1ac2b3ad016e44ebfc0405.png)

最后，我们可以通过指定高级筛选器来进一步缩小我们将处理的事件。当 Java Writer 完成写入文件时，它会调用 REST 接口 `FlushWithClose`。在高级筛选器中，我们指定 `data.api` 等于 `FlushWithClose`。

我们可以通过指定包含子字符串的`subject`来定位特定的Iceberg表或所有Iceberg表。在我们的案例中，我们使用了`/data/`，这是所有Iceberg表使用的数据文件夹。

![](../Images/87d00eac98fee6766dc8ce0f5cc8e1e8.png)

当Java Writers在`/data/`文件夹内创建parquet文件时，事件会发布到存储队列。我们可以使用Azure门户监控Event Grid中的活动。特别是，我们可以看到已交付的事件是所有发布事件的一个子集。

![](../Images/1f9b567ea15b1fdbb43e5591829fd7f8.png)

## Writers

Writers的实现基本上与我们之前的基于文件的解决方案相同。事实上，Writers甚至更简单，因为它们不再需要自己发布通知。Azure Event Grid处理发布通知的任务。Writers只需要写入parquet文件。

## 记账员

记账员仍然负责将新创建的数据文件注册到Iceberg表中。然而，它现在通过读取`FlushWithClose`事件的队列来确定需要追加的文件。我们像这样创建一个Java存储队列客户端

```py
DataLakeTokenCredential token = new DataLakeTokenCredential();
String queueURL = Constants.hadoopConf.get("queue.location");
String queueName = Constants.hadoopConf.get("queue.name");
this.queueClient = new QueueClientBuilder()
                      .endpoint(queueURL)
                      .credential(token)
                      .queueName(queueName)
                      .buildClient();
```

注意使用我们的`DataLakeTokenCredential`类。由于存储队列和数据湖（容器）都位于同一个存储帐户内，我们利用相同的认证机制。

检索事件就像调用`receiveMessages`一样简单。

```py
List<QueueMessageItem> messages = queueClient.receiveMessages(MAX_MESSAGES,
                    Duration.ofSeconds(60), Duration.ofSeconds(10))
                    .collect(Collectors.toList());
```

每条消息都是一个带有文件路径和文件大小的`FlushWithClose`。这些消息是JSON，格式如下：

```py
"data": {
  "api": "FlushWithClose",
  "contentType": "application/x-compressed",
  "contentLength": 115739,
  "blobType": "BlockBlob",
  "blobUrl": "https://<accountname>.blob.core.windows.net/users/iceberg/schema/data/file.parquet",
  "url": "https://<accountname>.blob.core.windows.net/users/iceberg/schema/data/file.parquet",
},
```

在我们之前的基于文件的解决方案中，Monikers（消息）由Java Writers写入，并包含序列化的Iceberg `DataFile`对象。因此，记账员可以读取这些对象并直接执行以下操作：

```py
List<DataFile> dataFiles = readMonikers();
AppendFiles append = this.table.newAppend();
for (DataFile dataFile : dataFiles) {
      append.appendFile(dataFile);
}
append.commit();
```

现在我们使用Azure Event Grid，我们无法控制消息的内容。因此，记账员需要使用Event Grid提供的文件路径和文件大小信息创建Iceberg `DataFile`对象。

我们缺少parquet指标；幸运的是，Iceberg提供了从parquet文件中检索指标的必要API。

```py
InputFile in = this.table.io().newInputFile(f);
Metrics metrics = ParquetUtil.fileMetrics(in, 
                      MetricsConfig.forTable(this.table));
```

感谢Samrose Ahmed向我展示了这个技巧。这里是他的博客[无服务器异步Iceberg数据摄取](https://www.matano.dev/blog/2022/09/13/ingesting-data-iceberg)，使用了AWS SQS（AWS等同于Azure Event Grid）。

我们现在拥有创建Iceberg `DataFile`对象并将其提交到Iceberg表所需的所有信息。

```py
AppendFiles append = this.table.newAppend();
DataFile dataFile = DataFiles.builder(this.partitionSpec)
                    .withPath(filePath)
                    .withFileSizeInBytes(fileSize)
                    .withFormat("PARQUET")
                    .withMetrics(metrics)
                    .build();
append.appendFile(dataFile);
append.commit();
```

## 性能

当我们对这个实现进行性能分析时，我们很快意识到大部分时间花费在从parquet文件中检索指标上。检索parquet文件指标非常高效，仅涉及读取parquet文件的尾部信息。然而，每个文件都需要执行一次REST调用。这些REST调用的延迟会迅速累积。幸运的是，可以通过使用Java执行器服务并行化这些请求来轻松解决这个问题。

```py
List<Callable<DataFile>> callableTasks = monikers.stream()
         .map(m -> new MetricResolver(m, Constants.partitionSpec))
         .collect(Collectors.toList());

List<Future<DataFile>> futures = executorService.invokeAll(callableTasks);
```

## 结果

通过利用执行器服务处理延迟操作，我们可以大幅提高性能。即使在追加大量数据文件（数百个）时，处理消息也不到一秒钟。读取 parquet 指标需要一到三秒钟。

唯一不能通过执行器服务优化的操作是 Iceberg 提交操作。尽管该操作相当迅速，通常在一到两秒钟之间，但有时在 Iceberg 重新组织清单时，这一操作可能需要长达20秒。

如果我们测量数据文件刷新时间与数据文件提交到 Iceberg 表的时间之间的差异，我们观察到总体延迟从3秒到20秒不等。

## 结论

使用 Azure Event Grid 的结果与我们之前的基于文件的解决方案类似。然而，Event Grid 解决方案提供了更一致的结果。我们不再受制于文件列表数据湖操作，这种操作有时可能需要较长时间，尤其是当数据湖使用频繁时。

我们还估算了使用 Event Grid 的“消息传递”成本约为基于文件的解决方案的1/10。实际数据文件的存储成本在两种解决方案中是相同的。

利用 Azure Event Grid 是通知 Bookkeeper 新创建的数据文件的好方法。然而，如果你的存储设备不支持通知机制，使用基于文件的解决方案也可以行得通。

如果你有兴趣自己评估这些实验，你可以在[这里](https://github.com/cccs-jc/java-iceberg-table)找到这些文章中使用的代码。
