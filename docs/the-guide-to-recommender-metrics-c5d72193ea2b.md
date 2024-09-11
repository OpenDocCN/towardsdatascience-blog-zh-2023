# 推荐系统指南

> 原文：[https://towardsdatascience.com/the-guide-to-recommender-metrics-c5d72193ea2b?source=collection_archive---------5-----------------------#2023-11-12](https://towardsdatascience.com/the-guide-to-recommender-metrics-c5d72193ea2b?source=collection_archive---------5-----------------------#2023-11-12)

## 在线评估推荐系统可能会比较棘手。

[](https://dr-robert-kuebler.medium.com/?source=post_page-----c5d72193ea2b--------------------------------)[![Dr. Robert Kübler](../Images/3b8d8b88f76c0c43d9c305e3885e7ab9.png)](https://dr-robert-kuebler.medium.com/?source=post_page-----c5d72193ea2b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c5d72193ea2b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c5d72193ea2b--------------------------------) [Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----c5d72193ea2b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d6b5fb431bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-guide-to-recommender-metrics-c5d72193ea2b&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=post_page-6d6b5fb431bf----c5d72193ea2b---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c5d72193ea2b--------------------------------) ·10 min 阅读·Nov 12, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc5d72193ea2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-guide-to-recommender-metrics-c5d72193ea2b&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=-----c5d72193ea2b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc5d72193ea2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-guide-to-recommender-metrics-c5d72193ea2b&source=-----c5d72193ea2b---------------------bookmark_footer-----------)![](../Images/a0e23c68bcbb7f9ec133307392f474e1.png)

图片由[Darius Cotoi](https://unsplash.com/@dariuscotoi?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)拍摄。

想象一下 YouTube 的主页，展示了可能会喜欢的视频，或者亚马逊建议购买更多它们销售的产品。这些都是试图展示你最有可能想与之互动的内容的**推荐系统**的例子。

假设您已经建立了一个推荐系统，并使用自己选择的方法。问题是：在将其**投入生产**并在网站上提供推荐之前，如何**离线**评估它呢？

在这篇文章中，你将确切地学到这些！此外，我还会告诉你**为什么你需要小心**这些指标。

要获得更详细的介绍，请参考我的另一篇文章，其中也展示了如何使用 TensorFlow 从零构建推荐系统。

[](/introduction-to-embedding-based-recommender-systems-956faceb1919?source=post_page-----c5d72193ea2b--------------------------------) [## 基于嵌入的推荐系统简介

### 学习在 TensorFlow 中构建一个简单的矩阵分解推荐系统

towardsdatascience.com](/introduction-to-embedding-based-recommender-systems-956faceb1919?source=post_page-----c5d72193ea2b--------------------------------)

# 推荐系统的离线评估

首先，让我们找到一个涵盖你可能设计或在实际中找到的大多数系统的推荐系统定义。
