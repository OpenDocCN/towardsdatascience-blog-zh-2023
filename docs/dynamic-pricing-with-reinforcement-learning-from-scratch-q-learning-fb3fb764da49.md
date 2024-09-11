# 从头开始的动态定价与强化学习：Q-Learning

> 原文：[`towardsdatascience.com/dynamic-pricing-with-reinforcement-learning-from-scratch-q-learning-fb3fb764da49?source=collection_archive---------1-----------------------#2023-08-26`](https://towardsdatascience.com/dynamic-pricing-with-reinforcement-learning-from-scratch-q-learning-fb3fb764da49?source=collection_archive---------1-----------------------#2023-08-26)

## Q-Learning 介绍及实际 Python 示例

[](https://nicolo-albanese.medium.com/?source=post_page-----fb3fb764da49--------------------------------)![Nicolo Cosimo Albanese](https://nicolo-albanese.medium.com/?source=post_page-----fb3fb764da49--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fb3fb764da49--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fb3fb764da49--------------------------------) [Nicolo Cosimo Albanese](https://nicolo-albanese.medium.com/?source=post_page-----fb3fb764da49--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7430df412ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-pricing-with-reinforcement-learning-from-scratch-q-learning-fb3fb764da49&user=Nicolo+Cosimo+Albanese&userId=7430df412ec&source=post_page-7430df412ec----fb3fb764da49---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fb3fb764da49--------------------------------) ·12 min 阅读·2023 年 8 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffb3fb764da49&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-pricing-with-reinforcement-learning-from-scratch-q-learning-fb3fb764da49&user=Nicolo+Cosimo+Albanese&userId=7430df412ec&source=-----fb3fb764da49---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffb3fb764da49&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-pricing-with-reinforcement-learning-from-scratch-q-learning-fb3fb764da49&source=-----fb3fb764da49---------------------bookmark_footer-----------)![](img/c762b0a93c8cd128f116b6cdd17b8eda.png)

探索价格以找到最优的动作-状态值以最大化利润。图像由作者提供。

# 目录

1.  介绍

1.  强化学习基础

    2.1 关键概念

    2.2 Q-函数

    2.3 Q-值

    2.4 Q-Learning

    2.5 贝尔曼方程

    2.6 探索与利用

    2.7 Q-表

1.  动态定价问题

    3.1 问题陈述

    3.2 实现

1.  结论

1.  参考文献

# 1\. 引言

在这篇文章中，我们介绍了强化学习的核心概念，并深入探讨了 Q-Learning，这是一种通过基于奖励和经验做出明智决策来赋能智能体学习最佳策略的方法。

我们还分享了一个从零开始构建的实际 Python 示例。特别是，我们训练一个智能体掌握定价艺术，这是商业中的一个关键方面，以便它可以学习如何最大化利润。

毫不拖延地，我们开始我们的旅程吧。

# 2\. 强化学习入门

## 2.1 关键概念
