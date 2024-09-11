# 从头开始的动态定价与强化学习：Q-Learning

> 原文：[https://towardsdatascience.com/dynamic-pricing-with-reinforcement-learning-from-scratch-q-learning-fb3fb764da49?source=collection_archive---------1-----------------------#2023-08-26](https://towardsdatascience.com/dynamic-pricing-with-reinforcement-learning-from-scratch-q-learning-fb3fb764da49?source=collection_archive---------1-----------------------#2023-08-26)

## Q-Learning 介绍及实际 Python 示例

[](https://nicolo-albanese.medium.com/?source=post_page-----fb3fb764da49--------------------------------)[![Nicolo Cosimo Albanese](../Images/9a2c26207146741b58c3742927d09450.png)](https://nicolo-albanese.medium.com/?source=post_page-----fb3fb764da49--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fb3fb764da49--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fb3fb764da49--------------------------------) [Nicolo Cosimo Albanese](https://nicolo-albanese.medium.com/?source=post_page-----fb3fb764da49--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7430df412ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-pricing-with-reinforcement-learning-from-scratch-q-learning-fb3fb764da49&user=Nicolo+Cosimo+Albanese&userId=7430df412ec&source=post_page-7430df412ec----fb3fb764da49---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fb3fb764da49--------------------------------) ·12 min 阅读·2023年8月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffb3fb764da49&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-pricing-with-reinforcement-learning-from-scratch-q-learning-fb3fb764da49&user=Nicolo+Cosimo+Albanese&userId=7430df412ec&source=-----fb3fb764da49---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffb3fb764da49&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-pricing-with-reinforcement-learning-from-scratch-q-learning-fb3fb764da49&source=-----fb3fb764da49---------------------bookmark_footer-----------)![](../Images/c762b0a93c8cd128f116b6cdd17b8eda.png)

探索价格以找到最优的动作-状态值以最大化利润。图像由作者提供。

# 目录

1.  [介绍](#502a)

1.  [强化学习基础](#9e6b)

    2.1 [关键概念](#6e64)

    2.2 [Q-函数](#b05f)

    2.3 [Q-值](#fed6)

    2.4 [Q-Learning](#3d03)

    2.5 [贝尔曼方程](#1112)

    2.6 [探索与利用](#3cf3)

    2.7 [Q-表](#7e4c)

1.  [动态定价问题](#f694)

    3.1 [问题陈述](#589b)

    3.2 [实现](#7607)

1.  [结论](#68d5)

1.  [参考文献](#6cb9)

# 1\. 引言

在这篇文章中，我们介绍了强化学习的核心概念，并深入探讨了 Q-Learning，这是一种通过基于奖励和经验做出明智决策来赋能智能体学习最佳策略的方法。

我们还分享了一个从零开始构建的实际 Python 示例。特别是，我们训练一个智能体掌握定价艺术，这是商业中的一个关键方面，以便它可以学习如何最大化利润。

毫不拖延地，我们开始我们的旅程吧。

# 2\. 强化学习入门

## 2.1 关键概念
