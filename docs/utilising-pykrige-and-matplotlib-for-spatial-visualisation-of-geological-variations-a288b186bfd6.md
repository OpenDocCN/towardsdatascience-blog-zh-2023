# 利用 pykrige 和 matplotlib 进行地质变化的空间可视化

> 原文：[https://towardsdatascience.com/utilising-pykrige-and-matplotlib-for-spatial-visualisation-of-geological-variations-a288b186bfd6?source=collection_archive---------8-----------------------#2023-06-13](https://towardsdatascience.com/utilising-pykrige-and-matplotlib-for-spatial-visualisation-of-geological-variations-a288b186bfd6?source=collection_archive---------8-----------------------#2023-06-13)

## 探索从井壁测量中的空间地质变化

[](https://andymcdonaldgeo.medium.com/?source=post_page-----a288b186bfd6--------------------------------)[![Andy McDonald](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----a288b186bfd6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a288b186bfd6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a288b186bfd6--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----a288b186bfd6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Futilising-pykrige-and-matplotlib-for-spatial-visualisation-of-geological-variations-a288b186bfd6&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----a288b186bfd6---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a288b186bfd6--------------------------------) ·7 min read·Jun 13, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa288b186bfd6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Futilising-pykrige-and-matplotlib-for-spatial-visualisation-of-geological-variations-a288b186bfd6&user=Andy+McDonald&userId=9c280f85f15c&source=-----a288b186bfd6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa288b186bfd6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Futilising-pykrige-and-matplotlib-for-spatial-visualisation-of-geological-variations-a288b186bfd6&source=-----a288b186bfd6---------------------bookmark_footer-----------)![](../Images/b6ab3c6febc3ef3385b82c9fba52aaca.png)

挪威大陆架上声波压缩慢度测量的空间变化。作者提供的图片。

当处理地质和岩相物理数据时，我们经常希望了解这些数据在我们的领域或地区如何变化。我们可以通过对实际测量值进行网格化，并推断那些尚未通过钻孔探测的区域可能具有的数值，来实现这一目标。

进行这种外推的一个特别方法是克里金法，这是一种以南非矿业工程师Danie G. Krige命名的地统计学程序。克里金法的核心思想在于其估算技术：它利用观察数据之间的空间相关性来预测未测量位置的值。

通过衡量变量在距离上的变化，这种方法建立了一个统计关系，可以用来预测一个区域的值，将分散的数据点转换为连贯的空间地图。

在本教程中，我们将探讨一个名为[**pykrige**](https://github.com/GeoStat-Framework/PyKrige)**的Python库**。这个库被设计用于2D和3D克里金计算，使用起来简单，适用于测井数据。

# 导入库和数据
