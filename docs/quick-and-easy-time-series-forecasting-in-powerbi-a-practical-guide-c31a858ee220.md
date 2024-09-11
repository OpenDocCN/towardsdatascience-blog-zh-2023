# 快速且简便的 PowerBI 时间序列预测：实用指南

> 原文：[https://towardsdatascience.com/quick-and-easy-time-series-forecasting-in-powerbi-a-practical-guide-c31a858ee220?source=collection_archive---------3-----------------------#2023-05-01](https://towardsdatascience.com/quick-and-easy-time-series-forecasting-in-powerbi-a-practical-guide-c31a858ee220?source=collection_archive---------3-----------------------#2023-05-01)

## 如何在几次点击中设置和配置预测系统

[](https://thomasdorfer.medium.com/?source=post_page-----c31a858ee220--------------------------------)[![Thomas A Dorfer](../Images/9258a1735cee805f1d9b02e2adf01096.png)](https://thomasdorfer.medium.com/?source=post_page-----c31a858ee220--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c31a858ee220--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c31a858ee220--------------------------------) [Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----c31a858ee220--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7c54f9b62b90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquick-and-easy-time-series-forecasting-in-powerbi-a-practical-guide-c31a858ee220&user=Thomas+A+Dorfer&userId=7c54f9b62b90&source=post_page-7c54f9b62b90----c31a858ee220---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c31a858ee220--------------------------------) ·7 分钟阅读·2023 年 5 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc31a858ee220&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquick-and-easy-time-series-forecasting-in-powerbi-a-practical-guide-c31a858ee220&user=Thomas+A+Dorfer&userId=7c54f9b62b90&source=-----c31a858ee220---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc31a858ee220&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquick-and-easy-time-series-forecasting-in-powerbi-a-practical-guide-c31a858ee220&source=-----c31a858ee220---------------------bookmark_footer-----------)![](../Images/0cfe8cbe7157a42e007712a666df026e.png)

Image by the Author.

# 介绍

时间序列预测已经成为商业、政府和个人的普遍工具。现在几乎在每个可以想象的领域中都应用它：在金融中预测股票和利率，在医疗保健中预测医院床位容量，在交通运输中规划路线和交通模式，在能源领域中预测供电和需求，等等。

因此，需要一个用户友好且易于上手的工具，该工具设置快速且无缝。幸运的是，PowerBI 可以满足这一需求。通过其内置工具，用户只需几次点击即可设置和配置预测系统。

本文将为您提供一个逐步指南，介绍如何实现这一目标。但在进入实际操作之前，让我们先了解一下运行在背后的算法。

# 算法：指数平滑

PowerBI 使用指数平滑——这是一种强大的算法，能够捕捉时间序列数据中的趋势，同时抑制噪声和不必要的变化。
