# 多元线性回归：深入探讨

> 原文：[https://towardsdatascience.com/multiple-linear-regression-a-deep-dive-f104c8ede236?source=collection_archive---------6-----------------------#2023-03-03](https://towardsdatascience.com/multiple-linear-regression-a-deep-dive-f104c8ede236?source=collection_archive---------6-----------------------#2023-03-03)

## 从零开始的多元线性回归：深刻理解

[](https://zubairhossain.medium.com/?source=post_page-----f104c8ede236--------------------------------)[![Md. Zubair](../Images/1b983a23226ce7561796fa5b28c00d65.png)](https://zubairhossain.medium.com/?source=post_page-----f104c8ede236--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f104c8ede236--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f104c8ede236--------------------------------) [Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----f104c8ede236--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2fdaeaeeea52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultiple-linear-regression-a-deep-dive-f104c8ede236&user=Md.+Zubair&userId=2fdaeaeeea52&source=post_page-2fdaeaeeea52----f104c8ede236---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f104c8ede236--------------------------------) · 10 分钟阅读 · 2023年3月3日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff104c8ede236&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultiple-linear-regression-a-deep-dive-f104c8ede236&user=Md.+Zubair&userId=2fdaeaeeea52&source=-----f104c8ede236---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff104c8ede236&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultiple-linear-regression-a-deep-dive-f104c8ede236&source=-----f104c8ede236---------------------bookmark_footer-----------)![](../Images/7e2b58dacca35db939172a64442a9a1a.png)

带有两个特征（x1 和 x2）的多元回归（图片来源：作者）

## 动机

我们人类很早就开始尝试创建智能系统。因为如果我们能自动化一个系统，它将使我们的生活更轻松，并且可以作为我们的助手。机器学习使这一切成为可能。有许多机器学习算法可以解决不同的问题。本文将介绍一种机器学习算法，***它可以解决具有多个变量的回归问题（连续值的预测）***。*假设你在经营一家房地产业务。* 作为业务所有者，你应该对建筑物、土地等的价格有一个正确的了解，以使你的业务盈利。在广泛的领域中跟踪价格对一个人来说是相当困难的。一个高效的机器学习回归模型可能会对你帮助很大。试想一下，你将位置、大小和其他相关信息输入系统，它就会自动显示价格。多元线性回归正好可以做到这一点。这不是很有趣吗！

我将解释多元线性回归的过程，并向您展示从零开始的实现。

`*[注意 — 如果你对简单线性回归没有清晰的概念，我建议你先了解一下*`…
