# 深度确定性策略梯度（DDPG）详解

> 原文：[https://towardsdatascience.com/deep-deterministic-policy-gradients-explained-4643c1f71b2e?source=collection_archive---------12-----------------------#2023-04-05](https://towardsdatascience.com/deep-deterministic-policy-gradients-explained-4643c1f71b2e?source=collection_archive---------12-----------------------#2023-04-05)

## 一种基于梯度的强化学习算法，用于学习适用于连续动作空间的确定性策略

[](https://wvheeswijk.medium.com/?source=post_page-----4643c1f71b2e--------------------------------)[![Wouter van Heeswijk, PhD](../Images/9c996bccd6fdfb6d9aa8b50b93338eb9.png)](https://wvheeswijk.medium.com/?source=post_page-----4643c1f71b2e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4643c1f71b2e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4643c1f71b2e--------------------------------) [Wouter van Heeswijk, PhD](https://wvheeswijk.medium.com/?source=post_page-----4643c1f71b2e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F33f45c9ab481&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-deterministic-policy-gradients-explained-4643c1f71b2e&user=Wouter+van+Heeswijk%2C+PhD&userId=33f45c9ab481&source=post_page-33f45c9ab481----4643c1f71b2e---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4643c1f71b2e--------------------------------) ·11 分钟阅读·2023年4月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4643c1f71b2e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-deterministic-policy-gradients-explained-4643c1f71b2e&user=Wouter+van+Heeswijk%2C+PhD&userId=33f45c9ab481&source=-----4643c1f71b2e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4643c1f71b2e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-deterministic-policy-gradients-explained-4643c1f71b2e&source=-----4643c1f71b2e---------------------bookmark_footer-----------)![](../Images/f2b615669a6ed7f8931e91857e17ab23.png)

图片来源：[Jonathan Ford](https://unsplash.com/@jonfordphotos?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文介绍了深度确定性策略梯度（DDPG）—— 一种适用于**连续动作空间中的确定性策略**的强化学习算法。通过将演员-评论家范式与深度神经网络结合，能够在不依赖于随机策略的情况下解决连续动作空间的问题。

特别是在连续控制任务中，如果不希望动作中有随机性，例如机器人或导航，DDPG可能正是你需要的算法。

# DDPG如何适应强化学习的领域？

DDPG展示了策略方法和价值方法的元素，使其成为一种混合策略类。

[](/the-four-policy-classes-of-reinforcement-learning-38185daa6c8a?source=post_page-----4643c1f71b2e--------------------------------) [## 强化学习的四类策略

towardsdatascience.com](/the-four-policy-classes-of-reinforcement-learning-38185daa6c8a?source=post_page-----4643c1f71b2e--------------------------------)

**策略梯度方法** 如 [REINFORCE](https://medium.com/towards-data-science/policy-gradients-in-reinforcement-learning-explained-ecec7df94245)、[TRPO](https://medium.com/towards-data-science/trust-region-policy-optimization-trpo-explained-4b56bd206fc2) 和 [PPO](/proximal-policy-optimization-ppo-explained-abed1952457b) 使用随机策略 *π:a~P(a|s)* 来探索和比较动作。这些方法从…
