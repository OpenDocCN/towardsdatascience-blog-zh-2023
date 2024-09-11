# 神经网络与深度学习的激活函数

> 原文：[https://towardsdatascience.com/activation-functions-non-linearity-neural-networks-101-ab0036a2e701?source=collection_archive---------8-----------------------#2023-10-12](https://towardsdatascience.com/activation-functions-non-linearity-neural-networks-101-ab0036a2e701?source=collection_archive---------8-----------------------#2023-10-12)

## 解释神经网络为什么能学习（几乎）任何事情

[](https://medium.com/@egorhowell?source=post_page-----ab0036a2e701--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----ab0036a2e701--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ab0036a2e701--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ab0036a2e701--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----ab0036a2e701--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Factivation-functions-non-linearity-neural-networks-101-ab0036a2e701&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----ab0036a2e701---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab0036a2e701--------------------------------) ·8 min read·Oct 12, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fab0036a2e701&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Factivation-functions-non-linearity-neural-networks-101-ab0036a2e701&user=Egor+Howell&userId=1cac491223b2&source=-----ab0036a2e701---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fab0036a2e701&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Factivation-functions-non-linearity-neural-networks-101-ab0036a2e701&source=-----ab0036a2e701---------------------bookmark_footer-----------)![](../Images/f20f48d260492d8ecb7fb2bf39ec6862.png)

由 Becris 创作的机器学习图标 — Flatico。[https://www.flaticon.com/free-icons/machine-learning](https://www.flaticon.com/free-icons/machine-learning)

# 背景

在我的前一篇文章中，我们介绍了[***多层感知器***](https://zh.wikipedia.org/wiki/%E5%A4%9A%E5%B1%82%E6%84%9F%E7%9F%A5%E5%99%A8) ***(MLP)***，它只是一组堆叠在一起的互相连接的[***感知器***](https://zh.wikipedia.org/wiki/%E6%84%9F%E7%9F%A5%E5%99%A8)。如果你对感知器和 MLP 不熟悉，我强烈建议你查看我之前的文章，因为我们在本文中将会大量讨论：

[](https://levelup.gitconnected.com/intro-perceptron-architecture-neural-networks-101-2a487062810c?source=post_page-----ab0036a2e701--------------------------------) [## 介绍，感知机与架构：神经网络101

### 神经网络及其构建模块的介绍

levelup.gitconnected.com](https://levelup.gitconnected.com/intro-perceptron-architecture-neural-networks-101-2a487062810c?source=post_page-----ab0036a2e701--------------------------------)

下图展示了一个具有两个隐藏层的MLP示例：

![](../Images/94cdac9897a5e852d9cd6edd425fcba0.png)

一个基础的双隐藏层多层感知机。图示由作者提供。

然而，MLP的问题在于它只能拟合一个[***线性分类器***](https://en.wikipedia.org/wiki/Linear_separability)。这是因为单个感知机具有一个[***阶跃函数***](https://en.wikipedia.org/wiki/Step_function)作为其[***激活函数***](https://en.wikipedia.org/wiki/Activation_function)，这是线性的：
