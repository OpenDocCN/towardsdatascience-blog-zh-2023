# 面试准备：因果推断

> 原文：[https://towardsdatascience.com/interview-preparation-causal-inference-44fbb8b0a5c6?source=collection_archive---------8-----------------------#2023-08-11](https://towardsdatascience.com/interview-preparation-causal-inference-44fbb8b0a5c6?source=collection_archive---------8-----------------------#2023-08-11)

[](https://juliezhang0826.medium.com/?source=post_page-----44fbb8b0a5c6--------------------------------)[![Julie Zhang](../Images/467796767f32f9a109fc1e0afb4fee49.png)](https://juliezhang0826.medium.com/?source=post_page-----44fbb8b0a5c6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----44fbb8b0a5c6--------------------------------)[![数据科学之道](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----44fbb8b0a5c6--------------------------------) [Julie Zhang](https://juliezhang0826.medium.com/?source=post_page-----44fbb8b0a5c6--------------------------------)

·

[点击查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F556c71436dd8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finterview-preparation-causal-inference-44fbb8b0a5c6&user=Julie+Zhang&userId=556c71436dd8&source=post_page-556c71436dd8----44fbb8b0a5c6---------------------post_header-----------) 发表在 [数据科学之道](https://towardsdatascience.com/?source=post_page-----44fbb8b0a5c6--------------------------------) ·5分钟阅读·2023年8月11日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F44fbb8b0a5c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finterview-preparation-causal-inference-44fbb8b0a5c6&source=-----44fbb8b0a5c6---------------------bookmark_footer-----------)![](../Images/09410ccb66b8d7ea2d1d599fac145a52.png)

图片由 [Isaac Smith](https://unsplash.com/@isaacmsmith?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

因果推断是数据科学的核心，使我们能够解读现实世界中复杂的因果关系。本文将探讨关键的因果推断技术，了解其优缺点，深入分析现实生活中的应用，展示其重要性，并为有志于成为数据科学家的读者提供面试问题及答案，以备数据科学面试之用。

# 随机对照试验（RCTs）：

随机对照试验（RCTs）是因果推断的基础，提供了严格的因果证据。它们涉及将参与者随机分配到治疗组和对照组，确保观察到的结果差异可以归因于治疗本身。

**优点：**

+   由于随机分配，它是因果推断的金标准。

+   提供高内部效度，帮助建立强因果关系。

+   结果可以在特定条件下推广到更大的人群中。

**缺点：**

+   可能昂贵、耗时或在伦理上具有挑战性。

+   并非所有研究问题都适用。

**现实生活中的应用：**

+   药物试验…
