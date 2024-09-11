# 递归神经网络，基础讲解与可视化

> 原文：[https://towardsdatascience.com/recurrent-neural-networks-explained-and-visualized-from-the-ground-up-51c023f2b6fe?source=collection_archive---------3-----------------------#2023-06-22](https://towardsdatascience.com/recurrent-neural-networks-explained-and-visualized-from-the-ground-up-51c023f2b6fe?source=collection_archive---------3-----------------------#2023-06-22)

![](../Images/0b3a777d2e37126f432db24797c1b6ba.png)

[Unsplash](https://unsplash.com/photos/rSrK-P0Wips)

## 应用于机器翻译

[](https://andre-ye.medium.com/?source=post_page-----51c023f2b6fe--------------------------------)[![Andre Ye](../Images/c69b022a665d3ee13f9fec2f1f73bf32.png)](https://andre-ye.medium.com/?source=post_page-----51c023f2b6fe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----51c023f2b6fe--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----51c023f2b6fe--------------------------------) [Andre Ye](https://andre-ye.medium.com/?source=post_page-----51c023f2b6fe--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbe743a65b006&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frecurrent-neural-networks-explained-and-visualized-from-the-ground-up-51c023f2b6fe&user=Andre+Ye&userId=be743a65b006&source=post_page-be743a65b006----51c023f2b6fe---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----51c023f2b6fe--------------------------------) · 23分钟阅读·2023年6月22日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F51c023f2b6fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frecurrent-neural-networks-explained-and-visualized-from-the-ground-up-51c023f2b6fe&user=Andre+Ye&userId=be743a65b006&source=-----51c023f2b6fe---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F51c023f2b6fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frecurrent-neural-networks-explained-and-visualized-from-the-ground-up-51c023f2b6fe&source=-----51c023f2b6fe---------------------bookmark_footer-----------)

递归神经网络（RNN）是可以顺序操作的神经网络。尽管它们现在不如几年前那么流行，但它们仍然在深度学习的发展中代表了一个重要的进步，并且是前馈网络的自然延伸。

在这篇文章中，我们将涵盖以下内容：

+   从前馈网络到递归网络的过渡

+   多层递归网络

+   长短期记忆网络（LSTM）

+   顺序输出（‘文本输出’）

+   双向性

+   自回归生成

+   机器翻译的应用（对Google Translate 2016年模型架构的高层次理解）

这篇文章的目的不仅仅是解释RNN的工作原理（这方面已有很多文章），而是通过插图深入探讨它们的设计选择和高层次的直观逻辑。希望这篇文章不仅能为你对这一特定技术话题的理解提供一些独特的价值，还能更普遍地提升你对深度学习设计灵活性的理解。
