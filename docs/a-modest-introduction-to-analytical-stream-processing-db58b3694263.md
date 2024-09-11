# 对分析流处理的简要介绍

> 原文：[https://towardsdatascience.com/a-modest-introduction-to-analytical-stream-processing-db58b3694263?source=collection_archive---------9-----------------------#2023-08-15](https://towardsdatascience.com/a-modest-introduction-to-analytical-stream-processing-db58b3694263?source=collection_archive---------9-----------------------#2023-08-15)

## 构建可靠分布式系统的架构基础。

[](https://newfrontcreative.medium.com/?source=post_page-----db58b3694263--------------------------------)[![Scott Haines](../Images/b53c166b64314b4a5fe41abbe1839716.png)](https://newfrontcreative.medium.com/?source=post_page-----db58b3694263--------------------------------)[](https://towardsdatascience.com/?source=post_page-----db58b3694263--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----db58b3694263--------------------------------) [Scott Haines](https://newfrontcreative.medium.com/?source=post_page-----db58b3694263--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3b4cab6af83e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-modest-introduction-to-analytical-stream-processing-db58b3694263&user=Scott+Haines&userId=3b4cab6af83e&source=post_page-3b4cab6af83e----db58b3694263---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----db58b3694263--------------------------------) ·23分钟阅读·2023年8月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdb58b3694263&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-modest-introduction-to-analytical-stream-processing-db58b3694263&user=Scott+Haines&userId=3b4cab6af83e&source=-----db58b3694263---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdb58b3694263&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-modest-introduction-to-analytical-stream-processing-db58b3694263&source=-----db58b3694263---------------------bookmark_footer-----------)![](../Images/1fba9140fab3628fef09399e2a81e5a8.png)

分布式流数据网络是无限的，并且以惊人的速度增长。图片由 [作者的 MidJourney](https://www.midjourney.com/app/jobs/3cf0ebab-3ec4-487d-a0ea-24ef01387eae/) 创建

# 流处理的基础

基础是结构建立的坚固、牢不可破的基础。在构建成功的数据架构时，数据是整个系统的核心要素和基础的主要组成部分。

> 鉴于数据流入我们的数据平台的最常见方式是通过像[Apache Kafka](https://kafka.apache.org/)和[Apache Pulsar](https://pulsar.apache.org/)这样的流处理平台，本文涵盖了这一主题领域。

因此，确保我们（作为软件工程师）提供清洁的能力和无摩擦的保护措施，以减少数据进入这些快速流动的数据网络后的数据质量问题，是至关重要的。

这意味着建立围绕我们数据的*架构（类型和结构）*、字段级*可用性（可为空等）*和字段类型*有效性（预期范围等）*的API级合同，成为我们数据基础的重要支撑，尤其是在当今现代数据系统的去中心化、分布式流性质下。

然而，为了达到建立盲目信任——或高信任数据网络的程度，我们必须首先建立智能系统级设计模式。

## 构建可靠的流数据系统

作为软件和数据工程师，构建可靠的数据系统实际上是我们的工作，这意味着数据停机时间应像业务的其他组件一样进行测量。你可能听说过*SLAs*、*SLOs*和*SLIs*这些术语。这些缩写词与**合同**、**承诺**和**实际度量**相关，帮助我们评估端到端系统的表现。

作为服务负责人，我们将对自己的成功与失败负责，但通过一些前期的努力，标准化的元数据、共同的标准和最佳实践可以确保各方面的运行顺利。

此外，相同的元数据还可以提供关于我们数据在传输过程中质量和可信度的宝贵见解，直到数据找到其终端区域休息。数据的血统本身就讲述了一个故事。

## 采用拥有者心态

例如，你的团队或组织与客户（包括内部和外部客户）之间的*服务水平协议*（SLAs）用于创建一个关于你所提供服务的有约束力的合同。对于数据团队来说，这意味着根据你的*服务水平目标*（SLOs）识别和捕捉指标（KPMs — 关键绩效指标）。SLOs是你基于SLAs打算遵守的承诺，这可以是从接近完美（99.999%）的服务正常运行时间（API或JDBC）的承诺，到简单的承诺，例如某特定数据集的90天数据保留。最后，你的*服务水平指标*（SLIs）是你按照服务水平合同运营的证明，通常以操作分析（仪表板）或报告的形式呈现。

知道我们想要去哪里可以帮助制定到达那里的计划。这段旅程从数据的插入（或摄取点）开始，特别是每个数据点的正式结构和身份。考虑到“越来越多的数据通过像Apache Kafka这样的流处理平台进入数据平台”的观察，*编译时保证*、*向后兼容性*和*快速的二进制序列化*都对这些数据流中的数据至关重要。数据责任本身就是一个挑战。让我们看看原因。

## 管理流数据责任

流处理系统全天候运行，每周7天，每年365天。如果没有在前期投入适当的工作，这可能会使问题复杂化，其中一个时常出现的问题是数据损坏，即*飞行中的数据问题*。

# 处理飞行中的数据问题

有两种常见的方法可以减少飞行中的数据问题。首先，你可以在数据网络的边缘引入门卫，使用传统的*应用程序编程接口*（APIs）来协商和验证数据；或者作为第二种选择，你可以创建和编译辅助库或软件开发工具包（SDKs），以强制执行数据协议并使分布式写入者（数据生产者）进入你的流处理数据基础设施，你甚至可以同时使用这两种策略。

## 数据门卫

在数据网络的边缘（前端）添加网关API的好处在于，你可以在数据生产点执行*认证*（这个系统能访问这个API吗？），*授权*（这个系统能否将数据发布到特定的数据流中？），以及*验证*（这些数据是否可接受或有效？）。下图1–1展示了数据网关的流动情况。

![](../Images/76491b63816c94a91e888be33d526e48.png)

**图1–1**：一个分布式系统架构图，显示了数据摄取网关的认证和授权层。从左到右，批准的数据被发布到Apache Kafka中进行下游处理。图像来源 [Scott Haines](https://medium.com/u/3b4cab6af83e?source=post_page-----db58b3694263--------------------------------)

**数据网关服务**充当你受保护（内部）数据网络的数字门卫（保镖）。其主要角色是控制、限制甚至阻止未经认证的访问（见上图1–1中的API/服务），通过授权哪些上游服务（或用户）可以发布数据（通常通过使用服务[ACLs](https://docs.confluent.io/platform/current/kafka/authorization.html)来处理），以及提供的身份（比如服务身份和访问[IAM](https://spiffe.io/)，网络身份和访问[JWT](https://jwt.io/)，以及我们老朋友OAUTH）。

网关服务的核心责任是验证传入的数据，在发布潜在的损坏或一般性差的数据之前。如果网关正确地执行其职责，只有“良好”的数据才会通过并进入数据网络，这个网络是事件和操作数据的传输通道，通过流处理进行消化，换句话说：

> “这意味着产生数据的上游系统可以在生成数据时*快速失败*。这可以阻止损坏的数据进入流数据或静态数据管道，并通过错误代码和有用的消息以更自动的方式与生产者建立对话，了解具体原因和问题。”

## 使用错误消息提供自助解决方案

好与坏体验的差异在于从坏到好的转变所需的努力程度。我们都可能曾经与或工作过，或听说过，服务出现无缘无故的故障（空指针异常抛出随机500）。

为了建立基本的信任，一点点就足够了。例如，从API端点获取HTTP 400，并附带以下消息体（见下文）

```py
{
  "error": {
    "code": 400,
    "message": "The event data is missing the userId, and the timestamp is invalid (expected a string with ISO8601 formatting). Please view the docs at http://coffeeco.com/docs/apis/customer/order#required-fields to adjust the payload."  
  }
}
```

提供了400的原因，并赋予向我们（作为服务拥有者）发送数据的工程师修复问题的能力，无需安排会议、响起警报或在Slack上联系每个人。当你可以时，请记住，每个人都是人，我们喜欢闭环系统！

## 数据API的利弊

这种API方法有其优缺点。

优点是大多数编程语言与HTTP（或HTTP/2）传输协议即开即用——或者通过添加一个小库——并且JSON数据现如今几乎是最通用的数据交换格式。

从另一方面（缺点）看，可以说对于任何新的数据领域，还有另一个服务需要编写和管理，如果没有某种形式的API自动化，或遵循像[OpenAPI](https://spec.openapis.org/oas/latest.html#format)这样的开放规范，每个新的API路由（端点）最终都会花费更多的时间。

> 在许多情况下，未能以“及时”的方式更新数据摄取API，或者扩展和/或API停机、随机故障，或只是人员沟通不畅，为人们绕过“愚蠢”API提供了必要的理由，进而尝试直接将事件数据发布到Kafka。虽然API可能会觉得阻碍，但在数据质量问题如事件损坏或意外混合事件开始破坏流处理梦想后，保持一个公共的门卫有强烈的论据。

要彻底解决这个问题（并几乎完全消除它），良好的文档、变更管理（CI/CD）以及包括实际单元测试和集成测试在内的一般软件开发卫生——能够实现快速的功能和迭代周期，而不会降低信任度。

> 理想情况下，数据本身（模式/格式）可以通过启用字段级验证（谓词）、生成有用的错误信息并维护自身利益，来制定其自身数据层合同的规则。嘿，凭借一些路由或数据级元数据和一些创造性思维，API 可以自动生成自定义的路由和行为。

最后，网关 API 可以被视为集中式的麻烦制造者，因为每次上游系统未能发出有效数据（例如，被门卫阻挡）都会导致有价值的信息（事件数据、指标）被丢失。*这里的责任问题也往往是双向的*，因为门卫部署不当可能使一个未设置为处理网关停机（即使是几秒钟）的重试的上游系统失明。

抛开所有优缺点不谈，使用网关 API 来阻止腐败数据在进入数据平台之前的传播意味着，当出现问题时（因为问题总是会出现），问题的表面面积将减少到某个特定服务。这肯定比调试分布式的数据管道、服务，以及无数的最终数据目标和上游系统要好，因为那样你要弄清楚坏数据是由公司中的“某人”直接发布的。

如果我们去掉中间人（网关服务），那么治理“预期”数据传输的能力将落在“库”这一形式的专用 SDKs 上。

# 软件开发工具包（SDKs）

SDKs 是导入到代码库中的库（或微框架），用于简化某个操作、活动或其他复杂操作。它们也被称为 *客户端*。以之前提到的使用良好错误信息和错误代码为例。这个过程是必要的，用于 *通知客户端* 他们之前的操作是无效的，但直接将适当的保护措施添加到 SDK 中可以减少潜在问题的表面面积。例如，假设我们有一个 API 设置来通过事件跟踪跟踪客户的咖啡相关行为。

## 通过 SDK 保护措施减少用户错误

理论上，客户端 SDK 可以包含 *管理与 API 服务器交互所需的所有工具*，包括身份验证、授权，而对于验证来说，如果 SDK 发挥作用，验证问题将迎刃而解。以下代码片段展示了一个可以可靠地跟踪客户事件的 SDK 示例。

```py
import com.coffeeco.data.sdks.client._
import com.coffeeco.data.sdks.client.protocol._

Customer.fromToken(token)
  .track(
    eventType=Events.Customer.Order,
    status=Status.Order.Initalized,
    data=Order.toByteArray
  )
```

通过一些额外的工作（即客户端 SDK），数据验证或事件损坏的问题几乎可以完全消除。额外的问题可以在 SDK 内部处理，例如当服务器离线时如何重试发送请求。与其让所有请求立即重试，或在某个循环中无限制地淹没网关负载均衡器，SDK 可以采取更智能的措施，比如采用指数回退。有关当事情出错时会发生什么的深入探讨，请参见“**雷鸣之 Herd 问题**”！

**雷鸣之 Herd 问题**

比如说，我们有一个单一的网关 API 服务器。你编写了一个很棒的 API，公司的许多团队都在向这个 API 发送事件数据。事情进展顺利，直到有一天，一个新的内部团队开始向服务器发送无效数据（他们没有尊重你的 http 状态码，而是将所有非 200 的 http 代码视为重试的理由。不过，他们忘记添加任何重试启发式算法，如指数回退，因此所有请求都会无限重试——在一个不断增加的重试队列中）。请注意，在这个新团队加入之前，根本没有理由运行多个 API 服务器实例，也没有必要使用任何服务级别的速率限制器，因为一切都在商定的 SLA 内顺利运行。

![](../Images/fe55a743c4f71ff3c3ccfce69fa0ad7f.png)

**非失败鲸**。当你解决问题并重新摆脱困境时可能发生的情况。图片来源 [Midjourney 通过作者](https://www.midjourney.com/app/jobs/fd36ca2e-848f-4916-8125-2d0105da8fb4/)

好吧，那是今天之前的事了。现在你的服务离线了。数据在备份，上游服务正在填充他们的队列，人们很不满，因为他们的服务现在因为你的单点故障而开始出现问题……

这些问题都源于一种资源枯竭形式，称为“**雷鸣之 Herd 问题**”。当许多进程在等待某个事件时，比如系统资源变得可用，或者在这个例子中，API 服务器重新上线时，就会发生这个问题。现在，所有进程争先恐后地竞争资源，在许多情况下，单个进程（API 服务器）上的负载足以使服务再次离线。不幸的是，这会导致资源枯竭的循环重新开始。除非你能平息“ Herd”现象或将负载分布到更多的工作进程上，从而减少网络负载，使资源再次有空间呼吸。

虽然上面的初始示例更多的是一种无意的 [分布式拒绝服务攻击](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/)（DDoS），这些问题可以在客户端（通过指数回退或自我限流）和 API 边缘通过负载均衡和速率限制来解决。

最终，如果没有正确的眼睛和耳朵，通过操作指标、监控和系统级别的([SLAs/SLIs/SLOs](https://cloud.google.com/blog/products/devops-sre/sre-fundamentals-slis-slas-and-slos))警报来实现，数据可能会消失，这可能是一个解决的挑战。

无论你决定在数据网络边缘添加一个*数据网关 API*，使用*自定义 SDK* 来保证上游一致性和可靠性，还是决定采取其他方法将数据引入数据平台，了解你的选项总是好的。无论数据以何种方式流入数据流，这篇关于流数据的介绍都不完整，必须适当地讨论数据格式、协议和二进制可序列化数据的话题。谁知道呢，也许我们会发现更好的处理数据可靠性问题的方法！

# 选择合适的数据协议来完成任务

当你想到结构化数据时，首先想到的可能是 JSON 数据。JSON 数据具有结构，是标准的基于网络的数据协议，而且无论如何，它都非常容易使用。这些都是快速入门的好处，但随着时间的推移，如果没有适当的保护措施，你可能会在标准化 JSON 用于流系统时遇到问题。

## 与 JSON 的爱恨关系

第一个问题是 JSON 数据是可变的。这意味着作为数据结构它是灵活的，因此也容易脆弱。数据必须保持一致性才能可靠，而在跨网络传输数据的情况下（在传输过程中），序列化格式（二进制表示）应该高度紧凑。使用 JSON 数据，你必须为每个对象的所有字段发送键。这不可避免地意味着你会为每个额外的记录（在一系列对象中）发送大量额外的负荷。

幸运的是，这并不是一个新问题，恰好有针对这些问题的最佳实践，以及关于最佳序列化数据策略的多种思想流派。这并不是说 JSON 没有它的优点。只是在建立一个坚实的数据基础时，结构越多越好，压缩水平越高越好，只要它不消耗大量 CPU 资源。

# 可序列化的结构化数据

在高效编码和传输二进制数据时，两种序列化框架常常被提及：[Apache Avro](https://avro.apache.org/)和Google [Protocol Buffers](https://developers.google.com/protocol-buffers)（protobuf）。这两个库提供了CPU高效的行数据结构序列化技术，并且这两种技术还提供了各自的*远程过程调用*（RPC）框架和功能。让我们先来看*avro*，然后是*protobuf*，最后再看看*远程过程调用*。

## Avro消息格式

使用[*avro*](https://avro.apache.org/)时，你可以通过记录的概念定义结构化数据的声明式模式。这些记录只是JSON格式的数据定义文件（模式），存储为文件类型*avsc*。以下示例展示了avro描述符格式中的Coffee模式。

```py
{
  "namespace": "com.coffeeco.data",
  "type": "record",
  "name": "Coffee",
  "fields": [
    ("name": "id", "type: "string"},
    {"name": "name", "type": "string"},
    {"name": "boldness", "type": "int", "doc": "from light to bold. 1 to 10"},
    {"name": "available", "type": "boolean"}
 ]
}
```

处理avro数据可以有两种路径，取决于你在运行时如何工作。你可以采取编译时的方法，或者在运行时按需解决问题。这种灵活性可以增强交互式数据发现会话。例如，avro最初被创建为一种高效的数据序列化协议，用于在Hadoop文件系统中长期存储大型数据集合，以分区文件的形式存储。由于数据通常是从一个位置读取，写入到HDFS中的另一个位置，avro可以每个文件存储一次模式（用于写入时）。

## Avro二进制格式

当你将一组avro记录写入磁盘时，该过程会将avro数据的模式直接编码到文件中（仅一次）。在Parquet文件编码中也有类似的过程，模式会被压缩并作为二进制文件尾部写入。我们在第4章结束时，经历了将StructField级文档添加到我们的*StructType*的过程。这个模式用于编码我们的DataFrame，并且在写入磁盘时，它在下一次读取时保留了我们的内联文档。

## 启用向后兼容性并防止数据损坏

在读取多个文件作为单一集合的情况下，当记录之间的模式发生变化时，可能会出现问题。Avro将二进制记录编码为字节数组，并在反序列化时（将字节数组转换回对象）应用模式。

这意味着你需要额外注意以保持向后兼容，否则你可能会遇到*ArrayIndexOutOfBounds*异常。

破坏模式承诺也可能以其他微妙的方式发生。例如，假设你需要将某个字段的整数值更改为长整数值。不要这样做。这将破坏向后兼容性，因为从 int 到 long 的字节大小增加。这是由于使用模式定义来定义记录中每个字段的字节数组的起始和结束位置。为了保持向后兼容性，你需要在 avro 定义中保留整数字段，并向模式中添加（附加）一个新字段以供今后使用。

## 流式 avro 数据的最佳实践

从静态 avro 文件（具有有用的嵌入模式）转移到无限的数据流时，主要的区别在于你需要 *带上你自己的模式*。这意味着你需要支持向后兼容性（以防需要在模式更改之前和之后回滚和重新处理数据），以及前向兼容性，以防你已经有现有的读取器正在从流中消费数据。

这里的挑战是支持两种兼容性形式，因为 avro 无法忽略未知字段，而这是支持前向兼容性的要求。为了应对 avro 的这些挑战，Confluence 的团队开源了他们的 [模式注册表](https://docs.confluent.io/platform/current/schema-registry/index.html)（用于 Kafka），它在 Kafka 主题（数据流）级别启用了模式版本控制。

在不使用模式注册表的情况下支持 avro，你必须确保在更新写入者的模式库版本之前，更新任何活动的读取器（spark 应用程序或其他）。否则，切换开关的那一刻，你可能会发现自己处于事故的开端。

# Protobuf 消息格式

使用 protobuf，你使用消息的概念定义结构化数据定义。消息以类似于定义 C 中的结构的格式编写。这些消息文件以 proto 文件名扩展名写入。协议缓冲区具有使用 *imports* 的优势。这意味着你可以定义常见的消息类型和枚举，这些类型可以在大型项目中使用，甚至可以导入到外部项目中，实现大规模重用。一个使用 protobuf 创建 Coffee 记录（消息类型）的简单示例。

```py
syntax = "proto3";
option java_package="com.coffeeco.protocol";
option java_outer_classname="Common";

message Coffee {
  string id       = 1;
  string name     = 2;
  uint32 boldness = 3;
  bool available  = 4;
}
```

使用 protobuf，你只需定义一次消息，然后为所选择的编程语言编译。例如，我们可以使用来自 [ScalaPB](https://scalapb.github.io/) 项目的独立编译器生成 Scala 的代码（*由* [*Nadav Samet*](https://www.linkedin.com/in/nadav-samet/) *创建和维护*），或利用 [Buf](https://buf.build/) 的卓越，这些工具和实用程序围绕 protobuf 和 grpc 创建了宝贵的工具集。

## 代码生成

编译protobuf可以实现简单的代码生成。以下示例取自/ch-09/data/protobuf目录。章节READMEj中的说明涵盖了如何安装ScalaPB，并包括了设置正确环境变量以执行命令的步骤。

```py
mkdir /Users/`whoami`/Desktop/coffee_protos
$SCALAPBC/bin/scalapbc -v3.11.1 \
  --scala_out=/Users/`whoami`/Desktop/coffee_protos \
  --proto_path=$SPARK_MDE_HOME/ch-09/data/protobuf/ \
  coffee.proto
```

这个过程从长远来看节省了时间，因为它免去了你需要编写额外的代码来序列化和反序列化你的数据对象（无论是跨语言边界还是在不同代码库内）。

## Protobuf二进制格式

序列化（即二进制传输格式）使用了[编码](https://developers.google.com/protocol-buffers/docs/encoding)的概念，即二进制字段级分隔符。这些分隔符用作标记，以识别序列化protobuf消息中封装的数据类型。在示例文件coffee.proto中，你可能注意到每个字段类型旁边都有一个索引标记（例如string id = 1;），这用于协助消息的编码/解码。与avro二进制格式相比，这意味着有一点额外的开销，但如果你阅读[编码规范](https://developers.google.com/protocol-buffers/docs/encoding)，你会看到其他效率上的优势足以弥补这些额外的字节（例如位打包、数值数据类型的高效处理，以及对每条消息前15个索引的特殊编码）。在选择protobuf作为流数据的二进制协议时，总体来看，优点远大于缺点。其中一个显著的优点是支持向后和向前的兼容性。

## 启用向后兼容性和防止数据损坏

在修改protobuf模式时需要记住类似于avro的规则。作为一个经验法则，你可以更改字段的名称，但你绝不能更改字段的类型或位置（索引），除非你愿意破坏向后兼容性。随着团队对protobuf使用的熟练程度提高，这些规则在长期支持任何数据时可能会被忽视，特别是当需要重新排列和优化时。如果不小心，这可能会带来麻烦。（有关更多背景，请参见下面名为*保持数据质量随时间*的提示。）

## 流式protobuf数据的最佳实践

鉴于 protobuf 支持 **向后** 和 **向前** 兼容，这意味着你可以在不担心首先更新读者的情况下部署新的写入器，读者也是如此，你可以用更新版本的 protobuf 定义来更新它们，而不必担心所有写入器的复杂部署。Protobuf 通过未知字段的概念支持向前兼容。这是 avro 规范中不存在的附加概念，用于跟踪由于 protobuf 本地版本与当前读取版本之间的差异而无法解析的索引和相关字节。这里的好处是你可以在任何时候 *选择* 更新 protobuf 定义中的新更改。

比如，假设你有两个流应用程序 (a) 和 (b)。应用程序 (a) 正在处理来自上游 Kafka 主题 (x) 的流数据，为每条记录增加额外的信息，然后将其写入一个新的 Kafka 主题 (y)。现在，应用程序 (b) 从 (y) 中读取数据并执行其操作。假设有一个更新的 protobuf 定义版本，而应用程序 (a) 尚未更新到最新版本，而上游 Kafka 主题 (x) 和应用程序 (b) 已经更新，并期望使用从升级中获得的新字段。令人惊讶的是，依然可以将未知字段通过应用程序 (a) 传递到应用程序 (b)，甚至不需要知道它们的存在。

参见 *“维护数据质量的技巧”* 以获取更多深入的探讨。

**提示：维护数据质量的技巧**

当使用 *avro* 或 *protobuf* 时，你应该像对待需要推送到生产环境的代码一样对待这些模式。这意味着要创建一个可以提交到你公司 *github*（或你使用的任何版本控制系统）的项目，这也意味着你 *应该* 为你的模式编写单元测试。这不仅提供了如何使用每种消息类型的实际示例，而且测试数据格式的一个重要原因是确保模式的更改不会破坏向后兼容性。更进一步的是，为了单元测试模式，你需要首先编译 (.avsc 或 .proto) 文件，并使用相应的库代码生成。这使得创建可发布的库代码变得更加容易，你还可以使用发布版本（版本 1.0.0）来记录每次对模式的更改。

一种简单的方法是通过在项目生命周期内序列化并存储每个消息的二进制副本，来使这一过程得以实现。我发现将这一步骤直接添加到单元测试中很有效，利用测试套件创建、读取和写入这些记录到项目测试资源目录中。这样，每个二进制版本在所有模式更改中都可以在代码库中找到。

通过一点额外的前期努力，你可以在整体方案中节省大量麻烦，并且可以安心入睡，知道你的数据是安全的（至少在生产和消费方面）。

## 使用 Buf 工具和 Protobuf 在 Spark 中

自从 2021 年撰写本章以来，**Buf Build** ([https://buf.build/](https://buf.build/)) 已经发展成了*一切 protobuf* 公司。他们的工具简单易用，免费且开源，并在合适的时机出现，为 Spark 社区的一些倡议提供了支持。[Apache Spark](https://spark.apache.org/) 项目引入了对 [Protocol Buffers in Spark 3.4](https://github.com/apache/spark/tree/v3.4.1/connector/protobuf/src/main/scala/org/apache/spark/sql/protobuf) 的全面原生支持，以支持 [spark-connect](https://spark.apache.org/docs/latest/spark-connect-overview.html)，并使用 Buf 编译 GRPC 服务和消息。毕竟，Spark Connect 是一个 GRPC 原生连接器，用于嵌入 JVM 外的 Spark 应用程序。

传统的 Apache Spark 应用必须在某处作为驱动程序应用运行，过去这意味着使用**pyspark** 或原生 spark，这两者仍然运行在 JVM 进程之上。

![](../Images/cec7730d94bae772f26b9508103edc50.png)

[目录结构](https://github.com/apache/spark/tree/v3.4.1/connector/connect/common/src/main) 通过 Spark Connect 显示了 protobuf 定义，以及 buf.gen.yaml 和 buf.work.yaml，这些文件有助于代码生成。

到头来，Buf Build 为构建过程带来了安心。为了生成代码，只需运行一个简单的命令：`buf generate`。对于简单的 lint 检查和一致的格式化，可以使用 `buf lint && buf format -w`。不过，最棒的是变更检测。`buf breaking --against .git#branch=origin/main` 就可以确保对消息定义的新更改不会对当前在生产环境中运行的内容产生负面影响。*未来，我会写一篇关于使用**buf**进行企业分析的文章，但现在，是时候结束这一章了。*

那么我们刚才讲到哪里了？你现在知道，在长期数据管理策略中使用 avro 或 protobuf 有其好处。通过使用这些与语言无关的、基于行的结构化数据格式，你减少了长期语言锁定的问题，为将来的任何编程语言打开了大门。说实话，支持遗留库和代码库是一项难以回报的任务。此外，序列化格式有助于减少与发送和接收大量数据相关的网络带宽成本和拥堵。这也有助于降低长期存储数据的开销。

最后，让我们看看这些结构化数据协议如何在通过远程过程调用发送和接收数据时实现额外的效率。

# 远程过程调用

*RPC* 框架，总的来说，使得 *客户端* 应用程序可以通过本地函数调用透明地调用 *远程*（服务器端）方法（过程），通过来回传递序列化的消息。*客户端* 和 *服务器端* 实现使用相同的 *公共接口* 定义来定义可用的功能性 *RPC* 方法和服务。接口定义语言（IDL）定义协议和消息定义，并作为客户端和服务器端之间的契约。让我们通过流行的开源 RPC 框架 [gRPC](https://grpc.io/) 来看看实际情况。

# gRPC

首先由 Google 设想并创建的，[gRPC](https://grpc.io/) 即“通用”远程过程调用，是一个强大的开源框架，用于高性能服务，从分布式数据库协调（如 [CockroachDB](https://www.cockroachlabs.com/docs/stable/architecture/distribution-layer.html)）到实时分析（如 [微软 Azure 视频分析](https://docs.microsoft.com/en-us/azure/media-services/live-video-analytics-edge/grpc-extension-protocol)）。

![](../Images/bb0e956f0dcd4a9d8154939f7d47bad4.png)

**图 1–2**。RPC（在此示例中是 gRPC）通过在客户端和服务器之间传递序列化消息来工作。客户端实现相同的接口定义语言（IDL）接口，这作为客户端和服务器之间的 API 合同。（图片来源: [https://grpc.io/docs/what-is-grpc/introduction/)](https://grpc.io/docs/what-is-grpc/introduction/))

图 9–3 显示了 gRPC 工作的示例。服务器端代码使用 C++ 编写以提高速度，而用 ruby 和 java 编写的客户端可以使用 protobuf 消息作为通信手段与服务进行互操作。

使用协议缓冲区进行消息定义、序列化以及服务的声明和定义，gRPC 可以简化你捕获数据和构建服务的方式。例如，假设我们想继续创建一个用于跟踪客户咖啡订单的 API。API 合同可以在一个简单的服务文件中定义，从而可以使用相同的服务定义和消息类型构建服务器端实现和任意数量的客户端实现。

## 定义 gRPC 服务

你可以像 1–2–3 一样轻松定义服务接口、请求和响应对象以及需要在客户端和服务器之间传递的消息类型。

```py
syntax = "proto3";

service CustomerService {
    rpc TrackOrder (Order) returns (Response) {}
    rpc TrackOrderStatus (OrderStatusTracker) returns (Response) {}
}

message Order {
    uint64 timestamp    = 1;
    string orderId      = 2;

    string userId       = 3;
    Status status       = 4;
}

enum Status {
  unknown_status = 0;
  initalized     = 1;
  started        = 2;
  progress       = 3;
  completed      = 4;
  failed         = 5;
  canceled       = 6;
}

message OrderStatusTracker {
  uint64 timestamp = 1;
  Status status    = 2;
  string orderId   = 3;
}

message Response {
    uint32 statusCode = 1;
    string message    = 2;
}
```

通过添加 gRPC，实现和维护数据基础设施中使用的服务器端和客户端代码将变得更容易。由于 protobuf 支持向后和向前兼容，这意味着旧版 gRPC 客户端仍可以向新版 gRPC 服务发送有效消息，而不会遇到常见问题和痛点（如之前在“飞行中的数据问题”中讨论的）。

## gRPC 使用 HTTP/2

作为额外的好处，对于现代服务栈，gRPC 能够使用 HTTP/2 作为其传输层。这也意味着你可以利用现代数据网格（如 [Envoy](https://www.envoyproxy.io/)）进行代理支持、路由和服务级别的 [认证](https://www.envoyproxy.io/docs/envoy/v1.17.2/api-v2/config/filter/http/ext_authz/v2/ext_authz.proto#envoy-api-file-envoy-config-filter-http-ext-authz-v2-ext-authz-proto)，同时减少标准 HTTP over TCP 中的 TCP 数据包拥塞问题。

减轻飞行中的数据问题并在数据责任方面取得成功，从数据开始，并从那个中心点向外扩展。对于如何将数据引入你的数据网络，建立相应的流程应该被视为在投入流数据之前需要完成的前提条件。

# 摘要

本文的目标是呈现所需的移动部件、概念和背景信息，以便在盲目从传统的（静态）批处理思维方式跳跃到理解实时流数据风险和回报的方式之前，武装好自己。

实时利用数据可以带来快速、可操作的洞察，并打开通往最先进的机器学习和人工智能的大门。

然而，分布式数据管理如果未考虑正确的步骤，也可能成为数据危机。请记住，如果没有建立在有效（值得信赖）数据之上的强大、坚实的数据基础，实时数据的道路将不是一条简单的道路，而是充满了颠簸和绕行。

我希望你喜欢第 9 章的下半部分。要阅读本系列的第一部分，请前往 [对分析流处理的温和介绍](https://medium.com/towards-data-science/a-gentle-introduction-to-stream-processing-f47912a2a2ea)。

[](/a-gentle-introduction-to-stream-processing-f47912a2a2ea?source=post_page-----db58b3694263--------------------------------) [## 对分析流处理的温和介绍

### 为工程师及其他人构建心理模型

towardsdatascience.com](/a-gentle-introduction-to-stream-processing-f47912a2a2ea?source=post_page-----db58b3694263--------------------------------)

— — — — — — — — — — — — — — — — — — — — — — — — 

如果你想深入了解，请查看我的书，或给我一个击掌支持。

[](https://www.amazon.com/Modern-Engineering-Apache-Spark-Hands/dp/1484274512?source=post_page-----db58b3694263--------------------------------) [## 使用 Apache Spark 进行现代数据工程：构建关键流处理的实用指南…

### Amazon.com: 使用 Apache Spark 进行现代数据工程：构建关键流处理的实用指南…

www.amazon.com](https://www.amazon.com/Modern-Engineering-Apache-Spark-Hands/dp/1484274512?source=post_page-----db58b3694263--------------------------------)

如果你有访问权限 [O’Reilly Media](https://medium.com/u/fbfa235a954c?source=post_page-----db58b3694263--------------------------------)，你也可以完全免费阅读这本书（对你有好处，对我则不然），但如果有机会，请在其他地方找到这本书的免费版本，或者获取电子书以节省运费（或避免寻找一本超过600页的实体书的地方）。

[](https://learning.oreilly.com/library/view/modern-data-engineering/9781484274521/?source=post_page-----db58b3694263--------------------------------) [## 使用 Apache Spark 进行现代数据工程：构建关键流处理的实用指南…

### 利用 Apache Spark 在现代数据工程生态系统中。本实用指南将教你如何编写完全…

learning.oreilly.com](https://learning.oreilly.com/library/view/modern-data-engineering/9781484274521/?source=post_page-----db58b3694263--------------------------------)
