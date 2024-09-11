# 超参数优化——网格搜索、随机搜索和贝叶斯优化的简介及实施

> 原文：[https://towardsdatascience.com/hyperparameter-optimization-intro-and-implementation-of-grid-search-random-search-and-bayesian-b2f16c00578a?source=collection_archive---------10-----------------------#2023-03-13](https://towardsdatascience.com/hyperparameter-optimization-intro-and-implementation-of-grid-search-random-search-and-bayesian-b2f16c00578a?source=collection_archive---------10-----------------------#2023-03-13)

## 最常见的超参数优化方法，提升机器学习效果

[](https://medium.com/@fmnobar?source=post_page-----b2f16c00578a--------------------------------)[![Farzad Mahmoodinobar](../Images/2d75209693b712300e6f0796bd2487d0.png)](https://medium.com/@fmnobar?source=post_page-----b2f16c00578a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b2f16c00578a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b2f16c00578a--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----b2f16c00578a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3c56b7d4893e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhyperparameter-optimization-intro-and-implementation-of-grid-search-random-search-and-bayesian-b2f16c00578a&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=post_page-3c56b7d4893e----b2f16c00578a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b2f16c00578a--------------------------------) ·10分钟阅读·2023年3月13日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb2f16c00578a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhyperparameter-optimization-intro-and-implementation-of-grid-search-random-search-and-bayesian-b2f16c00578a&source=-----b2f16c00578a---------------------bookmark_footer-----------)![](../Images/13056882b4fc4ae3b84019b4de503886.png)

图片由 [Jonas Jaeken](https://unsplash.com/@jonasjaekenmedia) 提供，来源于 [Unsplash](https://unsplash.com/photos/Gg2ttawakqE)

通常，当我们尝试改进机器学习模型时，第一个想到的解决方案就是增加更多的训练数据。额外的数据通常会有所帮助（除了某些情况），但生成高质量的数据可能相当昂贵。超参数优化可以通过利用现有数据来节省时间和资源，从而获得最佳模型性能。

超参数优化，顾名思义，是识别机器学习模型最佳超参数组合的过程，以满足优化函数（即在给定数据集的情况下最大化模型的性能）。换句话说，每个模型都有多个可调整的参数，我们可以进行调整，直到找到优化组合。在超参数优化过程中可以更改的一些参数示例包括学习率、神经网络的架构（例如，隐藏层的数量）、正则化等。

在这篇文章中，我们将从概念上介绍三种最常见的超参数优化方法，即网格搜索（Grid）……
