# 高效实时数据模型构建指南

> 原文：[https://towardsdatascience.com/a-guide-to-building-performant-real-time-data-models-d60b37bb07dc?source=collection_archive---------3-----------------------#2023-08-12](https://towardsdatascience.com/a-guide-to-building-performant-real-time-data-models-d60b37bb07dc?source=collection_archive---------3-----------------------#2023-08-12)

[](https://medium.com/@marietruong?source=post_page-----d60b37bb07dc--------------------------------)[![Marie Truong](../Images/2816e49beef958724dc0f38cfa49c4be.png)](https://medium.com/@marietruong?source=post_page-----d60b37bb07dc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d60b37bb07dc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d60b37bb07dc--------------------------------) [Marie Truong](https://medium.com/@marietruong?source=post_page-----d60b37bb07dc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4cfa1d0b321f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-building-performant-real-time-data-models-d60b37bb07dc&user=Marie+Truong&userId=4cfa1d0b321f&source=post_page-4cfa1d0b321f----d60b37bb07dc---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d60b37bb07dc--------------------------------) · 7 分钟阅读 · 2023年8月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd60b37bb07dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-building-performant-real-time-data-models-d60b37bb07dc&user=Marie+Truong&userId=4cfa1d0b321f&source=-----d60b37bb07dc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd60b37bb07dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-building-performant-real-time-data-models-d60b37bb07dc&source=-----d60b37bb07dc---------------------bookmark_footer-----------)![](../Images/3861b61d9bafd196194be9f60407c51b.png)

图片来源于 [Lukas Blazek](https://unsplash.com/@goumbik?utm_source=medium&utm_medium=referral) 的 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据已经成为决策的关键工具。为了能够实际使用，数据需要经过清理、转换和建模。

这个过程通常是 ELT 流水线的一部分，该流水线以特定的频率运行，例如每天一次。

另一方面，为了快速调整和决策，利益相关者有时需要访问最新的数据，以便能够快速反应。

比如，如果网站的用户数量出现大幅下降，他们需要迅速了解这个问题，并获得必要的信息来理解问题。

第一次我被要求构建一个实时数据的仪表盘时，我直接将其连接到实时的原始表，并提供了一些简单的KPI指标，比如用户数量和崩溃次数。对于月度图表和更深入的分析，我创建了另一个连接到我们数据模型的仪表盘，该模型每天更新。

这个策略并不理想：我在数据仓库和BI工具之间重复了逻辑，因此维护起来更困难。此外，实时仪表盘只能在几天的数据下表现良好，因此利益相关者必须切换到历史仪表盘来查看早期的数据。

我知道我们必须对此采取行动。我们需要实时数据模型而不妥协…
