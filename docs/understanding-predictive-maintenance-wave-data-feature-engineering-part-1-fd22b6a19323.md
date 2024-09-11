# 了解预测性维护——波数据：特征工程（第1部分）

> 原文：[https://towardsdatascience.com/understanding-predictive-maintenance-wave-data-feature-engineering-part-1-fd22b6a19323?source=collection_archive---------8-----------------------#2023-11-21](https://towardsdatascience.com/understanding-predictive-maintenance-wave-data-feature-engineering-part-1-fd22b6a19323?source=collection_archive---------8-----------------------#2023-11-21)

## 你需要了解的关于波数据信号处理的所有信息

[](https://marcin-staskopl.medium.com/?source=post_page-----fd22b6a19323--------------------------------)[![Marcin Stasko](../Images/5142b9a260a1cce7c6a2ebcc16f46fbb.png)](https://marcin-staskopl.medium.com/?source=post_page-----fd22b6a19323--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fd22b6a19323--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fd22b6a19323--------------------------------) [Marcin Stasko](https://marcin-staskopl.medium.com/?source=post_page-----fd22b6a19323--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd8736adba55&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-predictive-maintenance-wave-data-feature-engineering-part-1-fd22b6a19323&user=Marcin+Stasko&userId=d8736adba55&source=post_page-d8736adba55----fd22b6a19323---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fd22b6a19323--------------------------------) ·16 min read·2023年11月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffd22b6a19323&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-predictive-maintenance-wave-data-feature-engineering-part-1-fd22b6a19323&user=Marcin+Stasko&userId=d8736adba55&source=-----fd22b6a19323---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffd22b6a19323&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-predictive-maintenance-wave-data-feature-engineering-part-1-fd22b6a19323&source=-----fd22b6a19323---------------------bookmark_footer-----------)![](../Images/443d342974ccbd2a69ea10cb885c6e55.png)

[Lukas Tennie](https://unsplash.com/@luk10?utm_source=medium&utm_medium=referral) 的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 文章目的

我们即将深入探讨一个有趣的话题——波数据信号处理。这在预测性维护中非常重要，也涉及其他领域。我将在这一系列中逐步解析，使其易于理解。有什么想法可以补充吗？随时分享！

本文是系列文章《理解预测维护》的一部分。

[点击此链接查看整个系列](https://marcin-staskopl.medium.com/list/understanding-predictive-maintenance-series-e1f44d8a0cc3)。通过关注我，确保您不会错过新文章。所有无标题的图片均由我创建。

# 我们的分析如果在时间域操作，为什么重要呢？

时间域分析在信号处理中是一种基于信号行为和特征随时间变化的方法。与频率域分析不同，后者探索信号组成部分的频率内容，时间域分析提供了关于信号在不同时间间隔内如何变化的见解。这种方法使我们能够观察到信号展示的变化、模式和趋势，提供了有价值的信息…
