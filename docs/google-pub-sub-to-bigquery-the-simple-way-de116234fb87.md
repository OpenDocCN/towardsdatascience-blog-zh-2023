# 简单的 Google Pub/Sub 到 BigQuery 方法

> 原文：[`towardsdatascience.com/google-pub-sub-to-bigquery-the-simple-way-de116234fb87`](https://towardsdatascience.com/google-pub-sub-to-bigquery-the-simple-way-de116234fb87)

## 实施 BigQuery 订阅的动手指南，适用于 Pub/Sub 简单消息和流式数据摄取

[](https://jim-barlow.medium.com/?source=post_page-----de116234fb87--------------------------------)![Jim Barlow](https://jim-barlow.medium.com/?source=post_page-----de116234fb87--------------------------------)[](https://towardsdatascience.com/?source=post_page-----de116234fb87--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----de116234fb87--------------------------------) [Jim Barlow](https://jim-barlow.medium.com/?source=post_page-----de116234fb87--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----de116234fb87--------------------------------) ·阅读时长 8 分钟·2023 年 9 月 21 日

--

![](img/b31e6bb2ff00e2b71bba4bbff6d010fc.png)

Google 最新的行星规模数据仓库订阅基础的流式摄取水下军事能力：BigSub。在这种情况下，Pub 从未达到正式发布状态，因此你必须去其他地方寻找你的啤酒。照片由 [Thomas Haas](https://unsplash.com/@thomashaas?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 动机

我过去遇到过许多情况，我想将 Pub/Sub 消息导入 BigQuery 表中，但我从未找到一种特别简单的方法来实现这一点。

你可以设置一个 [dataflow pipeline](https://cloud.google.com/dataflow/docs/tutorials/dataflow-stream-to-bigquery)，但这需要额外的基础设施来理解、配置、管理和调试。而且 Dataflow（作为一种托管的 Apache Beam 服务）设计用于高吞吐量流式处理，因此对于简单的消息日志记录或监控系统来说总是显得过于复杂。

而且它是 Java。可是 Python 😀！而 Java… 😫！

```py
public static string args void main... public static string args void main... public static string args void main... public static string args void main... public static string args void main... arrrrrrrrrrrrgh
```

*对不起，我仍然会对我第一次尝试在 Java 中学习编码（上个世纪）时的经历感到闪回。请不要尝试使用那个代码片段……远离代码片段。*

然后我偶然发现了 [这个](https://medium.com/google-cloud/streaming-from-google-cloud-pub-sub-to-bigquery-without-the-middlemen-327ef24f4d15)，虽然它承诺简单性，但似乎比之前的方法（Debezium wtf？）还要复杂！

也可以部署一个轻量级的 Cloud Function 以在接收到 Pub/Sub 消息时触发，并将其流式传输或加载到 BigQuery 中，但这似乎仍然有些复杂，因为这本应是原生功能。

现在它已经实现了！

谷歌云的友善团队[宣布](https://cloud.google.com/blog/products/data-analytics/pub-sub-launches-direct-path-to-bigquery-for-streaming-analytics)了一段时间前从 Pub/Sub 到 BigQuery 的直接连接，太棒了！然而，尝试（并且失败）快速设置测试几次后，我终于有了一个实际的用例，需要为一个客户使其正常工作。

结果发现有几个细微之处，所以这篇文章旨在帮助你尽快地让这一切运行起来。

# 现状

Pub/Sub 是 Google Cloud 生态系统中一个极其有用、强大且可扩展的服务，具有两个核心用例：流式传输和消息传递。我将让 Google 自行解释（令人失望的剧透：这与位于潜艇上的公共房屋无关）。

> Pub/Sub 用于流式分析和数据集成管道，以摄取和分发数据。它同样有效地作为服务集成的消息中介或作为队列来并行化任务。
> 
> Pub/Sub 使你能够创建事件生产者和消费者系统，称为**发布者**和**订阅者**。发布者通过广播事件而不是通过同步远程过程调用（RPC）与订阅者异步通信。

消息被发布到一个主题，主题的订阅者可以接收消息并相应地采取行动。发布者对订阅者一无所知，但当消息被发布时，订阅者可以根据消息内容采取行动。

客户端库或[Cloud Storage Buckets 上的通知](https://cloud.google.com/storage/docs/pubsub-notifications)使发布包含可配置元数据的消息变得简单，而 Pub/Sub 会将这些消息发送到其他 Google Cloud 目的地以[触发 Cloud Functions](https://cloud.google.com/functions/docs/calling/pubsub)或各种不同的操作，只有你的想象力限制了这些操作。

现在我们可以将这些数据原生地导入 BigQuery（显然很简单），所以我抓住机会在几分钟内完成这个工作！但结果证明并没有那么简单。不过我完成了这个任务，所以我想写这篇文章来帮助其他需要快速设置的人，尽量减少麻烦。

# 解决方案

那么从哪里开始呢？我们从[文档](https://cloud.google.com/pubsub/docs/bigquery)开始吧。

> BigQuery 订阅在接收到消息时会将其写入现有的 BigQuery 表。你不需要单独配置订阅客户端。使用 Google Cloud 控制台、Google Cloud CLI、客户端库或 Pub/Sub API 来创建、更新、列出、分离或删除 BigQuery 订阅。

很好。我们开始吧。

> 作为简单数据摄取管道的替代方案，这些管道通常使用 Dataflow 写入 BigQuery，BigQuery 订阅具有以下优点：
> 
> **简单部署。** 你可以通过控制台、Google Cloud CLI、客户端库或 Pub/Sub API 通过一个工作流设置 BigQuery 订阅。
> 
> **提供低成本。** 除去了类似 Pub/Sub 管道中包含 Dataflow 作业的额外成本和延迟。这种成本优化对那些在存储前不需要额外处理的消息系统特别有用。
> 
> **减少监控。** BigQuery 订阅是多租户 Pub/Sub 服务的一部分，无需你运行单独的监控作业。

听起来不错！继续前往[模式部分](https://cloud.google.com/pubsub/docs/bigquery#properties_subscription)。我必须承认，阅读几遍后，我对需要做的事情感到有些困惑。

在普通环境中，管理模式可能相当复杂，在这种情况下，我需要创建一个 BigQuery 表，其模式完全镜像输入的 JSON 数据：

> **使用主题模式。** 这个选项允许 Pub/Sub 使用[Pub/Sub 主题的模式](https://cloud.google.com/pubsub/docs/admin#schemas)，即订阅所附加的主题。此外，Pub/Sub 将消息中的字段写入 BigQuery 表中的相应列。当你使用此选项时，请记得检查以下额外要求：
> 
> 主题模式中的字段和 BigQuery 模式中的字段必须具有相同的名称，并且它们的类型必须兼容。

哎呀。实际的输入数据可能非常复杂。它可能是嵌套的，包含数组并可能有数百个字段。JSON 和 BigQuery 结构映射得相当干净（数组 = `ARRAY`，对象 = `STRUCT`），但生成一个完全匹配的空 BigQuery 表并不是一件简单的事。

但也有另一种选择。

> 如果你没有选择 **使用主题模式** 选项，请确保 BigQuery 表中有一个名为 `data` 的 `BYTES` 或 `STRING` 类型的列。Pub/Sub 将消息写入这个 BigQuery 列。

好的，这看起来可能是一个可行的方法。我们先将数据导入 BigQuery，然后再处理解码 `JSON` 的问题。实际上，我倾向于这种架构，因为任何不合规的数据仍会被接收，我们可以尝试在下游解码它。

演变模式也是如此。将原始 `JSON` 导入 BigQuery 的好处在于——由于它仅仅是文本——这是一个极其稳健的数据导入点。然后我们可以使用最近扩展的[BigQuery JSON 函数](https://cloud.google.com/bigquery/docs/reference/standard-sql/json_functions)来执行各种操作。

下游转换也更为稳健：尝试选择在普通 SQL 中不存在的列会导致查询错误，而尝试从 JSON 对象中提取等效的不存在字段将简单地返回 NULL。这样处理后，会得到一个更为稳健的流程。

**输入表创建**

接下来是创建用于接收原始 JSON 数据的输入表。

你可以在 UI 中创建表格，但实际上通过[DDL](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language)在 BigQuery 控制台中完成这一操作非常简单（而且对我来说比截图更快 😀），然后当你第一次做错时，改变一个参数（或添加缺失的字符）并点击运行就会少很多挫败感。

表格模式实际上在文档的[编写元数据](https://cloud.google.com/pubsub/docs/create-bigquery-subscription#write-metadata)部分中解释，这几乎总是增加上下文和调试支持的好主意。

创建表格的 DDL（包含所有可能的字段）如下：

```py
CREATE OR REPLACE TABLE 
`[project_id].[dataset_name].[table_name]` 
(
subscription_name STRING,
message_id STRING,
publish_time TIMESTAMP,
data JSON,
attributes JSON
)
PARTITION BY TIMESTAMP_TRUNC(_PARTITIONTIME, DAY)
```

**表格分区**

在 BigQuery 中，分区几乎总是一个好主意（有时是必不可少的方法），因为它将数据物理上隔离到不同的分区中，这样可以直接查询这些分区，而无需昂贵的全表扫描。

表格分区有许多不同的[选项](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#partition_expression)，但需要注意的是，目前有一个[4000 个分区的限制](https://cloud.google.com/bigquery/docs/partitioned-tables#ingestion_time)，因此每日分区将在近 11 年后达到上限，但每小时分区将在不到 24 周后达到上限。

这里有一个附注，有时由于流数据的量，*确实*需要使用每小时分区（例如，我们已经实现了从[Tealium 的 EventStream 数据](https://tealium.com/integrations/google-cloud-pub-sub/)）。在这种情况下，需要部署额外的架构，将历史数据备份到 Google Cloud Storage，并设置[分区过期](https://cloud.google.com/bigquery/docs/managing-partitioned-tables#partition-expiration)。

另一个附注：为了简洁起见，我省略了我们为使这个过程可靠运行而采取的测试步骤。这就是从他人经验中学习的美妙之处：你可以跳过繁琐的部分，直接得到答案！

**设置 Pub/Sub 主题**

现在是实际设置 Pub/Sub 主题和 BigQuery 订阅的部分，这应该是一个相当快速的过程。前往[Cloud Console 中的 Pub/Sub](https://console.cloud.google.com/cloudpubsub/topic/)并点击`[+] CREATE TOPIC`按钮。你在这里有相当大的命名自由度，所以我倾向于给它和目标`dataset_id`完全相同的名称，不带`project_id`（即`dataset_id.table_name`）。取消勾选`Add a default subscription`框（因为我们接下来要创建一个 BigQuery 订阅），然后点击`CREATE`。

**设置 Pub/Sub 订阅**

一旦主题设置好，就可以开始创建订阅了。

点击你刚刚创建的主题，你应该看到一个底部区域，里面有多个选项卡，默认情况下选中的是`SUBSCRIPTIONS`。在这个区域点击`CREATE SUBSCRIPTIONS`并为订阅命名（实际上你可以将订阅名称与主题名称相同，我从未遇到过问题）。

在`Delivery Type`部分，选中`Write to BigQuery`旁边的单选按钮，选择之前创建的表的`project_id`、`dataset_id`和`table_name`。勾选`Write Metadata`复选框（我们喜欢元数据），并且—为了简单起见—将其他选项保留为默认设置。虽然建议 BigQuery 订阅应该启用死信队列，但这需要额外的主题，所以在这种情况下我们不启用。同时，由于我们不依赖使用主题模式，我们遇到失败消息的可能性也大大降低。

完成！

哦，等等。稍等一下。

**权限**

你很可能会看到一个错误信息，阻止你成功创建订阅。

```py
Service account service-#############@gcp-sa-pubsub.iam.gserviceaccount.com is missing permissions required to write to the BigQuery table: bigquery.tables.get, bigquery.tables.updateData.
```

实际上，这很简单，但你有几个选择。

最安全的选项，符合[最小权限](https://cloud.google.com/iam/docs/using-iam-securely#least_privilege)原则，是创建一个具有精确权限（`bigquery.tables.get`、`bigquery.tables.updateData`）的[自定义角色](https://console.cloud.google.com/iam-admin/roles)，然后将此角色分配给服务账户。

但是，由于这是一个在我的项目中从未用于其他用途的 Google Cloud 服务账户，我愿意给予它一个更简单、更宽松的角色。

无论如何，复制消息中的服务账户邮箱，进入入站表，点击`SHARE`和`ADD PRINCIPAL`。将服务账户邮箱复制到`New Principals`字段中。将`BigQuery Data Editor`角色或创建的自定义角色添加到分配角色中，然后`SAVE`。

希望这将顺利完成，你将拥有一个完全功能、简单的无服务器数据摄取流程，来自任何任意的 Pub/Sub 主题。

做得好！

**下一步**

现在你的数据摄取流程正常运行，我需要去做一些实际工作，所以我将留下来让你用一些真实的 Pub/Sub 消息进行测试。

使用最简单的方法之一是设置 BigQuery 定时查询的 Pub/Sub 通知，然后运行几次。你将看到每次运行时表格中都会增加一行，其中的`JSON`数据在`data`和`attributes`列中包含了每次运行的所有有用数据。

我的下一篇文章将解释将`JSON`有效负载解码为 BigQuery 数据类型的最佳设置，以便你可以开始使用和可视化数据。

如果你想接收第二部分（希望下周发布），记得关注我！
