# 构建有效训练管道以实现最大结果指南

> 原文：[`towardsdatascience.com/a-guide-to-building-effective-training-pipelines-for-maximum-results-6fdaef594cee?source=collection_archive---------2-----------------------#2023-05-09`](https://towardsdatascience.com/a-guide-to-building-effective-training-pipelines-for-maximum-results-6fdaef594cee?source=collection_archive---------2-----------------------#2023-05-09)

## [完整的 7 步 MLOps 框架](https://towardsdatascience.com/tagged/full-stack-mlops)

## 课程 2：训练管道。机器学习平台。超参数调整。

[](https://pauliusztin.medium.com/?source=post_page-----6fdaef594cee--------------------------------)![Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----6fdaef594cee--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6fdaef594cee--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6fdaef594cee--------------------------------) [Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----6fdaef594cee--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8323de62a1a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-building-effective-training-pipelines-for-maximum-results-6fdaef594cee&user=Paul+Iusztin&userId=8323de62a1a1&source=post_page-8323de62a1a1----6fdaef594cee---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6fdaef594cee--------------------------------) · 19 分钟阅读 · 2023 年 5 月 9 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6fdaef594cee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-building-effective-training-pipelines-for-maximum-results-6fdaef594cee&user=Paul+Iusztin&userId=8323de62a1a1&source=-----6fdaef594cee---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6fdaef594cee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-building-effective-training-pipelines-for-maximum-results-6fdaef594cee&source=-----6fdaef594cee---------------------bookmark_footer-----------)![](img/27d8728645d28e45ee8757d4bedf42c2.png)

照片由 [Hassan Pasha](https://unsplash.com/@hpzworkz?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本教程代表**7 节课中的第二部分**，将逐步指导你如何**设计、实施和部署 ML 系统**，并运用**MLOps 最佳实践**。在课程期间，你将构建一个生产就绪的模型，用于预测来自丹麦的不同消费者类型在接下来的 24 小时内的能耗水平。

*在本课程结束时，你将理解如何使用批处理服务架构设计、编码和部署 ML 系统的所有基础知识。*

本课程*面向中级/高级机器学习工程师*，旨在通过构建自己的端到端项目来提升技能。

> *如今，证书随处可见。构建先进的端到端项目，并在之后展示出来，是获得专业工程师认可的最佳方式。*

# 目录：

+   课程介绍

+   课程章节
