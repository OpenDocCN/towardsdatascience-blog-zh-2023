# 数据平台的持续集成和部署

> 原文：[https://towardsdatascience.com/continuous-integration-and-deployment-for-data-platforms-817bf1b6bed1?source=collection_archive---------10-----------------------#2023-04-14](https://towardsdatascience.com/continuous-integration-and-deployment-for-data-platforms-817bf1b6bed1?source=collection_archive---------10-----------------------#2023-04-14)

## 数据工程师和机器学习运维的CI/CD

[](https://mshakhomirov.medium.com/?source=post_page-----817bf1b6bed1--------------------------------)[![💡Mike Shakhomirov](../Images/bc6895c7face3244d488feb97ba0f68e.png)](https://mshakhomirov.medium.com/?source=post_page-----817bf1b6bed1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----817bf1b6bed1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----817bf1b6bed1--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----817bf1b6bed1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe06a48b3dd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcontinuous-integration-and-deployment-for-data-platforms-817bf1b6bed1&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=post_page-e06a48b3dd48----817bf1b6bed1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----817bf1b6bed1--------------------------------) ·9 min read·2023年4月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F817bf1b6bed1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcontinuous-integration-and-deployment-for-data-platforms-817bf1b6bed1&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=-----817bf1b6bed1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F817bf1b6bed1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcontinuous-integration-and-deployment-for-data-platforms-817bf1b6bed1&source=-----817bf1b6bed1---------------------bookmark_footer-----------)![](../Images/fd7ac2298a3ebce9921e027ae8163cd9.png)

[照片由Emmy Sobieski](https://unsplash.com/@emmy_s?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

什么是数据环境？数据工程师将基础设施资源分为实时和暂存，以创建隔离的区域（环境），在这些区域中，他们可以测试ETL服务和数据管道，然后再将其推广到生产环境中。

**数据环境**指的是一组应用程序和相关的物理基础设施资源，这些资源支持数据存储、传输、处理和数据转换，以支持公司的目标和任务。

**这个故事**提供了**数据领域的CI/CD技术概述**，以及一个用**Python**构建并通过**基础设施即代码**（IaC）和**Github Actions**部署的简单**ETL**服务的**工作示例**。

## 持续集成和持续交付（CI/CD）

持续集成和持续交付（CI/CD）是一种软件开发策略，其中所有开发人员在一个共同的代码库上协作，当进行更改时，使用自动化构建过程来发现任何潜在的代码问题。

![](../Images/6a68cf39e925c7b6f79982657c32ab9f.png)

图片来源于作者

## CI/CD的好处
