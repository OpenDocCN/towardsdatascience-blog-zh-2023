# 非欧几里得空间中的机器学习

> 原文：[`towardsdatascience.com/machine-learning-in-a-non-euclidean-space-99b0a776e92e?source=collection_archive---------2-----------------------#2023-06-16`](https://towardsdatascience.com/machine-learning-in-a-non-euclidean-space-99b0a776e92e?source=collection_archive---------2-----------------------#2023-06-16)

![](img/33e8dc96ecf4a66659e0c47e33bde03e.png)

图片由 [Greg Rosenke](https://unsplash.com/@greg_rosenke?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 第一章：为什么你应该了解非欧几里得机器学习

[](https://medium.com/@mastafa.foufa?source=post_page-----99b0a776e92e--------------------------------)![Mastafa Foufa](https://medium.com/@mastafa.foufa?source=post_page-----99b0a776e92e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----99b0a776e92e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----99b0a776e92e--------------------------------) [Mastafa Foufa](https://medium.com/@mastafa.foufa?source=post_page-----99b0a776e92e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1e28ced4c3b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-in-a-non-euclidean-space-99b0a776e92e&user=Mastafa+Foufa&userId=1e28ced4c3b5&source=post_page-1e28ced4c3b5----99b0a776e92e---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----99b0a776e92e--------------------------------) · 10 分钟阅读 · 2023 年 6 月 16 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F99b0a776e92e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-in-a-non-euclidean-space-99b0a776e92e&user=Mastafa+Foufa&userId=1e28ced4c3b5&source=-----99b0a776e92e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F99b0a776e92e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-in-a-non-euclidean-space-99b0a776e92e&source=-----99b0a776e92e---------------------bookmark_footer-----------)

> “我们的舒适和熟悉的欧几里得空间及其线性结构是否总是适合机器学习？近期研究对此提出了不同的观点：并非总是需要，有时还会带来负面影响，正如一系列激动人心的工作所展示的。从两年前对层次数据的双曲表示的概念开始，重要的推动带来了在非欧几里得空间中的新表示方法、新算法和模型，以及对非欧几里得机器学习基本功能的新视角。” *由* [*Fred Sala*](http://stanford.edu/~fredsala)*,* [*Ines Chami*](http://web.stanford.edu/~chami/)*,* [*Adva Wolf*](https://mathematics.stanford.edu/people/adva-wolf)*,* [*Albert Gu*](http://web.stanford.edu/~albertgu/)*,* [*Beliz Gunel*](http://web.stanford.edu/~bgunel/) *和* [*Chris Ré*](https://cs.stanford.edu/people/chrismre/)， [2019](https://dawn.cs.stanford.edu/2019/10/10/noneuclidean/)

## 你将会在本文中学到什么。

+   **失真**衡量在将数据表示在另一个空间时距离保留的程度。

+   对于某些数据，欧几里得空间会导致高失真，因此会使用如球面或双曲空间等非欧几里得空间。

+   黎曼几何工具，如流形和黎曼度量，被用于非欧几里得机器学习。

+   流形是局部上欧几里得的弯曲空间。

+   指数和对数映射用于从一个…
