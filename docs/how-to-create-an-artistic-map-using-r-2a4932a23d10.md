# 如何使用 R 创建一张艺术地图

> 原文：[`towardsdatascience.com/how-to-create-an-artistic-map-using-r-2a4932a23d10?source=collection_archive---------17-----------------------#2023-01-24`](https://towardsdatascience.com/how-to-create-an-artistic-map-using-r-2a4932a23d10?source=collection_archive---------17-----------------------#2023-01-24)

## OpenStreetMap + 数据可视化 = 艺术

[](https://medium.com/@irfanalghani11?source=post_page-----2a4932a23d10--------------------------------)![Irfan Alghani Khalid](https://medium.com/@irfanalghani11?source=post_page-----2a4932a23d10--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2a4932a23d10--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2a4932a23d10--------------------------------) [Irfan Alghani Khalid](https://medium.com/@irfanalghani11?source=post_page-----2a4932a23d10--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F44601cf05927&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-an-artistic-map-using-r-2a4932a23d10&user=Irfan+Alghani+Khalid&userId=44601cf05927&source=post_page-44601cf05927----2a4932a23d10---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2a4932a23d10--------------------------------) ·5 分钟阅读·2023 年 1 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2a4932a23d10&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-an-artistic-map-using-r-2a4932a23d10&user=Irfan+Alghani+Khalid&userId=44601cf05927&source=-----2a4932a23d10---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2a4932a23d10&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-an-artistic-map-using-r-2a4932a23d10&source=-----2a4932a23d10---------------------bookmark_footer-----------)![](img/82ec4cdcad55ffba246435203b137111.png)

阿姆斯特丹的地图。这张地图由作者制作。

> 简明扼要

本文将展示如何使用 R 创建一张艺术地图。我们将使用 OpenStreetMap 的数据，并通过 ggplot 库自定义视觉效果。

# 介绍

当你想到地图时，第一个浮现在脑海中的可能是它只是一块信息。凭借这些信息，我们可以导航到所需的位置。

随着时间的推移，地图可以通过自定义视觉效果变成一件艺术品，同时不会丧失任何关于位置的信息。

在开放数据出现之前，制作地图可能需要花费大量时间进行调查和制作地图本身。

但幸运的是，利用像 OpenStreetMap 这样的开放数据和编程，我们可以根据我们的视觉偏好创建自己的地图。如果你愿意，你可以将地图艺术品出售到市场上以获得额外收入。

在本文中，我将展示如何使用 R 创建艺术地图。废话不多说，让我们开始吧！

# 实现

## 加载库

要构建地图，我们需要几个库。这些库包括：

+   osmdata 用于下载和…
