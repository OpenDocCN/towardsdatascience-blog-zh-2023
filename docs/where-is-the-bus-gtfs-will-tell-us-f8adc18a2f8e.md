# 公交车在哪里？GTFS 将告诉我们！

> 原文：[`towardsdatascience.com/where-is-the-bus-gtfs-will-tell-us-f8adc18a2f8e?source=collection_archive---------8-----------------------#2023-01-19`](https://towardsdatascience.com/where-is-the-bus-gtfs-will-tell-us-f8adc18a2f8e?source=collection_archive---------8-----------------------#2023-01-19)

## 基于 GTFS 实时数据展示荷兰公共交通工具的实时位置

[](https://leo-vander-meulen.medium.com/?source=post_page-----f8adc18a2f8e--------------------------------)![Leo van der Meulen](https://leo-vander-meulen.medium.com/?source=post_page-----f8adc18a2f8e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f8adc18a2f8e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f8adc18a2f8e--------------------------------) [Leo van der Meulen](https://leo-vander-meulen.medium.com/?source=post_page-----f8adc18a2f8e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa051ea9a35df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhere-is-the-bus-gtfs-will-tell-us-f8adc18a2f8e&user=Leo+van+der+Meulen&userId=a051ea9a35df&source=post_page-a051ea9a35df----f8adc18a2f8e---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f8adc18a2f8e--------------------------------) ·15 分钟阅读·2023 年 1 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff8adc18a2f8e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhere-is-the-bus-gtfs-will-tell-us-f8adc18a2f8e&user=Leo+van+der+Meulen&userId=a051ea9a35df&source=-----f8adc18a2f8e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff8adc18a2f8e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhere-is-the-bus-gtfs-will-tell-us-f8adc18a2f8e&source=-----f8adc18a2f8e---------------------bookmark_footer-----------)![](img/d6c103c95ab09741daa4efd1bf8340fd.png)

乌特勒支实际位置公共交通工具的地图（图片来自作者）

公共交通和开放数据的结合具有巨大的潜力。时刻表、干扰、路线，这些信息都在公共领域中，随时可以用于各种应用。这次我们将查看荷兰可用的实时信息。实时数据以 GTFS 实时格式提供，并在 [ovapi.nl](https://ovapi.nl)（荷兰地区）可以获取。

警告：需要进行一些工作才能使其运行，并实现第一个用例。

## 一般公共交通数据规范

GTFS 是一个用于共享公共交通时间表的标准，包括相关的地理信息。它由两个部分组成。首先是关于交通服务的静态信息规范（GTFS Static），其次是实时状态信息（GTFS Realtime）。

该标准起源于 2005 年，当时为了将公共交通服务集成到 Google Maps 中进行了一些努力。那时还没有标准化的时间表信息共享格式。最初，GTFS 中的 G 代表*Google*，但为了增加适用性，它被更改为…
