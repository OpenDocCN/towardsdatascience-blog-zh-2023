# 应用强化学习 V：用于连续控制的归一化优势函数（NAF）

> 原文：[`towardsdatascience.com/applied-reinforcement-learning-v-normalized-advantage-function-naf-for-continuous-control-62ad143d3095?source=collection_archive---------11-----------------------#2023-01-19`](https://towardsdatascience.com/applied-reinforcement-learning-v-normalized-advantage-function-naf-for-continuous-control-62ad143d3095?source=collection_archive---------11-----------------------#2023-01-19)

## 介绍和解释 NAF 算法，该算法广泛应用于连续控制任务

[](https://medium.com/@JavierMtz5?source=post_page-----62ad143d3095--------------------------------)![哈维尔·马丁内斯·奥赫达](https://medium.com/@JavierMtz5?source=post_page-----62ad143d3095--------------------------------)[](https://towardsdatascience.com/?source=post_page-----62ad143d3095--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----62ad143d3095--------------------------------) [哈维尔·马丁内斯·奥赫达](https://medium.com/@JavierMtz5?source=post_page-----62ad143d3095--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F74d7213a71a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fapplied-reinforcement-learning-v-normalized-advantage-function-naf-for-continuous-control-62ad143d3095&user=Javier+Mart%C3%ADnez+Ojeda&userId=74d7213a71a8&source=post_page-74d7213a71a8----62ad143d3095---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----62ad143d3095--------------------------------) ·9 min read·2023 年 1 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F62ad143d3095&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fapplied-reinforcement-learning-v-normalized-advantage-function-naf-for-continuous-control-62ad143d3095&user=Javier+Mart%C3%ADnez+Ojeda&userId=74d7213a71a8&source=-----62ad143d3095---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F62ad143d3095&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fapplied-reinforcement-learning-v-normalized-advantage-function-naf-for-continuous-control-62ad143d3095&source=-----62ad143d3095---------------------bookmark_footer-----------)![](img/980806b6e676bcf2f2e28c3dca2000f0.png)

照片由 [Sufyan](https://unsplash.com/@blenderdesigner_1688?utm_source=medium&utm_medium=referral) 提供，刊登在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 如果你想在没有 Premium Medium 账户的情况下阅读这篇文章，可以通过这个朋友链接访问 :)
> 
> [`www.learnml.wiki/applied-reinforcement-learning-v-normalized-advantage-function-naf-for-continuous-control/`](https://www.learnml.wiki/applied-reinforcement-learning-v-normalized-advantage-function-naf-for-continuous-control/)

本系列的前几篇文章介绍并解释了两种自其诞生以来被广泛使用的强化学习算法：[**Q-Learning**](https://medium.com/towards-data-science/applied-reinforcement-learning-i-q-learning-d6086c1f437) 和 [**DQN**](https://medium.com/towards-data-science/applied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9)。

**Q-Learning** 将 Q-值存储在一个动作-状态矩阵中，因此要获得状态 *s* 下具有最大 Q-值的动作 *a*，必须找到 Q-值矩阵中第 *s* 行的最大元素，这使得它在连续状态或动作空间中的应用变得不可能，因为 Q-值矩阵将是无限的。

另一方面，**DQN** 部分解决了这个问题，通过使用神经网络来获得与状态 *s* 相关的 Q-值，使得神经网络的输出是代理的每个可能动作的 Q-值（相当于 Q-Learning 中的动作-状态矩阵的一行）。这个算法允许在具有连续状态的环境中进行训练……
