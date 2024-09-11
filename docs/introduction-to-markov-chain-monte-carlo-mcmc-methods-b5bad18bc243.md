# 马尔可夫链蒙特卡罗（MCMC）方法简介

> 原文：[https://towardsdatascience.com/introduction-to-markov-chain-monte-carlo-mcmc-methods-b5bad18bc243?source=collection_archive---------3-----------------------#2023-02-21](https://towardsdatascience.com/introduction-to-markov-chain-monte-carlo-mcmc-methods-b5bad18bc243?source=collection_archive---------3-----------------------#2023-02-21)

## 马尔可夫链、Metropolis-Hastings、吉布斯抽样，以及它们与贝叶斯推断的关系

[](https://medium.com/@hrmnmichaels?source=post_page-----b5bad18bc243--------------------------------)[![Oliver S](../Images/b5ee0fa2d5fb115f62e2e9dfcb92afdd.png)](https://medium.com/@hrmnmichaels?source=post_page-----b5bad18bc243--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b5bad18bc243--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b5bad18bc243--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----b5bad18bc243--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff2daf6260cca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-markov-chain-monte-carlo-mcmc-methods-b5bad18bc243&user=Oliver+S&userId=f2daf6260cca&source=post_page-f2daf6260cca----b5bad18bc243---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----b5bad18bc243--------------------------------) ·14分钟阅读·2023年2月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb5bad18bc243&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-markov-chain-monte-carlo-mcmc-methods-b5bad18bc243&user=Oliver+S&userId=f2daf6260cca&source=-----b5bad18bc243---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb5bad18bc243&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-markov-chain-monte-carlo-mcmc-methods-b5bad18bc243&source=-----b5bad18bc243---------------------bookmark_footer-----------)![](../Images/f1fc4e2428f11d573f2bd8434e5b64c7.png)

照片由[Edge2Edge Media](https://unsplash.com/@edge2edgemedia?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄于[Unsplash](https://unsplash.com/photos/uKlneQRwaxY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

本文是关于马尔可夫链蒙特卡洛（MCMC）采样方法的介绍。我们将特别考虑两种方法，即Metropolis-Hastings算法和Gibbs采样。我们将介绍它们，并证明它们的有效性，实施Python中的实际示例，最终解释如何将采样应用于贝叶斯推断以及它为何如此重要。

# 介绍

MCMC方法是一类利用马尔可夫链生成相关数据样本的采样方法。它们的基本思想是构建这样一些马尔可夫链，使得从中采样变得简单，并且它们的平稳分布就是我们的目标分布——这样，当跟随这些链时，在极限情况下，我们就能从目标分布中获得样本。

我们为什么需要这个？在上一篇文章中，我介绍了基本的采样方法，其中包括[复杂分布的拒绝采样和重要性采样](/introduction-to-sampling-methods-c934b64b6b08)。这些方法生成独立的数据样本，而在这里，我们生成的是相关样本——这并没有回答之前的问题，但这是一个重要的区别。然而，在之前的文章中我们看到…
