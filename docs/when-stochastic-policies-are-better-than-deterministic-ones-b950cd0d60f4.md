# 随机策略为何优于确定性策略

> 原文：[`towardsdatascience.com/when-stochastic-policies-are-better-than-deterministic-ones-b950cd0d60f4?source=collection_archive---------5-----------------------#2023-02-18`](https://towardsdatascience.com/when-stochastic-policies-are-better-than-deterministic-ones-b950cd0d60f4?source=collection_archive---------5-----------------------#2023-02-18)

## 我们为什么让随机性决定强化学习中的行动选择

[](https://wvheeswijk.medium.com/?source=post_page-----b950cd0d60f4--------------------------------)![Wouter van Heeswijk, PhD](https://wvheeswijk.medium.com/?source=post_page-----b950cd0d60f4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b950cd0d60f4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b950cd0d60f4--------------------------------) [Wouter van Heeswijk, PhD](https://wvheeswijk.medium.com/?source=post_page-----b950cd0d60f4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F33f45c9ab481&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-stochastic-policies-are-better-than-deterministic-ones-b950cd0d60f4&user=Wouter+van+Heeswijk%2C+PhD&userId=33f45c9ab481&source=post_page-33f45c9ab481----b950cd0d60f4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b950cd0d60f4--------------------------------) ·6 分钟阅读·2023 年 2 月 18 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb950cd0d60f4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-stochastic-policies-are-better-than-deterministic-ones-b950cd0d60f4&user=Wouter+van+Heeswijk%2C+PhD&userId=33f45c9ab481&source=-----b950cd0d60f4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb950cd0d60f4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-stochastic-policies-are-better-than-deterministic-ones-b950cd0d60f4&source=-----b950cd0d60f4---------------------bookmark_footer-----------)![](img/8175bb620e2361f15241cbfd404190f9.png)

石头剪刀布如果使用确定性策略会变得很无聊 [图片来源于 [Marcus Wallis](https://unsplash.com/@marcus_wallis?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)]

如果你习惯了确定性决策策略（例如，[深度 Q 学习](https://medium.com/towards-data-science/deep-q-learning-for-the-cliff-walking-problem-b54835409046)），你可能会对随机策略的需求和使用感到困惑。毕竟，**确定性策略**提供了一个便捷的状态-动作映射*π:s ↦ a*，理想情况下甚至是最优映射（即，如果所有的贝尔曼方程都被完美学习的话）。

相比之下，**随机策略**——通过在给定状态下对动作的条件概率分布来表示，*π:P(a|s)*——似乎相当不便且不精确。我们为何要让随机性来指引我们的行动，为什么将选择最佳决策的权利交给运气？

实际上，考虑到大量的演员-评论家算法，许多强化学习（RL）算法确实使用了随机策略。显然，这种方法必定有其好处。本文讨论了四种随机策略优于其确定性对手的情况。
