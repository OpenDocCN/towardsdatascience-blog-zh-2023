# 时间序列数据库的数据集成策略

> 原文：[https://towardsdatascience.com/data-integration-strategies-for-time-series-databases-f96cab274820?source=collection_archive---------24-----------------------#2023-01-31](https://towardsdatascience.com/data-integration-strategies-for-time-series-databases-f96cab274820?source=collection_archive---------24-----------------------#2023-01-31)

## 探索流行的数据集成策略，包括ETL、ELT和CDC

[](https://yitaek.medium.com/?source=post_page-----f96cab274820--------------------------------)[![Yitaek Hwang](../Images/8e85bfb443a1df284607fb5e5fb2afa5.png)](https://yitaek.medium.com/?source=post_page-----f96cab274820--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f96cab274820--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f96cab274820--------------------------------) [Yitaek Hwang](https://yitaek.medium.com/?source=post_page-----f96cab274820--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa9626034cb21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-integration-strategies-for-time-series-databases-f96cab274820&user=Yitaek+Hwang&userId=a9626034cb21&source=post_page-a9626034cb21----f96cab274820---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f96cab274820--------------------------------) ·6分钟阅读·2023年1月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff96cab274820&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-integration-strategies-for-time-series-databases-f96cab274820&user=Yitaek+Hwang&userId=a9626034cb21&source=-----f96cab274820---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff96cab274820&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-integration-strategies-for-time-series-databases-f96cab274820&source=-----f96cab274820---------------------bookmark_footer-----------)![](../Images/58b09c28f46fa6ad8943b13548433aec.png)

照片由 [fabio](https://unsplash.com/@fabioha?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

随着数字化转型进入更多行业，生成的数据点数量正在呈指数级增长。因此，从不同来源收集大量数据的集成策略，尤其是各种格式和结构的数据，现在成为数据工程团队的主要关注点。传统的数据集成方法，主要集中在将高度结构化的数据整理到数据仓库中，已经难以应对新数据集的体量和异质性。

时间序列数据呈现出额外的复杂性。由于时间的推移，每个时间序列数据点的价值会逐渐减小，因为数据的粒度随着时间的推移而变得不再相关。因此，团队必须精心规划数据集成策略，以将数据集成到时间序列数据库（TSDBs）中，以确保分析能够反映接近实时的趋势和情况。

在本文中，我们将探讨一些最受欢迎的时间序列数据库（TSDBs）数据集成方法：

+   **ETL**（提取、转换、加载）

+   **ELT**（提取、加载、转换）

+   **数据流与CDC**（变更数据捕获）
