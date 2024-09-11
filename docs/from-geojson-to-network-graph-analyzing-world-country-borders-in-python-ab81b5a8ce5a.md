# 从 GeoJSON 到网络图：在 Python 中分析世界国家边界

> 原文：[https://towardsdatascience.com/from-geojson-to-network-graph-analyzing-world-country-borders-in-python-ab81b5a8ce5a?source=collection_archive---------2-----------------------#2023-10-15](https://towardsdatascience.com/from-geojson-to-network-graph-analyzing-world-country-borders-in-python-ab81b5a8ce5a?source=collection_archive---------2-----------------------#2023-10-15)

## 利用 NetworkX 进行基于图的国家边界分析

[](https://amandaiglesiasmoreno.medium.com/?source=post_page-----ab81b5a8ce5a--------------------------------)[![Amanda Iglesias Moreno](../Images/7a2662fb88127b1a7203c27916e15a71.png)](https://amandaiglesiasmoreno.medium.com/?source=post_page-----ab81b5a8ce5a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ab81b5a8ce5a--------------------------------)[![数据科学进阶](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ab81b5a8ce5a--------------------------------) [Amanda Iglesias Moreno](https://amandaiglesiasmoreno.medium.com/?source=post_page-----ab81b5a8ce5a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1bace2932c65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-geojson-to-network-graph-analyzing-world-country-borders-in-python-ab81b5a8ce5a&user=Amanda+Iglesias+Moreno&userId=1bace2932c65&source=post_page-1bace2932c65----ab81b5a8ce5a---------------------post_header-----------) 发表于 [数据科学进阶](https://towardsdatascience.com/?source=post_page-----ab81b5a8ce5a--------------------------------) ·7分钟阅读·2023年10月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fab81b5a8ce5a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-geojson-to-network-graph-analyzing-world-country-borders-in-python-ab81b5a8ce5a&user=Amanda+Iglesias+Moreno&userId=1bace2932c65&source=-----ab81b5a8ce5a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fab81b5a8ce5a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-geojson-to-network-graph-analyzing-world-country-borders-in-python-ab81b5a8ce5a&source=-----ab81b5a8ce5a---------------------bookmark_footer-----------)![](../Images/77bc3f84ad2518f2a0543607dcb80e36.png)

[Maksim Shutov](https://unsplash.com/es/@maksimshutov) 来自 Unsplash

Python 提供了广泛的库，可以轻松快速地解决各种研究领域的问题。**地理空间数据分析和图论是两个研究领域，Python 提供了强大而实用的库集合**。本文将进行一项简单的世界边界分析，具体探索哪些国家共享边界。我们将首先利用包含全球所有国家多边形的 GeoJSON 文件中的信息。最终目标是使用 NetworkX 创建表示各种边界的图，并利用该图执行多重分析。

# GeoJSON 数据摄取：读取和加载全球国家数据

**GeoJSON 文件能够表示各种地理区域，在地理分析和可视化中被广泛使用**。我们分析的初始阶段涉及读取 `countries.geojson` 文件，并将其转换为 `GeoDataFrame` 使用 `GeoPandas`。此文件来源于以下 GitHub 仓库，并包含表示全球不同国家的多边形。
