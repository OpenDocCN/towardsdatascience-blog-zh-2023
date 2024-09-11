# 为什么使用卷积？理解深度网络中的卷积和特征提取

> 原文：[https://towardsdatascience.com/why-convolve-understanding-convolution-and-feature-extraction-in-deep-networks-ee45d1fdd17c?source=collection_archive---------2-----------------------#2023-02-05](https://towardsdatascience.com/why-convolve-understanding-convolution-and-feature-extraction-in-deep-networks-ee45d1fdd17c?source=collection_archive---------2-----------------------#2023-02-05)

## **一维/二维卷积的解释、其在特征学习中的作用以及可视化工具**

[](https://azad-wolf.medium.com/?source=post_page-----ee45d1fdd17c--------------------------------)[![J. Rafid Siddiqui, PhD](../Images/02280890ed87239c75cbcbfa7c5d686c.png)](https://azad-wolf.medium.com/?source=post_page-----ee45d1fdd17c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ee45d1fdd17c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ee45d1fdd17c--------------------------------) [J. Rafid Siddiqui, PhD](https://azad-wolf.medium.com/?source=post_page-----ee45d1fdd17c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Feb523dd294ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-convolve-understanding-convolution-and-feature-extraction-in-deep-networks-ee45d1fdd17c&user=J.+Rafid+Siddiqui%2C+PhD&userId=eb523dd294ec&source=post_page-eb523dd294ec----ee45d1fdd17c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ee45d1fdd17c--------------------------------) ·9分钟阅读·2023年2月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fee45d1fdd17c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-convolve-understanding-convolution-and-feature-extraction-in-deep-networks-ee45d1fdd17c&user=J.+Rafid+Siddiqui%2C+PhD&userId=eb523dd294ec&source=-----ee45d1fdd17c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fee45d1fdd17c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-convolve-understanding-convolution-and-feature-extraction-in-deep-networks-ee45d1fdd17c&source=-----ee45d1fdd17c---------------------bookmark_footer-----------)![](../Images/f66c4e50e3736ce4f33b148180814a20.png)

图1：一维/二维卷积和互动可视化工具（来源：作者）

如今，使用一组卷积层来构建深度神经网络已经成为一种常见做法。然而，早期的神经网络和其他机器学习框架并未采用卷积。特征提取和学习曾经是两个独立的研究领域，直到最近。这就是为什么了解卷积如何工作以及它为何在深度学习架构中占据如此重要的位置是重要的。本文将深入探讨卷积，您将能够通过互动工具更深入地理解这一概念。

**介绍**

卷积是通过应用另一个函数来改变一个函数形状的过程。它是两个函数之间的运算，就像任何其他算术运算一样，但它作用于函数而不是函数值。因此，它会产生一个第三个函数作为结果。更正式地说，卷积 ***C=f(x)*g(x)*** 是函数 ***f*** 和函数 ***g*** 之间的运算，由卷积运算符 ***** 表示。在数学公式方面，卷积是……
