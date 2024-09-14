# 数据流处理令人兴奋：在跳入之前你需要了解的事项

> 原文：[`towardsdatascience.com/data-streaming-is-exciting-what-you-need-to-know-before-jumping-in-e233e192337b`](https://towardsdatascience.com/data-streaming-is-exciting-what-you-need-to-know-before-jumping-in-e233e192337b)

## 数据流处理适合你的业务吗？需要考虑的关键事实

[](https://chengzhizhao.medium.com/?source=post_page-----e233e192337b--------------------------------)![Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----e233e192337b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e233e192337b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e233e192337b--------------------------------) [Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----e233e192337b--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e233e192337b--------------------------------) ·阅读时间 5 分钟·2023 年 2 月 27 日

--

![](img/1d33cd261fc165c0bb572ab116c342c5.png)

照片由 [Stephen Leonardi](https://unsplash.com/@stephenleo1982?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/DpvBWb4JW1M?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上提供

流数据是数据领域中一个令人兴奋的领域，近年来受到了极大的关注。由于这种兴奋，开源项目的领域变得非常拥挤。许多技术使流数据处理变得比以往更简单：Kafka、Flink、Spark、Storm、Beam 等技术已经在市场上存在多年，并建立了坚实的用户基础。

> “让我们做流处理。”

对于数据专业人士来说，这是一个不可避免的话题。然而，在有人告诉你关于流数据之前，我们应该退一步，用一个简单的问题与自己再次确认：**我在这个用例中需要流数据吗？** 在深入了解之前，让我们面对这个故事中的流数据事实。

# 流数据

在我们查看流数据的事实之前，让我们先看看什么是流数据。Hadoop 为处理大规模数据集奠定了基础，并赋予了数据专业人士设计更复杂的数据处理工具的能力。

[Tyler Akidau](https://www.oreilly.com/people/tyler-akidau/) 于 2013 年发表的关于 [MillWheel: Fault-Tolerant Stream Processing at Internet Scale](https://research.google/pubs/pub41378/) 的论文为现代流数据奠定了基础，并激发了像 [Apache Flink](https://flink.apache.org/) 这样的流处理框架。

泰勒著名的 [Streaming 101](https://www.oreilly.com/radar/the-world-beyond-batch-streaming-101/)、[Streaming 102](https://www.oreilly.com/radar/the-world-beyond-batch-streaming-102/) 以及他的书籍 [Streaming System](https://www.oreilly.com/library/view/streaming-systems/9781491983867/) 提供了流处理数据的广泛背景。

> 当我使用“流处理”这个术语时，你可以安全地假设我指的是一个为无限数据集设计的执行引擎，仅此而已。 — [泰勒·阿基道](https://www.oreilly.com/people/tyler-akidau/)

让我们遵循泰勒的确切定义，并在整个故事中关注无限数据。

# Kappa 与 Lambda 架构

我们都熟悉 Lambda 架构 — 我们使用两个独立的数据处理系统：批处理和流处理，重复编写类似的逻辑并处理相同的数据。流处理用于推测，批处理用于准确性和完整性。

另一方面，我们有 Kappa 架构。我们有一个单一的管道运行，无需重复代码，并利用 Kafka 实现可重放的操作，以便在需要准确性和完整性时使用。

总的来说，Kappa 是一个设计良好的系统的好想法。然而，这样的系统需要将数据处理视为首要任务。

# 流处理数据并非灵丹妙药。

不久前，对数据处理有一种印象，即“**流处理数据是灵丹妙药**”，我们都将转向流处理数据。批处理被视为过时的。

一段时间后，人们意识到流处理数据并不是解决问题的灵丹妙药，反而可能会使情况更糟：

+   流处理不足以生成完整的数据分析数据集。由于数据的迟到或处理错误，仍然需要批处理来弥补这一缺口。

+   流处理和批处理通常讲的是不同的语言。流处理通常使用 Java、Scala 和 Go 运行，配合 Apache Flink / Kafka Stream 等框架。批处理则通常使用 Python、SQL 和 R 运行，配合 Apache Spark / SQL 引擎等框架。为批处理和流处理重复相同的逻辑是一个麻烦。这是运行 Lambda 架构时最具挑战性的问题之一。

# 流处理数据的权衡

数据自然是以流处理的方式出现的。最初，批处理似乎不适合解决数据问题，但批处理因其几十年来的声誉而有其存在的理由。批处理是一种简化的哲学，用于解决复杂的问题。

批处理和流处理之间存在显著的权衡。

## **完整性与推测**

许多数据源不可避免地会产生延迟；主要是你的数据分析包括多个数据源。批处理在处理数据完整性方面表现出色，可以在一切准备好后延迟处理。

另一方面，数据流可以通过等待额外的时间，即在内存中保持数据几个小时或一天，来实现这一目标，这样做是昂贵的。流处理也可以提供完整的数据集，但需要上游数据生成器的配合，以解决数据整合和额外延迟的问题。

## **你的用例的准确 SLA**

你需要你的流处理系统处理数据的速度有多快？你的用例可以接受多少延迟？许多 ETL 批处理管道是每天处理的。这对你的业务来说是否太慢了？许多用例**不**受服务水平协议（SLA）的限制。与广告或日间交易不同，几个小时的延迟不会阻止公司正常运营。

# 流数据不容易维护

## 迟到的数据

对于任何数据处理系统来说，一个不可避免的事实是：**数据到达迟缓**。一个设计良好的系统有时可以规避这个问题，但只是偶尔。

在批处理过程中，迟到的数据不是一个大问题，因为数据处理的时间远晚于事件日期，并且 SLA 的要求不严格到分钟或小时。那些从事批处理的人对数据将在 24 小时或更长时间内到达的期望较低。

流处理不是“万无一失”的解决方案——像[水印](https://nightlies.apache.org/flink/flink-docs-master/docs/dev/datastream/event-time/generating_watermarks/)这样的概念为我们处理迟到的数据提供了额外的缓冲。然而，水印是另一种将数据保存在内存中的方式。内存不是免费的：在进一步的点上，水印必须推进，你决定丢弃记录或将其发送到死队列，以便另一个过程重新处理——批处理。

## 24/7 维护

维护流处理应用程序是很有挑战性的。与批处理不同，你在停机窗口中可以放松，决定是否修复一个错误或喝杯咖啡。

在流数据处理过程中，需要 24/7 的监控和最小的停机时间。你的值班团队必须监控并修复潜在的数据问题，以保持管道的运行。流处理可能听起来很令人兴奋，但作为维护流处理管道的数据工程师，值班需要付出很多努力。

## **连接数据的复杂性要高得多。**

在流处理中，与多个流的数据连接并不是简单的。在批处理时，通过将一组标准键与两个有界表连接，连接非常简单。在流处理中，必须仔细考虑两个无界数据集如何连接。

一个更大的问题出现了：我们如何知道是否还有需要考虑的传入记录？Tyler 的[Streaming 102](https://www.oreilly.com/radar/the-world-beyond-batch-streaming-102/)提供了一个很好的示例来演示这一点。简而言之，在不同流之间连接数据比批处理复杂得多。

# 最后思考

在采用流处理应用程序之前，了解你的用例是否适合流处理是至关重要的。

以流式方式处理数据是令人兴奋和吸引人的。

然而，激动的背后是成本。批处理更为直接，并且已被历史认可数十年。在盲目进入数据流处理之前，理解其优缺点应当被仔细评估。

我希望这个故事对你有所帮助。本文是我的工程与数据科学故事系列中的**一部分**，目前包括以下内容：

![Chengzhi Zhao](img/51b8d26809e870b4733e4e5b6d982a9f.png)

[Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----e233e192337b--------------------------------)

## 数据工程与数据科学故事

[查看列表](https://chengzhizhao.medium.com/list/data-engineering-data-science-stories-ddab37f718e7?source=post_page-----e233e192337b--------------------------------)53 个故事![](img/8b5085966553259eef85cc643e6907fa.png)![](img/9dcdca1fc00a5694849b2c6f36f038d4.png)![](img/2a6b2af56aa4d87fa1c30407e49c78f7.png)

你也可以[**订阅我的新文章**](https://chengzhizhao.medium.com/subscribe)或成为[**推荐的 Medium 会员**](https://chengzhizhao.medium.com/membership)，享受 Medium 上所有故事的无限访问。

如果有问题或评论，**请随时在本故事的评论区留言**，或通过[Linkedin](https://www.linkedin.com/in/chengzhizhao/)或[Twitter](https://twitter.com/ChengzhiZhao)与我直接联系。
