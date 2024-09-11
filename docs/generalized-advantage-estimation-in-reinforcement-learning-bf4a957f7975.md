# 强化学习中的广义优势估计

> 原文：[https://towardsdatascience.com/generalized-advantage-estimation-in-reinforcement-learning-bf4a957f7975?source=collection_archive---------14-----------------------#2023-03-27](https://towardsdatascience.com/generalized-advantage-estimation-in-reinforcement-learning-bf4a957f7975?source=collection_archive---------14-----------------------#2023-03-27)

## 策策梯度中的偏差与方差权衡

[](https://siwei-causevic.medium.com/?source=post_page-----bf4a957f7975--------------------------------)[![Siwei Causevic](../Images/bb8e4ec911272a8e381e94129eabe166.png)](https://siwei-causevic.medium.com/?source=post_page-----bf4a957f7975--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bf4a957f7975--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bf4a957f7975--------------------------------) [Siwei Causevic](https://siwei-causevic.medium.com/?source=post_page-----bf4a957f7975--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F301dc9114da0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeneralized-advantage-estimation-in-reinforcement-learning-bf4a957f7975&user=Siwei+Causevic&userId=301dc9114da0&source=post_page-301dc9114da0----bf4a957f7975---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bf4a957f7975--------------------------------) ·6分钟阅读·2023年3月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbf4a957f7975&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeneralized-advantage-estimation-in-reinforcement-learning-bf4a957f7975&user=Siwei+Causevic&userId=301dc9114da0&source=-----bf4a957f7975---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbf4a957f7975&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeneralized-advantage-estimation-in-reinforcement-learning-bf4a957f7975&source=-----bf4a957f7975---------------------bookmark_footer-----------)![](../Images/f6ba99d09255dd7c15926e13695c4125.png)

作者提供的照片

策策梯度方法是强化学习中最广泛使用的学习算法之一。它们旨在优化参数化的策略，并利用价值函数来帮助估计策略应该如何改进。

强化学习中的一个主要问题，尤其是对于策略梯度方法，是动作与其对奖励的正面或负面影响之间的长时间延迟，这使得奖励估计变得极其困难。尽管如此，RL 研究人员通常使用从回合中获得的引导奖励或价值函数，甚至有时两者结合，来估计长期奖励（回报）。然而，这两种方法都有其缺陷。前者的问题在于样本的高方差，后者的问题则在于估计的价值函数的高偏差。

在本文中，我们将深入探讨广义优势估计（GAE），一种可以显著降低方差同时保持可接受偏差水平的策略梯度估计方法。

以下内容假设对策略梯度方法有基本了解。如果你对强化学习不熟悉，请查看我之前的文章 [强化学习基础与算法概述](/an-overview-of-classic-reinforcement-learning-algorithms-part-1-f79c8b87e5af) 和 [深入探讨](https://medium.com/towards-data-science/policy-gradient-reinforce-algorithm-with-baseline-e95ace11c1c4)…
