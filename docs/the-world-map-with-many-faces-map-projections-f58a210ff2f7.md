# 《多面世界地图——地图投影》

> 原文：[https://towardsdatascience.com/the-world-map-with-many-faces-map-projections-f58a210ff2f7?source=collection_archive---------3-----------------------#2023-10-01](https://towardsdatascience.com/the-world-map-with-many-faces-map-projections-f58a210ff2f7?source=collection_archive---------3-----------------------#2023-10-01)

![](../Images/89cd72ef5256e73a309da79c2d5c2a6b.png)

作者提供的图片。

[](https://medium.com/@janosovm?source=post_page-----f58a210ff2f7--------------------------------)[![米兰·亚诺索夫](../Images/77b62460041f66ec4585a81baef81a03.png)](https://medium.com/@janosovm?source=post_page-----f58a210ff2f7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f58a210ff2f7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f58a210ff2f7--------------------------------) [米兰·亚诺索夫](https://medium.com/@janosovm?source=post_page-----f58a210ff2f7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F838408aa2ad4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-world-map-with-many-faces-map-projections-f58a210ff2f7&user=Milan+Janosov&userId=838408aa2ad4&source=post_page-838408aa2ad4----f58a210ff2f7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f58a210ff2f7--------------------------------) ·5分钟阅读·2023年10月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff58a210ff2f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-world-map-with-many-faces-map-projections-f58a210ff2f7&user=Milan+Janosov&userId=838408aa2ad4&source=-----f58a210ff2f7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff58a210ff2f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-world-map-with-many-faces-map-projections-f58a210ff2f7&source=-----f58a210ff2f7---------------------bookmark_footer-----------)

# 在这篇简短的文章中，我回顾了地图投影的概念，并展示了一系列使用 Python 和 Natural Earth 数据进行世界地图投影的示例。

地球或多或少是一个球体，确实是一个三维物体（尽管即使这样也存在一些挑战），而我们印刷的地图和数字屏幕是二维的。将球体转换为二维地图的中间步骤，无论是地图册还是华丽的地理信息系统应用程序，都被称为地图投影。

将地球的椭球表面映射到平面上有多种方法；然而，由于这些模型是近似的，我们通常需要注意一些不足之处。在某些投影中，相对角度和多边形（例如国家）形状被保留；在其他投影中，实际面积或特定的欧几里得距离被保持不变。这些特性也帮助你选择适合你用例的最佳投影。

至于投影类型，有多种方式，例如圆柱投影、圆锥投影、方位投影和伪圆柱投影。一个实际的转换方法是更改坐标参考系统（CRS），这时Python及其库PyProj和GeoPandas非常有用！
