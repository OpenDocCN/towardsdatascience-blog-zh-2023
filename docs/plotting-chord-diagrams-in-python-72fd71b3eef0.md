# 在 Python 中绘制和弦图

> 原文：[https://towardsdatascience.com/plotting-chord-diagrams-in-python-72fd71b3eef0?source=collection_archive---------11-----------------------#2023-02-15](https://towardsdatascience.com/plotting-chord-diagrams-in-python-72fd71b3eef0?source=collection_archive---------11-----------------------#2023-02-15)

## 如何使用 Holoviews 绘制和弦图以展示各种数据属性之间的关系

[](https://weimenglee.medium.com/?source=post_page-----72fd71b3eef0--------------------------------)[![Wei-Meng Lee](../Images/10fc13e8a6858502d6a7b89fcaad7a10.png)](https://weimenglee.medium.com/?source=post_page-----72fd71b3eef0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----72fd71b3eef0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----72fd71b3eef0--------------------------------) [Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----72fd71b3eef0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplotting-chord-diagrams-in-python-72fd71b3eef0&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----72fd71b3eef0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----72fd71b3eef0--------------------------------) ·7 min read·2023年2月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F72fd71b3eef0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplotting-chord-diagrams-in-python-72fd71b3eef0&user=Wei-Meng+Lee&userId=6599e1e08a48&source=-----72fd71b3eef0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F72fd71b3eef0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplotting-chord-diagrams-in-python-72fd71b3eef0&source=-----72fd71b3eef0---------------------bookmark_footer-----------)![](../Images/142065796e10c421ec28bd82450a0dbf.png)

图片由作者提供

迄今为止，当我们谈论数据可视化时，脑海中会浮现出几种常见的图表类型——条形图、饼图、直方图等。然而，还有一种非常有趣但很少讨论的图表类型——**和弦图**。那么，和弦图是什么呢？

和弦图表示几个实体（称为节点）之间的流动或连接。使用和弦图，你可以轻松地可视化数据集中各个数据点之间的连接或关系。考虑航班延误数据集。它包含从一个机场到另一个机场的航班详细信息。如果你想可视化不同机场之间的关系，和弦图（参见本文开头的图示）是展示这些信息的绝佳方式。

在这篇文章中，我将展示如何使用一个名为 **HoloViews** 的第三方库绘制和弦图。

> 所有图片均由作者提供，除非另有说明。

# 安装 Holoviews

**HoloViews** 是一个开源的 Python 库，旨在使数据分析和可视化变得无缝且简单。HoloViews 依赖于另外两个 Python 库…
