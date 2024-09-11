# 使用 Python 与 Kafka

> 原文：[https://towardsdatascience.com/using-kafka-with-python-54dc20717cf7?source=collection_archive---------2-----------------------#2023-03-20](https://towardsdatascience.com/using-kafka-with-python-54dc20717cf7?source=collection_archive---------2-----------------------#2023-03-20)

## 学习如何使用 Python 创建生产者和消费者，并绘制动态散点图

[](https://weimenglee.medium.com/?source=post_page-----54dc20717cf7--------------------------------)[![李伟明](../Images/10fc13e8a6858502d6a7b89fcaad7a10.png)](https://weimenglee.medium.com/?source=post_page-----54dc20717cf7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----54dc20717cf7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----54dc20717cf7--------------------------------) [李伟明](https://weimenglee.medium.com/?source=post_page-----54dc20717cf7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-kafka-with-python-54dc20717cf7&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----54dc20717cf7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----54dc20717cf7--------------------------------) ·7 分钟阅读·2023 年 3 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F54dc20717cf7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-kafka-with-python-54dc20717cf7&user=Wei-Meng+Lee&userId=6599e1e08a48&source=-----54dc20717cf7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F54dc20717cf7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-kafka-with-python-54dc20717cf7&source=-----54dc20717cf7---------------------bookmark_footer-----------)![](../Images/189593ad2da51f8a3bf4c211cfa02a53.png)

照片由 [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我之前关于 Kafka 的文章中，我介绍了 Kafka 用于数据流的用途。我还展示了如何启动 Kafka 代理服务，并演示了如何使用 Kafka 生产者控制台应用程序发送消息以及 Kafka 消费者控制台应用程序接收消息。

在本文中，我将展示如何使用 Python 利用 Kafka。具体来说，我将：

+   使用 Python 将消息发送到 Kafka 代理服务

+   使用 Python 从 Kafka 代理服务接收消息

+   构建一个动态图表应用程序，以绘制和更新散点图，当从代理服务接收到新数据时

# 使用 Python 与 Kafka

至少有三种 Python 库可供 Python 开发人员与 Kafka 代理服务接口。它们是：

+   Kafka-Python

+   PyKafka

+   Confluent Kafka Python

对于本文，我将使用 **Confluent Kafka Python** 包。

要安装 **Confluent Kafka Python** 包，请使用 `pip` 命令：
