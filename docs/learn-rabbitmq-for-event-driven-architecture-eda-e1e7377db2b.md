# 学习RabbitMQ以实现事件驱动架构（EDA）

> 原文：[https://towardsdatascience.com/learn-rabbitmq-for-event-driven-architecture-eda-e1e7377db2b?source=collection_archive---------13-----------------------#2023-04-05](https://towardsdatascience.com/learn-rabbitmq-for-event-driven-architecture-eda-e1e7377db2b?source=collection_archive---------13-----------------------#2023-04-05)

## 一份适合初学者的教程，介绍RabbitMQ的工作原理及如何在Go中使用RabbitMQ，这是学习事件驱动架构（EDA）的第一步。

[](https://programmingpercy.medium.com/?source=post_page-----e1e7377db2b--------------------------------)[![Percy Bolmér](../Images/34949a468cbbb5c609807903775afddb.png)](https://programmingpercy.medium.com/?source=post_page-----e1e7377db2b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e1e7377db2b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e1e7377db2b--------------------------------) [Percy Bolmér](https://programmingpercy.medium.com/?source=post_page-----e1e7377db2b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7fab1d3f6ce0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearn-rabbitmq-for-event-driven-architecture-eda-e1e7377db2b&user=Percy+Bolm%C3%A9r&userId=7fab1d3f6ce0&source=post_page-7fab1d3f6ce0----e1e7377db2b---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----e1e7377db2b--------------------------------) · 39分钟阅读 · 2023年4月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe1e7377db2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearn-rabbitmq-for-event-driven-architecture-eda-e1e7377db2b&user=Percy+Bolm%C3%A9r&userId=7fab1d3f6ce0&source=-----e1e7377db2b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe1e7377db2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearn-rabbitmq-for-event-driven-architecture-eda-e1e7377db2b&source=-----e1e7377db2b---------------------bookmark_footer-----------)![](../Images/4e653e64d73dd82ca8b9ea9f8cdbc21e.png)

照片由[Bradyn Trollip](https://unsplash.com/es/@bradyn?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

事件驱动架构（EDA）是我在编程中最喜欢的架构之一。这种架构允许我们构建微服务，并轻松地在它们之间共享信息。

在常规的顺序软件中，你会有一个函数触发另一个函数，或者一个定期脚本检查某些任务。

使用事件驱动架构，我们则利用队列或发布/订阅模式，允许不同的服务之间通知或传递信息，以触发代码执行。

事件驱动架构通常用于构建高度灵活和可扩展的软件。这是因为可以通过简单地用新服务监听事件来轻松添加或移除功能。

这也使得在生产环境中并行地影子部署和测试新服务变得非常容易，因为你可以使新服务对相同的事件做出反应，而不会干扰正在运行的系统。

然而，并非一切都是美好的，有些人认为EDA系统稍微复杂一些，并且在考虑…时有时更难测试。
