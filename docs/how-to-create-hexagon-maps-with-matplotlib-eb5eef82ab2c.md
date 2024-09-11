# 如何使用 Matplotlib 创建六边形地图

> 原文：[https://towardsdatascience.com/how-to-create-hexagon-maps-with-matplotlib-eb5eef82ab2c?source=collection_archive---------12-----------------------#2023-11-21](https://towardsdatascience.com/how-to-create-hexagon-maps-with-matplotlib-eb5eef82ab2c?source=collection_archive---------12-----------------------#2023-11-21)

## Matplotlib 教程

## 使用形状表示地理信息

[](https://medium.com/@oscarleo?source=post_page-----eb5eef82ab2c--------------------------------)[![Oscar Leo](../Images/7733c9147bad2875a35155fca3903aa8.png)](https://medium.com/@oscarleo?source=post_page-----eb5eef82ab2c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eb5eef82ab2c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----eb5eef82ab2c--------------------------------) [Oscar Leo](https://medium.com/@oscarleo?source=post_page-----eb5eef82ab2c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd7e5c1ca65b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-hexagon-maps-with-matplotlib-eb5eef82ab2c&user=Oscar+Leo&userId=d7e5c1ca65b7&source=post_page-d7e5c1ca65b7----eb5eef82ab2c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb5eef82ab2c--------------------------------) ·7 分钟阅读·2023年11月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feb5eef82ab2c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-hexagon-maps-with-matplotlib-eb5eef82ab2c&user=Oscar+Leo&userId=d7e5c1ca65b7&source=-----eb5eef82ab2c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feb5eef82ab2c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-hexagon-maps-with-matplotlib-eb5eef82ab2c&source=-----eb5eef82ab2c---------------------bookmark_footer-----------)![](../Images/8a0c286c2ed1192c80240ba0a37770bf.png)

作者创建的图表

**让我们做一些地图吧！ 🗺**

嗨，欢迎来到新的 matplotlib 教程。这次，我将教你如何创建像上面那样有洞察力的六边形地图。

可视化地理信息是困难的，因为区域（如国家）的大小和形状各异。

结果是，当你使用常规地图绘制数据时，有些区域难以看清。

另外，向可视化中添加诸如国家名称或数值的信息也是困难的。

解决这种差异的一种替代方法是使用六边形地图。

这个想法是将每个区域表示为六边形，并将它们排列成类似实际地图的方式。

由于每个六边形形状相同，因此很容易以结构化的方式添加信息，并创建美观的数据可视化。

本教程教你如何使用来自美国总统选举的数据来实现这一点。

(别忘了查看一下[我的其他 Matplotlib 教程](https://medium.com/@oscarleo/list/262e5d7f0847)哦)
