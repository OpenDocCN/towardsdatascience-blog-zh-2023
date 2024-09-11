# 挑战双塔模型的极限

> 原文：[`towardsdatascience.com/pushing-the-limits-of-the-two-tower-model-a577090e5140?source=collection_archive---------2-----------------------#2023-12-10`](https://towardsdatascience.com/pushing-the-limits-of-the-two-tower-model-a577090e5140?source=collection_archive---------2-----------------------#2023-12-10)

## 双塔模型架构假设的突破点——以及如何超越这些限制

[](https://medium.com/@samuel.flender?source=post_page-----a577090e5140--------------------------------)![Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----a577090e5140--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a577090e5140--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a577090e5140--------------------------------) [Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----a577090e5140--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce56d9dcd568&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpushing-the-limits-of-the-two-tower-model-a577090e5140&user=Samuel+Flender&userId=ce56d9dcd568&source=post_page-ce56d9dcd568----a577090e5140---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a577090e5140--------------------------------) ·8 min read·2023 年 12 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa577090e5140&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpushing-the-limits-of-the-two-tower-model-a577090e5140&user=Samuel+Flender&userId=ce56d9dcd568&source=-----a577090e5140---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa577090e5140&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpushing-the-limits-of-the-two-tower-model-a577090e5140&source=-----a577090e5140---------------------bookmark_footer-----------)![](img/0368a7208d8a12f6c14723d1d837cc2c.png)

（图像由作者使用生成 AI 创建）

[双塔模型](https://medium.com/towards-data-science/the-rise-of-two-tower-models-in-recommender-systems-be6217494831)是现代推荐系统中最常见的架构设计选择之一——其关键思想是有一个塔学习相关性，另一个较浅的塔学习观察性偏差，例如位置偏差。

在这篇文章中，我们将深入探讨双塔模型背后的两个假设，特别是：

+   **分解假设**，即我们可以简单地将两个塔计算的概率相乘（或将它们的 logits 相加）的假设，

+   **位置独立假设**，即唯一决定位置偏差的变量是项目本身的位置，而不是项目所处的上下文。

我们将看到这两个假设在哪些方面会出现问题，以及如何通过更新的算法，如 MixEM 模型、点积模型和 XPA，来超越这些限制。

让我们从一个非常简短的提醒开始。
