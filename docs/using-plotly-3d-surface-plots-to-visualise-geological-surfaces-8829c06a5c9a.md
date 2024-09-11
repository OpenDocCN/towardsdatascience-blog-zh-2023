# 使用 Plotly 3D 表面图可视化地质表面

> 原文：[https://towardsdatascience.com/using-plotly-3d-surface-plots-to-visualise-geological-surfaces-8829c06a5c9a?source=collection_archive---------6-----------------------#2023-06-21](https://towardsdatascience.com/using-plotly-3d-surface-plots-to-visualise-geological-surfaces-8829c06a5c9a?source=collection_archive---------6-----------------------#2023-06-21)

## 使用 Python 数据可视化库可视化地下层

[![Andy McDonald](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----8829c06a5c9a--------------------------------) [![数据科学的前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8829c06a5c9a--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----8829c06a5c9a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-plotly-3d-surface-plots-to-visualise-geological-surfaces-8829c06a5c9a&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----8829c06a5c9a---------------------post_header-----------) 发布于 [数据科学的前沿](https://towardsdatascience.com/?source=post_page-----8829c06a5c9a--------------------------------) ·9分钟阅读·2023年6月21日

--

![](../Images/7eca521450a3968109b5d97f148ad420.png)

Hugin 组的 3D 表面图。作者提供的图像。

在地球科学中，全面了解地下的地质表面至关重要。通过知道地层的确切位置及其几何形状，可以更容易地识别潜在的石油和天然气勘探新目标以及碳捕集和存储的潜在位置。我们可以使用各种方法来精炼这些表面，从地震数据到井记录导出的地层顶点。通常，这些技术会互相结合来精炼最终表面。

本文延续了我之前的文章，这些文章专注于如何在一个区域内外推井记录测量，以理解和可视化地理空间变化。在这篇文章中，我们将探讨如何使用交互式Plotly图表创建3D表面。

由于建模地质表面是一个复杂的过程，并且通常涉及多次迭代和精炼，本文展示了一个非常简单的示例，演示如何使用Python可视化这些数据。

为了查看如何使用Plotly在一个区域内可视化我们的地质地层顶点，我们将使用两组数据。
