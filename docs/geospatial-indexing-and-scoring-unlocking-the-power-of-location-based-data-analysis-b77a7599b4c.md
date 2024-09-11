# 地理空间索引与评分：解锁基于位置的数据分析的力量

> 原文：[https://towardsdatascience.com/geospatial-indexing-and-scoring-unlocking-the-power-of-location-based-data-analysis-b77a7599b4c?source=collection_archive---------7-----------------------#2023-01-25](https://towardsdatascience.com/geospatial-indexing-and-scoring-unlocking-the-power-of-location-based-data-analysis-b77a7599b4c?source=collection_archive---------7-----------------------#2023-01-25)

## 使用 Python 和 H3 的地理空间索引指南

[](https://ransakaravihara.medium.com/?source=post_page-----b77a7599b4c--------------------------------)[![Ransaka Ravihara](../Images/ac09746938c10ad8f157d46ea0de27ca.png)](https://ransakaravihara.medium.com/?source=post_page-----b77a7599b4c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b77a7599b4c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b77a7599b4c--------------------------------) [Ransaka Ravihara](https://ransakaravihara.medium.com/?source=post_page-----b77a7599b4c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F61b4d96de932&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-indexing-and-scoring-unlocking-the-power-of-location-based-data-analysis-b77a7599b4c&user=Ransaka+Ravihara&userId=61b4d96de932&source=post_page-61b4d96de932----b77a7599b4c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b77a7599b4c--------------------------------) · 7 分钟阅读 · 2023年1月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb77a7599b4c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-indexing-and-scoring-unlocking-the-power-of-location-based-data-analysis-b77a7599b4c&user=Ransaka+Ravihara&userId=61b4d96de932&source=-----b77a7599b4c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb77a7599b4c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-indexing-and-scoring-unlocking-the-power-of-location-based-data-analysis-b77a7599b4c&source=-----b77a7599b4c---------------------bookmark_footer-----------)![](../Images/6b75f0648ad78b2c59482c7d2ccc6497.png)

图片由 [Antoine Merour](https://unsplash.com/@amerour?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，我们将讨论地理空间索引和评分，以及如何使其对开发者更为简便。地理空间索引是创建一个在地理位置（如纬度和经度）相关的数据集上的索引的过程。另一方面，评分是根据特定标准对数据进行排名或排序的过程。地理空间索引和评分结合可以创建强大且高效的应用程序，能够快速检索和显示地理数据。

地理空间索引和评分可能复杂且耗时，但通过合适的工具和技术，可以将其简化。本文将探讨如何使用Uber的H3库进行索引和评分任务。

为了更好地理解，我们来定义一下假设问题陈述。

> 作为一家零售公司的数据科学家，你被委派了一个关键项目。公司需要组织和理解其运营的不同区域。
> 
> 本项目的最终目标是将区域拆分成不同的部分，并识别每个区域中的关键热点。这个信息将会…
