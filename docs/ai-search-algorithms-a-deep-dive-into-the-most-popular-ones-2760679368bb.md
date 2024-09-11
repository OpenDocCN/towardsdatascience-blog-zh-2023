# AI 搜索算法：深入了解最受欢迎的算法

> 原文：[https://towardsdatascience.com/ai-search-algorithms-a-deep-dive-into-the-most-popular-ones-2760679368bb?source=collection_archive---------11-----------------------#2023-08-23](https://towardsdatascience.com/ai-search-algorithms-a-deep-dive-into-the-most-popular-ones-2760679368bb?source=collection_archive---------11-----------------------#2023-08-23)

## 探索 AI 中最常用的四种搜索算法

[](https://polmarin.medium.com/?source=post_page-----2760679368bb--------------------------------)[![Pol Marin](../Images/a4f69a96717d453db9791f27b8f85e86.png)](https://polmarin.medium.com/?source=post_page-----2760679368bb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2760679368bb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2760679368bb--------------------------------) [Pol Marin](https://polmarin.medium.com/?source=post_page-----2760679368bb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fa43cc443e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-search-algorithms-a-deep-dive-into-the-most-popular-ones-2760679368bb&user=Pol+Marin&userId=1fa43cc443e7&source=post_page-1fa43cc443e7----2760679368bb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2760679368bb--------------------------------) ·11分钟阅读·2023年8月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2760679368bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-search-algorithms-a-deep-dive-into-the-most-popular-ones-2760679368bb&user=Pol+Marin&userId=1fa43cc443e7&source=-----2760679368bb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2760679368bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-search-algorithms-a-deep-dive-into-the-most-popular-ones-2760679368bb&source=-----2760679368bb---------------------bookmark_footer-----------)![](../Images/b054060379f1a626a70cb89b3b1a1aa8.png)

照片由[Mitchell Luo](https://unsplash.com/@mitchel3uo?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

就好像地球上的人类还不够，我们已经尝试了多年去创造像我们一样的机器。我们创建数学模型或代理，能够理性地行动，这样我们就不必依赖其他人的决策。

搜索算法曾经是使用最广泛的，但随着机器学习和深度学习的兴起，它们的使用有所减少。然而，我认为所有数据科学家都应该了解它们，因为它们是一个非常出色的工具集，在许多情况下都将证明非常有用。

它们可以应用于许多情况，但最具代表性的应用场景是游戏：井字棋、迷宫，甚至象棋……我们将使用这些例子来解释我们今天要介绍的算法。

我们将介绍四个最著名的术语，并对它们进行一些扩展，使用一些实际和直观的示例。

一如既往，请参阅本文底部的**资源**部分以获取更多信息和代码。

但在此之前，我们需要介绍一些定义，以便理解一些关键术语。

## 术语

+   **Agent**：它可以是人、模型或算法……
