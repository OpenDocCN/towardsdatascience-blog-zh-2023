# 使用 Terraform 部署 SageMaker 终端节点

> 原文：[https://towardsdatascience.com/deploying-sagemaker-endpoints-with-terraform-3b09fb3e1d59?source=collection_archive---------6-----------------------#2023-03-14](https://towardsdatascience.com/deploying-sagemaker-endpoints-with-terraform-3b09fb3e1d59?source=collection_archive---------6-----------------------#2023-03-14)

## 基础设施即代码与Terraform

[](https://ram-vegiraju.medium.com/?source=post_page-----3b09fb3e1d59--------------------------------)[![Ram Vegiraju](../Images/07d9334e905f710d9f3c6187cf69a1a5.png)](https://ram-vegiraju.medium.com/?source=post_page-----3b09fb3e1d59--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3b09fb3e1d59--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3b09fb3e1d59--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----3b09fb3e1d59--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6e49569edd2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-sagemaker-endpoints-with-terraform-3b09fb3e1d59&user=Ram+Vegiraju&userId=6e49569edd2b&source=post_page-6e49569edd2b----3b09fb3e1d59---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3b09fb3e1d59--------------------------------) · 7 分钟阅读 · 2023年3月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3b09fb3e1d59&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-sagemaker-endpoints-with-terraform-3b09fb3e1d59&user=Ram+Vegiraju&userId=6e49569edd2b&source=-----3b09fb3e1d59---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3b09fb3e1d59&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-sagemaker-endpoints-with-terraform-3b09fb3e1d59&source=-----3b09fb3e1d59---------------------bookmark_footer-----------)![](../Images/09fae694c66b87492fc2ff5f7e372912.png)

图片来自 [Unsplash](https://unsplash.com/photos/KNZHyTpre18)，由 [Krishna Pandey](https://unsplash.com/@krishna2803) 提供

[基础设施即代码 (IaC)](/infrastructure-as-code-with-aws-207239573de) 是优化资源和将基础设施投入生产的关键概念。IaC 是一种古老的 DevOps/软件实践，具有几个关键好处：资源通过代码集中维护，从而优化了将架构投入生产所需的速度和协作。

这种软件最佳实践，如同许多其他实践一样，也适用于你的机器学习工具和基础设施。今天的文章将探讨如何利用一个被称为[Terraform](https://www.terraform.io/)的IaC工具，将一个预训练的SKLearn模型部署到SageMaker端点进行推理。我们将探索如何创建一个可重复使用的模板，你可以根据需要调整它以更新你的资源/硬件。通过Terraform，我们可以从拥有散落各处的独立笔记本和单独的Python文件，转变为在一个模板文件中捕捉所有必要的资源。

另一种与SageMaker相关的基础设施即代码选项是[CloudFormation](/deploying-sagemaker-endpoints-with-cloudformation-b43f7d495640)。如果这是你使用的工具，你可以参考这篇文章。请注意，**Terraform是与云提供商无关的**，它跨越不同的云提供商，而CloudFormation则专门用于AWS服务。
