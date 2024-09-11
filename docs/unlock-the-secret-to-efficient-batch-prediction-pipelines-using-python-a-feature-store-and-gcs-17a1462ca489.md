# 揭示使用 Python、特征存储和 GCS 高效批量预测管道的秘密

> 原文：[`towardsdatascience.com/unlock-the-secret-to-efficient-batch-prediction-pipelines-using-python-a-feature-store-and-gcs-17a1462ca489?source=collection_archive---------10-----------------------#2023-05-12`](https://towardsdatascience.com/unlock-the-secret-to-efficient-batch-prediction-pipelines-using-python-a-feature-store-and-gcs-17a1462ca489?source=collection_archive---------10-----------------------#2023-05-12)

## [全栈 7 步骤 MLOps 框架](https://towardsdatascience.com/tagged/full-stack-mlops)

## 第 3 课：批量预测管道。使用 Poetry 打包 Python 模块

[](https://pauliusztin.medium.com/?source=post_page-----17a1462ca489--------------------------------)![Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----17a1462ca489--------------------------------)[](https://towardsdatascience.com/?source=post_page-----17a1462ca489--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----17a1462ca489--------------------------------) [Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----17a1462ca489--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8323de62a1a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-secret-to-efficient-batch-prediction-pipelines-using-python-a-feature-store-and-gcs-17a1462ca489&user=Paul+Iusztin&userId=8323de62a1a1&source=post_page-8323de62a1a1----17a1462ca489---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----17a1462ca489--------------------------------) · 15 分钟阅读·2023 年 5 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F17a1462ca489&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-secret-to-efficient-batch-prediction-pipelines-using-python-a-feature-store-and-gcs-17a1462ca489&user=Paul+Iusztin&userId=8323de62a1a1&source=-----17a1462ca489---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F17a1462ca489&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-secret-to-efficient-batch-prediction-pipelines-using-python-a-feature-store-and-gcs-17a1462ca489&source=-----17a1462ca489---------------------bookmark_footer-----------)![](img/d8383467c77df456d69215c8e1509ca6.png)

摄影： [Hassan Pasha](https://unsplash.com/@hpzworkz?utm_source=medium&utm_medium=referral) 通过 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本教程代表**7 节课程中的第三部分**，将逐步指导你如何**设计、实施和部署一个机器学习系统**，采用**MLOps 最佳实践**。在课程中，你将构建一个生产级模型，以预测未来 24 小时内来自丹麦的多个消费者类型的能源消耗水平。

*通过本课程，你将掌握设计、编码和部署一个使用批量服务架构的机器学习系统的所有基础知识。*

本课程*面向中级/高级机器学习工程师*，旨在通过构建自己的端到端项目来提升技能。

> 如今，证书无处不在。构建先进的端到端项目，并将其展示出来，是获得专业工程师认可的最佳方式。

# 目录：

+   课程简介
