# 在 1TB 数据集上训练 XGBoost

> 原文：[`towardsdatascience.com/training-xgboost-on-a-1tb-dataset-8790e2bc8672?source=collection_archive---------9-----------------------#2023-02-08`](https://towardsdatascience.com/training-xgboost-on-a-1tb-dataset-8790e2bc8672?source=collection_archive---------9-----------------------#2023-02-08)

## SageMaker 分布式训练数据并行

[](https://ram-vegiraju.medium.com/?source=post_page-----8790e2bc8672--------------------------------)![Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----8790e2bc8672--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8790e2bc8672--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8790e2bc8672--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----8790e2bc8672--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6e49569edd2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-xgboost-on-a-1tb-dataset-8790e2bc8672&user=Ram+Vegiraju&userId=6e49569edd2b&source=post_page-6e49569edd2b----8790e2bc8672---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8790e2bc8672--------------------------------) · 7 分钟阅读 · 2023 年 2 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8790e2bc8672&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-xgboost-on-a-1tb-dataset-8790e2bc8672&user=Ram+Vegiraju&userId=6e49569edd2b&source=-----8790e2bc8672---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8790e2bc8672&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-xgboost-on-a-1tb-dataset-8790e2bc8672&source=-----8790e2bc8672---------------------bookmark_footer-----------)![](img/8cc115fe1d75334d1b016c71f5e63cec.png)

图片来源于 [Unsplash](https://unsplash.com/photos/LqKhnDzSF-8) 由 [Joshua Sortino](https://unsplash.com/@sortino) 提供

随着机器学习的不断发展，我们看到 [更大的模型](https://docs.cohere.ai/docs/introduction-to-large-language-models) 和越来越多的参数。同时，我们也看到了非常大的数据集。归根结底，任何模型的效果都取决于它所训练的数据。处理大型模型和数据集可能会在计算上昂贵且难以在及时 manner 中进行迭代或实验。本文将重点关注数据集的大型部分。具体而言，我们将研究一种称为 [分布式数据并行](https://docs.aws.amazon.com/sagemaker/latest/dg/data-parallel.html) 的方法，利用 Amazon SageMaker 优化和缩短在大规模真实数据集上的训练时间。

在今天的示例中，我们将在人工生成的 **1 TB 数据集** 上训练 [SageMaker XGBoost 算法](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html)。通过这个示例，我们将深入了解如何准备和结构化数据源以加快训练速度，并了解如何利用 SageMaker 内置的 [数据并行库](https://docs.aws.amazon.com/sagemaker/latest/dg/data-parallel.html) 开始 **分布式训练**。

**注意**：本文将假设读者具有基本的 AWS 和 SageMaker 的知识，包括 SageMaker Training 和通过 SageMaker Python SDK 和 Boto3 AWS Python SDK 与 SageMaker 和 AWS 进行交互的相关子功能。关于 SageMaker Training 的详细介绍和概述，我建议参考这篇 文章。
