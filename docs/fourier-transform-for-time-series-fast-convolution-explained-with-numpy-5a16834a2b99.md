# 时间序列的傅里叶变换：使用 numpy 解释快速卷积

> 原文：[https://towardsdatascience.com/fourier-transform-for-time-series-fast-convolution-explained-with-numpy-5a16834a2b99?source=collection_archive---------7-----------------------#2023-07-03](https://towardsdatascience.com/fourier-transform-for-time-series-fast-convolution-explained-with-numpy-5a16834a2b99?source=collection_archive---------7-----------------------#2023-07-03)

## 从零实现与 numpy 的比较

[](https://mocquin.medium.com/?source=post_page-----5a16834a2b99--------------------------------)[![Yoann Mocquin](../Images/b30a0f70c56972aabd2bc0a74baa90bb.png)](https://mocquin.medium.com/?source=post_page-----5a16834a2b99--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5a16834a2b99--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5a16834a2b99--------------------------------) [Yoann Mocquin](https://mocquin.medium.com/?source=post_page-----5a16834a2b99--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F173731d06320&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffourier-transform-for-time-series-fast-convolution-explained-with-numpy-5a16834a2b99&user=Yoann+Mocquin&userId=173731d06320&source=post_page-173731d06320----5a16834a2b99---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5a16834a2b99--------------------------------) ·7 min read·2023年7月3日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5a16834a2b99&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffourier-transform-for-time-series-fast-convolution-explained-with-numpy-5a16834a2b99&user=Yoann+Mocquin&userId=173731d06320&source=-----5a16834a2b99---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5a16834a2b99&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffourier-transform-for-time-series-fast-convolution-explained-with-numpy-5a16834a2b99&source=-----5a16834a2b99---------------------bookmark_footer-----------)

傅里叶变换算法被认为是数学史上最伟大的发现之一。法国数学家让-巴普蒂斯特·约瑟夫·傅里叶在 1822 年的著作《热的解析理论》中奠定了谐波分析的基础。今天，傅里叶变换及其所有变种构成了现代世界的基础，推动了压缩、通信、图像处理等技术的发展。

![](../Images/8d84f9551a5a0d7adaec9020d03587d7.png)

法国数学家让·巴普蒂斯特·约瑟夫·傅里叶（1768–1830）的雕刻肖像，19 世纪初。[来源：[维基百科](https://commons.wikimedia.org/wiki/File:Fourier2.jpg?uselang=fr#Conditions%20d%E2%80%99utilisation)，图片来自公共领域]

这个奇妙的框架还提供了分析时间序列的绝佳工具……这就是我们在这里的原因！

这篇文章是傅里叶变换系列中的一部分。今天我们将讨论卷积，以及傅里叶变换如何提供最快的计算方法。

*所有图形和方程式均由作者制作。*

# **离散傅里叶变换（DFT）的定义**

让我们从基本定义开始。离散时间序列 x 的离散傅里叶变换为：

其中 k 表示 x 的频谱中的第 k 个频率。请注意，某些作者会在定义中添加一个缩放因子 1/N，但对于本文来说并不是特别重要——总的来说，这只是一个……
