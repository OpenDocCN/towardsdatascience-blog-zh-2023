# 方向改善图学习

> 原文：[https://towardsdatascience.com/direction-improves-graph-learning-170e797e94fe?source=collection_archive---------2-----------------------#2023-06-08](https://towardsdatascience.com/direction-improves-graph-learning-170e797e94fe?source=collection_archive---------2-----------------------#2023-06-08)

## **GNNs 在有向图上的应用**

## 聪明地利用方向在异质图上进行消息传递可以带来非常显著的提升。

[](https://michael-bronstein.medium.com/?source=post_page-----170e797e94fe--------------------------------)[![Michael Bronstein](../Images/1aa876fce70bb07bef159fecb74e85bf.png)](https://michael-bronstein.medium.com/?source=post_page-----170e797e94fe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----170e797e94fe--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----170e797e94fe--------------------------------) [Michael Bronstein](https://michael-bronstein.medium.com/?source=post_page-----170e797e94fe--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b1129ddd572&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdirection-improves-graph-learning-170e797e94fe&user=Michael+Bronstein&userId=7b1129ddd572&source=post_page-7b1129ddd572----170e797e94fe---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----170e797e94fe--------------------------------) ·10 min read·2023年6月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F170e797e94fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdirection-improves-graph-learning-170e797e94fe&user=Michael+Bronstein&userId=7b1129ddd572&source=-----170e797e94fe---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F170e797e94fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdirection-improves-graph-learning-170e797e94fe&source=-----170e797e94fe---------------------bookmark_footer-----------)

*图神经网络（GNNs）在建模关系数据方面非常有效。然而，当前的 GNN 模型经常假设输入图为无向图，忽视了许多现实世界图（如社交、交通、交易和引用网络）固有的方向性。在这篇博客文章中，我们探讨了边的方向性在异质图中的影响，并概述了 Dir-GNN，一种用于有向图的全新消息传递方案，允许对进出边进行单独汇聚。尽管其方案简单，但在多个现实世界异质有向图上的性能显著提升。*

![](../Images/f963a2136e394c365df0a6f3421821bb.png)

基于 Shutterstock。

*这篇文章由* [*Emanuele Rossi*](https://emanuelerossi.co.uk/) *共同撰写，并基于 E. Rossi 等人的论文“*[*边缘方向性提高异质图学习效果*](https://arxiv.org/abs/2305.10498)*” (2023) arXiv:2305.10498，该论文与* [*Bertrand Charpentier*](https://twitter.com/Bertrand_Charp)*,* [*Francesco Di Giovanni*](https://twitter.com/Francesco_dgv)*,* [*Fabrizio Frasca*](https://twitter.com/ffabffrasca) *和* [*Stephan Günnemann*](https://twitter.com/guennemann) *[1] 合作完成。论文的代码可以在* [*这里*](https://github.com/emalgorithm/directed-graph-neural-network)*找到。*

许多有趣的真实世界图在建模社交、交通、金融交易等方面遇到过……
