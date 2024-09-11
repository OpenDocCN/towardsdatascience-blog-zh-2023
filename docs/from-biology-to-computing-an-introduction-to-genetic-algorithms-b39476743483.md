# 遗传算法简介

> 原文：[https://towardsdatascience.com/from-biology-to-computing-an-introduction-to-genetic-algorithms-b39476743483?source=collection_archive---------12-----------------------#2023-04-17](https://towardsdatascience.com/from-biology-to-computing-an-introduction-to-genetic-algorithms-b39476743483?source=collection_archive---------12-----------------------#2023-04-17)

## 发现进化计算的力量以及如何将其应用于日常问题

[](https://medium.com/@egorhowell?source=post_page-----b39476743483--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----b39476743483--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b39476743483--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b39476743483--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----b39476743483--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-biology-to-computing-an-introduction-to-genetic-algorithms-b39476743483&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----b39476743483---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b39476743483--------------------------------) · 6 min read · 2023年4月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb39476743483&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-biology-to-computing-an-introduction-to-genetic-algorithms-b39476743483&user=Egor+Howell&userId=1cac491223b2&source=-----b39476743483---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb39476743483&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-biology-to-computing-an-introduction-to-genetic-algorithms-b39476743483&source=-----b39476743483---------------------bookmark_footer-----------)![](../Images/e410603becb8f4c414e88ace60f06e32.png)

图片来源：[Warren Umoh](https://unsplash.com/@warrenumoh?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

当查尔斯·达尔文在19世纪发展他的[***进化论***](https://en.wikipedia.org/wiki/Evolution)时，他没有意识到这一理论将对计算算法产生深远的影响***。***

自然选择、适者生存、突变和交叉的思想最终导致‘最佳’变体存活并携带‘最佳’特征。约翰·亨利·霍兰德在1975年将这些概念发扬光大，开创了[***元启发式***](https://en.wikipedia.org/wiki/Metaheuristic)[***遗传算法***](https://en.wikipedia.org/wiki/Genetic_algorithm)来解决数学优化问题。这并不是生物过程首次影响机器学习。也许最著名的是1958年弗兰克·罗森布拉特开发的[***神经网络感知机***](https://en.wikipedia.org/wiki/Perceptron)，灵感来自大脑中的真实神经元。

大多数遗传算法用于解决[***组合优化***](https://en.wikipedia.org/wiki/Combinatorial_optimization)问题，这些问题由于排列组合的巨大数量而使得[***暴力搜索***](https://en.wikipedia.org/wiki/Brute-force_search)不可行。例子包括[***背包问题***](https://en.wikipedia.org/wiki/Knapsack_problem)和[***旅行商问题***](https://en.wikipedia.org/wiki/Travelling_salesman_problem)。

如果你是数据科学家，你很可能会在职业生涯中遇到这些问题，因此了解如何处理它们是值得的，这篇文章将介绍一些最佳且最…
