# 使用贝叶斯超参数调整有效优化你的回归模型

> 原文：[https://towardsdatascience.com/effectively-optimize-your-regression-model-with-bayesian-hyperparameter-tuning-819c19f5dab3?source=collection_archive---------8-----------------------#2023-07-17](https://towardsdatascience.com/effectively-optimize-your-regression-model-with-bayesian-hyperparameter-tuning-819c19f5dab3?source=collection_archive---------8-----------------------#2023-07-17)

## 学习如何有效优化超参数，并防止为 XGBoost、CatBoost 和 LightBoost 创建过度训练的模型

[](https://erdogant.medium.com/?source=post_page-----819c19f5dab3--------------------------------)[![Erdogan Taskesen](../Images/8e62cdae0502687710d8ae4bbcd8966e.png)](https://erdogant.medium.com/?source=post_page-----819c19f5dab3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----819c19f5dab3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----819c19f5dab3--------------------------------) [Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----819c19f5dab3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e636e2ef813&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffectively-optimize-your-regression-model-with-bayesian-hyperparameter-tuning-819c19f5dab3&user=Erdogan+Taskesen&userId=4e636e2ef813&source=post_page-4e636e2ef813----819c19f5dab3---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----819c19f5dab3--------------------------------) ·15 分钟阅读·2023年7月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F819c19f5dab3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffectively-optimize-your-regression-model-with-bayesian-hyperparameter-tuning-819c19f5dab3&user=Erdogan+Taskesen&userId=4e636e2ef813&source=-----819c19f5dab3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F819c19f5dab3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffectively-optimize-your-regression-model-with-bayesian-hyperparameter-tuning-819c19f5dab3&source=-----819c19f5dab3---------------------bookmark_footer-----------)![](../Images/85d161655226ea3f51d0055d58fa9bfe.png)

照片由 [Alexey Ruban](https://unsplash.com/@intelligenciya?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/tune?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

梯度提升技术如*XGBoost、CatBoost*和*LightBoost*近年来在分类和回归任务中获得了极大的关注。过程中的一个重要部分是调整超参数，以获得最佳模型性能。关键在于优化超参数搜索空间，并找到一个能够对新数据进行泛化的模型。*在这篇博客中，我将演示1\. 如何通过贝叶斯优化学习一个具有优化超参数的提升决策树* ***回归*** *模型，2\. 如何选择一个能够泛化的模型（而不是过度训练的），3\. 如何解释并直观地展示优化的超参数空间以及模型性能准确性。* [*HGBoost*](https://erdogant.github.io/hgboost/) *库非常适合这项任务，它执行了双重交叉验证以防止过度训练。*

# 简要介绍。

梯度提升算法，如极端梯度提升（*XGboost*）、轻量级梯度提升（*Lightboost*）和*CatBoost*，是强大的集成机器学习方法。
