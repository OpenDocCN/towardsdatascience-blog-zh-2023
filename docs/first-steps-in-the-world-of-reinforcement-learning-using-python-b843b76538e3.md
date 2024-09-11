# 《使用 Python 进行强化学习的第一步》

> 原文：[https://towardsdatascience.com/first-steps-in-the-world-of-reinforcement-learning-using-python-b843b76538e3?source=collection_archive---------6-----------------------#2023-01-13](https://towardsdatascience.com/first-steps-in-the-world-of-reinforcement-learning-using-python-b843b76538e3?source=collection_archive---------6-----------------------#2023-01-13)

## 本文的 Python 实现演示了如何在强化学习的基本世界之一——网格世界中找到最佳位置。

[](https://eligijus-bujokas.medium.com/?source=post_page-----b843b76538e3--------------------------------)[![Eligijus Bujokas](../Images/061fd30136caea2ba927140e8b3fae3c.png)](https://eligijus-bujokas.medium.com/?source=post_page-----b843b76538e3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b843b76538e3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b843b76538e3--------------------------------) [Eligijus Bujokas](https://eligijus-bujokas.medium.com/?source=post_page-----b843b76538e3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd61597e07b4d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffirst-steps-in-the-world-of-reinforcement-learning-using-python-b843b76538e3&user=Eligijus+Bujokas&userId=d61597e07b4d&source=post_page-d61597e07b4d----b843b76538e3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b843b76538e3--------------------------------) ·15分钟阅读·2023年1月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb843b76538e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffirst-steps-in-the-world-of-reinforcement-learning-using-python-b843b76538e3&user=Eligijus+Bujokas&userId=d61597e07b4d&source=-----b843b76538e3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb843b76538e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffirst-steps-in-the-world-of-reinforcement-learning-using-python-b843b76538e3&source=-----b843b76538e3---------------------bookmark_footer-----------)![](../Images/b476cbb9f0b2f5a0f39114a7b0ebca24.png)

Gridworld 矩阵；作者提供的照片

本文旨在通过 Python 代码和注释介绍强化学习（以下简称 **RL**）的基本概念和定义。

这篇文章受到了伟大的 RL 课程的启发：[https://www.coursera.org/learn/fundamentals-of-reinforcement-learning](https://www.coursera.org/learn/fundamentals-of-reinforcement-learning)

该理论在书籍中阐述¹: [http://www.incompleteideas.net/book/RLbook2020.pdf](http://www.incompleteideas.net/book/RLbook2020.pdf)

我所有RL实验的代码可以在我的Gitlab仓库中查看: [https://github.com/Eligijus112/rl-snake-game](https://github.com/Eligijus112/rl-snake-game)

网格世界问题是RL中的经典问题，我们希望为代理创建一个最佳策略，以便在网格中遍历。

网格是一个方阵，代理可以在每个单元格中向四个方向（上、下、左、右）移动。代理每走一步会获得-1的奖励，如果到达目标单元格，则获得+10的奖励。这些奖励数字是任意的，可以由用户定义。
