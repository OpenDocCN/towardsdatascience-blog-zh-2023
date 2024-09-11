# 约束编程解释

> 原文：[https://towardsdatascience.com/constraint-programming-explained-2882dc3ad9df?source=collection_archive---------5-----------------------#2023-01-12](https://towardsdatascience.com/constraint-programming-explained-2882dc3ad9df?source=collection_archive---------5-----------------------#2023-01-12)

![](../Images/6cf518b9ff47ceb0982a963d081b8582.png)

图着色问题的解释，由 Dall-E 2 绘制。

## 约束编程求解器的核心及其与混合整数规划的关系

[](https://hennie-de-harder.medium.com/?source=post_page-----2882dc3ad9df--------------------------------)[![Hennie de Harder](../Images/3e4f2cccd6cb976ca3f8bf15597daea8.png)](https://hennie-de-harder.medium.com/?source=post_page-----2882dc3ad9df--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2882dc3ad9df--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2882dc3ad9df--------------------------------) [Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----2882dc3ad9df--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffb96be98b7b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconstraint-programming-explained-2882dc3ad9df&user=Hennie+de+Harder&userId=fb96be98b7b9&source=post_page-fb96be98b7b9----2882dc3ad9df---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2882dc3ad9df--------------------------------) ·9 分钟阅读·2023年1月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2882dc3ad9df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconstraint-programming-explained-2882dc3ad9df&user=Hennie+de+Harder&userId=fb96be98b7b9&source=-----2882dc3ad9df---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2882dc3ad9df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconstraint-programming-explained-2882dc3ad9df&source=-----2882dc3ad9df---------------------bookmark_footer-----------)

**有许多不同的方法来定义和解决优化问题。例如，你可以使用贪心算法、约束编程、混合整数规划、遗传算法或局部搜索。在这篇文章中，我们将深入探讨约束编程。作为例子，图着色问题被用来说明约束编程是如何工作的。**

如果你需要优化问题和搜索启发式算法的介绍，可以阅读下面的文章。

[](/mathematical-optimization-heuristics-every-data-scientist-should-know-b26de0bd43e6?source=post_page-----2882dc3ad9df--------------------------------) [## 每个数据科学家都应该知道的数学优化启发式方法

### 局部搜索、遗传算法等。

[towardsdatascience.com](/mathematical-optimization-heuristics-every-data-scientist-should-know-b26de0bd43e6?source=post_page-----2882dc3ad9df--------------------------------)

# 图着色

让我们从图着色问题开始。这个问题在整个文章中用于说明约束编程的概念。

对于给定的地图，你需要给每个国家上色。你可以使用无限数量的颜色。不能给相邻的国家使用相同的颜色。你需要多少种颜色才能填满地图？
