# 时间序列的傅里叶变换：复数绘图

> 原文：[`towardsdatascience.com/fourier-transform-for-time-series-plotting-complex-numbers-9743ffe8a8bb?source=collection_archive---------7-----------------------#2023-07-28`](https://towardsdatascience.com/fourier-transform-for-time-series-plotting-complex-numbers-9743ffe8a8bb?source=collection_archive---------7-----------------------#2023-07-28)

## 绘制傅里叶变换算法以理解它

[](https://mocquin.medium.com/?source=post_page-----9743ffe8a8bb--------------------------------)![Yoann Mocquin](https://mocquin.medium.com/?source=post_page-----9743ffe8a8bb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9743ffe8a8bb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9743ffe8a8bb--------------------------------) [Yoann Mocquin](https://mocquin.medium.com/?source=post_page-----9743ffe8a8bb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F173731d06320&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffourier-transform-for-time-series-plotting-complex-numbers-9743ffe8a8bb&user=Yoann+Mocquin&userId=173731d06320&source=post_page-173731d06320----9743ffe8a8bb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9743ffe8a8bb--------------------------------) ·12 分钟阅读·2023 年 7 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9743ffe8a8bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffourier-transform-for-time-series-plotting-complex-numbers-9743ffe8a8bb&user=Yoann+Mocquin&userId=173731d06320&source=-----9743ffe8a8bb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9743ffe8a8bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffourier-transform-for-time-series-plotting-complex-numbers-9743ffe8a8bb&source=-----9743ffe8a8bb---------------------bookmark_footer-----------)![](img/181123c59bab3c833c451456239aa279.png)

大多数时候，人们处理信号的傅里叶变换时会遇到困难，因为它的复杂形式。除非是非常特定的情况，否则时间序列的傅里叶变换通常是复数序列——而复数并不总是容易理解，尤其是当你不习惯处理这些类型的数字时。

**在这篇文章中，我想展示几种可视化一维实数序列傅里叶变换的方法**，这是你 99%时间处理的内容，特别是在数据分析和时间序列中。

*所有图片均由作者提供。*

这篇文章是我关于时间序列的傅里叶变换系列中的第三篇。查看之前的文章请点击这里：

+   复习一下卷积与傅里叶变换的关系以及它有多快：

[](/fourier-transform-for-time-series-fast-convolution-explained-with-numpy-5a16834a2b99?source=post_page-----9743ffe8a8bb--------------------------------) ## 时间序列的傅里叶变换：使用 numpy 解释的快速卷积

### 使用傅里叶变换实现**10000 倍**的卷积加速

towardsdatascience.com

+   通过图像示例加深对卷积的理解：
