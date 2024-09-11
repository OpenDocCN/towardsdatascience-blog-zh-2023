# 使用数据中心可解释的人工智能解读机器学习模型

> 原文：[https://towardsdatascience.com/interpreting-machine-learning-models-using-data-centric-explainable-ai-8415be070416?source=collection_archive---------0-----------------------#2023-02-26](https://towardsdatascience.com/interpreting-machine-learning-models-using-data-centric-explainable-ai-8415be070416?source=collection_archive---------0-----------------------#2023-02-26)

## 了解数据中心解释及其不同类型，请阅读本文

[](https://adib0073.medium.com/?source=post_page-----8415be070416--------------------------------)[![Aditya Bhattacharya](../Images/d0f79ad4a85330c58327aea499b7eea0.png)](https://adib0073.medium.com/?source=post_page-----8415be070416--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8415be070416--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8415be070416--------------------------------) [Aditya Bhattacharya](https://adib0073.medium.com/?source=post_page-----8415be070416--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4bb294d0fe6b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finterpreting-machine-learning-models-using-data-centric-explainable-ai-8415be070416&user=Aditya+Bhattacharya&userId=4bb294d0fe6b&source=post_page-4bb294d0fe6b----8415be070416---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8415be070416--------------------------------) ·8分钟阅读·2023年2月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8415be070416&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finterpreting-machine-learning-models-using-data-centric-explainable-ai-8415be070416&user=Aditya+Bhattacharya&userId=4bb294d0fe6b&source=-----8415be070416---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8415be070416&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finterpreting-machine-learning-models-using-data-centric-explainable-ai-8415be070416&source=-----8415be070416---------------------bookmark_footer-----------)![](../Images/64e9edc18d4081642554bc54fda2e7a9.png)

来源: [Pixabay](https://pixabay.com/illustrations/charts-tables-graph-statistics-6246450/)

[**可解释的人工智能 (XAI)**](https://amzn.to/3cY4c2h) 是一个新兴概念，旨在弥合人工智能与终端用户之间的差距，从而提升人工智能的采纳度。XAI 可以使AI/ML模型更加透明、可信和易于理解。这在医疗保健、金融和执法等关键领域尤其必要。

要了解 XAI，以下来自**2021 年 AI 加速器节 APAC**的 45 分钟演讲将非常有帮助：

[流行的 XAI 方法](https://amzn.to/3J2QNnz)，如 LIME、SHAP、显著性图等，是[**以模型为中心的解释方法**](https://amzn.to/3J2QNnz)。这些方法近似于机器学习模型用于生成预测的关键特征。然而，由于 ML 模型的归纳偏差，对预测模型考虑的关键特征的估计可能并不总是准确。因此，以模型为中心的特征重要性方法可能并不总是非常有用。
