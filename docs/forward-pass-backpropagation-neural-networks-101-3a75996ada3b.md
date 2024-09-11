# 前向传播与神经网络中的反向传播

> 原文：[https://towardsdatascience.com/forward-pass-backpropagation-neural-networks-101-3a75996ada3b?source=collection_archive---------6-----------------------#2023-11-04](https://towardsdatascience.com/forward-pass-backpropagation-neural-networks-101-3a75996ada3b?source=collection_archive---------6-----------------------#2023-11-04)

## 解释神经网络如何通过手工和代码使用 PyTorch 进行“训练”和“学习”数据中的模式

[](https://medium.com/@egorhowell?source=post_page-----3a75996ada3b--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----3a75996ada3b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3a75996ada3b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3a75996ada3b--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----3a75996ada3b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforward-pass-backpropagation-neural-networks-101-3a75996ada3b&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----3a75996ada3b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3a75996ada3b--------------------------------) ·10分钟阅读·2023年11月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3a75996ada3b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforward-pass-backpropagation-neural-networks-101-3a75996ada3b&user=Egor+Howell&userId=1cac491223b2&source=-----3a75996ada3b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3a75996ada3b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforward-pass-backpropagation-neural-networks-101-3a75996ada3b&source=-----3a75996ada3b---------------------bookmark_footer-----------)![](../Images/a894bd9e34501e8413c8bf96a918f1e5.png)

神经网络图标由 juicy_fish 创建 — Flaticon。 [https://www.flaticon.com/free-icons/neural-network](https://www.flaticon.com/free-icons/neural-network).

# 背景

在我之前的两篇文章中，我们深入探讨了神经网络的起源，从单一的 [***感知器***](https://en.wikipedia.org/wiki/Perceptron) 到大型互联的 ([***多层感知器***](https://en.wikipedia.org/wiki/Multilayer_perceptron) ***(MLP)*** ) 非线性优化引擎。如果你对感知器、MLP 和激活函数不太熟悉，我强烈推荐你查看我之前的帖子，因为我们将在这篇文章中讨论这些内容：

[***介绍、感知器及架构***](https://levelup.gitconnected.com/intro-perceptron-architecture-neural-networks-101-2a487062810c?source=post_page-----3a75996ada3b--------------------------------)：神经网络 101

### 神经网络及其构建块的介绍

[***激活函数***](https://levelup.gitconnected.com/intro-perceptron-architecture-neural-networks-101-2a487062810c?source=post_page-----3a75996ada3b--------------------------------) 和非线性：神经网络 101

### 解释为什么神经网络可以学习（几乎）任何和所有的东西

[***前向传播***](https://theneuralblog.com/forward-pass-backpropagation-example/) 和 [***反向传播***](https://en.wikipedia.org/wiki/Backpropagation) 是两个关键组成部分。让我们深入了解一下吧！

现在是了解这些神经网络如何“训练”和“学习”你传递给它的数据中的模式的时候了。
