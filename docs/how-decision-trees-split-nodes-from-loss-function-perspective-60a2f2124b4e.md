# 从损失函数的角度看决策树如何分裂节点

> 原文：[`towardsdatascience.com/how-decision-trees-split-nodes-from-loss-function-perspective-60a2f2124b4e?source=collection_archive---------6-----------------------#2023-05-15`](https://towardsdatascience.com/how-decision-trees-split-nodes-from-loss-function-perspective-60a2f2124b4e?source=collection_archive---------6-----------------------#2023-05-15)

## 了解决策树如何仅通过最小化其损失函数来分裂节点

[](https://jasonweiyi.medium.com/?source=post_page-----60a2f2124b4e--------------------------------)![Wei Yi](https://jasonweiyi.medium.com/?source=post_page-----60a2f2124b4e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----60a2f2124b4e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----60a2f2124b4e--------------------------------) [Wei Yi](https://jasonweiyi.medium.com/?source=post_page-----60a2f2124b4e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1b4bd5317a6e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-decision-trees-split-nodes-from-loss-function-perspective-60a2f2124b4e&user=Wei+Yi&userId=1b4bd5317a6e&source=post_page-1b4bd5317a6e----60a2f2124b4e---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----60a2f2124b4e--------------------------------) ·12 分钟阅读·2023 年 5 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F60a2f2124b4e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-decision-trees-split-nodes-from-loss-function-perspective-60a2f2124b4e&user=Wei+Yi&userId=1b4bd5317a6e&source=-----60a2f2124b4e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F60a2f2124b4e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-decision-trees-split-nodes-from-loss-function-perspective-60a2f2124b4e&source=-----60a2f2124b4e---------------------bookmark_footer-----------)![](img/de317baa764ffb421a758d3453dbd4f5.png)

[Ernest Brillo](https://unsplash.com/@ernest_brillo?utm_source=medium&utm_medium=referral)拍摄的照片，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上

在谈到决策树节点分裂时，我经常听到“方差减少”和“信息增益最大化”等词汇。每次听到这些词汇我都会感到恐惧。这些并不是我日常词汇中常用的术语，直到我意识到这些术语实际上是“最小化决策树损失函数”的同义词。

哪种损失函数？嗯，每个机器学习模型都需要一个损失函数。损失函数量化了模型预测与实际结果之间的*差异*。另一个术语是模型预测中的*误差*。决策树模型也不例外。

对我来说，理解损失函数是理解机器学习模型的最重要一步。这是因为损失函数用一个单一的数字来编码我们希望模型擅长的所有内容。就像我们用一个单一的数字来量化一个人的优点；我记得那是某种长度的度量，对吧，眨眨眼？

由于损失函数测量的是树的预测与实际结果之间的差异，要理解损失函数，我们首先需要了解决策树如何进行预测……
