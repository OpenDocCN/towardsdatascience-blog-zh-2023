# 在 Amazon SageMaker 上部署 LLMs 与 DJL Serving

> 原文：[https://towardsdatascience.com/deploying-llms-on-amazon-sagemaker-with-djl-serving-8220e3cfad0c?source=collection_archive---------8-----------------------#2023-06-07](https://towardsdatascience.com/deploying-llms-on-amazon-sagemaker-with-djl-serving-8220e3cfad0c?source=collection_archive---------8-----------------------#2023-06-07)

## 在 Amazon SageMaker 上实时推理部署 BART

[](https://ram-vegiraju.medium.com/?source=post_page-----8220e3cfad0c--------------------------------)[![Ram Vegiraju](../Images/07d9334e905f710d9f3c6187cf69a1a5.png)](https://ram-vegiraju.medium.com/?source=post_page-----8220e3cfad0c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8220e3cfad0c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8220e3cfad0c--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----8220e3cfad0c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6e49569edd2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-llms-on-amazon-sagemaker-with-djl-serving-8220e3cfad0c&user=Ram+Vegiraju&userId=6e49569edd2b&source=post_page-6e49569edd2b----8220e3cfad0c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8220e3cfad0c--------------------------------) ·8 分钟阅读·2023年6月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8220e3cfad0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-llms-on-amazon-sagemaker-with-djl-serving-8220e3cfad0c&user=Ram+Vegiraju&userId=6e49569edd2b&source=-----8220e3cfad0c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8220e3cfad0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-llms-on-amazon-sagemaker-with-djl-serving-8220e3cfad0c&source=-----8220e3cfad0c---------------------bookmark_footer-----------)![](../Images/27d3d2701b592cc5346c24eb2c67afe6.png)

图片来源 [Unsplash](https://unsplash.com/photos/CejqWHRRXUQ)

大型语言模型（LLMs）和生成式 AI 在 2023 年继续主导机器学习和科技领域。随着 LLM 的扩展，新模型的涌现不断以惊人的速度提升。

尽管这些模型的准确性和性能令人难以置信，但在托管这些模型时仍面临一系列挑战。如果没有模型托管，就很难认识到这些 LLM 在实际应用中的价值。LLM 托管和性能调优面临的具体挑战是什么？

+   我们如何加载这些扩展到超过100 GB的大型模型？

+   我们如何正确地应用模型划分技术以高效利用硬件，同时不妥协模型准确性？

+   我们如何将这些模型适配到单个GPU或多个GPU上？

这些都是通过被称为[DJL Serving](http://djl.ai/serving/)的模型服务器解决并抽象出来的挑战性问题。DJL Serving是一个高性能的通用解决方案，直接集成了各种模型划分框架，如[HuggingFace Accelerate](https://huggingface.co/docs/accelerate/index)、[DeepSpeed](https://github.com/microsoft/DeepSpeed)和[FasterTransformers](https://github.com/NVIDIA/FasterTransformer)。通过DJL Serving，你可以…
