# 构建生产就绪的特征工程管道框架

> 原文：[`towardsdatascience.com/a-framework-for-building-a-production-ready-feature-engineering-pipeline-f0b29609b20f?source=collection_archive---------0-----------------------#2023-04-28`](https://towardsdatascience.com/a-framework-for-building-a-production-ready-feature-engineering-pipeline-f0b29609b20f?source=collection_archive---------0-----------------------#2023-04-28)

## [全栈 7 步 MLOps 框架](https://towardsdatascience.com/tagged/full-stack-mlops)

## 第 1 课：批处理服务。特征存储。特征工程管道。

[](https://pauliusztin.medium.com/?source=post_page-----f0b29609b20f--------------------------------)![Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----f0b29609b20f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f0b29609b20f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0b29609b20f--------------------------------) [Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----f0b29609b20f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8323de62a1a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-framework-for-building-a-production-ready-feature-engineering-pipeline-f0b29609b20f&user=Paul+Iusztin&userId=8323de62a1a1&source=post_page-8323de62a1a1----f0b29609b20f---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0b29609b20f--------------------------------) ·13 分钟阅读·2023 年 4 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff0b29609b20f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-framework-for-building-a-production-ready-feature-engineering-pipeline-f0b29609b20f&user=Paul+Iusztin&userId=8323de62a1a1&source=-----f0b29609b20f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff0b29609b20f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-framework-for-building-a-production-ready-feature-engineering-pipeline-f0b29609b20f&source=-----f0b29609b20f---------------------bookmark_footer-----------)![](img/2ae381d9f40ec629b5dacf7b06536a4e.png)

照片由 [Hassan Pasha](https://unsplash.com/@hpzworkz?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本教程代表**7 节课中的第一部分课**，将逐步指导你如何**设计、实现和部署 ML 系统**，并使用**MLOps 最佳实践**。在课程中，你将构建一个生产就绪的模型，以预测来自丹麦的多个消费者类型在未来 24 小时内的能源消耗水平。

*在本课程结束时，你将掌握使用批处理服务架构设计、编码和部署 ML 系统的所有基础知识。*

本课程*针对中高级机器学习工程师*，旨在通过构建自己的端到端项目来提升技能。

> 如今，证书随处可见。构建先进的端到端项目，并在之后展示出来，是获得专业工程师认可的最佳途径。

# 目录：

+   课程介绍

+   课程内容
