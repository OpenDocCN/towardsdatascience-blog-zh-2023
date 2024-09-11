# 使用 Basemap 和 mplleaflet 可视化地理空间网络图

> 原文：[`towardsdatascience.com/visualizing-geospatial-network-graphs-using-basemap-and-mplleaflet-76a7f3d0c923?source=collection_archive---------13-----------------------#2023-03-06`](https://towardsdatascience.com/visualizing-geospatial-network-graphs-using-basemap-and-mplleaflet-76a7f3d0c923?source=collection_archive---------13-----------------------#2023-03-06)

## 学习如何在地图上绘制网络图

[](https://weimenglee.medium.com/?source=post_page-----76a7f3d0c923--------------------------------)![Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----76a7f3d0c923--------------------------------)[](https://towardsdatascience.com/?source=post_page-----76a7f3d0c923--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----76a7f3d0c923--------------------------------) [魏孟李](https://weimenglee.medium.com/?source=post_page-----76a7f3d0c923--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizing-geospatial-network-graphs-using-basemap-and-mplleaflet-76a7f3d0c923&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----76a7f3d0c923---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----76a7f3d0c923--------------------------------) ·9 分钟阅读·2023 年 3 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F76a7f3d0c923&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizing-geospatial-network-graphs-using-basemap-and-mplleaflet-76a7f3d0c923&user=Wei-Meng+Lee&userId=6599e1e08a48&source=-----76a7f3d0c923---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F76a7f3d0c923&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizing-geospatial-network-graphs-using-basemap-and-mplleaflet-76a7f3d0c923&source=-----76a7f3d0c923---------------------bookmark_footer-----------)![](img/2faf32825e4db3335fc9b1f3723c3e48.png)

照片由[Z](https://unsplash.com/@dead____artist?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我之前关于网络图的文章中，我展示了如何使用 NetworkX 和 pyvis 包绘制有向和无向图。在这篇文章中，我将使用航班延误数据集来可视化不同机场之间的航线，特别是向你展示如何使用地理空间网络图进行可视化。

[](/plotting-network-graphs-using-python-bc62f0d93b3f?source=post_page-----76a7f3d0c923--------------------------------) ## 使用 Python 绘制网络图

### 了解如何使用 NetworkX 包来可视化复杂的网络

[towardsdatascience.com [](/building-interactive-network-graphs-using-pyvis-5b8e6e25cf64?source=post_page-----76a7f3d0c923--------------------------------) ## 使用 pyvis 构建互动网络图

### 了解如何让你的网络图生动起来

[towardsdatascience.com

# 使用航班延误数据集

像往常一样，我将使用**2015 年航班延误**数据集。

> **2015 年航班延误数据集**（*airports.csv*）。*来源*：[`www.kaggle.com/datasets/usdot/flight-delays`](https://www.kaggle.com/datasets/usdot/flight-delays)。*许可* — [CC0: 公众领域](https://creativecommons.org/publicdomain/zero/1.0/)
