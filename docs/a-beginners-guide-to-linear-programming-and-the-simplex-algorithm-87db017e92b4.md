# 线性规划及单纯形算法初学者指南

> 原文：[https://towardsdatascience.com/a-beginners-guide-to-linear-programming-and-the-simplex-algorithm-87db017e92b4?source=collection_archive---------1-----------------------#2023-01-09](https://towardsdatascience.com/a-beginners-guide-to-linear-programming-and-the-simplex-algorithm-87db017e92b4?source=collection_archive---------1-----------------------#2023-01-09)

![](../Images/3a703168db41392764a4e5273ef4a826.png)

玉米农场。Dall-E 2 的静物画。

## 解决各种优化问题

[](https://hennie-de-harder.medium.com/?source=post_page-----87db017e92b4--------------------------------)[![Hennie de Harder](../Images/3e4f2cccd6cb976ca3f8bf15597daea8.png)](https://hennie-de-harder.medium.com/?source=post_page-----87db017e92b4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----87db017e92b4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----87db017e92b4--------------------------------) [Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----87db017e92b4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffb96be98b7b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-beginners-guide-to-linear-programming-and-the-simplex-algorithm-87db017e92b4&user=Hennie+de+Harder&userId=fb96be98b7b9&source=post_page-fb96be98b7b9----87db017e92b4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----87db017e92b4--------------------------------) ·8 分钟阅读·2023年1月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F87db017e92b4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-beginners-guide-to-linear-programming-and-the-simplex-algorithm-87db017e92b4&user=Hennie+de+Harder&userId=fb96be98b7b9&source=-----87db017e92b4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F87db017e92b4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-beginners-guide-to-linear-programming-and-the-simplex-algorithm-87db017e92b4&source=-----87db017e92b4---------------------bookmark_footer-----------)

**线性规划是一种强大的工具。它在工程、金融和运筹学等多个领域都有应用。线性规划已被用于解决各种各样的问题，如航班调度和制造过程设计。在这篇博客文章中，我们将探讨线性规划的基础知识以及它如何用于解决实际问题。**

线性规划（LP）是一种数学优化技术。它用于寻找涉及多个变量和约束条件的问题的最优解。通过将问题的约束条件和目标函数表示为线性方程组和不等式，线性规划算法可以系统地搜索解空间，并识别出最大化或最小化给定目标的解决方案。

首先，我将给出一个简单的例子。然后我们将深入探讨单纯形算法，该算法在后台用于快速找到线性规划问题的最优解。

# 线性规划问题的示例

假设一个农民有120英亩的土地来种植两种作物：小麦和玉米。小麦每吨需要2英亩土地，而…
