# 神经网络中的正则化

> 原文：[https://towardsdatascience.com/regularisation-techniques-neural-networks-101-1f746ad45b72?source=collection_archive---------6-----------------------#2023-12-02](https://towardsdatascience.com/regularisation-techniques-neural-networks-101-1f746ad45b72?source=collection_archive---------6-----------------------#2023-12-02)

## 如何在训练神经网络时避免过拟合

[](https://medium.com/@egorhowell?source=post_page-----1f746ad45b72--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----1f746ad45b72--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1f746ad45b72--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1f746ad45b72--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----1f746ad45b72--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregularisation-techniques-neural-networks-101-1f746ad45b72&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----1f746ad45b72---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1f746ad45b72--------------------------------) · 8分钟阅读 · 2023年12月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1f746ad45b72&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregularisation-techniques-neural-networks-101-1f746ad45b72&user=Egor+Howell&userId=1cac491223b2&source=-----1f746ad45b72---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1f746ad45b72&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregularisation-techniques-neural-networks-101-1f746ad45b72&source=-----1f746ad45b72---------------------bookmark_footer-----------)![](../Images/e56562ba2f8d215063ee836b97f5018d.png)

[https://www.flaticon.com/free-icons/neural-network](https://www.flaticon.com/free-icons/neural-network). title=”neural network icons.” 神经网络图标由 Vectors Tank 创建 — Flaticon.

# 目录

1.  **背景**

1.  **什么是过拟合？**

1.  **Lasso (L1) 和 Ridge (L2) 正则化**

1.  **早期停止**

1.  **Dropout**

1.  **其他方法**

1.  **总结**

# 背景

到目前为止，在这篇神经网络基础系列文章中，我们讨论了提高神经网络性能的两种方法：[**超参数调优**](/optimise-your-hyperparameter-tuning-with-hyperopt-861573239eb5)和更快的梯度下降优化器。你可以查看下面的这些帖子：

[](/hyperparameter-tuning-neural-networks-101-ca1102891b27?source=post_page-----1f746ad45b72--------------------------------) [## 超参数调优：神经网络101

### 如何通过调节超参数来改善神经网络的“学习”和“训练”

[超参数调优：神经网络101](https://towardsdatascience.com/hyperparameter-tuning-neural-networks-101-ca1102891b27?source=post_page-----1f746ad45b72--------------------------------) [## 优化算法：神经网络101

### 如何在“基本的”梯度下降算法之外提高训练效果

[优化算法：神经网络101](https://towardsdatascience.com/optimisation-algorithms-neural-networks-101-256e16a88412?source=post_page-----1f746ad45b72--------------------------------)

还有一套有助于性能的技术，那就是正则化。这有助于防止模型对数据过拟合……
