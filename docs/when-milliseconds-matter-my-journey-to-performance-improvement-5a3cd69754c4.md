# 毫秒至关重要——我在性能改进中的旅程

> 原文：[https://towardsdatascience.com/when-milliseconds-matter-my-journey-to-performance-improvement-5a3cd69754c4?source=collection_archive---------19-----------------------#2023-02-15](https://towardsdatascience.com/when-milliseconds-matter-my-journey-to-performance-improvement-5a3cd69754c4?source=collection_archive---------19-----------------------#2023-02-15)

## 从延迟改进项目中获得的经验教训

[](https://naomikriger.medium.com/?source=post_page-----5a3cd69754c4--------------------------------)[![Naomi Kriger](../Images/14839f859e1375965c046912f00df5b9.png)](https://naomikriger.medium.com/?source=post_page-----5a3cd69754c4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5a3cd69754c4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5a3cd69754c4--------------------------------) [Naomi Kriger](https://naomikriger.medium.com/?source=post_page-----5a3cd69754c4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce7969d594d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-milliseconds-matter-my-journey-to-performance-improvement-5a3cd69754c4&user=Naomi+Kriger&userId=ce7969d594d&source=post_page-ce7969d594d----5a3cd69754c4---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5a3cd69754c4--------------------------------) · 7 分钟阅读 · 2023年2月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5a3cd69754c4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-milliseconds-matter-my-journey-to-performance-improvement-5a3cd69754c4&user=Naomi+Kriger&userId=ce7969d594d&source=-----5a3cd69754c4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5a3cd69754c4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-milliseconds-matter-my-journey-to-performance-improvement-5a3cd69754c4&source=-----5a3cd69754c4---------------------bookmark_footer-----------)![](../Images/129986d3e73ac8cfaf2312955a537bf7.png)

图片由 [Giallo](https://www.pexels.com/@giallo/) 提供，来自 [Pexels](https://www.pexels.com/photo/assorted-silver-colored-pocket-watch-lot-selective-focus-photo-859895/)

*在严格的服务水平协议下工作，其中毫秒至关重要，同时维护一个具有多个依赖关系的复杂系统——当出现延迟相关问题时，这可能会带来许多挑战和非平凡的调查。*

*在这篇文章中，我将带你了解我如何通过一个描述的类型问题启动性能改进，并在过程中获得的经验教训。*

## 步骤 1 — 预期行为和问题描述

公司的产品处理事务，对于每个接收到的事务——我们要么批准，要么拒绝。

对于一些事务，我们会经过数据增强步骤，以获取更多的信息用于实时决策和未来的事务。

然而，对于每天成千上万的事务——增强过程根本没有发生，而且原因不明确。

## 步骤 2 — 在沙漠中寻找狮子

让我们看一下相关系统的简化视图：

![](../Images/605c2309f1d294ebe750048534e3d0e5.png)

作者草图

由于这是一个增强问题，第一个问题是——问题是发生在增强服务内部，还是在我们到达之前？显然，对于这些有问题的交易，我们甚至没有向增强服务发送获取请求。一个嫌疑犯排除。

## 步骤 3 — 了解我们的拓扑

让我们从故事中休息一下。我想向你介绍决策系统的拓扑。这个系统基于[Apache Storm](https://storm.apache.org/releases/2.2.0/Concepts.html)，旨在实时处理无限的数据流。

![](../Images/42e343f65beb71f9073f7722a1a645ab.png)

作者草图

简而言之，Spout从数据源（例如Kafka / RabbitMQ）接收数据，并将流输出到拓扑中。每个Bolt是拓扑中的一个组件，它接收并发出一个或多个流。一个Bolt进行简单的逻辑处理，如过滤、聚合、从数据库读取和写入等。

一些Bolts并行运行，而另一些则相互依赖。

一些Bolts会固定一个相对超时时间值，之后它们会继续（处理流并发出）无论是否接收到所有等待的输入。

超时的使用防止了Bolts和组件过长时间延迟拓扑，从而使我们能够满足预期的SLA。

## 步骤 **4 — 为什么没有增强？**

回到我们的故事。为什么决策系统没有调用增强服务？我添加了一些指标，发现问题交易的一个重要条件达到了——增强过程剩余的时间不够，所以没有执行获取调用以留出足够的时间给未来的Bolts。

这很让人惊讶。增强是流程中的核心组件，发生在拓扑的相对前期。为什么我们没有足够的时间来执行这个调用？

## 步骤 **5 — 依赖关系和延迟**

为了理解为什么我们超出了（相对）时间，我深入研究了一个内部工具提供的视图，并向后查看了我的增强Bolt所依赖的Bolts。

我发现一个较早的Bolt一直需要大约100毫秒。考虑到我们的SLA和一个Bolt应该花费的平均时间，这被认为是非常多的。

这个父 Bolt 中发生了什么事情，花了这么长时间？

当深入到父 Bolt 的代码时，我看到了一个 elasticsearch 查询，并怀疑这是否可能是我们瓶颈的原因。

结果是——当查看相关仪表板时，我发现我的 Bolt 高延迟的时间与这个集群高 CPU 使用率之间存在相关性。

在与维护这个集群的团队同步后，我了解到他们已经熟悉其长期的性能问题和逐渐恶化的情况。

## 第6步——这个依赖关系必要吗？

为什么增值-Bolt 依赖于这个 elasticsearch 查询？它是否值得我们在未增值事务中支付的代价？

在增值上下文中——我们在等待这个查询结果来处理一个特定功能，但进一步调查显示该功能存在一个漏洞，天知道有多久了，因此我们没有利用这个功能的期望输出。

![](../Images/469ba91929ea1e4a3dcafcbedfdf8da1.png)

作者草图

如果我们删除这个功能，我们可以断开增值与调用问题 elasticsearch 集群的 Bolt 之间的依赖。如果我们修复有漏洞的功能——我们将重新获得有人意图使用的数据，但会保留高延迟依赖，并需要寻找替代解决方案。

在考虑了几种潜在的解决路径——每种路径的努力和成本效益，并且得到了修复有漏洞的功能所有者的同意删除这段代码——我删除了有漏洞的功能。

## 第7步——何时对我们的拓扑进行微妙的更改

决策系统是基于依赖的，此时，我希望将增值组件依赖于一个发生在调用问题集群的组件之前的组件。这样的更改可以节省我们等待高延迟查询的时间，而且我们新的依赖组件出现得越早——增值及其后续组件将留有更多的空闲时间。

在调查代码并与高层选择新的父组件后，我进行了这项微妙的更改并监控了结果。

![](../Images/417d38b557468ae61dd8c183ead358c4.png)

作者草图

## 第8步——结果如何？

起初，我的更改上线后没有看到戏剧性的改善。真令人沮丧！几个月的调查和期待，变化幅度微小。但我们不应绝望！

我检查了未增值的事务，发现它们也符合超时条件。我调查了依赖关系视图，发现通过不同路径——它们仍然依赖于问题组件！

原因是增值 Bolt 等待几个字段，而这些字段的父 Bolt 再次指向我们的高延迟 Bolt，它查询了高延迟集群。

但这些字段在代码中没有被使用。

我删除了那些字段，并很高兴看到结果：

每日未丰富交易的数量从26K减少到200。

此外，在开始时——对于一些商户，这类有问题的交易的百分比高达20%，而在我的更改之后——所有商户的交易中问题的百分比不超过1%。

**大获成功！**

## **经验教训：**

我在这个过程中学到了很多。我使用各种技术进行了调查，深入复杂的代码，这些代码基于复杂的架构，分析了延迟，并考虑了不同的权衡以解决问题。但我今天想与大家分享一些提示：

+   **删除代码是有益的** 一位优秀的软件工程师的标准不是她写了多少新代码。删除代码是一项重要任务，删除那些过时的、使系统过载的代码对于提升系统性能至关重要。调查的过程可能需要耐心和毅力，但可能会带来宝贵的结果。

+   **代码删除应彻底进行**

    组件A依赖于组件B的原因之一是由于组件中未使用的输入字段。随着我们删除代码，花时间问自己是否删除了与之相关的所有内容并没有留下遗留问题是一个好习惯。

+   **投资于分析工具**

    使我能够检测到高延迟组件的一个因素是内部分析工具。我们需要了解我们的组件、它们的依赖关系以及它们的延迟，并且我们需要这样的工具直观易用。

+   **考虑监控流中特定组件的延迟**

    当毫秒很重要时，监控每个组件的延迟可能是识别瓶颈源的关键。考虑在创建组件时添加度量指标，以便在需要时能够访问这些数据。

+   **掌握你使用的技术，学习如何用它们进行调查** 我们团队使用的每种技术都有其强项和技巧。当我们遇到新技术或新工具时，我们可能会学习到用于日常工作的内容，然后就此离开。但投入时间学习如何利用这些工具获得额外的见解，当我们面临大量问题需要调查时，这可能会非常有用。

+   **咨询和头脑风暴** 复杂的调查可能很困难，一位同事可能熟悉我们未曾了解的调查工具，或者对我们的问题提出不同的看法。请记住，项目的成功就是团队和公司的成功，如果你感到困惑或需要另一个观点，请让同事参与进来。
