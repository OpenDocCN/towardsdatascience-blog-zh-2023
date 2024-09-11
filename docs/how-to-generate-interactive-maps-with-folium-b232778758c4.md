# 如何使用 Folium 生成交互式地图

> 原文：[`towardsdatascience.com/how-to-generate-interactive-maps-with-folium-b232778758c4?source=collection_archive---------10-----------------------#2023-06-26`](https://towardsdatascience.com/how-to-generate-interactive-maps-with-folium-b232778758c4?source=collection_archive---------10-----------------------#2023-06-26)

## 使用此 Python 库创建地图可视化

[](https://amolmavuduru.medium.com/?source=post_page-----b232778758c4--------------------------------)![Amol Mavuduru](https://amolmavuduru.medium.com/?source=post_page-----b232778758c4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b232778758c4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b232778758c4--------------------------------) [Amol Mavuduru](https://amolmavuduru.medium.com/?source=post_page-----b232778758c4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F511816e5976d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-generate-interactive-maps-with-folium-b232778758c4&user=Amol+Mavuduru&userId=511816e5976d&source=post_page-511816e5976d----b232778758c4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b232778758c4--------------------------------) ·5 分钟阅读·2023 年 6 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb232778758c4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-generate-interactive-maps-with-folium-b232778758c4&user=Amol+Mavuduru&userId=511816e5976d&source=-----b232778758c4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb232778758c4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-generate-interactive-maps-with-folium-b232778758c4&source=-----b232778758c4---------------------bookmark_footer-----------)![](img/80d43456ecb3cb56f0af209bf1515127.png)

佛罗里达州坦帕地图。图像使用 Folium 和 Open Street Map 数据创建。

数据可视化是数据科学中最被忽视的领域之一。机器学习和统计分析很重要，但能够可视化数据，尤其是不同类型的数据，是数据讲述的关键方面。虽然许多初级数据科学训练营和课程会介绍如何使用 matplotlib 和 seaborn 创建基本图表，但其中一些课程未涵盖如何在地图上可视化地理数据。

Folium 是一个 Python 库，利用 Leaflet.js 和 Open Street Map 数据创建高质量的地图可视化。**在这篇文章中，我将演示如何使用 Folium 生成互动式地图可视化。**

# 安装

我们可以像下面演示的那样，通过 pip 轻松安装 Folium。

```py
pip install folium
```

# 导入库

像往常一样，我会导入一些 Folium 库和我们在开始之前可能需要的其他库。请记住，你可以在[GitHub](https://github.com/AmolMavuduru/FoliumTutorial)上找到本教程的所有代码。

```py
import numpy as np
import pandas as pd
import folium
```

# 绘制默认世界地图
