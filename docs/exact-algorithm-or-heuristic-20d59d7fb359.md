# 精确算法还是启发式方法？

> 原文：[https://towardsdatascience.com/exact-algorithm-or-heuristic-20d59d7fb359?source=collection_archive---------5-----------------------#2023-02-28](https://towardsdatascience.com/exact-algorithm-or-heuristic-20d59d7fb359?source=collection_archive---------5-----------------------#2023-02-28)

![](../Images/d9bca324355d53b2b6ba3569da40cb8a.png)

一队卡车。图片由 Dall-E 2 提供。

## 逐步指南，帮助您为数学优化问题做出正确选择

[](https://hennie-de-harder.medium.com/?source=post_page-----20d59d7fb359--------------------------------)[![Hennie de Harder](../Images/3e4f2cccd6cb976ca3f8bf15597daea8.png)](https://hennie-de-harder.medium.com/?source=post_page-----20d59d7fb359--------------------------------)[](https://towardsdatascience.com/?source=post_page-----20d59d7fb359--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----20d59d7fb359--------------------------------) [Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----20d59d7fb359--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffb96be98b7b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexact-algorithm-or-heuristic-20d59d7fb359&user=Hennie+de+Harder&userId=fb96be98b7b9&source=post_page-fb96be98b7b9----20d59d7fb359---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----20d59d7fb359--------------------------------) ·6分钟阅读·2023年2月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F20d59d7fb359&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexact-algorithm-or-heuristic-20d59d7fb359&user=Hennie+de+Harder&userId=fb96be98b7b9&source=-----20d59d7fb359---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F20d59d7fb359&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexact-algorithm-or-heuristic-20d59d7fb359&source=-----20d59d7fb359---------------------bookmark_footer-----------)

**您是否在寻找解决优化问题的最佳方法？解决优化问题有两种主要方法：（元）启发式方法和精确算法。每种方法都有其自身的优缺点，选择哪种方法将取决于问题的具体特征。在这篇文章中，我们将探讨启发式方法和精确算法之间的差异，并帮助您决定哪种方法最适合您的问题。**

常用的精确算法有[线性规划](https://medium.com/towards-data-science/a-beginners-guide-to-linear-programming-and-the-simplex-algorithm-87db017e92b4)、[混合整数规划](https://medium.com/towards-data-science/how-to-handle-optimization-problems-daf97b3c248c)和[约束规划](https://medium.com/towards-data-science/constraint-programming-explained-2882dc3ad9df)。著名的[启发式算法](https://medium.com/towards-data-science/mathematical-optimization-heuristics-every-data-scientist-should-know-b26de0bd43e6)有局部搜索、遗传算法和粒子群优化。为了改进像局部搜索这样的启发式算法，将其与模拟退火或禁忌搜索等元启发式算法结合起来是很有趣的。

在本文中，我们将以一个例子开始，比较并解释精确算法和启发式算法的不同特点。接下来的部分将解释在选择精确算法或启发式算法时的一些考虑因素，首先通过流程图让你更容易做出正确的选择！

> 除了本文的考虑因素外，还有其他因素…
