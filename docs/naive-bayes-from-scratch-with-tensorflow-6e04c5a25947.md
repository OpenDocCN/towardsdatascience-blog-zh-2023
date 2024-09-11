# 朴素贝叶斯从零开始与 TensorFlow

> 原文：[https://towardsdatascience.com/naive-bayes-from-scratch-with-tensorflow-6e04c5a25947?source=collection_archive---------12-----------------------#2023-01-18](https://towardsdatascience.com/naive-bayes-from-scratch-with-tensorflow-6e04c5a25947?source=collection_archive---------12-----------------------#2023-01-18)

## 概率深度学习

[](https://medium.com/@luisroque?source=post_page-----6e04c5a25947--------------------------------)[![Luís Roque](../Images/e281d470b403375ba3c6f521b1ccf915.png)](https://medium.com/@luisroque?source=post_page-----6e04c5a25947--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6e04c5a25947--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6e04c5a25947--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----6e04c5a25947--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnaive-bayes-from-scratch-with-tensorflow-6e04c5a25947&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----6e04c5a25947---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6e04c5a25947--------------------------------) ·10分钟阅读·2023年1月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6e04c5a25947&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnaive-bayes-from-scratch-with-tensorflow-6e04c5a25947&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----6e04c5a25947---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6e04c5a25947&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnaive-bayes-from-scratch-with-tensorflow-6e04c5a25947&source=-----6e04c5a25947---------------------bookmark_footer-----------)

# 介绍

本文属于“概率深度学习”系列。这一周刊系列涵盖了深度学习中的概率方法。主要目标是扩展深度学习模型以量化不确定性，即了解它们不知道的内容。

在本文中，我们对使用酒样本数据集的朴素贝叶斯分类算法进行了详细的探讨。朴素贝叶斯算法是一种基于贝叶斯定理的概率机器学习技术，该技术对特征在给定目标标签下的独立性做出了假设。为了便于类的分离可视化，我们将模型限制为仅使用两个特征。

我们的目标是基于选择的特征对葡萄酒样本进行分类。为了实现这一目标，我们首先探索数据并选择能够有效区分类别的特征。接着，我们构建类别先验分布和类别条件密度，从而能够预测具有最高概率的类别。该研究使用的数据集包括葡萄酒的各种特征，如色调、酒精含量、类黄酮以及目标类别，数据来源于scikit-learn库 [1]。

目前已发表的文章：

1.  [TensorFlow Probability 简介：分布对象](https://medium.com/towards-data-science/gentle-introduction-to-tensorflow-probability-distribution-objects-1bb6165abee1)

1.  [TensorFlow Probability 简介：可训练参数](https://medium.com/towards-data-science/gentle-introduction-to-tensorflow-probability-trainable-parameters-5098ea4fed15)
