# 如何编程一个神经网络

> 原文：[https://towardsdatascience.com/how-to-program-a-neural-network-f28e3f38e811?source=collection_archive---------0-----------------------#2023-09-23](https://towardsdatascience.com/how-to-program-a-neural-network-f28e3f38e811?source=collection_archive---------0-----------------------#2023-09-23)

## 从头实现神经网络的逐步指南

[](https://medium.com/@callum.bruce1?source=post_page-----f28e3f38e811--------------------------------)[![Callum Bruce](../Images/4833a199a9449434777fdf5ce913a9cb.png)](https://medium.com/@callum.bruce1?source=post_page-----f28e3f38e811--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f28e3f38e811--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f28e3f38e811--------------------------------) [Callum Bruce](https://medium.com/@callum.bruce1?source=post_page-----f28e3f38e811--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa9c915837ab3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-program-a-neural-network-f28e3f38e811&user=Callum+Bruce&userId=a9c915837ab3&source=post_page-a9c915837ab3----f28e3f38e811---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f28e3f38e811--------------------------------) ·14分钟阅读·2023年9月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff28e3f38e811&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-program-a-neural-network-f28e3f38e811&user=Callum+Bruce&userId=a9c915837ab3&source=-----f28e3f38e811---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff28e3f38e811&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-program-a-neural-network-f28e3f38e811&source=-----f28e3f38e811---------------------bookmark_footer-----------)![](../Images/79509eca917d69c1d6ca7ef6178adf3d.png)

一个包含三个隐藏层的神经网络

在本文中，我们将从头构建一个神经网络，并用它来分类手写数字。

为什么要重新发明轮子/神经网络呢？我听到你们问。难道我不能直接使用我喜欢的机器学习框架吗？是的，确实有很多现成的框架可以用来构建神经网络（例如Keras、PyTorch和TensorFlow）。使用这些框架的一个问题是，它们使我们容易把神经网络当作黑箱来看待。

这并不总是坏事。我们常常需要这种抽象层次，以便能够着手解决眼前的问题，但如果我们要在工作中使用神经网络，仍然应该努力至少对其背后的运作有基本了解。

我认为，从零开始构建神经网络是培养对其工作原理深入理解的最佳方式。

在本文结束时，你将学到前馈和反向传播算法，什么是激活函数，epoch和batch之间的区别，以及如何训练神经网络。最后，我们将通过训练一个神经网络来识别手写数字来结束这个例子。
