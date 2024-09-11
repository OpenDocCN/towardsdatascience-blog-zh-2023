# 深入探讨处理 Apache Spark 数据倾斜

> 原文：[https://towardsdatascience.com/deep-dive-into-handling-apache-spark-data-skew-57ce0d94ee38?source=collection_archive---------11-----------------------#2023-01-03](https://towardsdatascience.com/deep-dive-into-handling-apache-spark-data-skew-57ce0d94ee38?source=collection_archive---------11-----------------------#2023-01-03)

## 终极指南：处理分布式计算中的数据倾斜

[](https://chengzhizhao.medium.com/?source=post_page-----57ce0d94ee38--------------------------------)[![Chengzhi Zhao](../Images/186bba91822dbcc0f926426e56faf543.png)](https://chengzhizhao.medium.com/?source=post_page-----57ce0d94ee38--------------------------------)[](https://towardsdatascience.com/?source=post_page-----57ce0d94ee38--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----57ce0d94ee38--------------------------------) [Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----57ce0d94ee38--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff956c63a9571&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-dive-into-handling-apache-spark-data-skew-57ce0d94ee38&user=Chengzhi+Zhao&userId=f956c63a9571&source=post_page-f956c63a9571----57ce0d94ee38---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----57ce0d94ee38--------------------------------) · 10 分钟阅读 · 2023年1月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F57ce0d94ee38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-dive-into-handling-apache-spark-data-skew-57ce0d94ee38&user=Chengzhi+Zhao&userId=f956c63a9571&source=-----57ce0d94ee38---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F57ce0d94ee38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-dive-into-handling-apache-spark-data-skew-57ce0d94ee38&source=-----57ce0d94ee38---------------------bookmark_footer-----------)![](../Images/2c9d079782f553e5b7931674b18b46c0.png)

图片由 [Lizzi Sassman](https://unsplash.com/@okaylizzi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**“*为什么我的 Spark 任务运行缓慢？*”** 是在使用 Apache Spark 时不可避免的问题。有关 Apache Spark 性能调优的最常见场景之一是 **数据倾斜**。在本文中，我们将讨论如何识别您的 Spark 任务的缓慢是否是由数据倾斜引起的，并深入探讨如何通过代码处理 Apache Spark 数据倾斜，解释三种处理数据倾斜的方法，包括“加盐”技术。

## 如何识别 Spark 中的数据倾斜

当涉及到 Spark 性能调优时，有许多因素需要考虑。鉴于分布式计算的复杂性，如果您能够将问题缩小到瓶颈，那么您已经成功了一半。

数据倾斜通常发生在分区之间的数据处理不均匀时。假设我们有三个分区在 Spark 中处理 150 万条记录。理想情况下，每个分区得到 50 万条记录进行均匀处理（见图 1 左）。然而，我们可能会遇到某个分区处理的数据远多于其他分区的情况（见图 1 右）。
