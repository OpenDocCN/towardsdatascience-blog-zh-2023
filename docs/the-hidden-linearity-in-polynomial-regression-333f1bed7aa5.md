# 多项式回归中的隐藏线性

> 原文：[https://towardsdatascience.com/the-hidden-linearity-in-polynomial-regression-333f1bed7aa5?source=collection_archive---------10-----------------------#2023-03-17](https://towardsdatascience.com/the-hidden-linearity-in-polynomial-regression-333f1bed7aa5?source=collection_archive---------10-----------------------#2023-03-17)

## 从两个角度深入理解线性模型

[](https://medium.com/@angela.shi?source=post_page-----333f1bed7aa5--------------------------------)[![Angela and Kezhan Shi](../Images/a89d678f2f3887c0c2ff3928f9d767b4.png)](https://medium.com/@angela.shi?source=post_page-----333f1bed7aa5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----333f1bed7aa5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----333f1bed7aa5--------------------------------) [Angela and Kezhan Shi](https://medium.com/@angela.shi?source=post_page-----333f1bed7aa5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2bf03e38122e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-hidden-linearity-in-polynomial-regression-333f1bed7aa5&user=Angela+and+Kezhan+Shi&userId=2bf03e38122e&source=post_page-2bf03e38122e----333f1bed7aa5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----333f1bed7aa5--------------------------------) ·9分钟阅读·2023年3月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F333f1bed7aa5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-hidden-linearity-in-polynomial-regression-333f1bed7aa5&user=Angela+and+Kezhan+Shi&userId=2bf03e38122e&source=-----333f1bed7aa5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F333f1bed7aa5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-hidden-linearity-in-polynomial-regression-333f1bed7aa5&source=-----333f1bed7aa5---------------------bookmark_footer-----------)

在这篇文章中，我们将讨论这一观点：

> 多项式回归是一种线性回归。

![](../Images/7a33640069803e36d823adefaa7d337a.png)

图片由 [Denny Müller](https://unsplash.com/de/@redaquamedia?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

初看之下，这一说法可能显得荒谬。多项式回归被认为是非线性回归，而线性回归则是线性的，那么这两种模型如何能被认为是相同的呢？

就像生活中的许多事情一样，你可以从不同的角度看待同样的事物，两个不同的人可能会得出显然不同的结论。然而，我们往往忽视的是，答案并不是最重要的部分，而是你用来达成结论的方式、方法论或框架。我最近发布了[一篇关于逻辑回归是回归器还是分类器的辩论的文章](https://medium.com/towards-data-science/is-logistic-regression-a-regressor-or-a-classifier-lets-end-the-debate-a01b024f7f65)。我提到了两种观点：统计学与机器学习。在这篇文章中，我们还将探讨这两种不同的观点对多项式回归的理解。

哦，是的，有些人可能会说多项式回归只是一些理论模型，因为将一个特征升高到高次方并没有实际意义。通过这篇文章，你将发现……
