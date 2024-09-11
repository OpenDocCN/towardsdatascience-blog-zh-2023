# 变量、协方差、相关性和因果关系的视觉解释

> 原文：[`towardsdatascience.com/a-visual-explanation-of-variance-covariance-correlation-and-causation-dcf762801029?source=collection_archive---------3-----------------------#2023-02-10`](https://towardsdatascience.com/a-visual-explanation-of-variance-covariance-correlation-and-causation-dcf762801029?source=collection_archive---------3-----------------------#2023-02-10)

![](img/eff5af349b9566526854900300e03652.png)

[Lucas Kapla](https://unsplash.com/@aznbokchoy?utm_source=medium&utm_medium=referral) 摄影，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 通过理解基本统计概念提高你的数据分析技能

[](https://medium.com/@marcellopoliti?source=post_page-----dcf762801029--------------------------------)![Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----dcf762801029--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dcf762801029--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dcf762801029--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----dcf762801029--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-visual-explanation-of-variance-covariance-correlation-and-causation-dcf762801029&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----dcf762801029---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dcf762801029--------------------------------) · 6 分钟阅读 · 2023 年 2 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdcf762801029&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-visual-explanation-of-variance-covariance-correlation-and-causation-dcf762801029&user=Marcello+Politi&userId=7390355d40fe&source=-----dcf762801029---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdcf762801029&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-visual-explanation-of-variance-covariance-correlation-and-causation-dcf762801029&source=-----dcf762801029---------------------bookmark_footer-----------)

## 介绍

在机器学习中，我们通常对数据之间存在的关系感兴趣。假设你有一个结构化的数据集，以 DataFrame 中的表格形式表示。你想知道是否有任何列之间存在关系，可能是为了了解是否有冗余信息。知道某个变量是否是另一个变量的原因也会很有用。让我们在本文中探讨如何做到这一点！

## 方差

现在让我们假设我们有一个一维的数据集（一个点集）。我们想做的第一件事是计算这些点的方差。

方差直观地告诉我们数据点距离均值的远近。例如，如果我吃了一个冰淇淋，你也吃了一个，平均来说我们吃的冰淇淋数量是相同的，即一个。但即使我吃了两个冰淇淋而你吃了零个，平均来说我们每个人仍然吃了一个冰淇淋。变化的是方差！

在我们的数据集中，均值是：*m = 1*

要计算方差，我们首先计算每个点距离均值的距离。
