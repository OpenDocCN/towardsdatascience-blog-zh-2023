# Pandas 处理时间序列

> 原文：[https://towardsdatascience.com/pandas-for-time-series-c6cb7c0a3680?source=collection_archive---------4-----------------------#2023-07-26](https://towardsdatascience.com/pandas-for-time-series-c6cb7c0a3680?source=collection_archive---------4-----------------------#2023-07-26)

## Python 中的数据处理

## 本文介绍了 pandas 处理时间序列的方法。让我们像专业人士一样处理时间序列。

[](https://kahemchu.medium.com/?source=post_page-----c6cb7c0a3680--------------------------------)[![KahEm Chu](../Images/2f89d02e85f61f08f048773990f4d53f.png)](https://kahemchu.medium.com/?source=post_page-----c6cb7c0a3680--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c6cb7c0a3680--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c6cb7c0a3680--------------------------------) [KahEm Chu](https://kahemchu.medium.com/?source=post_page-----c6cb7c0a3680--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cc8007b91ef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-for-time-series-c6cb7c0a3680&user=KahEm+Chu&userId=7cc8007b91ef&source=post_page-7cc8007b91ef----c6cb7c0a3680---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c6cb7c0a3680--------------------------------) ·13 分钟阅读·2023年7月26日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc6cb7c0a3680&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-for-time-series-c6cb7c0a3680&source=-----c6cb7c0a3680---------------------bookmark_footer-----------)![](../Images/0d95a391ba99fb3f6fd922f3c4bbba8f.png)

照片由 [Aron Visuals](https://unsplash.com/@aronvisuals?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/s/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

自从我成为数据科学家进入职场以来，我处理的大部分数据都是时间序列。嗯，对于***时间序列***，通常有很多定义，一般来说，它被定义为在一段时间内收集的数据点集合。或者用 Python 的说法，它指的是具有日期时间索引和至少一个数值列的数据集。

这可能是过去几个月的股票价格，过去几周的大型超市销售额，甚至是一个病人几个月内收集的血糖水平记录。

在这篇文章中，我将展示如何使用 pandas 处理时间序列数据集，以生成的血糖水平记录为例。

基于此，本文将按如下结构组织：

1.  [日期时间格式处理](#aca7) — *将日期时间序列转换为所需格式*

1.  [将日期时间转换为特定周期](#0342) — *将每个数据点转换为特定时间周期*

1.  [根据条件过滤日期时间序列](#db28) — *根据选定的时间段过滤数据点*
