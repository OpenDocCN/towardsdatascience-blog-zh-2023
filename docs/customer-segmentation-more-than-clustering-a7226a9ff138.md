# 客户细分：不仅仅是聚类

> 原文：[https://towardsdatascience.com/customer-segmentation-more-than-clustering-a7226a9ff138?source=collection_archive---------5-----------------------#2023-12-15](https://towardsdatascience.com/customer-segmentation-more-than-clustering-a7226a9ff138?source=collection_archive---------5-----------------------#2023-12-15)

## 帮助你的客户细分工作被业务采纳的框架

[](https://medium.com/@guillaume.colley?source=post_page-----a7226a9ff138--------------------------------)[![Guillaume Colley](../Images/97ea637a566255b6724d4079ca2d5180.png)](https://medium.com/@guillaume.colley?source=post_page-----a7226a9ff138--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a7226a9ff138--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a7226a9ff138--------------------------------) [Guillaume Colley](https://medium.com/@guillaume.colley?source=post_page-----a7226a9ff138--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F40c66a92e9d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcustomer-segmentation-more-than-clustering-a7226a9ff138&user=Guillaume+Colley&userId=40c66a92e9d&source=post_page-40c66a92e9d----a7226a9ff138---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a7226a9ff138--------------------------------) ·14 min read·2023年12月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa7226a9ff138&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcustomer-segmentation-more-than-clustering-a7226a9ff138&user=Guillaume+Colley&userId=40c66a92e9d&source=-----a7226a9ff138---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa7226a9ff138&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcustomer-segmentation-more-than-clustering-a7226a9ff138&source=-----a7226a9ff138---------------------bookmark_footer-----------)![](../Images/a1e4c02b119d53aab6f776be499ea768.png)

图片由作者提供

当数据科学团队需要构建客户细分模型时，这通常是业务的要求，或者更少见的是数据科学家的主动决策。在这两种情况下，需求都是相同的：更深入地了解客户基础，以便对每个细分市场做出更精细、差异化的决策。

然而，我看到许多聚类工作的成果并未被业务采纳，因为所得到的细分没有引起共鸣或未能对业务利益相关者产生实际作用。在本文中，我将概述一些关键步骤和策略，以最大化业务对你细分工作的采纳。

+   工作说明

+   整理你的数据

+   确定你的算法

+   理解你的聚类

+   多少个聚类？

+   检查稳定性

+   翻译成业务规则（或不翻译）

鉴于本文关注于“应用数据科学”而非“技术机器学习”，因此不会深入探讨各种聚类算法的复杂细节，因为对于那些寻求更深入了解的读者，已有大量资源可供参考…
