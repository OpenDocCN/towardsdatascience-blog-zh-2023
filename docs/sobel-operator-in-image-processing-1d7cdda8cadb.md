# Sobel 算子在图像处理中的应用

> 原文：[https://towardsdatascience.com/sobel-operator-in-image-processing-1d7cdda8cadb?source=collection_archive---------3-----------------------#2023-12-31](https://towardsdatascience.com/sobel-operator-in-image-processing-1d7cdda8cadb?source=collection_archive---------3-----------------------#2023-12-31)

## 什么是Sobel算子及其用法示例

[](https://medium.com/@egorhowell?source=post_page-----1d7cdda8cadb--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----1d7cdda8cadb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1d7cdda8cadb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1d7cdda8cadb--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----1d7cdda8cadb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsobel-operator-in-image-processing-1d7cdda8cadb&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----1d7cdda8cadb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1d7cdda8cadb--------------------------------) · 7分钟阅读·2023年12月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1d7cdda8cadb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsobel-operator-in-image-processing-1d7cdda8cadb&user=Egor+Howell&userId=1cac491223b2&source=-----1d7cdda8cadb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1d7cdda8cadb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsobel-operator-in-image-processing-1d7cdda8cadb&source=-----1d7cdda8cadb---------------------bookmark_footer-----------)![](../Images/1a9c4a638e4549d5bdf35076e9ba2b0e.png)

href=”[https://www.flaticon.com/free-icons/image-processing](https://www.flaticon.com/free-icons/image-processing)" title=”图像处理图标” 图像处理图标由 juicy_fish 创作 — Flaticon。

# 卷积概述

在我之前的文章中，我们深入探讨了[***卷积神经网络 (CNNs)***](https://en.wikipedia.org/wiki/Convolutional_neural_network)的核心构建块，即卷积数学算子。我强烈建议你查看那篇文章，因为它为这篇文章提供了背景和理解：

[](/convolution-explained-introduction-to-convolutional-neural-networks-5babc47fbcaa?source=post_page-----1d7cdda8cadb--------------------------------) [## 卷积解释 — 卷积神经网络简介

### CNNs 的基本构建块

[towardsdatascience.com](/convolution-explained-introduction-to-convolutional-neural-networks-5babc47fbcaa?source=post_page-----1d7cdda8cadb--------------------------------)

简而言之，对于图像处理，卷积是将一个小矩阵，称为卷积核，应用到输入图像上，以创建一个应用了某些效果的输出图像，比如模糊或锐化。

从数学角度来看，我们得到的是：

+   ***f*∗*g:*** 函数***f***与***g***之间的卷积。

+   ***f:*** 输入图像。

+   ***g***: 卷积核矩阵，也称为滤波器。

+   ***t:*** 正在计算卷积的像素位置。

+   ***f(τ):*** 图像 ***f*** 在像素 ***τ*** 处的像素值。

+   ***g(t−τ):*** ***g*** 的像素值被 ***τ*** 移动并在 ***t*** 处进行评估。
