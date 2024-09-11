# 帕累托、幂律和尾部厚尾现象

> 原文：[https://towardsdatascience.com/pareto-power-laws-and-fat-tails-0355a187ee6a?source=collection_archive---------2-----------------------#2023-11-11](https://towardsdatascience.com/pareto-power-laws-and-fat-tails-0355a187ee6a?source=collection_archive---------2-----------------------#2023-11-11)

## *统计学不会教给你的*

[](https://shawhin.medium.com/?source=post_page-----0355a187ee6a--------------------------------)[![Shaw Talebi](../Images/1449cc7c08890e2078f9e5d07897e3df.png)](https://shawhin.medium.com/?source=post_page-----0355a187ee6a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----0355a187ee6a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----0355a187ee6a--------------------------------) [Shaw Talebi](https://shawhin.medium.com/?source=post_page-----0355a187ee6a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff3998e1cd186&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpareto-power-laws-and-fat-tails-0355a187ee6a&user=Shaw+Talebi&userId=f3998e1cd186&source=post_page-f3998e1cd186----0355a187ee6a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----0355a187ee6a--------------------------------) ·12 分钟阅读·2023年11月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F0355a187ee6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpareto-power-laws-and-fat-tails-0355a187ee6a&user=Shaw+Talebi&userId=f3998e1cd186&source=-----0355a187ee6a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F0355a187ee6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpareto-power-laws-and-fat-tails-0355a187ee6a&source=-----0355a187ee6a---------------------bookmark_footer-----------)![](../Images/f585c7dbe469709a07fe0df37d23409f.png)

一只黑天鹅。图片来自 Canva。

统计学是数据科学和分析的基石。它为我们提供了一个强大的工具箱，以客观地回答复杂问题。然而，当应用于特定类型的数据 —— 幂律时，我们许多喜爱的统计工具就变得无用了。

在本文中，我将为初学者提供关于幂律的友好指南，并描述使用传统统计方法分析它们时的三个主要问题。

## 目录

1.  **背景** — *高斯分布、帕累托80-20法则、幂律，以及权重与财富的区别*。

1.  **统计101的3个问题** — *你需要（更多）的数据*。

1.  **尾部厚尾现象** — *避免争议和量化高斯与帕累托之间的差距*。

# **称重你的咖啡师**

自然界中许多量 tend to clump around a typical value.
