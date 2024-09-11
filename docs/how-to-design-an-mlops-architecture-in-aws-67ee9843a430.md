# 如何在AWS中设计MLOps架构？

> 原文：[https://towardsdatascience.com/how-to-design-an-mlops-architecture-in-aws-67ee9843a430?source=collection_archive---------12-----------------------#2023-04-14](https://towardsdatascience.com/how-to-design-an-mlops-architecture-in-aws-67ee9843a430?source=collection_archive---------12-----------------------#2023-04-14)

## 为开发人员和架构师，特别是那些不专门从事机器学习的人员提供设计MLOps架构的指南

[](https://medium.com/@harminder1?source=post_page-----67ee9843a430--------------------------------)[![哈敏德·辛格](../Images/e02deb224f2943feefe2d075d196c4e8.png)](https://medium.com/@harminder1?source=post_page-----67ee9843a430--------------------------------)[](https://towardsdatascience.com/?source=post_page-----67ee9843a430--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----67ee9843a430--------------------------------) [哈敏德·辛格](https://medium.com/@harminder1?source=post_page-----67ee9843a430--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb46b54d2be3f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-design-an-mlops-architecture-in-aws-67ee9843a430&user=Harminder+Singh&userId=b46b54d2be3f&source=post_page-b46b54d2be3f----67ee9843a430---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----67ee9843a430--------------------------------) ·8分钟阅读·2023年4月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F67ee9843a430&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-design-an-mlops-architecture-in-aws-67ee9843a430&user=Harminder+Singh&userId=b46b54d2be3f&source=-----67ee9843a430---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F67ee9843a430&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-design-an-mlops-architecture-in-aws-67ee9843a430&source=-----67ee9843a430---------------------bookmark_footer-----------)

## **介绍**

根据 Gartner 的研究，仅有 53% 的机器学习（ML）项目从概念验证（POC）阶段进入生产阶段。公司战略目标与数据科学家构建的机器学习模型之间往往存在不一致。DevOps、安全、法律、IT 和数据科学家之间可能缺乏沟通，这给将模型推向生产阶段带来了挑战。最后，团队可能在推送新模型的同时发现维护生产环境中的模型困难重重。这导致了 MLOps 的兴起，它将 DevOps 的原则，如持续集成和持续交付（CI/CD）、自动化和协作，带入机器学习生命周期——开发、部署和监控。

在这篇文章中，我将探讨以下内容：

+   机器学习过程中的各种步骤

+   不同的 MLOps 组件及其必要性解释，而不深入探讨数据科学家需要了解的细节

+   基于组织规模和成熟度的 MLOps 架构图

+   关于启动 MLOps 的一般建议…
