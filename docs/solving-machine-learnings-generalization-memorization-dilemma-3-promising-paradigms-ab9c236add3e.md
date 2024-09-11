# 像我们一样学习的机器：解决泛化-记忆化困境

> 原文：[https://towardsdatascience.com/solving-machine-learnings-generalization-memorization-dilemma-3-promising-paradigms-ab9c236add3e?source=collection_archive---------9-----------------------#2023-04-05](https://towardsdatascience.com/solving-machine-learnings-generalization-memorization-dilemma-3-promising-paradigms-ab9c236add3e?source=collection_archive---------9-----------------------#2023-04-05)

## 解决机器学习中最重要问题之一的3种范式

[](https://medium.com/@samuel.flender?source=post_page-----ab9c236add3e--------------------------------)[![Samuel Flender](../Images/390d82a673de8a8bb11cef66978269b5.png)](https://medium.com/@samuel.flender?source=post_page-----ab9c236add3e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ab9c236add3e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ab9c236add3e--------------------------------) [Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----ab9c236add3e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce56d9dcd568&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-machine-learnings-generalization-memorization-dilemma-3-promising-paradigms-ab9c236add3e&user=Samuel+Flender&userId=ce56d9dcd568&source=post_page-ce56d9dcd568----ab9c236add3e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab9c236add3e--------------------------------) ·7分钟阅读·2023年4月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fab9c236add3e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-machine-learnings-generalization-memorization-dilemma-3-promising-paradigms-ab9c236add3e&user=Samuel+Flender&userId=ce56d9dcd568&source=-----ab9c236add3e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fab9c236add3e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-machine-learnings-generalization-memorization-dilemma-3-promising-paradigms-ab9c236add3e&source=-----ab9c236add3e---------------------bookmark_footer-----------)![](../Images/d07a01c4830a1e5c49f4bbf135d147d5.png)

(Midjourney)

机器学习的“圣杯”是构建能够同时记忆训练数据中的已知模式以及对实际环境中的未知模式进行泛化的系统。

这项“圣杯”因为这也是我们人类的学习方式。你可以在一张老照片中认出你的奶奶，但即使你以前从未见过Xoloitzcuintli犬，你也可以将其识别为狗。如果没有记忆，我们就

传统的统计学习理论告诉我们这是不可能的：模型要么很好地泛化，要么很好地记忆，但不能两者兼顾。这就是著名的偏差-方差权衡，是我们在标准机器学习课程中首先学习的内容之一。

那么我们如何构建这样的通用学习系统呢？这项“圣杯”是否触手可及？

在这篇文章中，让我们深入探讨文献中的三个范式，

1.  先泛化，再记忆

1.  同时泛化和记忆
