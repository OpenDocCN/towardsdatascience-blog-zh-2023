# 解锁缓慢变化维度（SCD）的秘密：8种类型的全面视角

> 原文：[https://towardsdatascience.com/unlocking-the-secrets-of-slowly-changing-dimension-scd-a-comprehensive-view-of-8-types-a5ea052e4b36?source=collection_archive---------12-----------------------#2023-07-17](https://towardsdatascience.com/unlocking-the-secrets-of-slowly-changing-dimension-scd-a-comprehensive-view-of-8-types-a5ea052e4b36?source=collection_archive---------12-----------------------#2023-07-17)

## 深入指南：何时以及如何使用8种SCD类型

[](https://chengzhizhao.medium.com/?source=post_page-----a5ea052e4b36--------------------------------)[![Chengzhi Zhao](../Images/186bba91822dbcc0f926426e56faf543.png)](https://chengzhizhao.medium.com/?source=post_page-----a5ea052e4b36--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a5ea052e4b36--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a5ea052e4b36--------------------------------) [Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----a5ea052e4b36--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff956c63a9571&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlocking-the-secrets-of-slowly-changing-dimension-scd-a-comprehensive-view-of-8-types-a5ea052e4b36&user=Chengzhi+Zhao&userId=f956c63a9571&source=post_page-f956c63a9571----a5ea052e4b36---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a5ea052e4b36--------------------------------) · 12分钟阅读 · 2023年7月17日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa5ea052e4b36&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlocking-the-secrets-of-slowly-changing-dimension-scd-a-comprehensive-view-of-8-types-a5ea052e4b36&user=Chengzhi+Zhao&userId=f956c63a9571&source=-----a5ea052e4b36---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa5ea052e4b36&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlocking-the-secrets-of-slowly-changing-dimension-scd-a-comprehensive-view-of-8-types-a5ea052e4b36&source=-----a5ea052e4b36---------------------bookmark_footer-----------)![](../Images/62d6484416ffb7efede0a361a2155b00.png)

照片由 [Donald Tran](https://unsplash.com/@donald_tr4n?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/Lvy_S3ELzIs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果你正在设计数据仓库，理解维度建模的方法论至关重要。这个方法论中一个最重要的概念是 Slowly Changing Dimension（SCD）。许多在线资源提供了对缓慢变化维度的概述，但往往没有深入解释应该使用哪种类型的 SCD。

本文将讨论八种类型的 SCD，每种类型都有其独特的特征和应用场景。到文章结尾，你将清楚理解每种类型，并在创建下一个维度建模中的 SCD 时能够区分它们。

# 为什么在维度建模中需要 SCD？

通常，OLTP 仅存储最新数据。当执行更新语句时，当前数据会被替换，之前的数据会丢失，除非系统特别设计用于保留历史数据，否则可能会导致额外的延迟。

另一方面，OLAP 系统通常使用维度建模，这类系统关注趋势并从中获取商业洞察，通常会保留历史记录……
