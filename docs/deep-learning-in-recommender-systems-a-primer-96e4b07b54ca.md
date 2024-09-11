# 推荐系统中的深度学习：入门指南

> 原文：[https://towardsdatascience.com/deep-learning-in-recommender-systems-a-primer-96e4b07b54ca?source=collection_archive---------5-----------------------#2023-06-26](https://towardsdatascience.com/deep-learning-in-recommender-systems-a-primer-96e4b07b54ca?source=collection_archive---------5-----------------------#2023-06-26)

## 现代工业推荐系统背后最重要的技术突破概述

[](https://medium.com/@samuel.flender?source=post_page-----96e4b07b54ca--------------------------------)[![Samuel Flender](../Images/390d82a673de8a8bb11cef66978269b5.png)](https://medium.com/@samuel.flender?source=post_page-----96e4b07b54ca--------------------------------)[](https://towardsdatascience.com/?source=post_page-----96e4b07b54ca--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----96e4b07b54ca--------------------------------) [Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----96e4b07b54ca--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce56d9dcd568&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-learning-in-recommender-systems-a-primer-96e4b07b54ca&user=Samuel+Flender&userId=ce56d9dcd568&source=post_page-ce56d9dcd568----96e4b07b54ca---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----96e4b07b54ca--------------------------------) · 9分钟阅读 · 2023年6月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F96e4b07b54ca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-learning-in-recommender-systems-a-primer-96e4b07b54ca&user=Samuel+Flender&userId=ce56d9dcd568&source=-----96e4b07b54ca---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F96e4b07b54ca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-learning-in-recommender-systems-a-primer-96e4b07b54ca&source=-----96e4b07b54ca---------------------bookmark_footer-----------)![](../Images/b5168e4df2a891b3242b01347e9ac486.png)

图片来源：[Pixabay](https://pixabay.com/illustrations/network-cloud-computing-data-4851119/)

推荐系统是当前发展最快的工业机器学习应用之一。从商业角度来看，这并不令人惊讶：更好的推荐带来更多用户。就是这么简单。

然而，底层技术远非简单。自从深度学习的兴起——[由GPU的商品化推动](/algorithms-are-not-enough-fdee1d65e536)——推荐系统变得越来越复杂。

在这篇文章中，我们将深入探讨过去十年间几个最重要的建模突破，大致重构标志深度学习在推荐系统中崛起的关键点。这是一个技术突破、科学探索的故事，以及跨越大陆和合作的军备竞赛。

系好安全带。我们的旅程从2017年的新加坡开始。

# NCF（新加坡大学，2017年）

![](../Images/fd240f60ca9cd1b84333433f89cc813c.png)

图片来源：[He et al (2017)](https://arxiv.org/abs/1708.05031)
