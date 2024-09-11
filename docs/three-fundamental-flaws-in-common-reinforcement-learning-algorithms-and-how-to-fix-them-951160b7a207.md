# 常见强化学习算法中的三个根本缺陷（及其解决方法）

> 原文：[https://towardsdatascience.com/three-fundamental-flaws-in-common-reinforcement-learning-algorithms-and-how-to-fix-them-951160b7a207?source=collection_archive---------9-----------------------#2023-01-30](https://towardsdatascience.com/three-fundamental-flaws-in-common-reinforcement-learning-algorithms-and-how-to-fix-them-951160b7a207?source=collection_archive---------9-----------------------#2023-01-30)

## 针对日常强化学习算法中遇到的这些缺陷进行自我提升

[](https://wvheeswijk.medium.com/?source=post_page-----951160b7a207--------------------------------)[![Wouter van Heeswijk, PhD](../Images/9c996bccd6fdfb6d9aa8b50b93338eb9.png)](https://wvheeswijk.medium.com/?source=post_page-----951160b7a207--------------------------------)[](https://towardsdatascience.com/?source=post_page-----951160b7a207--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----951160b7a207--------------------------------) [Wouter van Heeswijk, PhD](https://wvheeswijk.medium.com/?source=post_page-----951160b7a207--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F33f45c9ab481&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthree-fundamental-flaws-in-common-reinforcement-learning-algorithms-and-how-to-fix-them-951160b7a207&user=Wouter+van+Heeswijk%2C+PhD&userId=33f45c9ab481&source=post_page-33f45c9ab481----951160b7a207---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----951160b7a207--------------------------------) ·8 min read·2023年1月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F951160b7a207&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthree-fundamental-flaws-in-common-reinforcement-learning-algorithms-and-how-to-fix-them-951160b7a207&user=Wouter+van+Heeswijk%2C+PhD&userId=33f45c9ab481&source=-----951160b7a207---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F951160b7a207&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthree-fundamental-flaws-in-common-reinforcement-learning-algorithms-and-how-to-fix-them-951160b7a207&source=-----951160b7a207---------------------bookmark_footer-----------)![](../Images/9dda11eec29ec887f349772c00f43653.png)

图片由 [Varvara Grabova](https://unsplash.com/@santabarbara77?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

强化学习算法，如Q学习和REINFORCE，已经存在了几十年，其教科书式的实现仍然被广泛使用。不幸的是，它们存在一些根本性的缺陷，这大大增加了学习良好策略的难度。

本文讨论了经典强化学习算法的三个主要缺陷，并提供了克服这些缺陷的解决方案。

# 一、选择过高估值的动作

## 问题

大多数强化学习算法使用某种价值函数的概念来捕捉后续奖励，其中许多基于著名的Q学习算法。Q学习的机制是**选择期望值最高的动作**。根据初始化情况，这一机制可能会陷入第一次尝试的动作，因此我们还会以概率ϵ（通常设置为0.05左右）选择随机动作。

从理论上讲，我们会无限次探索每个动作，Q值会收敛到真实值。然而在实践中，我们处理的是…
