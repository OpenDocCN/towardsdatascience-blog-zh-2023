# 强大的优化技术介绍：模拟退火

> 原文：[https://towardsdatascience.com/an-introduction-to-a-powerful-optimization-technique-simulated-annealing-87fd1e3676dd?source=collection_archive---------0-----------------------#2023-03-15](https://towardsdatascience.com/an-introduction-to-a-powerful-optimization-technique-simulated-annealing-87fd1e3676dd?source=collection_archive---------0-----------------------#2023-03-15)

![](../Images/14be4f75d59bab24c155145334bc6078.png)

这完全是关于温度的。图像由 Dall-E 2 制作。

## 解释、参数、优点、缺点及应用案例

[](https://hennie-de-harder.medium.com/?source=post_page-----87fd1e3676dd--------------------------------)[![Hennie de Harder](../Images/3e4f2cccd6cb976ca3f8bf15597daea8.png)](https://hennie-de-harder.medium.com/?source=post_page-----87fd1e3676dd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----87fd1e3676dd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----87fd1e3676dd--------------------------------) [Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----87fd1e3676dd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffb96be98b7b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-introduction-to-a-powerful-optimization-technique-simulated-annealing-87fd1e3676dd&user=Hennie+de+Harder&userId=fb96be98b7b9&source=post_page-fb96be98b7b9----87fd1e3676dd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----87fd1e3676dd--------------------------------) ·9分钟阅读·2023年3月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F87fd1e3676dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-introduction-to-a-powerful-optimization-technique-simulated-annealing-87fd1e3676dd&user=Hennie+de+Harder&userId=fb96be98b7b9&source=-----87fd1e3676dd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F87fd1e3676dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-introduction-to-a-powerful-optimization-technique-simulated-annealing-87fd1e3676dd&source=-----87fd1e3676dd---------------------bookmark_footer-----------)

**模拟退火是一种优化技术，旨在找到数学优化问题的全局最优解。它是一种非常出色的技术，你可以将其应用于广泛的问题。在这篇文章中，我们将深入探讨模拟退火的定义、优点、缺点及应用案例。**

你不应该有最喜欢的机器学习模型或优化技术。但是，人们确实有他们的最爱，比如LightGBM。在优化方面，我喜欢的一种技术是模拟退火。它容易理解，并适用于许多不同类型的问题。

> 这篇文章需要关于数学优化问题和局部搜索的知识。如果你没有这些知识，[这篇文章](https://medium.com/towards-data-science/mathematical-optimization-heuristics-every-data-scientist-should-know-b26de0bd43e6)是一个很好的起点。

# 什么是模拟退火？

模拟退火（SA）是一种随机优化算法，用于寻找成本函数的全局最小值。它是一种局部搜索的元启发式算法，用于解决陷入局部最小值的问题。该算法基于[冶金中的退火](https://www.chemeurope.com/en/encyclopedia/Annealing_%28metallurgy%29.html)物理过程，其中金属被加热然后缓慢冷却。通过这样做……
