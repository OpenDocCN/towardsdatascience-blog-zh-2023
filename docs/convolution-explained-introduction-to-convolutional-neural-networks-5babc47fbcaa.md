# 卷积解释 — 卷积神经网络简介

> 原文：[https://towardsdatascience.com/convolution-explained-introduction-to-convolutional-neural-networks-5babc47fbcaa?source=collection_archive---------1-----------------------#2023-12-27](https://towardsdatascience.com/convolution-explained-introduction-to-convolutional-neural-networks-5babc47fbcaa?source=collection_archive---------1-----------------------#2023-12-27)

## CNN的基本构建块

[](https://medium.com/@egorhowell?source=post_page-----5babc47fbcaa--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----5babc47fbcaa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5babc47fbcaa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5babc47fbcaa--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----5babc47fbcaa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvolution-explained-introduction-to-convolutional-neural-networks-5babc47fbcaa&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----5babc47fbcaa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5babc47fbcaa--------------------------------) · 8分钟阅读 · 2023年12月27日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5babc47fbcaa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvolution-explained-introduction-to-convolutional-neural-networks-5babc47fbcaa&user=Egor+Howell&userId=1cac491223b2&source=-----5babc47fbcaa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5babc47fbcaa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvolution-explained-introduction-to-convolutional-neural-networks-5babc47fbcaa&source=-----5babc47fbcaa---------------------bookmark_footer-----------)![](../Images/725d855cae28fe1f795da02e71253227.png)

”[https://www.flaticon.com/free-icons/neural-network](https://www.flaticon.com/free-icons/neural-network)" title=”神经网络图标。” 神经网络图标由Freepik — Flaticon创建。

我最近的文章是一系列关于神经网络的内容，从简单的[***感知机***](https://medium.com/gitconnected/intro-perceptron-architecture-neural-networks-101-2a487062810c)到复杂的架构以及如何处理[***深度学习中的常见问题***](https://medium.com/towards-data-science/vanishing-exploding-gradient-problem-neural-networks-101-c8f48ec6a80b)。如果你感兴趣，可以在这里查看该系列：

![Egor Howell](../Images/e969a9f3c3357e1c80dcd0092d9a1288.png)

[Egor Howell](https://medium.com/@egorhowell?source=post_page-----5babc47fbcaa--------------------------------)

## 神经网络

[查看列表](https://medium.com/@egorhowell/list/neural-networks-616db722dbbb?source=post_page-----5babc47fbcaa--------------------------------)9个故事！[](../Images/66f86f14ddf20d9da9cac53dee54bae3.png)![](../Images/d968b0bd4358bb0d60bd296aef08a720.png)![](../Images/30f3f4354836e6d3d56175fe20cf6b7c.png)

神经网络在一个令人兴奋的领域取得了重大进展，即[***计算机视觉***](https://en.wikipedia.org/wiki/Computer_vision)。想象一下用于自动驾驶汽车和面部识别的人工智能！

然而，大多数人了解的常规全连接神经网络不适合许多实际图像识别任务。它在著名的[***MNIST***](https://en.wikipedia.org/wiki/MNIST_database#:~:text=Article%20Talk,the%20field%20of%20machine%20learning.)数据集上工作良好，但该数据集的图像小到***28×28***像素。

高清（HD）图像有***1280×720***像素。这大约是***1,000,000***像素，这意味着输入层有***1,000,000个神经元***。更不用说隐藏层所需的数百万个权重，使得常规神经网络因维度复杂性而不适用。
