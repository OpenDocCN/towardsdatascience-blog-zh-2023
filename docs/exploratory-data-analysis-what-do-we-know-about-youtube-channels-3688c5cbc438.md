# 探索性数据分析：我们对YouTube频道了解多少（第一部分）

> 原文：[https://towardsdatascience.com/exploratory-data-analysis-what-do-we-know-about-youtube-channels-3688c5cbc438?source=collection_archive---------3-----------------------#2023-10-28](https://towardsdatascience.com/exploratory-data-analysis-what-do-we-know-about-youtube-channels-3688c5cbc438?source=collection_archive---------3-----------------------#2023-10-28)

## 使用Pandas和YouTube数据API获取统计见解

[![Dmitrii Eliuseev](../Images/7c48f0c016930ead59ddb785eaf3e0e6.png)](https://dmitryelj.medium.com/?source=post_page-----3688c5cbc438--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3688c5cbc438--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3688c5cbc438--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----3688c5cbc438--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploratory-data-analysis-what-do-we-know-about-youtube-channels-3688c5cbc438&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----3688c5cbc438---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3688c5cbc438--------------------------------) ·20分钟阅读·2023年10月28日

--

![](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3688c5cbc438&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploratory-data-analysis-what-do-we-know-about-youtube-channels-3688c5cbc438&source=-----3688c5cbc438---------------------bookmark_footer-----------)![](../Images/c687fb3e4373288c8b36bfc7df3c92ff.png)

图片来源：Glenn Carstens-Peters，[Unsplash](https://unsplash.com/photos/turned-on-flat-screen-television-EOQhsfFBhRk)

目前，全球有超过27亿活跃的YouTube用户，对于许多人来说，YouTube不仅是娱乐，更是重要的收入来源。但它是如何运作的呢？不同的YouTube频道能获得多少观看次数或订阅者？借助Python、Pandas和YouTube数据API，我们可以获得一些有趣的见解。

## 方法论

本文将分为几个部分：

+   使用 YouTube 数据 API。通过这个 API，我们将能够获取不同搜索请求的 YouTube 频道列表。对于每个频道，我们将获得有关视频数量、观看次数和订阅者数量的信息。

+   获取我们感兴趣的频道列表。这只能做一次。

+   收集频道数据。为了获得统计洞察，我们需要在一段时间内收集数据。

+   数据分析。

事不宜迟，我们直接开始吧。

## 1\. YouTube 数据 API

首先，对所有有兴趣从大规模数据中收集信息的人来说，有个好消息…
