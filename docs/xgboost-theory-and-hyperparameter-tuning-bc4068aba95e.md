# XGBoost: 理论与超参数调整

> 原文：[https://towardsdatascience.com/xgboost-theory-and-hyperparameter-tuning-bc4068aba95e?source=collection_archive---------4-----------------------#2023-02-16](https://towardsdatascience.com/xgboost-theory-and-hyperparameter-tuning-bc4068aba95e?source=collection_archive---------4-----------------------#2023-02-16)

## 一份包含Python示例的完整指南

[](https://jorgemartinlasaosa.medium.com/?source=post_page-----bc4068aba95e--------------------------------)[![Jorge Martín Lasaosa](../Images/21b4e500b7d14204ea76f579c3e2433f.png)](https://jorgemartinlasaosa.medium.com/?source=post_page-----bc4068aba95e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bc4068aba95e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bc4068aba95e--------------------------------) [Jorge Martín Lasaosa](https://jorgemartinlasaosa.medium.com/?source=post_page-----bc4068aba95e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdd62b41dbbf5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-theory-and-hyperparameter-tuning-bc4068aba95e&user=Jorge+Mart%C3%ADn+Lasaosa&userId=dd62b41dbbf5&source=post_page-dd62b41dbbf5----bc4068aba95e---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----bc4068aba95e--------------------------------) · 17分钟阅读 · 2023年2月16日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbc4068aba95e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-theory-and-hyperparameter-tuning-bc4068aba95e&user=Jorge+Mart%C3%ADn+Lasaosa&userId=dd62b41dbbf5&source=-----bc4068aba95e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbc4068aba95e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-theory-and-hyperparameter-tuning-bc4068aba95e&source=-----bc4068aba95e---------------------bookmark_footer-----------)![](../Images/c1e8bc4dcd14576319bd1f517bfeaa2b.png)

图片由[Joanne Francis](https://unsplash.com/@nipawinnews?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上

# 简介

几个月后，我将有三年的数据科学家工作经验。我知道这还不是很长的职业生涯，但结合我的学术经验，我已经能够参与多个来自不同领域（能源、客户体验等）的机器学习项目。所有这些项目都依赖于*表格数据*，即结构化数据（以行和列组织）。相比之下，还有些项目依赖于如图像或文本这样的非结构化数据，这些项目更与计算机视觉或自然语言处理（NLP）等机器学习领域相关。

根据我的经验，XGBoost*通常*在*表格数据*项目中表现良好。尽管“无免费午餐定理”[1]指出，当性能在所有可能的问题上进行平均时，任何两个算法都是等效的，但在Bojan Tunguz的推特[2]上，你可以阅读到与其他专业人士关于为什么基于树的模型（尤其是XGBoost）通常是处理*表格数据*项目的最佳候选者的频繁讨论，即使在对深度学习技术在这类数据上的应用进行越来越多的研究的情况下。[3]

此外，看到一位Kaggle大师[4]开玩笑说自己是XGBoost的传播者也相当有趣。
