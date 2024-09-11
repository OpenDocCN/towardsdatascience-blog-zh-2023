# 使用 Folium 创建量身定制的旅游地图

> 原文：[https://towardsdatascience.com/create-tailor-made-tourist-maps-with-folium-954706cc04e8?source=collection_archive---------7-----------------------#2023-05-17](https://towardsdatascience.com/create-tailor-made-tourist-maps-with-folium-954706cc04e8?source=collection_archive---------7-----------------------#2023-05-17)

## 为你的下一次假期增添点儿趣味吧！

[](https://medium.com/@lee_vaughan?source=post_page-----954706cc04e8--------------------------------)[![Lee Vaughan](../Images/9f6b90bb76102f438ab0b9a4a62ffa3f.png)](https://medium.com/@lee_vaughan?source=post_page-----954706cc04e8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----954706cc04e8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----954706cc04e8--------------------------------) [李·沃恩](https://medium.com/@lee_vaughan?source=post_page-----954706cc04e8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-tailor-made-tourist-maps-with-folium-954706cc04e8&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----954706cc04e8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----954706cc04e8--------------------------------) ·7 分钟阅读·2023年5月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F954706cc04e8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-tailor-made-tourist-maps-with-folium-954706cc04e8&user=Lee+Vaughan&userId=5d604015c08b&source=-----954706cc04e8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F954706cc04e8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-tailor-made-tourist-maps-with-folium-954706cc04e8&source=-----954706cc04e8---------------------bookmark_footer-----------)![](../Images/1cfaa27bc421326f160d3d3bc945c27f.png)

图片由 [凯拉·杜洪](https://unsplash.com/@kayla_marie?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/zsqF_j9ZHXw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我对那些俗气的旅游地图情有独钟。你知道我说的是哪些地图：那些五彩斑斓、卡通风格的地图，上面有城堡、纪念碑和伦敦眼的三维渲染。要是能为你自己的假期制作一个定制版本，那该多好啊！好啦，抓紧你的护照吧，因为如果你知道 Python，那正是我们要做的事情！

在这个*快速成功的数据科学*项目中，我们将使用*Folium*库为我的冰岛度假制作一个定制的旅游地图。我们将为火山和瀑布使用自定义图标，叠加地形和道路地图，并标记住宿位置。如果你从未去过冰岛也不用担心，因为你可以将这个项目作为你*自己*旅行的模板。

# 涵盖的关键编程主题

这个项目并非全是乐趣和游戏。你还将学习一些有用的地理空间技术，例如：

1.  如何在 Folium 中堆叠基础地图切片，以在同一地图上显示多种特征类型。

1.  如何为地图上的特征使用自定义标记。

1.  如何向 Folium 地图添加静态文本并更改其大小和颜色（这并不像你想象的那么简单）。

1.  如何添加索引地图。
