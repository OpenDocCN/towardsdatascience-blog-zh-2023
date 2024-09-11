# 估计总体实验影响

> 原文：[`towardsdatascience.com/estimating-total-experimentation-impact-ab6cd56bffb?source=collection_archive---------7-----------------------#2023-09-20`](https://towardsdatascience.com/estimating-total-experimentation-impact-ab6cd56bffb?source=collection_archive---------7-----------------------#2023-09-20)

## 如何在测量组织总体影响时控制虚假发现和选择偏倚

[](https://dataneversleeps.medium.com/?source=post_page-----ab6cd56bffb--------------------------------)![贾里德·M·马鲁斯金博士](https://dataneversleeps.medium.com/?source=post_page-----ab6cd56bffb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ab6cd56bffb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab6cd56bffb--------------------------------) [贾里德·M·马鲁斯金博士](https://dataneversleeps.medium.com/?source=post_page-----ab6cd56bffb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F37ef2450ad04&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Festimating-total-experimentation-impact-ab6cd56bffb&user=Jared+M.+Maruskin%2C+PhD&userId=37ef2450ad04&source=post_page-37ef2450ad04----ab6cd56bffb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab6cd56bffb--------------------------------) ·16 分钟阅读·2023 年 9 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fab6cd56bffb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Festimating-total-experimentation-impact-ab6cd56bffb&user=Jared+M.+Maruskin%2C+PhD&userId=37ef2450ad04&source=-----ab6cd56bffb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fab6cd56bffb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Festimating-total-experimentation-impact-ab6cd56bffb&source=-----ab6cd56bffb---------------------bookmark_footer-----------)![](img/94df2a1643befd711d43b8bf8f43ca3d.png)

照片由 [CHUTTERSNAP](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上

# 引言

数据驱动的组织通常会在任何给定时间进行数百或数千个实验，但这些实验的总体影响是什么呢？一种幼稚的方法是将所有结果显著且积极的处理效应的实验的均值差异相加，然后将其推向生产。然而，这种估计可能会极其偏差，即使我们假设各个实验之间没有相关性。我们将进行 10,000 个实验的模拟，并展示这种幼稚的方法高估了实际影响*45%*！

我们回顾了*李和沈* [1] 提出的理论偏差修正公式。然而，这种方法存在两个缺陷：首先，尽管它在理论上没有偏差，但我们展示了其相应的插件估计器仍然由于与原始问题类似的原因而存在显著的偏差。其次，它没有将影响归因于单个实验。

在这篇文章中，我们探讨了两种偏差来源：

+   *虚假发现偏差*——由于假阳性，估计值被夸大了；
