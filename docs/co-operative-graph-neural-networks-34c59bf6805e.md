# 协作图神经网络

> 原文：[https://towardsdatascience.com/co-operative-graph-neural-networks-34c59bf6805e?source=collection_archive---------4-----------------------#2023-12-06](https://towardsdatascience.com/co-operative-graph-neural-networks-34c59bf6805e?source=collection_archive---------4-----------------------#2023-12-06)

## 新的 GNN 架构

## ***大多数图神经网络（GNN）遵循消息传递范式，其中节点状态基于聚合的邻居消息进行更新。在这篇文章中，我们描述了协作 GNN（Co-GNNs），一种新的消息传递架构，其中每个节点被视为一个可以选择‘监听’、‘广播’、‘监听 & 广播’或‘隔离’的参与者。标准的消息传递是一种特殊情况，在这种情况下，每个节点‘监听 & 广播’给所有邻居。我们展示了 Co-GNNs 是异步的，更具表达力，并且可以解决标准消息传递 GNN 的常见困境，例如过度压缩和过度平滑。***

[](https://michael-bronstein.medium.com/?source=post_page-----34c59bf6805e--------------------------------)[![Michael Bronstein](../Images/1aa876fce70bb07bef159fecb74e85bf.png)](https://michael-bronstein.medium.com/?source=post_page-----34c59bf6805e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----34c59bf6805e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----34c59bf6805e--------------------------------) [Michael Bronstein](https://michael-bronstein.medium.com/?source=post_page-----34c59bf6805e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b1129ddd572&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fco-operative-graph-neural-networks-34c59bf6805e&user=Michael+Bronstein&userId=7b1129ddd572&source=post_page-7b1129ddd572----34c59bf6805e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----34c59bf6805e--------------------------------) ·11 分钟阅读·2023年12月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F34c59bf6805e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fco-operative-graph-neural-networks-34c59bf6805e&user=Michael+Bronstein&userId=7b1129ddd572&source=-----34c59bf6805e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F34c59bf6805e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fco-operative-graph-neural-networks-34c59bf6805e&source=-----34c59bf6805e---------------------bookmark_footer-----------)![](../Images/a2870ac967748470424f265c9e2a651e.png)

*Co-GNNs 中节点动作的示意图：标准、监听、广播和隔离。图片来源：DALL-E 3。*

这篇文章由 Ben Finkelshtein、Ismail Ceylan 和 Xingyue Huang 共同撰写，基于论文 B. Finkelshtein 等人， [Cooperative Graph Neural Networks](https://arxiv.org/abs/2310.01267) (2023) arXiv:2310.01267。
