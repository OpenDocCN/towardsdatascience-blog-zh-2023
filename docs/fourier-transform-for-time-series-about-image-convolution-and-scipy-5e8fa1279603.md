# 傅里叶变换在时间序列中的应用：关于图像卷积和SciPy

> 原文：[https://towardsdatascience.com/fourier-transform-for-time-series-about-image-convolution-and-scipy-5e8fa1279603?source=collection_archive---------17-----------------------#2023-07-21](https://towardsdatascience.com/fourier-transform-for-time-series-about-image-convolution-and-scipy-5e8fa1279603?source=collection_archive---------17-----------------------#2023-07-21)

## 傅里叶变换卷积也适用于图像

[](https://mocquin.medium.com/?source=post_page-----5e8fa1279603--------------------------------)[![Yoann Mocquin](../Images/b30a0f70c56972aabd2bc0a74baa90bb.png)](https://mocquin.medium.com/?source=post_page-----5e8fa1279603--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5e8fa1279603--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5e8fa1279603--------------------------------) [Yoann Mocquin](https://mocquin.medium.com/?source=post_page-----5e8fa1279603--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F173731d06320&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffourier-transform-for-time-series-about-image-convolution-and-scipy-5e8fa1279603&user=Yoann+Mocquin&userId=173731d06320&source=post_page-173731d06320----5e8fa1279603---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5e8fa1279603--------------------------------) ·5分钟阅读·2023年7月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5e8fa1279603&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffourier-transform-for-time-series-about-image-convolution-and-scipy-5e8fa1279603&user=Yoann+Mocquin&userId=173731d06320&source=-----5e8fa1279603---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5e8fa1279603&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffourier-transform-for-time-series-about-image-convolution-and-scipy-5e8fa1279603&source=-----5e8fa1279603---------------------bookmark_footer-----------)

本文是傅里叶变换在时间序列中的第二篇，第一篇请点击这里：

[](/fourier-transform-for-time-series-fast-convolution-explained-with-numpy-5a16834a2b99?source=post_page-----5e8fa1279603--------------------------------) [## 傅里叶变换在时间序列中的应用：使用numpy解释快速卷积

### 使用傅里叶变换实现10000倍更快的卷积

towardsdatascience.com](/fourier-transform-for-time-series-fast-convolution-explained-with-numpy-5a16834a2b99?source=post_page-----5e8fa1279603--------------------------------)

# **对上一篇文章的快速回顾**

在第一篇文章中，我解释了如何利用傅里叶变换**非常高效**地进行信号卷积。我展示了在* numpy* 中使用傅里叶变换进行卷积的速度比标准代数方法快了许多倍，并且这对应于一种称为**循环卷积**的卷积类型。

在这篇文章中，我想强调循环卷积的含义及其如何应用于图像。图像也是将一维直觉扩展到二维的好方法。

*所有图像均由作者制作。*

# **使用 scipy 进行图像卷积**

如果你曾经处理过图像进行图像处理，你很可能遇到过应用卷积的函数。图像卷积无处不在——图像增强、去噪、分割、特征提取、压缩——并且是…
