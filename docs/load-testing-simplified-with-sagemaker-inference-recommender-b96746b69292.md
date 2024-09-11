# 使用 SageMaker 推理推荐器简化负载测试

> 原文：[`towardsdatascience.com/load-testing-simplified-with-sagemaker-inference-recommender-b96746b69292?source=collection_archive---------13-----------------------#2023-03-07`](https://towardsdatascience.com/load-testing-simplified-with-sagemaker-inference-recommender-b96746b69292?source=collection_archive---------13-----------------------#2023-03-07)

## 在 SageMaker 实时终端上测试 TensorFlow ResNet50

[](https://ram-vegiraju.medium.com/?source=post_page-----b96746b69292--------------------------------)![Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----b96746b69292--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b96746b69292--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b96746b69292--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----b96746b69292--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6e49569edd2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fload-testing-simplified-with-sagemaker-inference-recommender-b96746b69292&user=Ram+Vegiraju&userId=6e49569edd2b&source=post_page-6e49569edd2b----b96746b69292---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b96746b69292--------------------------------) ·7 min read·2023 年 3 月 7 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb96746b69292&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fload-testing-simplified-with-sagemaker-inference-recommender-b96746b69292&source=-----b96746b69292---------------------bookmark_footer-----------)![](img/df9ae88a89ce233ff5f795f837fb53bf.png)

图片来自 [Unsplash](https://unsplash.com/photos/o4WaeT3XhV4)，由**Amokrane Ait-Kaci**提供

过去我曾广泛讨论过在将机器学习模型部署到生产环境之前进行 负载测试 的重要性。特别是对于实时推理用例，确保你的解决方案满足目标延迟和吞吐量是至关重要的。我们还探讨了如何使用 Python 库 [Locust](https://locust.io/) 定义脚本，以模拟我们期望的流量模式。

虽然 Locust 是一个非常强大的工具，但设置起来可能比较困难，并且需要在不同的超参数和硬件上进行大量的迭代，以确定适合生产的配置。对于 [SageMaker 实时推理](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints.html)，一个值得关注的关键工具是 [SageMaker 推理推荐器](https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender.html)。你可以将一系列 EC2 实例类型传入以测试你的端点，以及针对你特定模型容器的超参数，以便进行更高级的部署，而无需反复运行 Locust 脚本。在今天的博客中，我们将探讨如何配置这一功能以及如何简化对 SageMaker 实时端点的负载测试。
