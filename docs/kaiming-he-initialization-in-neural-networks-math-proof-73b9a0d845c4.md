# Kaiming He 初始化在神经网络中的应用 — 数学证明

> 原文：[https://towardsdatascience.com/kaiming-he-initialization-in-neural-networks-math-proof-73b9a0d845c4?source=collection_archive---------7-----------------------#2023-02-15](https://towardsdatascience.com/kaiming-he-initialization-in-neural-networks-math-proof-73b9a0d845c4?source=collection_archive---------7-----------------------#2023-02-15)

## 在具有 ReLU 激活函数的神经网络层中推导权重矩阵的最佳初始方差

[](https://medium.com/@EsterHlav?source=post_page-----73b9a0d845c4--------------------------------)[![Ester Hlav](../Images/5fe679b93c42d568d0d5b331a8bf92b9.png)](https://medium.com/@EsterHlav?source=post_page-----73b9a0d845c4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----73b9a0d845c4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----73b9a0d845c4--------------------------------) [Ester Hlav](https://medium.com/@EsterHlav?source=post_page-----73b9a0d845c4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7476ea235ae9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fkaiming-he-initialization-in-neural-networks-math-proof-73b9a0d845c4&user=Ester+Hlav&userId=7476ea235ae9&source=post_page-7476ea235ae9----73b9a0d845c4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----73b9a0d845c4--------------------------------) ·10 min read·2023年2月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F73b9a0d845c4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fkaiming-he-initialization-in-neural-networks-math-proof-73b9a0d845c4&user=Ester+Hlav&userId=7476ea235ae9&source=-----73b9a0d845c4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F73b9a0d845c4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fkaiming-he-initialization-in-neural-networks-math-proof-73b9a0d845c4&source=-----73b9a0d845c4---------------------bookmark_footer-----------)![](../Images/0e072a98caece16e7471a537515610cc.png)

初始化技术是成功训练深度学习架构的先决条件之一。传统上，权重初始化方法需要与激活函数的选择兼容，因为不匹配可能会对训练产生负面影响。

ReLU 是深度学习中最常用的激活函数之一。它的特性使其成为扩展到大型神经网络的非常便利的选择。一方面，由于它是一个线性函数且具有阶跃函数的导数，因此在反向传播过程中计算导数的成本很低。另一方面，ReLU 作为一种非负激活函数，有助于减少特征相关性，*即* 特征只能对后续层产生积极贡献。在卷积架构中，输入维度较大且神经网络往往非常深的情况下，ReLU 是一种常见的选择。

在*“深入探讨整流器：超越人类水平的 ImageNet 分类”* ⁽*¹* ⁾ 由 He *et al.*（2015年）中，作者提出了一种方法论，以使用 ReLU 激活函数来最佳初始化神经网络层。这…
