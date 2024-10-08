# 数据管道设计模式

> 原文：[`towardsdatascience.com/data-pipeline-design-patterns-100afa4b93e3`](https://towardsdatascience.com/data-pipeline-design-patterns-100afa4b93e3)

## 选择合适的架构及其示例

[](https://mshakhomirov.medium.com/?source=post_page-----100afa4b93e3--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----100afa4b93e3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----100afa4b93e3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----100afa4b93e3--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----100afa4b93e3--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----100afa4b93e3--------------------------------) ·9 分钟阅读·2023 年 1 月 2 日

--

![](img/074dc1570ad17f76947283225050b8db.png)

照片由 [israel palacio](https://unsplash.com/@othentikisra?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

通常数据在多个步骤中被处理、提取和转换。因此，一系列数据处理阶段可以称为**数据管道**。

> *选择哪个设计模式？*

需要考虑许多因素，即**使用哪个数据栈？考虑哪些工具？如何从概念上设计数据管道？ETL 还是 ELT？也许是 ETLT？什么是变更数据捕获？**

我将在这里尝试涵盖这些问题。

# 一个数据管道

所以它是一个数据处理步骤的序列。由于这些阶段之间的***逻辑数据流连接***，每个阶段都会生成一个**输出**，作为下一个阶段的**输入**。

> 每当在 A 点和 B 点之间进行数据处理时，就会有一个数据管道。

数据管道的三个主要部分是**源、处理步骤或步骤、以及目标**。从外部 API（源）提取的数据可以加载到数据仓库（目标）中。这是一个最常见的数据管道的例子，其中源和目标是不同的。然而，这并非总是如此，因为*目标到目标*的管道也存在。

例如，数据可以最初作为数据仓库中的`reference`表格存在，然后经过一些数据转换后，可以*落地*到一个新的模式中，例如，在`analytics`中用于报告解决方案。

> *每当在源和目标之间处理数据时，总会有一个数据管道。*

![](img/02c0d6d925b544a4cc7d82adbc9c1da9.png)

数据管道。图片由作者提供

由后端仅一个`source`创建的事件数据，通过使用 Kinesis Firehose 或 Kafka 流构建的事件流，可以提供给多个不同的`destinations`。

考虑来自*Google Analytics*的***用户参与数据***，因为它作为事件流流动，可以用于用户活动的分析仪表板以及用于流失预测的机器学习（ML）管道中。

尽管使用相同的数据源，但两个管道独立操作，必须成功完成后用户才能看到结果。

或者，可以将来自两个或更多`source`位置的数据汇总到一个`destination`中。例如，来自不同支付商提供商的数据可以转换为业务智能（BI）仪表板的收入报告。

> 数据质量检查、数据清洗、转换、丰富、过滤、分组、聚合以及对数据应用算法是数据管道中的常见步骤。

# 架构类型和示例

数据管道架构这一术语根据情况可能有多种含义。一般来说，它可以分为**概念性**（逻辑）和**平台**级别或架构类型。

**概念性**逻辑部分描述了数据集从采集到服务的处理和转换方式，而**平台架构**则专注于特定场景中使用的一组工具和框架，以及它们各自的功能。

这是数据仓库管道的**逻辑结构**：

![](img/64499bbb581656e8ea35b7133f50d63d.png)

概念性数据管道设计。图片由作者提供

在这篇文章中，我找到了一种从 Firebase/Google Analytics 4 中提取实时数据并将其加载到 BigQuery 的方法：

[## 如何从 Google Analytics 4 和 Firebase 中提取实时的日内数据并加载到 BigQuery](https://example.org/how-to-extract-real-time-intraday-data-from-google-analytics-4-and-firebase-in-bigquery-65c9b859550c?source=post_page-----100afa4b93e3--------------------------------)

### 并且始终为您的自定义报告保持最新的数据

[towardsdatascience.com](https://example.org/how-to-extract-real-time-intraday-data-from-google-analytics-4-and-firebase-in-bigquery-65c9b859550c?source=post_page-----100afa4b93e3--------------------------------)

这是一个概念性数据湖管道示例：

![](img/b08c6ce7fe26c3b711572352fe360af2.png)

概念性数据管道设计。图片由作者提供

例如，在之前的帖子中，我写了如何从 MySQL 数据库中提取数据，将其保存到 Cloud Storage，以便稍后进行分析：

[## MySQL 数据连接器用于您的数据仓库解决方案](https://example.org/mysql-data-connector-for-your-data-warehouse-solution-db0d338b782d?source=post_page-----100afa4b93e3--------------------------------)

### 如何构建一个并分块导出数百万行、流式传输、捕获实时数据变化或提取数据并保存……

[towardsdatascience.com](https://example.org/mysql-data-connector-for-your-data-warehouse-solution-db0d338b782d?source=post_page-----100afa4b93e3--------------------------------)

这是一个平台级架构示例：

![](img/0a7917159d66471313aedd34549ca170.png)

平台级数据管道设计。图片由作者提供

这是许多湖仓架构解决方案中非常常见的模式。在这篇博客文章中，我创建了一个定制的数据摄取管理器，它在 Cloud Storage 中创建新对象事件时被触发：

[](/how-to-handle-data-loading-in-bigquery-with-serverless-ingest-manager-and-node-js-4f99fba92436?source=post_page-----100afa4b93e3--------------------------------) ## 如何使用无服务器摄取管理器和 Node.js 处理 BigQuery 中的数据加载

### 文件格式、yaml 管道定义以及用于简单可靠的数据摄取的转换和事件触发器…

towardsdatascience.com

# 流处理

应用程序可以通过流处理对新数据事件做出即时响应。

> 流处理是企业数据的“必备”解决方案。

流处理会在数据生成时收集和处理数据，而不是像批处理那样在预定的频率下进行汇总。常见的使用案例包括**异常检测和欺诈预防**、**实时个性化和营销**以及**物联网**。数据和事件通常由“发布者”或“源”生成，并传输到“流处理应用程序”，在这里数据被立即处理，然后发送到“订阅者”。通常，作为`源`，你可以遇到使用**Hadoop、Apache Kafka、Amazon Kinesis**等构建的流应用程序。“**发布者/订阅者**”模式通常被称为`pub/sub`。

![](img/925417fa8cc74444da6b6fc344e96fe1.png)

在这个例子中，我们可以设置一个**ELT 流式**数据管道到**AWS Redshift**。AWS Firehose 传输流可以提供这种无缝集成，当流式数据直接上传到数据仓库表时。然后，数据将被转化为报告，使用**AWS Quicksight**作为 BI 工具。

# 批处理

批处理是一种模型，其中数据根据预定的阈值或频率被收集，无论是微批处理还是传统批处理，然后进行处理。历史上，数据环境中的工作负载主要是批处理导向的。然而，现代应用程序持续生成大量数据，企业倾向于微批处理和流处理，在这种处理方式中，数据被立即处理以保持竞争优势。**微批处理**加载的技术包括**Apache Spark Streaming、Fluentd 和 Logstash**，这与**传统批处理**非常相似，在传统批处理中，事件按计划或小组处理。

> 当数据的准确性此时并不重要时，这是一个很好的选择。

![](img/c47854fffb5d5468b0eade33ef679833.png)

这种数据管道设计模式在需要持续处理的小型数据集上效果更好，因为 Athena 是根据扫描的数据量收费的。

> *假设你不想在 MySQL 数据库实例上使用* `*change log*` *。这将是理想的情况，因为支付数据集并不庞大。*

# Lambda/Kappa 架构

这种架构结合了批处理和流处理方法。它结合了两者的优点，并建议必须保留原始数据，例如，保存在数据湖中，以便在你想要再次使用它来构建新管道或调查故障时使用。它具有**批处理**和**流处理**（速度）层，这有助于快速响应变化的业务和市场条件。Lambda 架构有时可能会非常复杂，需要维护多个代码库。

![](img/85af40b044d54265d18fb38f643a7c49.png)

Lambda 数据管道设计。图像由作者提供

# 先转换然后加载？

**ETL** 被认为是一种传统的方法，并且历史上最广泛使用。随着数据仓库的兴起，**ELT** 变得越来越流行。

> *确实，如果我们可以在数据仓库中集中处理，为什么我们需要先进行转换呢？*

**虚拟化** 是另一种数据仓库的流行方法，我们在数据上创建**视图**而不是物化表。对业务敏捷性的新增要求将成本效益置于次要位置，数据用户可以查询**视图**而不是**表**。

**变更数据捕获** 是另一种在变化发生时即时更新数据的方法。当通常使用延迟数据管道时，CDC 技术可以识别数据变化并提供有关这些变化的信息。变化通常会被推送到消息队列或作为流提供。

# 如何选择数据管道架构？

近年来，数据架构组件如数据管道已经发展，以支持大量数据。术语“大数据”可以描述为具有三个特征：体量、种类和速度。大数据可以在各种用例中开启新的机会，包括预测分析、实时报告和警报。由于数据量、种类和速度的显著增加，架构师和开发者不得不适应新的“大数据”需求。新的数据处理框架不断出现。由于现代数据流的高速，我们可能需要使用*流式*数据管道。数据可以实时收集和分析，从而允许立即采取行动。

> *然而，流式数据管道设计模式并不总是最具成本效益的。*

例如，在大多数数据仓库解决方案中，批量数据摄取是免费的。然而，流式处理可能会带来费用。同样的说法适用于数据处理。在大多数情况下，**流式**处理是处理数据的最昂贵方式。

大数据`volumes`要求数据管道能够同时处理事件，因为这些事件通常是同时发送的。数据解决方案必须具备可扩展性。

`variety`意味着数据可能以不同的格式通过管道传输，通常是非结构化或半结构化的。

架构类型取决于各种因素，即`destination`类型和数据最终要到达的位置、**成本考虑**以及你团队的**开发栈**和你与同事们已经具备的某些数据处理技能。

> 你的数据管道是否必须是可管理且基于云的，还是更愿意将其部署在本地？

实际上，有许多变量组合可以帮助选择最佳的数据平台架构。**那个管道中的速度或数据流率会是多少？你是否需要实时分析，还是近实时的分析就足够了？**这将解决你是否需要“流”管道的问题。

例如，有些服务可以创建和运行**流**和**批处理**数据管道，即[Google Dataflow](https://cloud.google.com/dataflow/docs/guides/data-pipelines)。那么它与数据仓库解决方案中的其他管道有何不同？选择取决于现有的基础设施。例如，如果你有一些现有的**Hadoop**工作负载，那么 GCP DataFlow 将是一个错误的选择，因为它不允许你重用代码（它使用的是 Apache Beam）。在这种情况下，你会希望使用**GCP Dataproc**，它可以处理**Hadoop/Spark**代码。

> *一个经验法则是，如果处理依赖于 Hadoop 生态系统中的任何工具，则应使用 Dataproc。*

它基本上是一个 Hadoop 扩展服务。

另一方面，如果你不受现有代码的限制，并且希望可靠地处理不断增加的**流数据**，那么**Dataflow**是推荐的选择。如果你喜欢用**JAVA**做事，可以查看这些[Dataflow 模板](https://github.com/GoogleCloudPlatform/DataflowTemplates)。

这里有一个来自 Google 的[系统设计指南](https://cloud.google.com/architecture)。

# 结论

大数据提出了对数据架构的新挑战要求。不断增加的数据格式和数据源的多样性提高了数据集成的重要性，而不影响生产应用。企业越来越倾向于自动化数据集成程序、实时处理流数据，并简化数据湖和数据仓库的生命周期。考虑到过去十年数据量、速度和数据格式的多样性不断增加，这确实成为了一项挑战。现在，数据管道设计必须既稳健又灵活，以简化和自动化创建新的数据管道。日益增加的使用 `data mesh`/ `data mart` 平台的趋势要求创建数据目录。为了创建受控的、企业级的数据，并为数据消费者提供便捷的方法来查找、检查和自助提供数据，这个过程也应尽可能自动化。因此，选择合适的数据管道架构可以有效解决这些问题。

*最初发布于* [*https://mydataschool.com*](https://mydataschool.com/blog/data-pipeline-design-patterns/)*。*

## 推荐阅读：

[](https://cloud.google.com/dataflow/docs/guides/data-pipelines?source=post_page-----100afa4b93e3--------------------------------) [## 使用数据管道 | Cloud Dataflow | Google Cloud

### 你可以在 [Note: google-data-pipelines-feedback](https://example.com) 报告 Dataflow 数据管道问题并请求新功能。你…

[cloud.google.com](https://cloud.google.com/dataflow/docs/guides/data-pipelines?source=post_page-----100afa4b93e3--------------------------------) [](https://cloud.google.com/architecture?source=post_page-----100afa4b93e3--------------------------------) [## 云架构指导和拓扑 | 云架构中心 | Google Cloud

### 发现参考架构、指导和最佳实践，以帮助你在 Google 上构建或迁移工作负载…

[cloud.google.com](https://cloud.google.com/architecture?source=post_page-----100afa4b93e3--------------------------------) [](https://github.com/GoogleCloudPlatform/DataflowTemplates?source=post_page-----100afa4b93e3--------------------------------) [## GitHub - GoogleCloudPlatform/DataflowTemplates: Google 提供的 Cloud Dataflow 模板管道…

### 这些 Dataflow 模板旨在解决简单但大规模的云端数据任务，包括数据…

[github.com](https://github.com/GoogleCloudPlatform/DataflowTemplates?source=post_page-----100afa4b93e3--------------------------------) [## 教程

### 教程 - AWS 数据管道 以下教程将逐步引导你创建和使用…

[docs.aws.amazon.com](https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/welcome.html?source=post_page-----100afa4b93e3--------------------------------)

AWS 数据管道文档 [[`docs.aws.amazon.com/data-pipeline`](https://docs.aws.amazon.com/data-pipeline/index.html)/]
