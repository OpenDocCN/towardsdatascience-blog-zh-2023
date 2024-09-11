# 探索时间到事件的生存分析

> 原文：[`towardsdatascience.com/exploring-time-to-event-with-survival-analysis-8b0a7a33a7be?source=collection_archive---------2-----------------------#2023-11-12`](https://towardsdatascience.com/exploring-time-to-event-with-survival-analysis-8b0a7a33a7be?source=collection_archive---------2-----------------------#2023-11-12)

![](img/0881811ed71a9c0e7064a43cf24d95ba.png)

图片由 [Edge2Edge Media](https://unsplash.com/@edge2edgemedia?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 生存分析及其在 Python 中的应用

[](https://tanuwidjajaolivia.medium.com/?source=post_page-----8b0a7a33a7be--------------------------------)![Olivia Tanuwidjaja](https://tanuwidjajaolivia.medium.com/?source=post_page-----8b0a7a33a7be--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8b0a7a33a7be--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b0a7a33a7be--------------------------------) [Olivia Tanuwidjaja](https://tanuwidjajaolivia.medium.com/?source=post_page-----8b0a7a33a7be--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff43d6dd597&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-time-to-event-with-survival-analysis-8b0a7a33a7be&user=Olivia+Tanuwidjaja&userId=f43d6dd597&source=post_page-f43d6dd597----8b0a7a33a7be---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b0a7a33a7be--------------------------------) · 8 分钟阅读·2023 年 11 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8b0a7a33a7be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-time-to-event-with-survival-analysis-8b0a7a33a7be&user=Olivia+Tanuwidjaja&userId=f43d6dd597&source=-----8b0a7a33a7be---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8b0a7a33a7be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-time-to-event-with-survival-analysis-8b0a7a33a7be&source=-----8b0a7a33a7be---------------------bookmark_footer-----------)

生存分析是统计学的一个分支，专注于**分析直到某一事件发生的预期时间**。它在医疗保健领域得到了广泛应用，主要用于理解在医学试验中一个人存活的概率。

这种方法也可以应用于其他领域和用例，目的是探讨在给定时间某一事件发生的可能性。在本文中，我们将深入探讨生存分析的概念、技术及其在 Python 中的应用。

# 生存分析概念

在进行生存分析时，需要定义一个“事件”和与该事件相关的“生存时间”或持续时间。

+   **事件**：发生在研究对象身上的事物。这需要是**明确且二元**的，例如生物对象的死亡。在更模糊的领域如机器故障中，需要一个明确的定义来识别事件（即完全故障，或生产力 < X%）。

+   **生存时间 / 持续时间**：直到上述感兴趣的事件发生的时间（或观察结束）。
