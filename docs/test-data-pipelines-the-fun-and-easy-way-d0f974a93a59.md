# 以有趣和简单的方式测试数据管道

> 原文：[https://towardsdatascience.com/test-data-pipelines-the-fun-and-easy-way-d0f974a93a59?source=collection_archive---------12-----------------------#2023-02-22](https://towardsdatascience.com/test-data-pipelines-the-fun-and-easy-way-d0f974a93a59?source=collection_archive---------12-----------------------#2023-02-22)

## 初学者指南：为什么单元测试和集成测试对您的数据平台如此重要

[](https://mshakhomirov.medium.com/?source=post_page-----d0f974a93a59--------------------------------)[![💡Mike Shakhomirov](../Images/bc6895c7face3244d488feb97ba0f68e.png)](https://mshakhomirov.medium.com/?source=post_page-----d0f974a93a59--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d0f974a93a59--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d0f974a93a59--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----d0f974a93a59--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe06a48b3dd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftest-data-pipelines-the-fun-and-easy-way-d0f974a93a59&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=post_page-e06a48b3dd48----d0f974a93a59---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----d0f974a93a59--------------------------------) · 10分钟阅读 · 2023年2月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd0f974a93a59&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftest-data-pipelines-the-fun-and-easy-way-d0f974a93a59&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=-----d0f974a93a59---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd0f974a93a59&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftest-data-pipelines-the-fun-and-easy-way-d0f974a93a59&source=-----d0f974a93a59---------------------bookmark_footer-----------)![](../Images/ec79de49e60c28a7517c3d1f8392b35b.png)

图片由 [Simon Wilkes](https://unsplash.com/@simonfromengland?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文适用于那些希望学习如何编码以及如何运行**测试、自动化CI/CD检查，并在包括本地在内的任何环境中运行这些测试**的人。

**单元测试**是现在机器学习工程师必备的技能，它在简历上看起来很棒，增加了获得工作的机会。

我是一名数据工程师，常常需要创建**用于处理数据（ETL）的微服务**。根据任务的不同，我们可能需要执行以下操作（这不是一个详尽的列表）：

+   从一个来源提取数据并传递到另一个来源。

+   在此过程中转换数据，即更改格式、进行个人信息保护等。

+   将数据加载到其他地方，即数据仓库解决方案。

在这些情况下，我们希望保证我们的数据服务按要求执行，同时在进行更改时，自动化测试将运行以确保逻辑一致性。

> 简化

我为我构思的数据管道的每一部分使用简单的原子设计，并通过 AWS Lambda 或 Cloud Functions（Google Cloud Platform）进行部署。通过这种方式，我们可以……
