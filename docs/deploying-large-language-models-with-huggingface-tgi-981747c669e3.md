# 使用 HuggingFace TGI 部署大型语言模型

> 原文：[`towardsdatascience.com/deploying-large-language-models-with-huggingface-tgi-981747c669e3?source=collection_archive---------3-----------------------#2023-07-14`](https://towardsdatascience.com/deploying-large-language-models-with-huggingface-tgi-981747c669e3?source=collection_archive---------3-----------------------#2023-07-14)

## 使用 Amazon SageMaker 高效托管和扩展您的 LLMs

[](https://ram-vegiraju.medium.com/?source=post_page-----981747c669e3--------------------------------)![Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----981747c669e3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----981747c669e3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----981747c669e3--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----981747c669e3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6e49569edd2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-large-language-models-with-huggingface-tgi-981747c669e3&user=Ram+Vegiraju&userId=6e49569edd2b&source=post_page-6e49569edd2b----981747c669e3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----981747c669e3--------------------------------) · 5 分钟阅读 · 2023 年 7 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F981747c669e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-large-language-models-with-huggingface-tgi-981747c669e3&user=Ram+Vegiraju&userId=6e49569edd2b&source=-----981747c669e3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F981747c669e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-large-language-models-with-huggingface-tgi-981747c669e3&source=-----981747c669e3---------------------bookmark_footer-----------)![](img/87080a29c8a17cddf9ed8b4ece860f12.png)

图片来自 [Unsplash](https://unsplash.com/photos/4NYtYSiZVlA)

随着几乎每周都有新的大型语言模型（LLMs）发布，这些模型的受欢迎程度持续上升。随着这些模型数量的增加，我们托管它们的选项也在增多。在我上一篇文章中，我们探讨了如何利用 [DJL Serving](https://github.com/deepjavalibrary/djl-serving) 在 Amazon SageMaker 中高效托管 LLMs。本文我们将深入探讨另一个优化的模型服务器和解决方案，即 [HuggingFace 文本生成推理（TGI）](https://github.com/huggingface/text-generation-inference)。

**注意**: 对于那些刚接触 AWS 的人，如果你想跟进，请务必在以下[链接](https://aws.amazon.com/console/)注册一个账户。本文还假设你对 SageMaker 部署有中级的理解，我建议阅读这篇[文章](https://aws.amazon.com/blogs/machine-learning/part-2-model-hosting-patterns-in-amazon-sagemaker-getting-started-with-deploying-real-time-models-on-sagemaker/)更深入地了解部署/推理。

**免责声明**: 我是 AWS 的机器学习架构师，本文中的观点仅代表个人观点。

## 为什么选择 HuggingFace 文本生成推理？它如何与 Amazon SageMaker 协同工作？

TGI 是由 HuggingFace 创建的 Rust、Python、gRPC 模型服务器，可用于托管特定的大型语言模型。HuggingFace 长期以来一直是 NLP 领域的中心枢纽，拥有大量的...
