# SciPy 优化约束介绍

> 原文：[https://towardsdatascience.com/introduction-to-optimization-constraints-with-scipy-7abd44f6de25?source=collection_archive---------2-----------------------#2023-01-31](https://towardsdatascience.com/introduction-to-optimization-constraints-with-scipy-7abd44f6de25?source=collection_archive---------2-----------------------#2023-01-31)

## 探索界限、线性和非线性约束，通过实际的 Python 示例进行学习

[](https://nicolo-albanese.medium.com/?source=post_page-----7abd44f6de25--------------------------------)[![Nicolo Cosimo Albanese](../Images/9a2c26207146741b58c3742927d09450.png)](https://nicolo-albanese.medium.com/?source=post_page-----7abd44f6de25--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7abd44f6de25--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7abd44f6de25--------------------------------) [Nicolo Cosimo Albanese](https://nicolo-albanese.medium.com/?source=post_page-----7abd44f6de25--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7430df412ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-optimization-constraints-with-scipy-7abd44f6de25&user=Nicolo+Cosimo+Albanese&userId=7430df412ec&source=post_page-7430df412ec----7abd44f6de25---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7abd44f6de25--------------------------------) ·8分钟阅读·2023年1月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7abd44f6de25&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-optimization-constraints-with-scipy-7abd44f6de25&user=Nicolo+Cosimo+Albanese&userId=7430df412ec&source=-----7abd44f6de25---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7abd44f6de25&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-optimization-constraints-with-scipy-7abd44f6de25&source=-----7abd44f6de25---------------------bookmark_footer-----------)![](../Images/d21debedc54b93104a0d8720c23d510f.png)

图片作者提供。

# 目录

1.  [介绍](#d414)

1.  [实现](#5bb0)

    2.1 [无约束优化](#b0a9)

    2.2 [界限](#5ca2)

    2.3 [线性约束](#a9d0)

    2.4 [非线性约束](#e5dd)

    2.5 [组合不同约束类型](#694a)

1.  [结论](#1887)

# 1\. 介绍

优化是从一组潜在候选中选择最佳元素以达到特定目标的过程。

我们在日常生活中会执行许多优化任务：寻找到达目的地的最短或最快路线，准备按优先级排序的待办事项清单，购买杂货。

我们可以从定义**目标函数** `**f(x)**` 开始描述这些问题。

设想一下，我们正在组织一次到另一个城市的旅行，并试图评估一个合适的出发时间。在这个例子中，目标函数 `f(x)` 是出发时间 `x` 的旅行持续时间。

我们可以将优化问题公式化为**目标函数的最小值或最大值的确定**。在我们的……
