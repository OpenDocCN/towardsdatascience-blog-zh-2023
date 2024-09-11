# 使用 Python 的 Plotly 和 Folium 库创建地理空间热力图

> 原文：[https://towardsdatascience.com/creating-geospatial-heatmaps-with-pythons-plotly-and-folium-libraries-4159e98a1ae8?source=collection_archive---------3-----------------------#2023-03-17](https://towardsdatascience.com/creating-geospatial-heatmaps-with-pythons-plotly-and-folium-libraries-4159e98a1ae8?source=collection_archive---------3-----------------------#2023-03-17)

## 两种出色的 Python 可视化地理空间变异的选项

[](https://andymcdonaldgeo.medium.com/?source=post_page-----4159e98a1ae8--------------------------------)[![Andy McDonald](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----4159e98a1ae8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4159e98a1ae8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4159e98a1ae8--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----4159e98a1ae8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-geospatial-heatmaps-with-pythons-plotly-and-folium-libraries-4159e98a1ae8&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----4159e98a1ae8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4159e98a1ae8--------------------------------) · 6 分钟阅读 · 2023年3月17日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4159e98a1ae8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-geospatial-heatmaps-with-pythons-plotly-and-folium-libraries-4159e98a1ae8&source=-----4159e98a1ae8---------------------bookmark_footer-----------)![](../Images/07f55e060065099f6e715799e103d191.png)

图片由 [KOBU Agency](https://unsplash.com/@kobuagency?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**热力图**，也称为 **密度图**，是显示变量在地理区域内空间分布的数据可视化工具。它们是可视化和识别趋势的极佳工具，支持决策制定、检测异常值，并为演示创建引人注目的视觉效果。

有几个映射的 Python 库可供选择，然而，两个非常受欢迎且易于使用的库是**Folium**和**Plotly Express**。

[**Folium**](https://python-visualization.github.io/folium/)是一个很棒的库，可以轻松可视化地理空间数据。它由[**Leaflet.js**](https://leafletjs.com/)提供支持，后者是一个领先的 JavaScript 映射库，且平台无关。[**Plotly**](https://plotly.com/graphing-libraries/)是一个流行的库，用于创建强大的互动数据可视化，代码量非常少，并且可以用来与 MapBox 一起创建互动地图。

在本文中，我们将探讨如何使用这两个库在挪威大陆架上可视化声波压缩慢度数据。

使用 Plotly Express 的此教程的视频版本可以在我的 YouTube 频道找到。请查看下面的链接。
