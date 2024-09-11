# 零和博弈和混合策略

> 原文：[https://towardsdatascience.com/zero-sum-games-and-mixed-strategies-21c158aecf07?source=collection_archive---------15-----------------------#2023-01-06](https://towardsdatascience.com/zero-sum-games-and-mixed-strategies-21c158aecf07?source=collection_archive---------15-----------------------#2023-01-06)

## 建模战略互动要求我们考虑不确定性

[](https://michaelkingston-49283.medium.com/?source=post_page-----21c158aecf07--------------------------------)[![Michael Kingston](../Images/6e4cd37ec7f13029c300b66d61c5f793.png)](https://michaelkingston-49283.medium.com/?source=post_page-----21c158aecf07--------------------------------)[](https://towardsdatascience.com/?source=post_page-----21c158aecf07--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----21c158aecf07--------------------------------) [Michael Kingston](https://michaelkingston-49283.medium.com/?source=post_page-----21c158aecf07--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F286c31a3cb65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fzero-sum-games-and-mixed-strategies-21c158aecf07&user=Michael+Kingston&userId=286c31a3cb65&source=post_page-286c31a3cb65----21c158aecf07---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----21c158aecf07--------------------------------) ·12 min 阅读·2023年1月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F21c158aecf07&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fzero-sum-games-and-mixed-strategies-21c158aecf07&user=Michael+Kingston&userId=286c31a3cb65&source=-----21c158aecf07---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F21c158aecf07&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fzero-sum-games-and-mixed-strategies-21c158aecf07&source=-----21c158aecf07---------------------bookmark_footer-----------)

我在早期文章中重复了许多关于为什么我认为博弈论对数据科学至关重要的陈词滥调。我只是想重复我最喜欢的一句话：*“博弈论就像是带有激励的概率论”*。

如果你感兴趣，我早期的一些关于博弈论的故事可以在这里找到：

+   博弈论：基础知识

[](/game-theory-666abe63215e?source=post_page-----21c158aecf07--------------------------------) [## 博弈论

### 基础知识：同时行动博弈

towardsdatascience.com](/game-theory-666abe63215e?source=post_page-----21c158aecf07--------------------------------)

+   纳什均衡简介

[](https://builtin.com/data-science/nash-equilibrium?source=post_page-----21c158aecf07--------------------------------) [## 纳什均衡简介

### 纳什均衡是博弈论中的一个策略配置，其中没有玩家拥有主导策略。每个玩家…

builtin.com](https://builtin.com/data-science/nash-equilibrium?source=post_page-----21c158aecf07--------------------------------)

+   纳什均衡的应用

[](/applications-of-the-nash-equilibrium-5538be8dd443?source=post_page-----21c158aecf07--------------------------------) [## 纳什均衡的应用

### 贝特朗、库尔诺和霍特林

towardsdatascience.com](/applications-of-the-nash-equilibrium-5538be8dd443?source=post_page-----21c158aecf07--------------------------------)

+   博弈论中的主导策略解释
