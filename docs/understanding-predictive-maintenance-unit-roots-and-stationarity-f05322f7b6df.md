# 理解预测性维护 — 单位根和稳定性

> 原文：[https://towardsdatascience.com/understanding-predictive-maintenance-unit-roots-and-stationarity-f05322f7b6df?source=collection_archive---------5-----------------------#2023-11-13](https://towardsdatascience.com/understanding-predictive-maintenance-unit-roots-and-stationarity-f05322f7b6df?source=collection_archive---------5-----------------------#2023-11-13)

[](https://marcin-staskopl.medium.com/?source=post_page-----f05322f7b6df--------------------------------)[![Marcin Stasko](../Images/5142b9a260a1cce7c6a2ebcc16f46fbb.png)](https://marcin-staskopl.medium.com/?source=post_page-----f05322f7b6df--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f05322f7b6df--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f05322f7b6df--------------------------------) [Marcin Stasko](https://marcin-staskopl.medium.com/?source=post_page-----f05322f7b6df--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd8736adba55&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-predictive-maintenance-unit-roots-and-stationarity-f05322f7b6df&user=Marcin+Stasko&userId=d8736adba55&source=post_page-d8736adba55----f05322f7b6df---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f05322f7b6df--------------------------------) · 13分钟阅读 · 2023年11月13日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff05322f7b6df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-predictive-maintenance-unit-roots-and-stationarity-f05322f7b6df&user=Marcin+Stasko&userId=d8736adba55&source=-----f05322f7b6df---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff05322f7b6df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-predictive-maintenance-unit-roots-and-stationarity-f05322f7b6df&source=-----f05322f7b6df---------------------bookmark_footer-----------)![](../Images/2fe2b9db20b9852d4ce6f3bfb405bd63.png)

图片由 [Emma Gossett](https://unsplash.com/@emmagossett?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 文章目的

在本文中，我们将深入探讨单位根和稳定性的关键概念。请做好准备，探索为何检查稳定性至关重要、单位根是什么，以及这些因素如何在我们的预测性维护工具中发挥关键作用。我们还将掌握混沌！

本文是《理解预测性维护》系列的一部分。我计划以类似风格创建整个系列。

[查看完整系列内容](https://marcin-staskopl.medium.com/list/understanding-predictive-maintenance-series-e1f44d8a0cc3)。确保通过关注我来不错过新文章。

# 数据平稳性——捉迷藏的分析游戏

![](../Images/9cb429f26f1318dd3344ef4e4f314a93.png)

图片来源于[Mitchell Luo](https://unsplash.com/@mitchel3uo?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

是否曾想过你的数据是否在玩捉迷藏？直截了当——我们说的是平稳性。这不仅仅是一个花哨的术语，它是理解你的时间相关数据有多稳定和可预测的秘诀。系好安全带，我们来深入探讨为什么数据平稳性在建模和预测中是颠覆性的。

## 平稳性的关键规则
