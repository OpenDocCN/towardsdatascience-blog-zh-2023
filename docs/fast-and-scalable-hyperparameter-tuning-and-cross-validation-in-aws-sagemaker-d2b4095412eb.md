# AWS SageMaker 中的快速且可扩展的超参数调整和交叉验证

> 原文：[`towardsdatascience.com/fast-and-scalable-hyperparameter-tuning-and-cross-validation-in-aws-sagemaker-d2b4095412eb?source=collection_archive---------3-----------------------#2023-03-03`](https://towardsdatascience.com/fast-and-scalable-hyperparameter-tuning-and-cross-validation-in-aws-sagemaker-d2b4095412eb?source=collection_archive---------3-----------------------#2023-03-03)

## 使用 SageMaker 托管的热池

[](https://medium.com/@joao.pereira.abt?source=post_page-----d2b4095412eb--------------------------------)![João Pereira](https://medium.com/@joao.pereira.abt?source=post_page-----d2b4095412eb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d2b4095412eb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d2b4095412eb--------------------------------) [João Pereira](https://medium.com/@joao.pereira.abt?source=post_page-----d2b4095412eb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6743ea128017&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffast-and-scalable-hyperparameter-tuning-and-cross-validation-in-aws-sagemaker-d2b4095412eb&user=Jo%C3%A3o+Pereira&userId=6743ea128017&source=post_page-6743ea128017----d2b4095412eb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d2b4095412eb--------------------------------) ·8 min read·2023 年 3 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd2b4095412eb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffast-and-scalable-hyperparameter-tuning-and-cross-validation-in-aws-sagemaker-d2b4095412eb&user=Jo%C3%A3o+Pereira&userId=6743ea128017&source=-----d2b4095412eb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd2b4095412eb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffast-and-scalable-hyperparameter-tuning-and-cross-validation-in-aws-sagemaker-d2b4095412eb&source=-----d2b4095412eb---------------------bookmark_footer-----------)![](img/125320a8ffef1abf9fcab6a60572290d.png)

图片由 [SpaceX](https://unsplash.com/@spacex?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

本文分享了一个配方，通过利用 SageMaker 托管的热池来**提高 60%**超参数调整与交叉验证的速度。通过使用热池，具有 120 个顺序作业的调整步骤的运行时间从**10 小时减少到 4 小时**。

提升和评估机器学习模型的性能通常需要多种因素。超参数调优和交叉验证就是这两种因素。前者找到模型的最佳版本，而后者估计模型对未见数据的泛化能力。这些步骤结合在一起，会带来计算挑战，因为它们需要多次训练和验证模型，可能是并行或顺序进行的。

***本文内容…***

+   什么是暖池（Warm Pools），以及如何利用它们来加速超参数调优与交叉验证。

+   如何设计一个生产级别的 SageMaker 管道，包括处理、调优、训练和 Lambda 步骤。

我们将考虑贝叶斯优化用于超参数调优，该方法利用已经测试过的超参数组合的评分来选择下一个要测试的超参数集……
