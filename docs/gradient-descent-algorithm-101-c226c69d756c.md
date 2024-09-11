# 梯度下降算法 101

> 原文：[`towardsdatascience.com/gradient-descent-algorithm-101-c226c69d756c?source=collection_archive---------18-----------------------#2023-04-25`](https://towardsdatascience.com/gradient-descent-algorithm-101-c226c69d756c?source=collection_archive---------18-----------------------#2023-04-25)

## 初学者友好的指南

## 了解在机器学习和深度学习中广泛使用的优化算法

[](https://polmarin.medium.com/?source=post_page-----c226c69d756c--------------------------------)![Pol Marin](https://polmarin.medium.com/?source=post_page-----c226c69d756c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c226c69d756c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c226c69d756c--------------------------------) [Pol Marin](https://polmarin.medium.com/?source=post_page-----c226c69d756c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fa43cc443e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-descent-algorithm-101-c226c69d756c&user=Pol+Marin&userId=1fa43cc443e7&source=post_page-1fa43cc443e7----c226c69d756c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c226c69d756c--------------------------------) · 6 min read · 2023 年 4 月 25 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc226c69d756c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-descent-algorithm-101-c226c69d756c&user=Pol+Marin&userId=1fa43cc443e7&source=-----c226c69d756c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc226c69d756c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-descent-algorithm-101-c226c69d756c&source=-----c226c69d756c---------------------bookmark_footer-----------)![](img/a718894153ffc2de3d1b8a5ecdc19786.png)

山坡 — 摄影：[Ralph (Ravi) Kayden](https://unsplash.com/fr/@ralphkayden?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上

想象一下你是一滴水，位于山顶，你的目标是到达山脚下的湖泊。那座高山有不同的坡度和障碍，因此沿着直线向下可能不是最佳解决方案。你会如何处理这个问题？最好的解决方案可能是一步步前进，每次都朝着能让你更接近最终目标的方向前进。

梯度下降（GD）就是执行这些操作的算法，任何数据科学家都必须理解它。它很基础且相当简单，但至关重要，任何想要进入这一领域的人都应该能够解释它是什么。

在这篇文章中，我的目标是制作一个完整且适合初学者的指南，以帮助每个人理解什么是 GD，它的用途是什么，它是如何工作的，并提及它的不同变体。

一如既往，你将在文章末尾找到*资源*部分。

首先，必须处理最重要的事情。

## 介绍

根据维基百科的定义[1]，**梯度下降是一种一阶迭代**…
