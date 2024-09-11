# 2023 年 10 个令人困惑的 XGBoost 超参数及其专业调优方法

> 原文：[https://towardsdatascience.com/10-confusing-xgboost-hyperparameters-and-how-to-tune-them-like-a-pro-in-2023-e305057f546?source=collection_archive---------0-----------------------#2023-06-11](https://towardsdatascience.com/10-confusing-xgboost-hyperparameters-and-how-to-tune-them-like-a-pro-in-2023-e305057f546?source=collection_archive---------0-----------------------#2023-06-11)

## XGBoost 超参数的风格与视觉效果

[](https://ibexorigin.medium.com/?source=post_page-----e305057f546--------------------------------)[![Bex T.](../Images/516496f32596e8ad56bf07f178a643c6.png)](https://ibexorigin.medium.com/?source=post_page-----e305057f546--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e305057f546--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e305057f546--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----e305057f546--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-confusing-xgboost-hyperparameters-and-how-to-tune-them-like-a-pro-in-2023-e305057f546&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----e305057f546---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e305057f546--------------------------------) · 9 分钟阅读 · 2023 年 6 月 11 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe305057f546&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-confusing-xgboost-hyperparameters-and-how-to-tune-them-like-a-pro-in-2023-e305057f546&user=Bex+T.&userId=39db050c2ac2&source=-----e305057f546---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe305057f546&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-confusing-xgboost-hyperparameters-and-how-to-tune-them-like-a-pro-in-2023-e305057f546&source=-----e305057f546---------------------bookmark_footer-----------)![](../Images/4e9f3c8752e76146b330a1b3b4bdf911.png)

图片由我使用 Midjourney 制作

## 介绍

今天，我将展示如何将 XGBoost 调优到极致，直到两个‘o’都弹出来。我们将通过精细调整其超参数来实现这一点，以至于它在给我们提供所有性能后，将再也无法*bst*。

这不仅仅是一篇超参数检查清单的文章。哦不。我将详细解释每一个十个超参数的功能、接受的值范围、最佳实践，以及如何使用 Optuna 进行超参数调优。

让我们深入探讨吧！

## 我们一直想要的……

一个愚蠢的欠拟合XGBoost模型几乎前所未闻。即使使用默认参数值，它在许多表格任务上表现也相当不错。然而，它最大的问题在于过度拟合。

为了解决这个问题，大多数XGBoost超参数的设置是为了驯服这个潜在的“怪物”，避免它仅仅吞噬训练集而在测试时只吐出骨头。

因此，通过超参数调优，我们的目标是找到复杂模型过拟合和…之间的最佳平衡。
