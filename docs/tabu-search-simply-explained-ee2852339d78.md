# 什么是 Tabu 搜索？

> 原文：[`towardsdatascience.com/tabu-search-simply-explained-ee2852339d78?source=collection_archive---------12-----------------------#2023-03-13`](https://towardsdatascience.com/tabu-search-simply-explained-ee2852339d78?source=collection_archive---------12-----------------------#2023-03-13)

## 对 Tabu 搜索优化算法的直观解释，以及如何将其应用于旅行商问题

[](https://medium.com/@egorhowell?source=post_page-----ee2852339d78--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----ee2852339d78--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ee2852339d78--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ee2852339d78--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----ee2852339d78--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftabu-search-simply-explained-ee2852339d78&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----ee2852339d78---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ee2852339d78--------------------------------) ·5 分钟阅读·2023 年 3 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fee2852339d78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftabu-search-simply-explained-ee2852339d78&user=Egor+Howell&userId=1cac491223b2&source=-----ee2852339d78---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fee2852339d78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftabu-search-simply-explained-ee2852339d78&source=-----ee2852339d78---------------------bookmark_footer-----------)![](img/6d13a379bededdf19a70772ac5ff5521.png)

照片由 [Clint Adair](https://unsplash.com/@clintadair?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

在我之前的一篇[文章](https://medium.com/towards-data-science/how-to-solve-travelling-salesman-problem-with-simulated-annealing-c248447a8bcd)中，我们讨论了[***元启发式算法***](https://en.wikipedia.org/wiki/Metaheuristic)优化算法[***模拟退火***](https://medium.com/towards-data-science/how-to-solve-travelling-salesman-problem-with-simulated-annealing-c248447a8bcd)***。*** 这是一种随机搜索算法，用于尝试寻找[***全局最优***](https://en.wikipedia.org/wiki/Maximum_and_minimum)解，在如著名的[***旅行商问题 (TSP)***](https://en.wikipedia.org/wiki/Travelling_salesman_problem)和[***背包问题***](https://en.wikipedia.org/wiki/Knapsack_problem)***]这样的问题中，尤其是[***组合优化***](https://en.wikipedia.org/wiki/Combinatorial_optimization)问题中。

还有另一种类似的算法，名为[***禁忌搜索***](https://en.wikipedia.org/wiki/Tabu_search)***，*** 可以看作是模拟退火算法的推广。在这篇文章中，我想讨论并解释禁忌搜索，回顾 TSP，然后在 Python 中实现禁忌搜索来解决 TSP。

# 禁忌搜索

## 概述

禁忌搜索是一种[由 Fred Glover 在 1980 年代末期提出的元启发式优化算法](https://www.sciencedirect.com/science/article/abs/pii/0305054886900481?via%3Dihub=)。与模拟退火类似，禁忌搜索使用[***局部搜索***](https://en.wikipedia.org/wiki/Local_search_%28optimization%29)，但可以接受较差的解以避免陷入***局部最小值。*** 其另一个主要关键成分是通过使用*记忆结构*来防止算法访问之前观察过的解，从而更广泛地探索搜索空间。换句话说，它有一个‘TABU’列表！
