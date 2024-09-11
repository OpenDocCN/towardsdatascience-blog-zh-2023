# 带重启策略的模拟退火

> 原文：[https://towardsdatascience.com/simulated-annealing-with-restart-a19a53d914c8?source=collection_archive---------7-----------------------#2023-02-13](https://towardsdatascience.com/simulated-annealing-with-restart-a19a53d914c8?source=collection_archive---------7-----------------------#2023-02-13)

## 经典模拟退火优化算法的变体及其在旅行商问题上的应用

[](https://medium.com/@egorhowell?source=post_page-----a19a53d914c8--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----a19a53d914c8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a19a53d914c8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a19a53d914c8--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----a19a53d914c8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimulated-annealing-with-restart-a19a53d914c8&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----a19a53d914c8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a19a53d914c8--------------------------------) · 阅读时间 5 分钟 · 2023年2月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa19a53d914c8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimulated-annealing-with-restart-a19a53d914c8&user=Egor+Howell&userId=1cac491223b2&source=-----a19a53d914c8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa19a53d914c8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimulated-annealing-with-restart-a19a53d914c8&source=-----a19a53d914c8---------------------bookmark_footer-----------)![](../Images/6c17609952a9449fb2de3dd465a9a0ee.png)

图片由 [Jonathan Greenaway](https://unsplash.com/es/@jogaway?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

在我之前的文章中，我们讨论了如何使用 [***旅行商问题 (TSP)***](https://en.wikipedia.org/wiki/Travelling_salesman_problem) 的 [***元启发式***](https://en.wikipedia.org/wiki/Metaheuristic) 优化算法 [***模拟退火***](https://en.wikipedia.org/wiki/Simulated_annealing) 来解决问题。你可以在这里查看那篇文章：

[](/how-to-solve-travelling-salesman-problem-with-simulated-annealing-c248447a8bcd?source=post_page-----a19a53d914c8--------------------------------) [## 如何通过模拟退火算法解决旅行商问题

### 使用模拟退火优化算法获取旅行商问题的最优解

towardsdatascience.com](/how-to-solve-travelling-salesman-problem-with-simulated-annealing-c248447a8bcd?source=post_page-----a19a53d914c8--------------------------------)

TSP 是一个著名的[***组合优化***](https://en.wikipedia.org/wiki/Combinatorial_optimization)和[***运筹学***](https://en.wikipedia.org/wiki/Operations_research)问题。其目标是找到一个销售员在***n***个城市中旅行的最短距离，每个城市访问一次，并以原始/起始城市结束。

这个问题看起来简单，但随着城市数量的增加，可能的路线数会经历[***组合爆炸***](https://en.wikipedia.org/wiki/Combinatorial_explosion)。例如，***4***个城市的可能路线数是***3***，***6***个城市是***60***，而对于***20***个城市则是巨大的***60,822,550,200,000,000!*** 实际上，对于***20***个城市来说，尝试每一条路线需要大约[***2000***年](https://www.sciencedirect.com/topics/earth-and-planetary-sciences/traveling-salesman-problem)的时间，使用[***暴力破解***](https://blog.routific.com/blog/travelling-salesman-problem)！
