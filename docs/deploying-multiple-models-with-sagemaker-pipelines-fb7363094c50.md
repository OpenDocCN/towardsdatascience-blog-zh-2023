# 使用 SageMaker Pipelines 部署多个模型

> 原文：[https://towardsdatascience.com/deploying-multiple-models-with-sagemaker-pipelines-fb7363094c50?source=collection_archive---------7-----------------------#2023-03-23](https://towardsdatascience.com/deploying-multiple-models-with-sagemaker-pipelines-fb7363094c50?source=collection_archive---------7-----------------------#2023-03-23)

## 应用 MLOps 最佳实践于高级服务选项

[](https://ram-vegiraju.medium.com/?source=post_page-----fb7363094c50--------------------------------)[![Ram Vegiraju](../Images/07d9334e905f710d9f3c6187cf69a1a5.png)](https://ram-vegiraju.medium.com/?source=post_page-----fb7363094c50--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fb7363094c50--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fb7363094c50--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----fb7363094c50--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6e49569edd2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-multiple-models-with-sagemaker-pipelines-fb7363094c50&user=Ram+Vegiraju&userId=6e49569edd2b&source=post_page-6e49569edd2b----fb7363094c50---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fb7363094c50--------------------------------) · 8 分钟阅读 · 2023年3月23日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffb7363094c50&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-multiple-models-with-sagemaker-pipelines-fb7363094c50&user=Ram+Vegiraju&userId=6e49569edd2b&source=-----fb7363094c50---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffb7363094c50&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-multiple-models-with-sagemaker-pipelines-fb7363094c50&source=-----fb7363094c50---------------------bookmark_footer-----------)![](../Images/c86ec39899ba0ad0ba89fd4df2d483db.png)

图片来自 [Unsplash](https://unsplash.com/photos/f7uCQxhucw4) 由 [Growtika](https://unsplash.com/@growtika) 提供

MLOps 是将机器学习工作流投入生产的关键实践。通过 MLOps，你可以建立针对 ML 生命周期的工作流。这些工作流使得集中维护资源、更新/跟踪模型变得更加容易，并且在机器学习实验扩展时，整体上简化了过程。

在[Amazon SageMaker](https://aws.amazon.com/sagemaker/)生态系统中，一个关键的 MLOps 工具是[SageMaker Pipelines](https://aws.amazon.com/sagemaker/pipelines/)。使用 SageMaker Pipelines，你可以定义由不同的 ML **步骤** 组成的工作流。你还可以通过定义 **参数** 来构建这些工作流，将其作为变量注入到你的 Pipeline 中。有关 SageMaker Pipelines 的更一般介绍，请参见[相关文章](/an-introduction-to-sagemaker-pipelines-4018a819352d)。

定义一个 Pipeline 本身并不复杂，但有一些高级用例需要额外的配置。具体来说，假设你在训练多个模型，这些模型在你的机器学习用例中是用于推理的。在 SageMaker 中，有一个称为[多模型端点](https://docs.aws.amazon.com/sagemaker/latest/dg/multi-model-endpoints.html)（MME）的托管选项，你可以在一个单一的端点上托管多个模型并调用目标模型。然而，在 SageMaker Pipelines 中，目前还没有原生支持定义或部署 MME 的功能。在这篇博客文章中…
