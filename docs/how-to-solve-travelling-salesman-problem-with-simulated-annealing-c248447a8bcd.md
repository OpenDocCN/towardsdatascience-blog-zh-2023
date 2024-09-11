# 如何使用模拟退火解决旅行商问题

> 原文：[https://towardsdatascience.com/how-to-solve-travelling-salesman-problem-with-simulated-annealing-c248447a8bcd?source=collection_archive---------0-----------------------#2023-02-10](https://towardsdatascience.com/how-to-solve-travelling-salesman-problem-with-simulated-annealing-c248447a8bcd?source=collection_archive---------0-----------------------#2023-02-10)

## 使用模拟退火优化算法解决旅行商问题的最佳方案

[](https://medium.com/@egorhowell?source=post_page-----c248447a8bcd--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----c248447a8bcd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c248447a8bcd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c248447a8bcd--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----c248447a8bcd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-solve-travelling-salesman-problem-with-simulated-annealing-c248447a8bcd&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----c248447a8bcd---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c248447a8bcd--------------------------------) ·6分钟阅读·2023年2月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc248447a8bcd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-solve-travelling-salesman-problem-with-simulated-annealing-c248447a8bcd&user=Egor+Howell&userId=1cac491223b2&source=-----c248447a8bcd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc248447a8bcd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-solve-travelling-salesman-problem-with-simulated-annealing-c248447a8bcd&source=-----c248447a8bcd---------------------bookmark_footer-----------)![](../Images/32255d463d6594ee2b3194c9ece853c7.png)

图片由[Annie Spratt](https://unsplash.com/de/@anniespratt?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

[***旅行推销员问题 (TSP)***](https://en.wikipedia.org/wiki/Travelling_salesman_problem)是[***组合优化***](https://en.wikipedia.org/wiki/Combinatorial_optimization)和数学中普遍存在的问题。这个问题提出了一个问题：“给定城市列表及其距离，访问每个城市一次并返回原始城市的最短路径是什么？”

> TSP 的一个更正式的数学定义是[**哈密顿回路**](https://en.wikipedia.org/wiki/Hamiltonian_path)。

这个问题之所以著名，是因为它是[***NP-困难***](https://en.wikipedia.org/wiki/NP-hardness)的。***这意味着，随着新城市的增加，无法在合理时间内找到最优解，因为这会导致[***组合爆炸***](https://en.wikipedia.org/wiki/Combinatorial_explosion)的可能路径数量。例如，***4*** 个城市的可能路径数量是 ***3***，***6*** 个城市是 ***60***，然而 ***20*** 个城市则是巨大的 ***60,822,550,200,000,000!***

> TSP 的解的数量是 **(n-1)!/2**，其中 **n** 是城市的数量。对于大的 **n**，根据现代计算标准，解的数量是[**不可处理的**](https://en.wiktionary.org/wiki/intractable)。

所以这就引出了一个问题，我们如何找到最短的距离，如果我们有一些……
