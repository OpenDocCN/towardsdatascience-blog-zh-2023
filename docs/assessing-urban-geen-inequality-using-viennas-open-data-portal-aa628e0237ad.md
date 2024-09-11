# 使用维也纳开放数据门户评估城市绿地平等性

> 原文：[https://towardsdatascience.com/assessing-urban-geen-inequality-using-viennas-open-data-portal-aa628e0237ad?source=collection_archive---------2-----------------------#2023-09-11](https://towardsdatascience.com/assessing-urban-geen-inequality-using-viennas-open-data-portal-aa628e0237ad?source=collection_archive---------2-----------------------#2023-09-11)

![](../Images/41e3d2093728fbc151cea2cb522983a7.png)

照片来源：[CHUTTERSNAP](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 尽管自然环境和绿地的许多优点，但在高度城市化的地区，接触自然变得越来越困难。一些人担心，服务不足的社区更容易面临这些问题。在这里，我提出了一种数据驱动的方法来探讨这一问题。

[](https://medium.com/@janosovm?source=post_page-----aa628e0237ad--------------------------------)[![米兰·贾诺索夫](../Images/77b62460041f66ec4585a81baef81a03.png)](https://medium.com/@janosovm?source=post_page-----aa628e0237ad--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aa628e0237ad--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----aa628e0237ad--------------------------------) [米兰·贾诺索夫](https://medium.com/@janosovm?source=post_page-----aa628e0237ad--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F838408aa2ad4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fassessing-urban-geen-inequality-using-viennas-open-data-portal-aa628e0237ad&user=Milan+Janosov&userId=838408aa2ad4&source=post_page-838408aa2ad4----aa628e0237ad---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----aa628e0237ad--------------------------------) ·11 min read·Sep 11, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faa628e0237ad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fassessing-urban-geen-inequality-using-viennas-open-data-portal-aa628e0237ad&user=Milan+Janosov&userId=838408aa2ad4&source=-----aa628e0237ad---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faa628e0237ad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fassessing-urban-geen-inequality-using-viennas-open-data-portal-aa628e0237ad&source=-----aa628e0237ad---------------------bookmark_footer-----------)

特别是，我提出了一个近年来在专业圈子和地方政府中越来越受到关注的城市发展问题——即绿色平等。这个概念指的是人们在城市不同区域获得绿色空间的差异。在这里，我将探讨其财务维度，并查看每人可用的绿色面积与该城市单位经济水平之间是否存在明显的关系。

我将使用奥地利政府开放数据门户提供的 Esri Shapefiles 探索城市的两种不同空间分辨率——区和普查区。我还将把表格统计数据（人口和收入）纳入地理参考的行政区域中。然后，我将行政区域与官方绿色区域数据集叠加，记录每个绿色空间的位置，采用地理空间格式。接下来，我将结合这些…
