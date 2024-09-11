# 在 GCP 上使用 GitHub Actions 实现无缝 CI/CD 流水线：有效 MLOps 的工具

> 原文：[https://towardsdatascience.com/seamless-ci-cd-pipelines-with-github-actions-on-gcp-your-tools-for-effective-mlops-96f676f72012?source=collection_archive---------9-----------------------#2023-06-15](https://towardsdatascience.com/seamless-ci-cd-pipelines-with-github-actions-on-gcp-your-tools-for-effective-mlops-96f676f72012?source=collection_archive---------9-----------------------#2023-06-15)

## [完整堆栈 7 步 MLOps 框架](https://towardsdatascience.com/tagged/full-stack-mlops)

## 第 7 课：将所有 ML 组件部署到 GCP。使用 GitHub Actions 构建 CI/CD 流水线。

[](https://pauliusztin.medium.com/?source=post_page-----96f676f72012--------------------------------)[![Paul Iusztin](../Images/d07551a78fa87940220b49d9358f3166.png)](https://pauliusztin.medium.com/?source=post_page-----96f676f72012--------------------------------)[](https://towardsdatascience.com/?source=post_page-----96f676f72012--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----96f676f72012--------------------------------) [Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----96f676f72012--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8323de62a1a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fseamless-ci-cd-pipelines-with-github-actions-on-gcp-your-tools-for-effective-mlops-96f676f72012&user=Paul+Iusztin&userId=8323de62a1a1&source=post_page-8323de62a1a1----96f676f72012---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----96f676f72012--------------------------------) · 19 分钟阅读 · 2023年6月15日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F96f676f72012&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fseamless-ci-cd-pipelines-with-github-actions-on-gcp-your-tools-for-effective-mlops-96f676f72012&user=Paul+Iusztin&userId=8323de62a1a1&source=-----96f676f72012---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F96f676f72012&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fseamless-ci-cd-pipelines-with-github-actions-on-gcp-your-tools-for-effective-mlops-96f676f72012&source=-----96f676f72012---------------------bookmark_footer-----------)![](../Images/7c34e9ba645996ba5d4b808f1f130f93.png)

图片由 [Hassan Pasha](https://unsplash.com/@hpzworkz?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本教程代表了**第7课中的第7节课**，将逐步指导你如何**设计、实施和部署ML系统**，采用**MLOps良好实践**。在课程中，你将构建一个生产就绪的模型，以预测来自丹麦的多个消费者类型在接下来24小时的能耗水平。

*课程结束时，你将理解使用批量服务架构设计、编码和部署ML系统的所有基础知识。*

本课程*针对中级/高级机器学习工程师*，希望通过构建自己的端到端项目来提升技能。

> 现在，证书随处可见。构建先进的端到端项目，并在后续展示，这是获得专业工程师认可的最佳途径。

# 目录：
