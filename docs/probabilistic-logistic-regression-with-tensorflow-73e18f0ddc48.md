# **带 TensorFlow 的概率逻辑回归**

> 原文：[`towardsdatascience.com/probabilistic-logistic-regression-with-tensorflow-73e18f0ddc48?source=collection_archive---------12-----------------------#2023-01-25`](https://towardsdatascience.com/probabilistic-logistic-regression-with-tensorflow-73e18f0ddc48?source=collection_archive---------12-----------------------#2023-01-25)

## **概率深度学习**

[](https://medium.com/@luisroque?source=post_page-----73e18f0ddc48--------------------------------)![Luís Roque](https://medium.com/@luisroque?source=post_page-----73e18f0ddc48--------------------------------)[](https://towardsdatascience.com/?source=post_page-----73e18f0ddc48--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----73e18f0ddc48--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----73e18f0ddc48--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprobabilistic-logistic-regression-with-tensorflow-73e18f0ddc48&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----73e18f0ddc48---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----73e18f0ddc48--------------------------------) · 9 分钟阅读 · 2023 年 1 月 25 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F73e18f0ddc48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprobabilistic-logistic-regression-with-tensorflow-73e18f0ddc48&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----73e18f0ddc48---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F73e18f0ddc48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprobabilistic-logistic-regression-with-tensorflow-73e18f0ddc48&source=-----73e18f0ddc48---------------------bookmark_footer-----------)

# **简介**

本文属于“**概率深度学习**”系列。该每周系列涵盖了深度学习的概率方法。主要目标是扩展深度学习模型以量化不确定性，即了解它们不知道的东西。

在这篇文章中，我们将介绍概率逻辑回归的概念，这是一种强大的技术，允许在预测过程中纳入不确定性。我们将探讨这种方法如何带来更强健和准确的预测，尤其是在数据噪声大或模型过拟合的情况下。此外，通过对模型参数引入先验分布，我们可以对模型进行正则化，从而防止过拟合。这种方法是进入激动人心的贝叶斯深度学习世界的一个很好的第一步。

迄今为止已发布的文章：

1.  [温和介绍 TensorFlow Probability：分布对象](https://medium.com/towards-data-science/gentle-introduction-to-tensorflow-probability-distribution-objects-1bb6165abee1)

1.  [温和介绍 TensorFlow Probability：可训练参数](https://medium.com/towards-data-science/gentle-introduction-to-tensorflow-probability-trainable-parameters-5098ea4fed15)

1.  从头开始实现 TensorFlow Probability 中的最大似然估计

1.  从头开始实现 TensorFlow 中的概率线性回归

1.  [Tensorflow 中的概率回归 vs. 确定性回归](https://medium.com/towards-data-science/probabilistic-vs-deterministic-regression-with-tensorflow-85ef791beeef)

1.  [频率主义与贝叶斯统计在 Tensorflow 中的比较](https://medium.com/towards-data-science/frequentist-vs-bayesian-statistics-with-tensorflow-fbba2c6c9ae5)

1.  确定性 vs…
