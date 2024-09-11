# 识别城市区域中的时尚热点

> 原文：[https://towardsdatascience.com/identifying-topical-hot-spots-in-urban-areas-3c47cde5ae10?source=collection_archive---------3-----------------------#2023-10-16](https://towardsdatascience.com/identifying-topical-hot-spots-in-urban-areas-3c47cde5ae10?source=collection_archive---------3-----------------------#2023-10-16)

![](../Images/76d9a53bc4caa856f34174d3080e2287.png)

布达佩斯的时尚热点。

## 使用 OpenStreetMap 和 DBSCAN 空间聚类的通用框架，捕捉最受热议的城市区域

[](https://medium.com/@janosovm?source=post_page-----3c47cde5ae10--------------------------------)[![米兰·贾诺索夫](../Images/77b62460041f66ec4585a81baef81a03.png)](https://medium.com/@janosovm?source=post_page-----3c47cde5ae10--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3c47cde5ae10--------------------------------)[![数据科学之路](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3c47cde5ae10--------------------------------) [米兰·贾诺索夫](https://medium.com/@janosovm?source=post_page-----3c47cde5ae10--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F838408aa2ad4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fidentifying-topical-hot-spots-in-urban-areas-3c47cde5ae10&user=Milan+Janosov&userId=838408aa2ad4&source=post_page-838408aa2ad4----3c47cde5ae10---------------------post_header-----------) 发布于 [数据科学之路](https://towardsdatascience.com/?source=post_page-----3c47cde5ae10--------------------------------) ·9 分钟阅读·2023年10月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3c47cde5ae10&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fidentifying-topical-hot-spots-in-urban-areas-3c47cde5ae10&user=Milan+Janosov&userId=838408aa2ad4&source=-----3c47cde5ae10---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3c47cde5ae10&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fidentifying-topical-hot-spots-in-urban-areas-3c47cde5ae10&source=-----3c47cde5ae10---------------------bookmark_footer-----------)

在这篇文章中，我展示了一种快速且易于使用的方法，该方法能够基于从[OpenStreeetMap](https://help.openstreetmap.org/questions/64731/place-type-categories) ([OSM](https://help.openstreetmap.org/questions/64731/place-type-categories)) 收集的兴趣点（POI）识别热点区域，使用的是sklearn的[DBSCAN](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.DBSCAN.html)算法。首先，我将收集来自ChatGPT的几个类别的POI原始数据，我假设这些数据具有某种称为“hip-lifestyle”（例如咖啡馆、酒吧、市场、瑜伽工作室）的特征；在将这些数据转换为便捷的GeoDataFrame后，我进行地理空间聚类，最后，根据不同城市功能在每个簇中的混合情况评估结果。

虽然我称之为“hipster”的主题及其相关的POI类别有些随意，但它们可以很容易地被其他主题和类别替换——自动热点检测方法保持不变。这种易于采用的方法的优点包括从识别本地[创新中心](https://scholar.google.com/citations?view_op=view_citation&hl=en&user=5_ep83MAAAAJ&citation_for_view=5_ep83MAAAAJ%3AWF5omc3nYNoC)以支持创新规划，到检测支持城市规划倡议的城市次中心、评估企业市场机会、分析房地产投资机会，等等。
