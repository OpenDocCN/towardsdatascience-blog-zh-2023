# 部署 Cohere 语言模型在 Amazon SageMaker 上

> 原文：[https://towardsdatascience.com/deploying-cohere-language-models-on-amazon-sagemaker-23a3f79639b1?source=collection_archive---------10-----------------------#2023-05-18](https://towardsdatascience.com/deploying-cohere-language-models-on-amazon-sagemaker-23a3f79639b1?source=collection_archive---------10-----------------------#2023-05-18)

## 在 AWS 上扩展和托管 LLMs

[](https://ram-vegiraju.medium.com/?source=post_page-----23a3f79639b1--------------------------------)[![Ram Vegiraju](../Images/07d9334e905f710d9f3c6187cf69a1a5.png)](https://ram-vegiraju.medium.com/?source=post_page-----23a3f79639b1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----23a3f79639b1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----23a3f79639b1--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----23a3f79639b1--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6e49569edd2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-cohere-language-models-on-amazon-sagemaker-23a3f79639b1&user=Ram+Vegiraju&userId=6e49569edd2b&source=post_page-6e49569edd2b----23a3f79639b1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----23a3f79639b1--------------------------------) ·7 分钟阅读·2023 年 5 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F23a3f79639b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-cohere-language-models-on-amazon-sagemaker-23a3f79639b1&user=Ram+Vegiraju&userId=6e49569edd2b&source=-----23a3f79639b1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F23a3f79639b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-cohere-language-models-on-amazon-sagemaker-23a3f79639b1&source=-----23a3f79639b1---------------------bookmark_footer-----------)![](../Images/57c4cb0d9ebd119ae5f3cd5986778694.png)

来自 [Unsplash](https://unsplash.com/photos/EgwhIBec0Ck) 的图片，由 [Sigmund](https://unsplash.com/@sigmund) 拍摄

大型语言模型（LLMs）和生成式人工智能正在加速各行各业的机器学习发展。LLMs 扩展了机器学习的应用范围到不可思议的高度，但也伴随着一系列新的挑战。

LLM 的规模导致了在机器学习生命周期的训练和托管部分中出现困难的问题。特别是在托管 LLM 时，有许多挑战需要考虑。我们如何将模型适配到单个 GPU 上进行推理？我们如何在不影响准确性的情况下应用模型压缩和分区技术？我们如何提高这些 LLM 的推理延迟和吞吐量？

要解决这些问题，需要高级机器学习工程技术，其中我们必须在一个能够应用压缩和并行化技术的平台上协调模型托管。有一些解决方案，如[DJL Serving](https://github.com/deepjavalibrary/djl-serving/tree/master)，提供了针对 LLM 托管优化的[容器](https://aws.amazon.com/blogs/machine-learning/deploy-bloom-176b-and-opt-30b-on-amazon-sagemaker-with-large-model-inference-deep-learning-containers-and-deepspeed/)，但我们在本文中不会探讨这些解决方案。

在本文中，我们将探讨[SageMaker JumpStart 基础模型](https://aws.amazon.com/sagemaker/jumpstart/getting-started/?sagemaker-jumpstart-cards.sort-by=item.additionalFields.priority&sagemaker-jumpstart-cards.sort-order=asc&awsf.sagemaker-jumpstart-filter-product-type=*all&awsf.sagemaker-jumpstart-filter-text=*all&awsf.sagemaker-jumpstart-filter-vision=*all&awsf.sagemaker-jumpstart-filter-tabular=*all&awsf.sagemaker-jumpstart-filter-audio-tasks=*all&awsf.sagemaker-jumpstart-filter-multimodal=*all&awsf.sagemaker-jumpstart-filter-RL=*all)。使用基础模型时，我们不需要担心容器、模型并行化和压缩技术，而是主要关注直接部署…
