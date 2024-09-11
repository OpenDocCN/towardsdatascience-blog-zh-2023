# 优化：Python中的容量限制设施选址问题

> 原文：[https://towardsdatascience.com/optimization-capacitated-facility-location-problem-in-python-57c08f259fe0?source=collection_archive---------2-----------------------#2023-02-28](https://towardsdatascience.com/optimization-capacitated-facility-location-problem-in-python-57c08f259fe0?source=collection_archive---------2-----------------------#2023-02-28)

## 找到最优的仓库数量和位置以降低成本并满足需求

[](https://nicolo-albanese.medium.com/?source=post_page-----57c08f259fe0--------------------------------)[![Nicolo Cosimo Albanese](../Images/9a2c26207146741b58c3742927d09450.png)](https://nicolo-albanese.medium.com/?source=post_page-----57c08f259fe0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----57c08f259fe0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----57c08f259fe0--------------------------------) [Nicolo Cosimo Albanese](https://nicolo-albanese.medium.com/?source=post_page-----57c08f259fe0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7430df412ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimization-capacitated-facility-location-problem-in-python-57c08f259fe0&user=Nicolo+Cosimo+Albanese&userId=7430df412ec&source=post_page-7430df412ec----57c08f259fe0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----57c08f259fe0--------------------------------) · 12 min read · 2023年2月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F57c08f259fe0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimization-capacitated-facility-location-problem-in-python-57c08f259fe0&user=Nicolo+Cosimo+Albanese&userId=7430df412ec&source=-----57c08f259fe0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F57c08f259fe0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimization-capacitated-facility-location-problem-in-python-57c08f259fe0&source=-----57c08f259fe0---------------------bookmark_footer-----------)![](../Images/cfab4658e033226dcba6aac3728625d1.png)

图片由作者提供。

# 目录

1.  [介绍](#f74a)

1.  [问题陈述](#bd29)

1.  [实现](#7bcf)

    3.1\. [数据集](#5775)

    3.2\. [客户、仓库与需求](#11c3)

    3.3\. [供应和固定成本](#6211)

    3.4\. [运输成本](#9569)

    3.5\. [优化](#35b1)

1.  [探索结果](#1790)

1.  [结论](#280b)

# 1\. 介绍

*设施选址问题（FLPs）* 是经典的优化任务。它们旨在确定仓库或工厂的最佳潜在位置。

仓库可能有或没有有限的容量。这将*有容量的（CFLP）*与*无容量的（UFLP）*问题变体区分开来。

商业目标是找到一组仓库位置，以最小化成本。原始问题定义由[Balinski (1965)](https://doi.org/10.1287/mnsc.12.3.253) 最小化两个（年度）成本项的总和：

+   运输成本

+   仓库固定成本

运输成本包括由从仓库位置到达客户所产生的费用。仓库固定成本……
