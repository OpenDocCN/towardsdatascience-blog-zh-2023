# 在GCP无服务器环境下部署TFLite模型

> 原文：[https://towardsdatascience.com/deploying-tflite-model-on-gcp-serverless-b4cd84f86de1?source=collection_archive---------21-----------------------#2023-07-21](https://towardsdatascience.com/deploying-tflite-model-on-gcp-serverless-b4cd84f86de1?source=collection_archive---------21-----------------------#2023-07-21)

## 如何以无服务器的方式部署量化模型

[](https://vishal-ai.medium.com/?source=post_page-----b4cd84f86de1--------------------------------)[![Vishal Rajput](../Images/c43407d7df1f099832cbaa5381a0bb74.png)](https://vishal-ai.medium.com/?source=post_page-----b4cd84f86de1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b4cd84f86de1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b4cd84f86de1--------------------------------) [Vishal Rajput](https://vishal-ai.medium.com/?source=post_page-----b4cd84f86de1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9003acebe0b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-tflite-model-on-gcp-serverless-b4cd84f86de1&user=Vishal+Rajput&userId=9003acebe0b5&source=post_page-9003acebe0b5----b4cd84f86de1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b4cd84f86de1--------------------------------) ·11 min read·2023年7月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb4cd84f86de1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-tflite-model-on-gcp-serverless-b4cd84f86de1&user=Vishal+Rajput&userId=9003acebe0b5&source=-----b4cd84f86de1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb4cd84f86de1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-tflite-model-on-gcp-serverless-b4cd84f86de1&source=-----b4cd84f86de1---------------------bookmark_footer-----------)

模型部署非常复杂；由于云平台和其他AI相关库几乎每周都会更新，向后兼容性以及找到正确的部署方法是一个大挑战。在今天的博客文章中，我们将探讨如何在**Google Cloud Platform**上以**无服务器**的方式部署**tflite模型**。

本博客文章的结构如下：

+   理解无服务器架构和其他部署方式

+   什么是量化和TFLite？

+   使用GCP Cloud Run API部署TFLite模型

![](../Images/c1719de604d34543f57af5f33b0bf5ce.png)

图片来源：[https://pixabay.com/photos/man-pier-silhouette-sunrise-fog-8091933/](https://pixabay.com/photos/man-pier-silhouette-sunrise-fog-8091933/)

# 了解无服务器和其他部署方式

> **首先，我们需要理解无服务器的含义，因为无服务器并不意味着没有服务器。**

一个 AI 模型，或者任何应用程序，都可以通过几种不同的方式进行部署，主要有三大分类。

**无服务器：** 在这种情况下，模型存储在云容器注册表中，仅在用户发出请求时运行。当请求被发出时，会自动启动一个服务器实例来满足用户请求，之后会在一段时间后关闭…
