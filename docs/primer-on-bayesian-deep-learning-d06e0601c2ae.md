# 贝叶斯深度学习概论

> 原文：[https://towardsdatascience.com/primer-on-bayesian-deep-learning-d06e0601c2ae?source=collection_archive---------4-----------------------#2023-02-01](https://towardsdatascience.com/primer-on-bayesian-deep-learning-d06e0601c2ae?source=collection_archive---------4-----------------------#2023-02-01)

## 概率深度学习

[](https://medium.com/@luisroque?source=post_page-----d06e0601c2ae--------------------------------)[![Luís Roque](../Images/e281d470b403375ba3c6f521b1ccf915.png)](https://medium.com/@luisroque?source=post_page-----d06e0601c2ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d06e0601c2ae--------------------------------)[![数据科学的前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d06e0601c2ae--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----d06e0601c2ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprimer-on-bayesian-deep-learning-d06e0601c2ae&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----d06e0601c2ae---------------------post_header-----------) 发表在 [数据科学的前沿](https://towardsdatascience.com/?source=post_page-----d06e0601c2ae--------------------------------) ·8 分钟阅读·2023年2月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd06e0601c2ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprimer-on-bayesian-deep-learning-d06e0601c2ae&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----d06e0601c2ae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd06e0601c2ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprimer-on-bayesian-deep-learning-d06e0601c2ae&source=-----d06e0601c2ae---------------------bookmark_footer-----------)

# 介绍

本文属于“概率深度学习”系列。这个每周系列涵盖了深度学习的概率方法。主要目标是扩展深度学习模型以量化不确定性，即知道它们不知道什么。

贝叶斯深度学习是一个新兴领域，它将深度学习的表达能力和表征能力与贝叶斯方法的不确定性建模能力相结合。这两种范式的整合提供了一个有原则的框架，用于解决深度学习中的各种挑战，如过拟合、权重不确定性和模型比较。

在这篇文章中，我们提供了对贝叶斯深度学习的全面介绍，涵盖了其基础、方法论和最新进展。我们的目标是以清晰易懂的方式呈现基本概念和思想，使其成为研究人员和新领域从业者的理想资源。

迄今为止发布的文章：

1.  [TensorFlow Probability 的温和介绍：分布对象](https://medium.com/towards-data-science/gentle-introduction-to-tensorflow-probability-distribution-objects-1bb6165abee1)

1.  [TensorFlow Probability 的温和介绍：可训练参数](https://medium.com/towards-data-science/gentle-introduction-to-tensorflow-probability-trainable-parameters-5098ea4fed15)

1.  [从头开始在 TensorFlow Probability 中进行最大似然估计](/maximum-likelihood-estimation-from-scratch-in-tensorflow-probability-2fc0eefdbfc2)

1.  [从头开始在 TensorFlow 中进行概率线性回归](/probabilistic-linear-regression-from-scratch-in-tensorflow-2eb633fffc00)

1.  [概率回归与确定性回归](https://medium.com/towards-data-science/probabilistic-vs-deterministic-regression-with-tensorflow-85ef791beeef)…
