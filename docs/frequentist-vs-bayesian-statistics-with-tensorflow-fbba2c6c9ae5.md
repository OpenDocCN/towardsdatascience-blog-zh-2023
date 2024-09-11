# 频率派与贝叶斯统计在 Tensorflow 中的应用

> 原文：[https://towardsdatascience.com/frequentist-vs-bayesian-statistics-with-tensorflow-fbba2c6c9ae5?source=collection_archive---------14-----------------------#2023-01-05](https://towardsdatascience.com/frequentist-vs-bayesian-statistics-with-tensorflow-fbba2c6c9ae5?source=collection_archive---------14-----------------------#2023-01-05)

## 概率深度学习

[](https://medium.com/@luisroque?source=post_page-----fbba2c6c9ae5--------------------------------)[![路易斯·罗克](../Images/e281d470b403375ba3c6f521b1ccf915.png)](https://medium.com/@luisroque?source=post_page-----fbba2c6c9ae5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fbba2c6c9ae5--------------------------------)[![数据科学之路](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fbba2c6c9ae5--------------------------------) [路易斯·罗克](https://medium.com/@luisroque?source=post_page-----fbba2c6c9ae5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrequentist-vs-bayesian-statistics-with-tensorflow-fbba2c6c9ae5&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----fbba2c6c9ae5---------------------post_header-----------) 发表在 [数据科学之路](https://towardsdatascience.com/?source=post_page-----fbba2c6c9ae5--------------------------------) ·10分钟阅读·2023年1月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffbba2c6c9ae5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrequentist-vs-bayesian-statistics-with-tensorflow-fbba2c6c9ae5&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----fbba2c6c9ae5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffbba2c6c9ae5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrequentist-vs-bayesian-statistics-with-tensorflow-fbba2c6c9ae5&source=-----fbba2c6c9ae5---------------------bookmark_footer-----------)

# 介绍

本文属于“概率深度学习”系列。该系列每周介绍深度学习中的概率方法。主要目标是扩展深度学习模型以量化不确定性，即了解它们所不知道的东西。

频率主义的统计方法基于重复抽样和长期相对频率的思想。它涉及构建关于总体的假设并使用样本数据进行测试。另一方面，贝叶斯方法基于主观概率，并涉及使用收集到的数据更新对总体的初步信念。这两种方法各有优缺点，选择使用哪种方法取决于问题和分析目标。在本文中，我们将探讨频率主义和贝叶斯方法之间的区别，并讨论如何使用 TensorFlow 和 TensorFlow Probability 实现这些方法。

目前已发布的文章：

1.  [TensorFlow Probability 的温和介绍：分布对象](https://medium.com/towards-data-science/gentle-introduction-to-tensorflow-probability-distribution-objects-1bb6165abee1)

1.  [TensorFlow Probability 的温和介绍：可训练参数](https://medium.com/towards-data-science/gentle-introduction-to-tensorflow-probability-trainable-parameters-5098ea4fed15)

1.  [从头开始在 TensorFlow Probability 中实现最大似然估计](/maximum-likelihood-estimation-from-scratch-in-tensorflow-probability-2fc0eefdbfc2)

1.  [从头开始在 TensorFlow 中实现概率线性回归](/probabilistic-linear-regression-from-scratch-in-tensorflow-2eb633fffc00)

1.  [使用 TensorFlow 的概率回归与确定性回归](https://medium.com/towards-data-science/probabilistic-vs-deterministic-regression-with-tensorflow-85ef791beeef)
