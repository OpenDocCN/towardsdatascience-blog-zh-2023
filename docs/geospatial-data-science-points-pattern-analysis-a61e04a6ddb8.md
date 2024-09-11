# 地理空间数据科学：点模式分析

> 原文：[https://towardsdatascience.com/geospatial-data-science-points-pattern-analysis-a61e04a6ddb8?source=collection_archive---------2-----------------------#2023-09-25](https://towardsdatascience.com/geospatial-data-science-points-pattern-analysis-a61e04a6ddb8?source=collection_archive---------2-----------------------#2023-09-25)

## 一个关于如何在 Python 中进行地理空间点模式分析的快速教程。

[](https://gustavorsantos.medium.com/?source=post_page-----a61e04a6ddb8--------------------------------)[![古斯塔沃·桑托斯](../Images/a19a9f4525cdeb6e7a76cd05246aa622.png)](https://gustavorsantos.medium.com/?source=post_page-----a61e04a6ddb8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a61e04a6ddb8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a61e04a6ddb8--------------------------------) [古斯塔沃·桑托斯](https://gustavorsantos.medium.com/?source=post_page-----a61e04a6ddb8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-science-points-pattern-analysis-a61e04a6ddb8&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----a61e04a6ddb8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a61e04a6ddb8--------------------------------) ·8分钟阅读·2023年9月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa61e04a6ddb8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-science-points-pattern-analysis-a61e04a6ddb8&user=Gustavo+Santos&userId=4429d99b1245&source=-----a61e04a6ddb8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa61e04a6ddb8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-science-points-pattern-analysis-a61e04a6ddb8&source=-----a61e04a6ddb8---------------------bookmark_footer-----------)![](../Images/4e4be86ffe20aa9ae48c9e9cdbc209cf.png)

图片来源：[伯纳德·赫尔曼](https://unsplash.com/@bernardhermant?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/Jlh7u4NjNAQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

地理空间数据科学是数据领域的一个子领域，专注于分析数据点并考虑事件发生的空间位置。

比如说我们拥有一个销售智能手机的零售连锁店。我们的连锁店有几个配送中心，并且我们即将开设几个新店。我们应该在哪里开设这些新店呢？

这样的洞察力可能来自地理空间分析，它可以向我们展示销售集中在哪些地方，是否存在销售高或低的聚集区域以及其他见解。

当我们想确保我们在查看地理上聚集的数据集时，点模式分析就会发挥作用。就像我们作为数据科学家的大多数工作一样，点模式涉及到创建假设并通过统计学应用于数据来消除大部分不确定性，以确认假设是否正确。在这种情况下，也不例外。需要进行几个统计测试，这些将在本篇文章中展示。

顺便提一下，我们最近在我的博客中一直在研究地理空间数据科学。如果你对这个主题了解不多，以下是两个不错的阅读材料供你参考……
