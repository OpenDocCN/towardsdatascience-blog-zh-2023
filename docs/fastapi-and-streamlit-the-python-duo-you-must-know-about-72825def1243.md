# FastAPI 和 Streamlit：你必须了解的 Python 双雄

> 原文：[https://towardsdatascience.com/fastapi-and-streamlit-the-python-duo-you-must-know-about-72825def1243?source=collection_archive---------2-----------------------#2023-06-12](https://towardsdatascience.com/fastapi-and-streamlit-the-python-duo-you-must-know-about-72825def1243?source=collection_archive---------2-----------------------#2023-06-12)

## [完整的 7 步 MLOps 框架](https://towardsdatascience.com/tagged/full-stack-mlops)

## 第6课：使用 FastAPI 和 Streamlit 消耗并可视化模型的预测。容器化所有内容

[](https://pauliusztin.medium.com/?source=post_page-----72825def1243--------------------------------)[![Paul Iusztin](../Images/d07551a78fa87940220b49d9358f3166.png)](https://pauliusztin.medium.com/?source=post_page-----72825def1243--------------------------------)[](https://towardsdatascience.com/?source=post_page-----72825def1243--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----72825def1243--------------------------------) [Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----72825def1243--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8323de62a1a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffastapi-and-streamlit-the-python-duo-you-must-know-about-72825def1243&user=Paul+Iusztin&userId=8323de62a1a1&source=post_page-8323de62a1a1----72825def1243---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----72825def1243--------------------------------) ·14分钟阅读·2023年6月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F72825def1243&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffastapi-and-streamlit-the-python-duo-you-must-know-about-72825def1243&user=Paul+Iusztin&userId=8323de62a1a1&source=-----72825def1243---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F72825def1243&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffastapi-and-streamlit-the-python-duo-you-must-know-about-72825def1243&source=-----72825def1243---------------------bookmark_footer-----------)![](../Images/7beb5df667e2b1bf00cd37002c98447f.png)

照片由 [Hassan Pasha](https://unsplash.com/@hpzworkz?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本教程代表了**7节课中的第6节**，将逐步指导你如何**设计、实现和部署一个机器学习系统**，使用**MLOps的最佳实践**。在课程中，你将构建一个生产就绪的模型，以预测丹麦多种消费者类型未来24小时的能源消耗水平。

*到本课程结束时，你将理解使用批处理服务架构设计、编码和部署机器学习系统的所有基础知识。*

本课程*面向中级/高级机器学习工程师*，旨在通过构建自己的端到端项目来提升技能。

> *如今，证书随处可见。构建高级端到端项目并展示出来是获得专业工程师认可的最佳方式。*

# 目录：

+   课程介绍
