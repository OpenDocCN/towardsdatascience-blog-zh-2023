# 数据工程中的数据流处理

> 原文：[`towardsdatascience.com/streaming-in-data-engineering-2bb2b9b3b603?source=collection_archive---------7-----------------------#2023-12-12`](https://towardsdatascience.com/streaming-in-data-engineering-2bb2b9b3b603?source=collection_archive---------7-----------------------#2023-12-12)

## 数据流水线和实时分析

[](https://mshakhomirov.medium.com/?source=post_page-----2bb2b9b3b603--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----2bb2b9b3b603--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2bb2b9b3b603--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2bb2b9b3b603--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----2bb2b9b3b603--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe06a48b3dd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstreaming-in-data-engineering-2bb2b9b3b603&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=post_page-e06a48b3dd48----2bb2b9b3b603---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2bb2b9b3b603--------------------------------) ·9 分钟阅读·2023 年 12 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2bb2b9b3b603&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstreaming-in-data-engineering-2bb2b9b3b603&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=-----2bb2b9b3b603---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2bb2b9b3b603&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstreaming-in-data-engineering-2bb2b9b3b603&source=-----2bb2b9b3b603---------------------bookmark_footer-----------)![](img/b5bdd62c71b5b0a4888786ad3318772d.png)

Photo by [DESIGNECOLOGIST](https://unsplash.com/@designecologist?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**流数据**是最流行的数据管道设计模式之一。使用事件作为单一数据点创建了从一个点到另一个点的持续数据流，使得实时数据摄取和分析成为可能。如果你想了解数据流，并学习如何构建实时数据管道，那么这篇文章就是为你准备的。学习如何测试解决方案，并模拟事件流的测试数据。本文是一个很好的机会，可以掌握一些受欢迎的数据工程技能，使用流行的流数据工具和框架，如 Kinesis、Kafka 和 Spark。我将讨论数据流的好处、示例和应用场景。

## 数据流到底是什么？

流数据，也称为事件流处理，是一种数据管道设计模式，其中数据点不断地从源头流向目的地。它可以实时处理，使得实时分析能力能够对数据流和分析事件进行快速反应。由于流处理，应用程序可以对新的数据事件触发即时响应，这通常是处理企业级数据的最流行解决方案之一。

> 这是一个数据…
