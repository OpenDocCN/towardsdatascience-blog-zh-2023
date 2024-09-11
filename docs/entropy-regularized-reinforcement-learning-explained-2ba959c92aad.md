# 熵正则化的强化学习解释

> 原文：[`towardsdatascience.com/entropy-regularized-reinforcement-learning-explained-2ba959c92aad?source=collection_archive---------11-----------------------#2023-10-26`](https://towardsdatascience.com/entropy-regularized-reinforcement-learning-explained-2ba959c92aad?source=collection_archive---------11-----------------------#2023-10-26)

## 通过在算法中添加熵奖励，学习更可靠、稳健和可转移的策略

[](https://wvheeswijk.medium.com/?source=post_page-----2ba959c92aad--------------------------------)![Wouter van Heeswijk, PhD](https://wvheeswijk.medium.com/?source=post_page-----2ba959c92aad--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2ba959c92aad--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2ba959c92aad--------------------------------) [Wouter van Heeswijk, PhD](https://wvheeswijk.medium.com/?source=post_page-----2ba959c92aad--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F33f45c9ab481&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fentropy-regularized-reinforcement-learning-explained-2ba959c92aad&user=Wouter+van+Heeswijk%2C+PhD&userId=33f45c9ab481&source=post_page-33f45c9ab481----2ba959c92aad---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2ba959c92aad--------------------------------) · 8 min read · Oct 26, 2023 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2ba959c92aad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fentropy-regularized-reinforcement-learning-explained-2ba959c92aad&user=Wouter+van+Heeswijk%2C+PhD&userId=33f45c9ab481&source=-----2ba959c92aad---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2ba959c92aad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fentropy-regularized-reinforcement-learning-explained-2ba959c92aad&source=-----2ba959c92aad---------------------bookmark_footer-----------)![](img/2fce76a87f64dcb27147a0868dc84b72.png)

由 [Jeremy Thomas](https://unsplash.com/@jeremythomasphoto?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供的照片

*熵* 是与混乱、随机或不确定状态相关的概念。它可以被视为**随机变量的信息度量**。传统上，它与热力学等领域相关，但这一术语已扩展到许多其他领域。

1948 年，克劳德·香农在信息理论中引入了熵的概念。在这种情况下，如果一个事件发生的概率较低，则该事件被认为提供了更多的信息；**事件的信息与其发生概率呈反比关系**。直观地说：我们从稀有事件中学到的更多。

熵的概念可以形式化如下：

在强化学习（RL）中，熵的概念也被应用于鼓励探索。在这种情况下，熵是**由随机策略返回的动作的可预测性度量**。

具体来说，**RL 将策略的熵（即动作的概率分布）作为奖励的一部分，并将其嵌入奖励组件中**。本文讨论了基本情况，但熵奖励是许多…
