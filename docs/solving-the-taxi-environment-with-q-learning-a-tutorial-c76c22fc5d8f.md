# 使用 Q-Learning 解决出租车环境——教程

> 原文：[https://towardsdatascience.com/solving-the-taxi-environment-with-q-learning-a-tutorial-c76c22fc5d8f?source=collection_archive---------6-----------------------#2023-03-20](https://towardsdatascience.com/solving-the-taxi-environment-with-q-learning-a-tutorial-c76c22fc5d8f?source=collection_archive---------6-----------------------#2023-03-20)

## 一个 Python 实现的 Q-learning，用于在动画 Jupyter Notebook 中解决 OpenAI Gym 的 Taxi-v3 环境

[](https://wvheeswijk.medium.com/?source=post_page-----c76c22fc5d8f--------------------------------)[![Wouter van Heeswijk, PhD](../Images/9c996bccd6fdfb6d9aa8b50b93338eb9.png)](https://wvheeswijk.medium.com/?source=post_page-----c76c22fc5d8f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c76c22fc5d8f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c76c22fc5d8f--------------------------------) [Wouter van Heeswijk, PhD](https://wvheeswijk.medium.com/?source=post_page-----c76c22fc5d8f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F33f45c9ab481&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-the-taxi-environment-with-q-learning-a-tutorial-c76c22fc5d8f&user=Wouter+van+Heeswijk%2C+PhD&userId=33f45c9ab481&source=post_page-33f45c9ab481----c76c22fc5d8f---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c76c22fc5d8f--------------------------------) ·8分钟阅读·2023年3月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc76c22fc5d8f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-the-taxi-environment-with-q-learning-a-tutorial-c76c22fc5d8f&user=Wouter+van+Heeswijk%2C+PhD&userId=33f45c9ab481&source=-----c76c22fc5d8f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc76c22fc5d8f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-the-taxi-environment-with-q-learning-a-tutorial-c76c22fc5d8f&source=-----c76c22fc5d8f---------------------bookmark_footer-----------)![](../Images/c60a0923b3fdf481b97b7be803bcebf7.png)

照片由[亚历山大·雷德尔](https://unsplash.com/@alexanderredl?utm_source=medium&utm_medium=referral)提供，出处：[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

OpenAI Gym 的出租车环境的目标——是的，就是[ChatGPT](https://medium.com/datadriveninvestor/how-does-the-company-behind-chatgpt-and-dall-e-make-its-money-d6aa5121a849)和 Dall⋅E 的开发公司——简单而直接，是深入强化学习（RL）领域的绝佳入门。

本文提供了一个逐步指南，帮助你实现环境、使用表格Q学习来学习策略，并在动画中可视化学习到的行为。如果你想在强化学习中迈出第一步，这个教程可能正适合你。

# 问题设置

在出租车环境中，出租车需要在一个小型停车场内接送乘客，并沿最短路径行驶。用强化学习来学习这样的任务有多难？让我们来看看。

在深入实施之前，最好先将设置形式化为一个[马尔可夫决策过程](https://medium.com/towards-data-science/the-five-building-blocks-of-markov-decision-processes-997dc1ab48a7)（MDP），以掌握当前问题的数学结构。

## 状态空间

一个MDP状态包含了（i）确定行动、（ii）计算……所需的信息。
