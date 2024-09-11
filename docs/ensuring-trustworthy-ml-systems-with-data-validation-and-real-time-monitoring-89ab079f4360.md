# 确保可信赖的机器学习系统通过数据验证和实时监控

> 原文：[https://towardsdatascience.com/ensuring-trustworthy-ml-systems-with-data-validation-and-real-time-monitoring-89ab079f4360?source=collection_archive---------0-----------------------#2023-06-03](https://towardsdatascience.com/ensuring-trustworthy-ml-systems-with-data-validation-and-real-time-monitoring-89ab079f4360?source=collection_archive---------0-----------------------#2023-06-03)

## [全栈 7 步 MLOps 框架](https://towardsdatascience.com/tagged/full-stack-mlops)

## 第5课：使用 GE 进行质量和完整性的数据验证。模型性能的持续监控。

[](https://pauliusztin.medium.com/?source=post_page-----89ab079f4360--------------------------------)[![Paul Iusztin](../Images/d07551a78fa87940220b49d9358f3166.png)](https://pauliusztin.medium.com/?source=post_page-----89ab079f4360--------------------------------)[](https://towardsdatascience.com/?source=post_page-----89ab079f4360--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----89ab079f4360--------------------------------) [Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----89ab079f4360--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8323de62a1a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fensuring-trustworthy-ml-systems-with-data-validation-and-real-time-monitoring-89ab079f4360&user=Paul+Iusztin&userId=8323de62a1a1&source=post_page-8323de62a1a1----89ab079f4360---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----89ab079f4360--------------------------------) · 12 分钟阅读 · 2023年6月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F89ab079f4360&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fensuring-trustworthy-ml-systems-with-data-validation-and-real-time-monitoring-89ab079f4360&user=Paul+Iusztin&userId=8323de62a1a1&source=-----89ab079f4360---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F89ab079f4360&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fensuring-trustworthy-ml-systems-with-data-validation-and-real-time-monitoring-89ab079f4360&source=-----89ab079f4360---------------------bookmark_footer-----------)![](../Images/07469861a1fd728e3dce4224cc62ce4f.png)

照片由 [Hassan Pasha](https://unsplash.com/@hpzworkz?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本教程代表了**7节课中的第5节**，将逐步指导你如何**设计、实现和部署一个机器学习系统**，使用**MLOps良好实践**。在课程中，你将构建一个生产就绪的模型，以预测来自丹麦的多个消费者类型在未来24小时的能源消耗水平。

*到课程结束时，你将掌握使用批量服务架构设计、编码和部署机器学习系统的所有基本知识。*

本课程*面向中级/高级机器学习工程师*，旨在通过构建自己的端到端项目来提升技能。

> 现如今，证书无处不在。构建高级端到端项目并展示出来是获得专业工程师认可的最佳方式。

# 目录：
