# 学习网络游戏

> 原文：[https://towardsdatascience.com/learning-network-games-29970aee44bb?source=collection_archive---------4-----------------------#2023-04-20](https://towardsdatascience.com/learning-network-games-29970aee44bb?source=collection_archive---------4-----------------------#2023-04-20)

## 图论 ML 遇上博弈论

## 网络游戏是一种强大的工具，用于建模在网络上进行的个体或组织之间的战略互动，其中玩家的收益不仅取决于他们自己的行为，还取决于邻居的行为。这类游戏在经济学和社会科学中有着广泛的应用，包括研究社交网络中的影响传播、金融市场的动态以及国际关系中的联盟形成。网络游戏的研究通常假设网络结构是已知的，这往往是不切实际的。最近，已经提出了 ML 方法，通过利用玩家观察到的行为来学习底层网络结构。在这篇博客文章中，我们提出了一种新颖的方法，使用类似 Transformer 的架构来推断游戏的网络结构，而无需明确知道与之相关的效用函数。

[](https://michael-bronstein.medium.com/?source=post_page-----29970aee44bb--------------------------------)[![Michael Bronstein](../Images/1aa876fce70bb07bef159fecb74e85bf.png)](https://michael-bronstein.medium.com/?source=post_page-----29970aee44bb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----29970aee44bb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----29970aee44bb--------------------------------) [Michael Bronstein](https://michael-bronstein.medium.com/?source=post_page-----29970aee44bb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b1129ddd572&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearning-network-games-29970aee44bb&user=Michael+Bronstein&userId=7b1129ddd572&source=post_page-7b1129ddd572----29970aee44bb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----29970aee44bb--------------------------------) ·10 分钟阅读·2023年4月20日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F29970aee44bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearning-network-games-29970aee44bb&source=-----29970aee44bb---------------------bookmark_footer-----------)
