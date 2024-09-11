# 爬山算法优化：初学者简单指南

> 原文：[https://towardsdatascience.com/hill-climbing-optimization-algorithm-simply-explained-dbf1e1e3cf6c?source=collection_archive---------5-----------------------#2023-03-14](https://towardsdatascience.com/hill-climbing-optimization-algorithm-simply-explained-dbf1e1e3cf6c?source=collection_archive---------5-----------------------#2023-03-14)

## 这是最流行的优化算法之一的直观解释

[](https://medium.com/@egorhowell?source=post_page-----dbf1e1e3cf6c--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----dbf1e1e3cf6c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dbf1e1e3cf6c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dbf1e1e3cf6c--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----dbf1e1e3cf6c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhill-climbing-optimization-algorithm-simply-explained-dbf1e1e3cf6c&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----dbf1e1e3cf6c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dbf1e1e3cf6c--------------------------------) · 5 min read · 2023年3月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdbf1e1e3cf6c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhill-climbing-optimization-algorithm-simply-explained-dbf1e1e3cf6c&user=Egor+Howell&userId=1cac491223b2&source=-----dbf1e1e3cf6c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdbf1e1e3cf6c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhill-climbing-optimization-algorithm-simply-explained-dbf1e1e3cf6c&source=-----dbf1e1e3cf6c---------------------bookmark_footer-----------)![](../Images/ebf4d1173e50c52791f9cb997a38767e.png)

照片由 [Isaac Burke](https://unsplash.com/@isaacburkevideo?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

许多工业和研究问题需要某种形式的[***优化***](https://en.wikipedia.org/wiki/Mathematical_optimization)以达到最佳解或结果。这些问题中的一些属于[***组合优化***](https://en.wikipedia.org/wiki/Combinatorial_optimization)类别，这意味着它们通常不能在合理的时间内通过[***穷举***](https://en.wikipedia.org/wiki/Brute-force_search)来解决。因此，我们转向[***启发式***](https://en.wikipedia.org/wiki/Heuristic)和[***元启发式***](https://en.wikipedia.org/wiki/Metaheuristic)算法，这些算法虽然不能保证找到最佳的[***全局解***](https://en.wikipedia.org/wiki/Global_optimization)，但通常能在合理的时间内计算出足够的解。

其中一种元启发式算法是[***登山算法***](https://en.wikipedia.org/wiki/Hill_climbing)***，*** 这是本文的主题。我们将深入探讨其理论、优缺点，并通过实现该算法来解决著名的[***旅行商问题 (TSP)***](https://en.wikipedia.org/wiki/Travelling_salesman_problem)。

# 登山算法

## 概述

登山算法是一种元启发式的[***迭代***](https://en.wikipedia.org/wiki/Iterative_method) [***局部搜索***](https://en.wikipedia.org/wiki/Local_search_(optimization))算法。它通过对当前解进行小的[***扰动***](https://en.wikipedia.org/wiki/Perturbation_function)来寻找最佳解，并持续这一过程直到找不到更好的解。此外，它是一种[***贪婪算法***](https://en.wikipedia.org/wiki/Greedy_algorithm)，因为它只关注进行局部最优移动，因此常常…
