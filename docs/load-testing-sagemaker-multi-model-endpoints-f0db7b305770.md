# 负载测试 SageMaker 多模型端点

> 原文：[https://towardsdatascience.com/load-testing-sagemaker-multi-model-endpoints-f0db7b305770?source=collection_archive---------11-----------------------#2023-02-24](https://towardsdatascience.com/load-testing-sagemaker-multi-model-endpoints-f0db7b305770?source=collection_archive---------11-----------------------#2023-02-24)

## 利用 Locust 在模型间分配流量权重

[](https://ram-vegiraju.medium.com/?source=post_page-----f0db7b305770--------------------------------)[![Ram Vegiraju](../Images/07d9334e905f710d9f3c6187cf69a1a5.png)](https://ram-vegiraju.medium.com/?source=post_page-----f0db7b305770--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f0db7b305770--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f0db7b305770--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----f0db7b305770--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6e49569edd2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fload-testing-sagemaker-multi-model-endpoints-f0db7b305770&user=Ram+Vegiraju&userId=6e49569edd2b&source=post_page-6e49569edd2b----f0db7b305770---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0db7b305770--------------------------------) ·9 分钟阅读·2023 年 2 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff0db7b305770&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fload-testing-sagemaker-multi-model-endpoints-f0db7b305770&user=Ram+Vegiraju&userId=6e49569edd2b&source=-----f0db7b305770---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff0db7b305770&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fload-testing-sagemaker-multi-model-endpoints-f0db7b305770&source=-----f0db7b305770---------------------bookmark_footer-----------)![](../Images/2d73100b5f9f21fc0048bf4d480216c2.png)

图片来自 [Unsplash](https://unsplash.com/photos/mTorQ9gFfOg) 由 Luis Reyes 提供

将机器学习模型投入生产是一个复杂的过程。这需要大量的迭代，包括不同的模型参数、硬件配置、流量模式等，您需要进行测试以尝试确定生产级部署的最终方案。[负载测试](https://www.blazemeter.com/blog/performance-testing-vs-load-testing-vs-stress-testing#:~:text=but%20remain%20stable.-,What%20is%20Load%20Testing%3F,systems%20handle%20heavy%20load%20volumes.) 是一项重要的软件工程实践，但在 MLOps 领域也至关重要，以评估您的模型在真实世界中的表现。

**我们如何进行负载测试？** 一个简单而高效的框架是 Python 包：[Locust](https://locust.io/)。 Locust 可以以普通模式和分布式模式模拟每秒高达数千次的事务（TPS）。在今天的博客中，我们将假设对这个包有基本了解，并简要介绍其基本原理，但有关更一般的介绍，请参考这篇[文章](/why-load-testing-is-essential-to-take-your-ml-app-to-production-faab0df1c4e1)。

**我们将测试什么模型/端点？** [SageMaker 实时推理](https://aws.amazon.com/blogs/machine-learning/part-2-model-hosting-patterns-in-amazon-sagemaker-getting-started-with-deploying-real-time-models-on-sagemaker/)是提供低延迟、高吞吐量负载的 REST 端点上服务 ML 模型的最佳选择之一。在这篇博客中，我们将特别查看一种高级托管选项，即[**SageMaker 多模型端点**](https://aws.amazon.com/blogs/machine-learning/part-3-model-hosting-patterns-in-amazon-sagemaker-run-and-optimize-multi-model-inference-with-amazon-sagemaker-multi-model-endpoints/)。在这里，我们可以在单一 REST 端点后托管数千个模型，并为每个 API 调用指定我们想要调用的目标模型……
