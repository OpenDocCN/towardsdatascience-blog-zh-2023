# 使用 Airflow 解锁 MLOps：ML 系统编排的全面指南

> 原文：[`towardsdatascience.com/unlocking-mlops-using-airflow-a-comprehensive-guide-to-ml-system-orchestration-880aa9be8cff?source=collection_archive---------3-----------------------#2023-05-23`](https://towardsdatascience.com/unlocking-mlops-using-airflow-a-comprehensive-guide-to-ml-system-orchestration-880aa9be8cff?source=collection_archive---------3-----------------------#2023-05-23)

## [完整的 7 步 MLOps 框架](https://towardsdatascience.com/tagged/full-stack-mlops)

## 第 4 课：私有 PyPi 服务器。用 Airflow 编排一切。

[](https://pauliusztin.medium.com/?source=post_page-----880aa9be8cff--------------------------------)![Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----880aa9be8cff--------------------------------)[](https://towardsdatascience.com/?source=post_page-----880aa9be8cff--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----880aa9be8cff--------------------------------) [Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----880aa9be8cff--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8323de62a1a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlocking-mlops-using-airflow-a-comprehensive-guide-to-ml-system-orchestration-880aa9be8cff&user=Paul+Iusztin&userId=8323de62a1a1&source=post_page-8323de62a1a1----880aa9be8cff---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----880aa9be8cff--------------------------------) · 17 分钟阅读 · 2023 年 5 月 23 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F880aa9be8cff&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlocking-mlops-using-airflow-a-comprehensive-guide-to-ml-system-orchestration-880aa9be8cff&user=Paul+Iusztin&userId=8323de62a1a1&source=-----880aa9be8cff---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F880aa9be8cff&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlocking-mlops-using-airflow-a-comprehensive-guide-to-ml-system-orchestration-880aa9be8cff&source=-----880aa9be8cff---------------------bookmark_footer-----------)![](img/3c440d7e58f12e1041fcde014eea4fda.png)

图片由 [Hassan Pasha](https://unsplash.com/@hpzworkz?utm_source=medium&utm_medium=referral) 拍摄，刊登于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本教程代表**7 节课程中的第四部分课**，将一步步指导你如何**设计、实施和部署一个 ML 系统**，并应用**MLOps 最佳实践**。在课程中，你将构建一个生产就绪的模型，以预测未来 24 小时丹麦多个消费类型的能耗水平。

*到课程结束时，你将理解使用批处理服务架构设计、编码和部署 ML 系统的所有基础知识。*

本课程*针对中级/高级机器学习工程师*，希望通过构建自己的端到端项目来提升技能。

> *如今，证书随处可见。构建先进的端到端项目，并在以后展示，这是获得专业工程师认可的最佳方式。*

# 目录：

+   课程简介
