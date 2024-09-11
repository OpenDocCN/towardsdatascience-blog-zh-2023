# 一种数据驱动的《大富翁》策略模拟

> 原文：[`towardsdatascience.com/a-data-driven-tactics-simulation-for-monopoly-864e7cffe508?source=collection_archive---------17-----------------------#2023-01-09`](https://towardsdatascience.com/a-data-driven-tactics-simulation-for-monopoly-864e7cffe508?source=collection_archive---------17-----------------------#2023-01-09)

## 使用成千上万次 MATLAB 模拟来主宰你的下一场《大富翁》游戏的视觉指南

[](https://medium.com/@Jake_Mitchell?source=post_page-----864e7cffe508--------------------------------)![Jake Mitchell](https://medium.com/@Jake_Mitchell?source=post_page-----864e7cffe508--------------------------------)[](https://towardsdatascience.com/?source=post_page-----864e7cffe508--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----864e7cffe508--------------------------------) [Jake Mitchell](https://medium.com/@Jake_Mitchell?source=post_page-----864e7cffe508--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3d9ea428abd9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-driven-tactics-simulation-for-monopoly-864e7cffe508&user=Jake+Mitchell&userId=3d9ea428abd9&source=post_page-3d9ea428abd9----864e7cffe508---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----864e7cffe508--------------------------------) ·7 min read·2023 年 1 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F864e7cffe508&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-driven-tactics-simulation-for-monopoly-864e7cffe508&user=Jake+Mitchell&userId=3d9ea428abd9&source=-----864e7cffe508---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F864e7cffe508&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-driven-tactics-simulation-for-monopoly-864e7cffe508&source=-----864e7cffe508---------------------bookmark_footer-----------)![](img/4cb410d22e0ec5c6fcc90cbdb4c9c39d.png)

图片来源：[Joshua Hoehne](https://unsplash.com/@mrthetrain?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 介绍：

在这篇文章的续集中，[模拟《大富翁》](https://medium.com/towards-data-science/simulating-monopoly-finding-the-best-properties-using-matlab-130fe557b1ae)，我将探讨玩家的购买策略以及这些策略如何影响游戏结果。我将通过在 MATLAB 中建模游戏，然后模拟成千上万场游戏，以寻找玩家选择中的模式。

## 游戏编码：

要模拟大富翁游戏，你只需模拟掷骰子并根据骰子的值移动玩家。然后根据他们落在什么类型的格子上应用一系列条件语句。如果他们落在一个房产上，要么决定购买该房产，要么向所有者支付租金。如果他们落在“去监狱”上，则将玩家的位置更改为“监狱”。这一部分在代码中相当直观，详细说明见我上面链接的上一篇文章。

对我来说最令人兴奋的代码是关于购买房产、购买房屋/酒店和交易的决策。当玩家决定购买房产时，会考虑两个因素：

+   **价格比率**——玩家的钱除以...
