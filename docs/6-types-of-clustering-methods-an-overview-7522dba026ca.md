# 6 种聚类方法 — 概述

> 原文：[https://towardsdatascience.com/6-types-of-clustering-methods-an-overview-7522dba026ca?source=collection_archive---------1-----------------------#2023-03-24](https://towardsdatascience.com/6-types-of-clustering-methods-an-overview-7522dba026ca?source=collection_archive---------1-----------------------#2023-03-24)

## 聚类方法和算法的类型以及何时使用它们

[](https://kayjanwong.medium.com/?source=post_page-----7522dba026ca--------------------------------)[![Kay Jan Wong](../Images/28e803eca6327d97b6aa97ee4095d7bd.png)](https://kayjanwong.medium.com/?source=post_page-----7522dba026ca--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7522dba026ca--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7522dba026ca--------------------------------) [Kay Jan Wong](https://kayjanwong.medium.com/?source=post_page-----7522dba026ca--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffee8693930fb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-types-of-clustering-methods-an-overview-7522dba026ca&user=Kay+Jan+Wong&userId=fee8693930fb&source=post_page-fee8693930fb----7522dba026ca---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7522dba026ca--------------------------------) ·8分钟阅读·2023年3月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7522dba026ca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-types-of-clustering-methods-an-overview-7522dba026ca&user=Kay+Jan+Wong&userId=fee8693930fb&source=-----7522dba026ca---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7522dba026ca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-types-of-clustering-methods-an-overview-7522dba026ca&source=-----7522dba026ca---------------------bookmark_footer-----------)![](../Images/78a95243080c867f084198a587c15efc.png)

图片由 [Kier in Sight](https://unsplash.com/@kierinsight?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

聚类是无监督学习的一个分支，其中**未标记的数据**被分为多个组，具有相似数据实例的被分配到同一簇，而不相似的数据实例则被分配到不同的簇。聚类在市场细分、异常检测和网络分析等方面有各种应用。

聚类方法有不同的类型，每种方法都有其优缺点。本文介绍了不同类型的聚类方法及其算法示例，并说明了何时使用每种算法。

# 目录

+   基于质心的 / 划分（*K-means*）

+   基于连通性的（*层次聚类*）

+   基于密度的（*DBSCAN*）

+   基于图的（*亲和传播*）

+   基于分布的（*高斯混合模型*）

+   基于压缩的（*谱聚类, BIRCH*）

# 基于质心的 / 划分

> 基于质心的方法将数据点根据其与质心（簇中心）的接近程度进行分组
