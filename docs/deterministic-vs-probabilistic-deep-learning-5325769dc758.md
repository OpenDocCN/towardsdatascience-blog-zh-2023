# 确定性与概率性深度学习

> 原文：[https://towardsdatascience.com/deterministic-vs-probabilistic-deep-learning-5325769dc758?source=collection_archive---------7-----------------------#2023-01-11](https://towardsdatascience.com/deterministic-vs-probabilistic-deep-learning-5325769dc758?source=collection_archive---------7-----------------------#2023-01-11)

## 概率性深度学习

[](https://medium.com/@luisroque?source=post_page-----5325769dc758--------------------------------)[![路易斯·罗克](../Images/e281d470b403375ba3c6f521b1ccf915.png)](https://medium.com/@luisroque?source=post_page-----5325769dc758--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5325769dc758--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5325769dc758--------------------------------) [路易斯·罗克](https://medium.com/@luisroque?source=post_page-----5325769dc758--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeterministic-vs-probabilistic-deep-learning-5325769dc758&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----5325769dc758---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5325769dc758--------------------------------) ·9分钟阅读·2023年1月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5325769dc758&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeterministic-vs-probabilistic-deep-learning-5325769dc758&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----5325769dc758---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5325769dc758&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeterministic-vs-probabilistic-deep-learning-5325769dc758&source=-----5325769dc758---------------------bookmark_footer-----------)

# 介绍

本文属于“概率深度学习”系列。这一系列每周讨论概率性深度学习的方法。主要目标是扩展深度学习模型以量化不确定性，即知道它们不知道的内容。

本文涵盖了确定性与概率性深度学习的主要区别。确定性深度学习模型旨在优化标量值的损失函数，而概率性深度学习模型则优化概率目标函数。确定性模型为每个输入提供单一预测，而概率性模型则提供对其预测中不确定性的概率性表征，并能够从模型中生成新的样本。

目前发布的文章：

1.  [TensorFlow 概率论的温和介绍：分布对象](https://medium.com/towards-data-science/gentle-introduction-to-tensorflow-probability-distribution-objects-1bb6165abee1)

1.  [TensorFlow 概率论的温和介绍：可训练参数](https://medium.com/towards-data-science/gentle-introduction-to-tensorflow-probability-trainable-parameters-5098ea4fed15)

1.  [从头开始进行最大似然估计（TensorFlow Probability）](/maximum-likelihood-estimation-from-scratch-in-tensorflow-probability-2fc0eefdbfc2)

1.  [从头开始进行概率线性回归（TensorFlow）](/probabilistic-linear-regression-from-scratch-in-tensorflow-2eb633fffc00)

1.  [Tensorflow 中的概率回归与确定性回归](https://medium.com/towards-data-science/probabilistic-vs-deterministic-regression-with-tensorflow-85ef791beeef)

1.  [频率派与贝叶斯统计学在 Tensorflow 中的比较](https://medium.com/towards-data-science/frequentist-vs-bayesian-statistics-with-tensorflow-fbba2c6c9ae5)

1.  确定性深度学习与概率深度学习
