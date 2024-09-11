# 使用 Python 进行重要性采样

> 原文：[https://towardsdatascience.com/importance-sampling-with-python-93b03eb9ca22?source=collection_archive---------9-----------------------#2023-05-10](https://towardsdatascience.com/importance-sampling-with-python-93b03eb9ca22?source=collection_archive---------9-----------------------#2023-05-10)

![](../Images/f7f54b21a31c0992de0b707432134091.png)

图片由 [Edge2Edge Media](https://unsplash.com/@edge2edgemedia?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 学习如何在只能访问另一种分布时从一个分布中采样

[](https://medium.com/@marcellopoliti?source=post_page-----93b03eb9ca22--------------------------------)[![Marcello Politi](../Images/484e44571bd2e75acfe5fef3146ab3c2.png)](https://medium.com/@marcellopoliti?source=post_page-----93b03eb9ca22--------------------------------)[](https://towardsdatascience.com/?source=post_page-----93b03eb9ca22--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----93b03eb9ca22--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----93b03eb9ca22--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimportance-sampling-with-python-93b03eb9ca22&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----93b03eb9ca22---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----93b03eb9ca22--------------------------------) ·5 分钟阅读·2023年5月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F93b03eb9ca22&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimportance-sampling-with-python-93b03eb9ca22&user=Marcello+Politi&userId=7390355d40fe&source=-----93b03eb9ca22---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F93b03eb9ca22&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimportance-sampling-with-python-93b03eb9ca22&source=-----93b03eb9ca22---------------------bookmark_footer-----------)

## 介绍

在数据科学家必须掌握的各种采样方法中，最重要的之一是被称为重要性采样的方法。

这种方法允许我们**从一个分布中采样，即使我们实际上只能从另一个分布中采样！** 让我们看看它是如何工作的。

## 重要性采样

例如，假设从分布 g(x) 中采样是不可行的，因为例如成本太高。但与此同时，我们有一个分布 f(x)，我们称之为重要性分布，从中我们能够进行采样。

我们可以通过对分布 f(x) 的采样来计算我们真正感兴趣的分布 g(x) 的统计数据。我们来看看怎么做。

想象一下，我们有一个分布 f(x)，表示骰子每个面的概率。如果骰子是“公平”的，那么每个面的概率都是 1/6，因此我们可以将这个分布表示如下。
