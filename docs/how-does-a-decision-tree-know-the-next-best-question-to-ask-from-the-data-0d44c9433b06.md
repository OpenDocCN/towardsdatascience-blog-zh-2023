# 决策树如何知道从数据中问下一个最佳问题？

> 原文：[https://towardsdatascience.com/how-does-a-decision-tree-know-the-next-best-question-to-ask-from-the-data-0d44c9433b06?source=collection_archive---------9-----------------------#2023-11-17](https://towardsdatascience.com/how-does-a-decision-tree-know-the-next-best-question-to-ask-from-the-data-0d44c9433b06?source=collection_archive---------9-----------------------#2023-11-17)

## 构建你自己的决策树分类器（用Python从零开始）并了解它如何使用熵来分割节点

[](https://medium.com/@gurjinderkaur95?source=post_page-----0d44c9433b06--------------------------------)[![Gurjinder Kaur](../Images/d5c6746466025dad06077b1a89a789d1.png)](https://medium.com/@gurjinderkaur95?source=post_page-----0d44c9433b06--------------------------------)[](https://towardsdatascience.com/?source=post_page-----0d44c9433b06--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----0d44c9433b06--------------------------------) [Gurjinder Kaur](https://medium.com/@gurjinderkaur95?source=post_page-----0d44c9433b06--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F79ee1ef48e0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-does-a-decision-tree-know-the-next-best-question-to-ask-from-the-data-0d44c9433b06&user=Gurjinder+Kaur&userId=79ee1ef48e0c&source=post_page-79ee1ef48e0c----0d44c9433b06---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----0d44c9433b06--------------------------------) · 14分钟阅读 · 2023年11月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F0d44c9433b06&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-does-a-decision-tree-know-the-next-best-question-to-ask-from-the-data-0d44c9433b06&user=Gurjinder+Kaur&userId=79ee1ef48e0c&source=-----0d44c9433b06---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F0d44c9433b06&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-does-a-decision-tree-know-the-next-best-question-to-ask-from-the-data-0d44c9433b06&source=-----0d44c9433b06---------------------bookmark_footer-----------)![](../Images/4f9f5b9174a45c2bfe56d92a8282fcbc.png)

照片由 [Daniele Levis Pelusi](https://unsplash.com/@yogidan2012?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 介绍

决策树是一种多才多艺的机器学习算法，可以执行分类和回归任务。它们通过根据数据的特征提出问题来做出决策，使用IF-ELSE结构跟随路径，最终导致最终预测。挑战在于找出在决策过程的每一步中应该提出什么问题，这也等同于询问如何确定每个决策节点上的最佳分裂。

在本文中，我们将尝试为一个简单的二元分类任务构建决策树。本文的目标是理解如何在每个节点使用不纯度度量（*例如熵*）来确定最佳分裂，最终构建一种树形结构，该结构使用基于规则的方法来得出最终预测。

要对熵和基尼不纯度（另一种用于衡量随机性并确定决策树分裂质量的指标）背后的直觉有所了解，快速查看这篇[文章](https://medium.com/@gurjinderkaur95/entropy-and-gini-index-c04b7452efbe)。

## 问题定义和…
