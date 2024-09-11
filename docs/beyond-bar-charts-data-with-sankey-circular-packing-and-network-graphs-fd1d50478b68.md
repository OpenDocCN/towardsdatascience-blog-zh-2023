# 超越条形图：使用桑基图、圆形打包图和网络图的数据

> 原文：[`towardsdatascience.com/beyond-bar-charts-data-with-sankey-circular-packing-and-network-graphs-fd1d50478b68?source=collection_archive---------6-----------------------#2023-08-26`](https://towardsdatascience.com/beyond-bar-charts-data-with-sankey-circular-packing-and-network-graphs-fd1d50478b68?source=collection_archive---------6-----------------------#2023-08-26)

## 非传统可视化：何时以及何时不应运用其力量

[](https://medium.com/@MahamsMultiverse?source=post_page-----fd1d50478b68--------------------------------)![Maham Haroon](https://medium.com/@MahamsMultiverse?source=post_page-----fd1d50478b68--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fd1d50478b68--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fd1d50478b68--------------------------------) [Maham Haroon](https://medium.com/@MahamsMultiverse?source=post_page-----fd1d50478b68--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F398c9514a58b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-bar-charts-data-with-sankey-circular-packing-and-network-graphs-fd1d50478b68&user=Maham+Haroon&userId=398c9514a58b&source=post_page-398c9514a58b----fd1d50478b68---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fd1d50478b68--------------------------------) ·12 分钟阅读·2023 年 8 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffd1d50478b68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-bar-charts-data-with-sankey-circular-packing-and-network-graphs-fd1d50478b68&user=Maham+Haroon&userId=398c9514a58b&source=-----fd1d50478b68---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffd1d50478b68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-bar-charts-data-with-sankey-circular-packing-and-network-graphs-fd1d50478b68&source=-----fd1d50478b68---------------------bookmark_footer-----------)![](img/2e8aaf8196668f54a02ff892d70ad943.png)

图片来源：[Firmbee.com](https://unsplash.com/@firmbee?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/jrh5lAq-mIs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

你好！

如果你深入过数据分析的世界，你可能对条形图、折线图、散点图和饼图等图表的强大功能非常熟悉。这些可视化不仅使数据更易于访问，还能增强各种受众的理解——无论是利益相关者、客户，还是你自己在从数据中寻找洞察。然而，有时数据的复杂性需要更复杂和引人注目的展示方式。

想象一下这个场景：你正站在我们虚构公司 MM Awesome Data Inc.的一名初学数据科学家的位置上。管理层正在苦苦挣扎于将新的数据源整合到现有的数据框架中，他们真正需要了解全局。虽然饼图可能能够部分满足需求，但想象一下呈现一个流程图的影响力和魅力，例如一个引人入胜的桑基图或一个动态流图。

本文围绕这些场景展开。在广阔的数据可视化领域中，有一些隐藏的宝石往往未被充分利用。认识到我们不能讨论所有这些...
