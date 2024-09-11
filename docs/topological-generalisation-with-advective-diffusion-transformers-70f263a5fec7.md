# 基于对流扩散Transformer的拓扑泛化

> 原文：[https://towardsdatascience.com/topological-generalisation-with-advective-diffusion-transformers-70f263a5fec7?source=collection_archive---------6-----------------------#2023-10-19](https://towardsdatascience.com/topological-generalisation-with-advective-diffusion-transformers-70f263a5fec7?source=collection_archive---------6-----------------------#2023-10-19)

## GNN中的拓扑泛化

## 在图神经网络（GNNs）的研究中，一个关键的开放问题是它们的**泛化能力**，尤其是在图的拓扑结构发生变化时。在这篇文章中，我们从图扩散方程的角度探讨了这一问题，这些方程与GNNs有着密切的关系，并且在过去曾被用作分析GNN动态、表达能力以及验证架构选择的框架。我们描述了一种基于**对流扩散**的新架构，该架构结合了消息传递神经网络（MPNNs）和Transformers的计算结构，并展现出优越的拓扑**泛化能力**。

[](https://michael-bronstein.medium.com/?source=post_page-----70f263a5fec7--------------------------------)[![迈克尔·布朗斯坦](../Images/1aa876fce70bb07bef159fecb74e85bf.png)](https://michael-bronstein.medium.com/?source=post_page-----70f263a5fec7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----70f263a5fec7--------------------------------)[![面向数据科学](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----70f263a5fec7--------------------------------) [迈克尔·布朗斯坦](https://michael-bronstein.medium.com/?source=post_page-----70f263a5fec7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b1129ddd572&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftopological-generalisation-with-advective-diffusion-transformers-70f263a5fec7&user=Michael+Bronstein&userId=7b1129ddd572&source=post_page-7b1129ddd572----70f263a5fec7---------------------post_header-----------) 发布于 [面向数据科学](https://towardsdatascience.com/?source=post_page-----70f263a5fec7--------------------------------) · 9 分钟阅读 · 2023年10月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F70f263a5fec7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftopological-generalisation-with-advective-diffusion-transformers-70f263a5fec7&user=Michael+Bronstein&userId=7b1129ddd572&source=-----70f263a5fec7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F70f263a5fec7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftopological-generalisation-with-advective-diffusion-transformers-70f263a5fec7&source=-----70f263a5fec7---------------------bookmark_footer-----------)![](../Images/4454e0295635120bca008b7c8f313e5a.png)

图片：Unsplash

*这篇文章由* [*Qitian Wu*](https://qitianwu.github.io/) *和* [*Chenxiao Yang*](https://chr26195.github.io/) *联合撰写，并基于Q. Wu等人的论文* [*Advective Diffusion Transformer for Topological Generalization in Graph Learning*](https://arxiv.org/abs/2310.06417) *(2023)*…
