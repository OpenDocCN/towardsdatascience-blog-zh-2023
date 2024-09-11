# 为什么所有地图都不准确？

> 原文：[`towardsdatascience.com/why-are-all-maps-inaccurate-e566f08d91fe?source=collection_archive---------5-----------------------#2023-12-31`](https://towardsdatascience.com/why-are-all-maps-inaccurate-e566f08d91fe?source=collection_archive---------5-----------------------#2023-12-31)

## 了解地图投影以及为什么它们在一些最受欢迎的地图中被使用

[](https://medium.com/@teosiyang?source=post_page-----e566f08d91fe--------------------------------)![Jake Teo](https://medium.com/@teosiyang?source=post_page-----e566f08d91fe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e566f08d91fe--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e566f08d91fe--------------------------------) [Jake Teo](https://medium.com/@teosiyang?source=post_page-----e566f08d91fe--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F52b0d82d5bf5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-are-all-maps-inaccurate-e566f08d91fe&user=Jake+Teo&userId=52b0d82d5bf5&source=post_page-52b0d82d5bf5----e566f08d91fe---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e566f08d91fe--------------------------------) ·7 min read·2023 年 12 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe566f08d91fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-are-all-maps-inaccurate-e566f08d91fe&user=Jake+Teo&userId=52b0d82d5bf5&source=-----e566f08d91fe---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe566f08d91fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-are-all-maps-inaccurate-e566f08d91fe&source=-----e566f08d91fe---------------------bookmark_footer-----------)![](img/0e3763678c08b8971449f644ba864129.png)

图片由[Kyle Glenn](https://unsplash.com/@kylejglenn?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我们大多数人知道地球是球形的，或更准确地说是一个椭球体。由于我们需要将弯曲的三维表面表示为二维的平面纸张或屏幕，这就意味着会有一些扭曲。这种将地球表面“投影”到二维图像上的方法被称为**地图投影**。

有数百种地图投影，每种投影在不同程度上最小化**形状、距离、面积和方向**的**失真**。然而，没有任何地图投影可以完全消除这些失真，因此了解每种投影的优缺点对于确定项目的使用非常重要。

# 目录

正射投影

墨卡托投影

横轴墨卡托投影

朗伯特正形圆锥投影

罗宾逊投影

总结

# 源代码

要可视化各种投影，我们可以使用 Python 库 `matplotlib` 和 `cartopy`。

```py
pip install matplotlib cartopy
```
