# 初学者的卷积神经网络

> 原文：[https://towardsdatascience.com/convolutional-neural-networks-for-beginners-c1de55eee2b2?source=collection_archive---------7-----------------------#2023-10-17](https://towardsdatascience.com/convolutional-neural-networks-for-beginners-c1de55eee2b2?source=collection_archive---------7-----------------------#2023-10-17)

## 卷积神经网络的基础

[](https://medium.com/@mina.ghashami?source=post_page-----c1de55eee2b2--------------------------------)[![Mina Ghashami](../Images/745f53b94f5667a485299b49913c7a21.png)](https://medium.com/@mina.ghashami?source=post_page-----c1de55eee2b2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c1de55eee2b2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c1de55eee2b2--------------------------------) [Mina Ghashami](https://medium.com/@mina.ghashami?source=post_page-----c1de55eee2b2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc99ed9ed7b9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvolutional-neural-networks-for-beginners-c1de55eee2b2&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=post_page-c99ed9ed7b9a----c1de55eee2b2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1de55eee2b2--------------------------------) · 6 分钟阅读 · 2023年10月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc1de55eee2b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvolutional-neural-networks-for-beginners-c1de55eee2b2&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=-----c1de55eee2b2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc1de55eee2b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvolutional-neural-networks-for-beginners-c1de55eee2b2&source=-----c1de55eee2b2---------------------bookmark_footer-----------)![](../Images/81572a08a73278e57437c5f0d487cdca.png)

图片来自 [unsplash.com](https://unsplash.com/photos/an-abstract-image-of-a-sphere-with-dots-and-lines-nGoCBxiaRO0)

我撰写这篇文章是为了准备我在 [*Interview Kickstart*](https://www.interviewkickstart.com/) 的讲座，该讲座旨在帮助专业人士找到顶级科技公司的职位。如果你正在准备面试或只是想巩固基础，这篇文章可能对你有帮助。

在这篇文章中，我们将探讨卷积神经网络及其基础知识。我们将从卷积操作开始，然后继续讲解卷积层是什么以及卷积网络是如何构建的。

让我们开始吧。

卷积神经网络（CNNs）由几个“卷积层”组成。这些层执行“卷积操作”。卷积是信号和图像处理中的基本操作。让我们首先了解这个操作是什么。

## 卷积操作是什么？

卷积是核心（滤波器）与输入特征图之间的数学操作。

核心通常是一个小矩阵，例如3x3或5x5。输入总是一个具有高度、宽度和通道的特征图。卷积操作的工作原理是*核心在输入上滑动*，*并计算两者之间的点积*……
