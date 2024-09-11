# 装袋法是否有助于防止决策树的过拟合？

> 原文：[`towardsdatascience.com/does-bagging-help-to-prevent-overfitting-in-decision-trees-42262943a81e?source=collection_archive---------5-----------------------#2023-12-13`](https://towardsdatascience.com/does-bagging-help-to-prevent-overfitting-in-decision-trees-42262943a81e?source=collection_archive---------5-----------------------#2023-12-13)

## 理解为何决策树容易出现过拟合以及可能的解决办法

[](https://medium.com/@gurjinderkaur95?source=post_page-----42262943a81e--------------------------------)![Gurjinder Kaur](https://medium.com/@gurjinderkaur95?source=post_page-----42262943a81e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----42262943a81e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----42262943a81e--------------------------------) [Gurjinder Kaur](https://medium.com/@gurjinderkaur95?source=post_page-----42262943a81e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F79ee1ef48e0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdoes-bagging-help-to-prevent-overfitting-in-decision-trees-42262943a81e&user=Gurjinder+Kaur&userId=79ee1ef48e0c&source=post_page-79ee1ef48e0c----42262943a81e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----42262943a81e--------------------------------) · 12 分钟阅读 · 2023 年 12 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F42262943a81e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdoes-bagging-help-to-prevent-overfitting-in-decision-trees-42262943a81e&user=Gurjinder+Kaur&userId=79ee1ef48e0c&source=-----42262943a81e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F42262943a81e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdoes-bagging-help-to-prevent-overfitting-in-decision-trees-42262943a81e&source=-----42262943a81e---------------------bookmark_footer-----------)![](img/49dc05d1ffe1c84260864fe2ab15c16c.png)

图片来源：[Jan Huber](https://unsplash.com/@jan_huber?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

决策树是一类广为人知的机器学习算法，因其能够解决分类和回归问题以及提供易于解释的结果而受到关注。然而，它们容易出现过拟合，并且如果控制不当，可能会无法很好地推广。

在这篇文章中，我们将讨论什么是过拟合，决策树在多大程度上会过拟合训练数据，为什么这是一个问题，以及如何解决它。

然后，我们将了解一种集成技术，即 *袋装法*（bagging），并查看它是否可以用于增强决策树的鲁棒性。

我们将涵盖以下内容：

+   使用 NumPy 创建我们的回归数据集。

+   使用 scikit-learn 训练一个决策树模型。

+   通过观察同一模型在训练集和测试集上的表现，理解什么是过拟合。

+   讨论为什么过拟合在非参数模型如决策树中更为常见（并且当然要了解这个术语的含义）。
