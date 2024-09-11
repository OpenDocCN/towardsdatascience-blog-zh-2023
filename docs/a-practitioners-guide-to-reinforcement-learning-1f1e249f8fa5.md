# 强化学习实践指南

> 原文：[https://towardsdatascience.com/a-practitioners-guide-to-reinforcement-learning-1f1e249f8fa5?source=collection_archive---------2-----------------------#2023-11-18](https://towardsdatascience.com/a-practitioners-guide-to-reinforcement-learning-1f1e249f8fa5?source=collection_archive---------2-----------------------#2023-11-18)

## [强化学习](https://medium.com/tag/reinforcement-learning)

## 迈出编写制胜AI代理的第一步

[](https://dr-robert-kuebler.medium.com/?source=post_page-----1f1e249f8fa5--------------------------------)[![Dr. Robert Kübler](../Images/3b8d8b88f76c0c43d9c305e3885e7ab9.png)](https://dr-robert-kuebler.medium.com/?source=post_page-----1f1e249f8fa5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1f1e249f8fa5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1f1e249f8fa5--------------------------------) [Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----1f1e249f8fa5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d6b5fb431bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-practitioners-guide-to-reinforcement-learning-1f1e249f8fa5&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=post_page-6d6b5fb431bf----1f1e249f8fa5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1f1e249f8fa5--------------------------------) · 15分钟阅读 · 2023年11月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1f1e249f8fa5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-practitioners-guide-to-reinforcement-learning-1f1e249f8fa5&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=-----1f1e249f8fa5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1f1e249f8fa5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-practitioners-guide-to-reinforcement-learning-1f1e249f8fa5&source=-----1f1e249f8fa5---------------------bookmark_footer-----------)![](../Images/93f09881c02997c942a1dd69ddc3b8eb.png)

照片由 [Vincent Guth](https://unsplash.com/@vingtcent?utm_source=medium&utm_medium=referral) 提供，刊登于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在机器学习领域，数据科学家主要在监督学习和无监督学习的领域中航行。然而，还有一个独特而有趣的子领域——**强化学习**！

在强化学习中，我们试图教一个所谓的**代理**如何在*游戏*的复杂性中导航，将它置于一个模拟环境中，在那里它探索策略，成功的举动会获得奖励，而失误则会受到惩罚。

![](../Images/b539adcd0ea6511de8d0f141fb96c4dd.png)

强化学习的典型概述。图片由作者提供。

强化学习领域的一个显著成果是 [AlphaGo](https://en.wikipedia.org/wiki/AlphaGo)，这是一个击败了世界围棋冠军的模型。围棋是一种比国际象棋更复杂的游戏。

> 强化学习的伟大之处在于我们不需要告诉代理**如何**获胜。我们只需告诉它获胜或失败的标准。

例如，在国际象棋中，就是将对方的国王将死，而这就是我们提供的唯一指导……
