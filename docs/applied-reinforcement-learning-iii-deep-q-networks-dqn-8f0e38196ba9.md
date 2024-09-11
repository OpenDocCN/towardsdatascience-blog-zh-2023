# 应用强化学习 III：深度 Q 网络（DQN）

> 原文：[https://towardsdatascience.com/applied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9?source=collection_archive---------6-----------------------#2023-01-02](https://towardsdatascience.com/applied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9?source=collection_archive---------6-----------------------#2023-01-02)

## 逐步学习 DQN 算法的行为，以及与之前的强化学习算法相比的改进

[](https://medium.com/@JavierMtz5?source=post_page-----8f0e38196ba9--------------------------------)[![哈维尔·马丁内斯·奥赫达](../Images/5b5df4220fa64c13232c29de9b4177af.png)](https://medium.com/@JavierMtz5?source=post_page-----8f0e38196ba9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8f0e38196ba9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8f0e38196ba9--------------------------------) [哈维尔·马丁内斯·奥赫达](https://medium.com/@JavierMtz5?source=post_page-----8f0e38196ba9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F74d7213a71a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fapplied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9&user=Javier+Mart%C3%ADnez+Ojeda&userId=74d7213a71a8&source=post_page-74d7213a71a8----8f0e38196ba9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8f0e38196ba9--------------------------------) ·7 分钟阅读·2023年1月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8f0e38196ba9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fapplied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9&user=Javier+Mart%C3%ADnez+Ojeda&userId=74d7213a71a8&source=-----8f0e38196ba9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8f0e38196ba9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fapplied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9&source=-----8f0e38196ba9---------------------bookmark_footer-----------)![](../Images/cc0aa599232d7c5acfafd8073654bc95.png)

照片由 [Андрей Сизов](https://unsplash.com/@alpridephoto?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 如果你想在没有 Premium Medium 账户的情况下阅读这篇文章，你可以通过这个朋友链接来阅读 :)
> 
> [https://www.learnml.wiki/applied-reinforcement-learning-iii-deep-q-networks-dqn/](https://www.learnml.wiki/applied-reinforcement-learning-iii-deep-q-networks-dqn/)

深度Q网络，最早由Mnih等人在2013年的论文**[1]**中报告，是迄今为止最著名的强化学习算法之一，由于其发布以来在无数Atari游戏中表现出超越人类的能力，如*图1*所示。

![](../Images/870e39adce5d7679af12de6a4e2dbcd3.png)

**图1**。每个代理在57款Atari游戏中的中位数人类标准化评分图。**图片摘自** [**deepmind/dqn_zoo GitHub 存储库**](https://github.com/deepmind/dqn_zoo)

此外，除了看到DQN代理像职业玩家一样玩这些Atari游戏的迷人之处外，DQN还解决了一个已经存在几十年的算法问题：Q学习，这在本系列的第一篇文章中已有介绍和解释。
