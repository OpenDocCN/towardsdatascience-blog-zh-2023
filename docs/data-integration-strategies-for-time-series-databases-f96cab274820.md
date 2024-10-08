# 时间序列数据库的数据集成策略

> 原文：[`towardsdatascience.com/data-integration-strategies-for-time-series-databases-f96cab274820`](https://towardsdatascience.com/data-integration-strategies-for-time-series-databases-f96cab274820)

## 探讨用于时间序列数据库（TSDB）的流行数据集成策略，包括 ETL、ELT 和 CDC

[](https://yitaek.medium.com/?source=post_page-----f96cab274820--------------------------------)![Yitaek Hwang](https://yitaek.medium.com/?source=post_page-----f96cab274820--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f96cab274820--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f96cab274820--------------------------------) [Yitaek Hwang](https://yitaek.medium.com/?source=post_page-----f96cab274820--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f96cab274820--------------------------------) ·6 分钟阅读·2023 年 1 月 31 日

--

![](img/58b09c28f46fa6ad8943b13548433aec.png)

图片来源：[fabio](https://unsplash.com/@fabioha?utm_source=medium&utm_medium=referral) 于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

随着数字化转型扩展到更多行业，生成的数据点数量正呈指数增长。因此，如何从不同来源以各种格式和结构收集如此大量的数据成为数据工程团队的主要关注点。传统的数据集成方法主要集中在将高度结构化的数据整理到数据仓库中，这些方法难以应对新数据集的体量和异质性。

时间序列数据增加了额外的复杂性。由于时间的推移，每个时间序列数据点的价值会减少，因为数据的粒度在数据陈旧时会失去相关性。因此，团队必须仔细规划数据集成策略到时间序列数据库（TSDB）中，以确保分析能够反映接近实时的趋势和情况。

在这篇文章中，我们将审视一些最流行的时间序列数据库数据集成方法：

+   **ETL**（提取、转换、加载）

+   **提取、加载、转换**（ELT）

+   **数据流处理与 CDC**（变更数据捕捉）

鉴于时间序列数据对实时洞察的需求，现代事件驱动架构通常采用 CDC 进行数据流处理。为了展示其实际操作，我们将通过[QuestDB](https://questdb.io/)（一个快速的 TSDB）来展示 CDC 如何灵活地满足时间序列数据源的需求。

# 提取、转换、加载（ETL）

**ETL** 是一种传统且流行的数据集成策略，它首先将数据转换成预定结构，然后再将数据加载到目标系统（通常是数据仓库）中。

![](img/c7d5a31d9b7a8cd6137b9e2c0d17d0f7.png)

作者提供的图片

**ETL** 的主要优势之一是它提供了最高程度的自定义。由于数据首先被提取到一个临时区域，在那里被转换为干净、标准化的格式，**ETL** 系统可以处理各种格式和结构。此外，一旦数据加载到数据仓库中，数据科学团队可以运行高效的查询和分析。最后，鉴于 **ETL** 生态系统的成熟，选择企业级工具的选项非常丰富。

另一方面，**ETL** 维护起来既耗时又费资源。数据清理和转换的逻辑可能复杂且计算开销大。这也是为什么大多数 **ETL** 系统通常是批处理导向的，只在周期性地将数据加载到仓库中。随着数据量和数据源的增加，这可能会成为瓶颈。

鉴于这些特性，**ETL** 系统通常用于需要在分析之前进行复杂转换逻辑的数据集。它也适合那些不需要实时洞察并可以长期存储用于趋势分析的数据集。

# 提取、加载、转换（**ELT**）

正如名称所示，**ELT** 首先将数据加载到目标系统（通常是数据湖）中，并在系统内部进行转换。由于目标系统负责处理快速加载和转换，**ELT** 流水线通常利用现代的、基于云的数据湖，以应对处理需求。

![](img/d4a554c0daf7d71f8254d42f4ced5db5.png)

作者提供的图片

与 **ETL** 流水线相比，**ELT** 系统可以提供更多实时的数据分析，因为原始数据会被实时摄取和转换。大多数基于云的数据湖提供 SDK 或端点，以高效摄取微批量数据并提供几乎无限的可扩展性。然而，**ELT** 并非没有缺点。由于转换由目标系统完成，这些操作受限于数据湖支持的功能。如果需要更复杂的转换逻辑，可能需要额外的步骤来重新提取数据并以更友好的格式存储。

对于大多数使用场景而言，**ELT** 是比 **ETL** 更高效的数据集成策略。如果你的应用程序可以利用基于云的工具并且不需要复杂的处理，**ELT** 可以是处理大量数据的极佳选择，接近实时完成处理。

# 更改数据捕获（**CDC**）

对于新项目，团队可以计划将 TSDB 作为 ETL 或 ELT 管道中的目标系统之一。然而，对于现有项目，无论是迁移到 TSDB 还是将 TSDB 添加到系统中都可能面临挑战。一方面，可能需要修改或使用新的驱动程序/SDK 来将数据流式传输到 TSDB。即使支持相同的驱动程序，数据格式也可能需要更改以充分利用 TSDB 的功能。CDC 工具可以有效地弥合这一差距。

CDC 在原则上很简单：CDC 工具如 Debezium 持续监控源系统中的变化，并在发生变化时通知你的数据管道。引起变化的应用程序通常甚至不知道有 CDC 进程在监听变化。这使得 CDC 非常适合将新的实时数据管道集成到现有架构中，因为它对现有应用程序的更改很少或没有。因此，CDC 可以与 ETL 或 ELT 管道一起使用。例如，源系统可以是 SQL RDBMS（例如 MySQL、PostgreSQL 等）或 NoSQL 数据库（例如 MongoDB、Cassandra），而目标系统之一可以是 TSDB 以及其他数据湖或数据仓库。

![](img/fd1e89ed545b347b5ace7779fc175dc8.png)

图片来源：作者

使用 CDC（Change Data Capture）进行数据集成的主要优势在于它提供了实时数据复制。与传统的批量处理 ETL 和 ELT 系统不同，CDC 会将源系统的变化持续流式传输到一个或多个目标系统。这对于在多个数据库之间近实时地复制部分数据或全部数据非常有用。目标数据库甚至可能位于不同的地理区域或服务于不同的目的（即长期存储与实时分析）。对于时间序列数据，随着时间变化的数值通常最为有用，CDC 能有效捕捉这些增量数据，以便提供实时洞察。

# CDC 的参考实现

为了更具体地说明 CDC 如何工作，我们可以看一下我最近写的[参考实现](https://github.com/questdb/kafka-questdb-connector)，它将股票价格流式传输到 QuestDB。

![](img/2818fe615a4f7ec0ffd9fba5adeaebec.png)

图片来源：作者

在高层次上，一个 Java Spring 应用将股票价格信息发布到 PostgreSQL。然后 Debezium Connector 从 PostgreSQL 中读取这些变化，并将其发布到 Kafka。另一方面，QuestDB 的 Kafka Connector 从 Kafka 主题中读取数据，并将其流式传输到 QuestDB。

要深入了解，请参阅：

[](https://questdb.io/blog/2023/01/03/change-data-capture-with-questdb-and-debezium?source=post_page-----f96cab274820--------------------------------) [## 使用 QuestDB 和 Debezium 进行变更数据捕获 | QuestDB：时间序列数据库]

### 在本教程中，我们的社区贡献者 Yitaek Hwang 向我们展示了如何使用变更数据流将数据传输到 QuestDB。

[questdb.io](https://questdb.io/blog/2023/01/03/change-data-capture-with-questdb-and-debezium?source=post_page-----f96cab274820--------------------------------)

在这个参考架构中，Java Spring 应用程序在将数据复制到 TSDB 进行进一步分析之前，将数据转换并加载到 PostgreSQL 中。如果需要更类似 ELT 的管道，可以将来自其他提供商的原始数据直接加载到 PostgreSQL 中，并在 QuestDB 中进行转换。

这个架构中需要注意的关键点是，CDC 可以无缝集成到现有系统中。从应用程序的角度来看，它可以保留 PostgreSQL 的事务保证，同时在管道中添加一个新的 TSDB 组件。

# 结论

数据集成在使用 TSDB 来存储和分析时间序列数据的组织中扮演着重要角色。在本文中，我们探讨了使用 ETL 或 ELT 的一些优缺点。我们还研究了如何将 CDC 与这些管道结合使用，以实现对 TSDB 的实时复制。

考虑到时间序列数据的特殊性质，使用 TSDB 来正确存储和分析这些数据非常重要。如果你是从零开始，考虑建立一个 ELT 管道，将数据流入 TSDB。要与现有系统集成，可以利用 CDC 工具以减少对当前架构的干扰。
