# 如何使用 FastAPI 和 Google Cloud Run 部署和测试你的模型

> 原文：[https://towardsdatascience.com/how-to-deploy-and-test-your-models-using-fastapi-and-google-cloud-run-82981a44c4fe?source=collection_archive---------4-----------------------#2023-03-08](https://towardsdatascience.com/how-to-deploy-and-test-your-models-using-fastapi-and-google-cloud-run-82981a44c4fe?source=collection_archive---------4-----------------------#2023-03-08)

## 在这个完整的教程中，学习如何将你的模型转换成在云中运行的服务

[](https://medium.com/@antonsruberts?source=post_page-----82981a44c4fe--------------------------------)[![Antons Tocilins-Ruberts](../Images/363a4f32aa793cca7a67dea68e76e3cf.png)](https://medium.com/@antonsruberts?source=post_page-----82981a44c4fe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----82981a44c4fe--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----82981a44c4fe--------------------------------) [Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----82981a44c4fe--------------------------------)

·

[阅读](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9dee50b0374b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-deploy-and-test-your-models-using-fastapi-and-google-cloud-run-82981a44c4fe&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=post_page-9dee50b0374b----82981a44c4fe---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----82981a44c4fe--------------------------------) ·12分钟阅读·2023年3月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F82981a44c4fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-deploy-and-test-your-models-using-fastapi-and-google-cloud-run-82981a44c4fe&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=-----82981a44c4fe---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F82981a44c4fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-deploy-and-test-your-models-using-fastapi-and-google-cloud-run-82981a44c4fe&source=-----82981a44c4fe---------------------bookmark_footer-----------)![](../Images/109864dbe39d0cc00200a8a32e4dab4a.png)

图片来源：[Taylor Vick](https://unsplash.com/ko/@tvick?utm_source=medium&utm_medium=referral) 由 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

# 介绍

MLOps（机器学习操作）在数据专业人员中越来越受欢迎。越来越多的全栈数据科学家能够将机器学习（ML）项目从训练到生产，因此如果你在这方面感到有些不安，或想要快速复习一下，这篇文章就是为你准备的！如果你从未部署过模型，我强烈建议你查看我之前的帖子，这将为你提供更多关于ML部署的介绍，并解释许多将在本文中使用的概念。

[## ML 部署简介：Flask、Docker & Locust](/introduction-to-ml-deployment-flask-docker-locust-b87b5bd78a17?source=post_page-----82981a44c4fe--------------------------------)

### 学习如何在 Python 中部署模型并使用 Locust 测量性能

[towardsdatascience.com](/introduction-to-ml-deployment-flask-docker-locust-b87b5bd78a17?source=post_page-----82981a44c4fe--------------------------------)

本文将涵盖相当多的主题，因此可以随意跳到你最感兴趣的章节。本文的结构是：

1.  **使用 FastAPI 来指定推理端点的 API**

1.  **使用 Docker 对端点进行容器化**
