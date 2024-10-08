# 爬山算法优化：简单的初学者指南

> 原文：[`towardsdatascience.com/hill-climbing-optimization-algorithm-simply-explained-dbf1e1e3cf6c`](https://towardsdatascience.com/hill-climbing-optimization-algorithm-simply-explained-dbf1e1e3cf6c)

## 最流行的优化算法之一的直觉

[](https://medium.com/@egorhowell?source=post_page-----dbf1e1e3cf6c--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----dbf1e1e3cf6c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dbf1e1e3cf6c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dbf1e1e3cf6c--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----dbf1e1e3cf6c--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----dbf1e1e3cf6c--------------------------------) ·阅读时间 5 分钟·2023 年 3 月 14 日

--

![](img/ebf4d1173e50c52791f9cb997a38767e.png)

照片由[Isaac Burke](https://unsplash.com/@isaacburkevideo?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

许多工业和研究问题需要某种形式的[***优化***](https://en.wikipedia.org/wiki/Mathematical_optimization)来获得最佳解决方案或结果。其中一些问题属于[***组合优化***](https://en.wikipedia.org/wiki/Combinatorial_optimization)类别，这意味着它们通常不能在合理的时间内通过[***暴力破解***](https://en.wikipedia.org/wiki/Brute-force_search)解决。因此，我们转向[***启发式***](https://en.wikipedia.org/wiki/Heuristic)和[***元启发式***](https://en.wikipedia.org/wiki/Metaheuristic)算法，这些算法虽然不能保证找到最佳的[***全局解决方案***](https://en.wikipedia.org/wiki/Global_optimization)，但通常能在合理的时间内计算出足够的解决方案。

其中一种元启发式算法是[***爬山算法***](https://en.wikipedia.org/wiki/Hill_climbing)***，***这是本文的主题。我们将深入探讨理论、优缺点，并通过实现该算法来解决著名的[***旅行商问题 (TSP)***](https://en.wikipedia.org/wiki/Travelling_salesman_problem)。

# 爬山算法

## 概述

攀登算法是一种元启发式的 [***迭代***](https://en.wikipedia.org/wiki/Iterative_method) [***局部搜索***](https://en.wikipedia.org/wiki/Local_search_(optimization)) 算法。它通过对当前解决方案进行小的 [***扰动***](https://en.wikipedia.org/wiki/Perturbation_function) 并继续这一过程，直到找不到更好的解决方案，旨在找到最佳解决方案。此外，它是一种 [***贪心算法***](https://en.wikipedia.org/wiki/Greedy_algorithm)，因为它只关注局部最优的移动，因此常常会陷入 [***局部最优***](https://mathworld.wolfram.com/LocalMinimum.html)（见下图）。

![](img/9487cb5390a0f374f48023ac0a8fd25d.png)

一个包含局部和全局最优的函数示例。图由作者提供。

## 算法

攀登算法的一般流程如下：

+   *生成初始解决方案，该解决方案现在是最佳解决方案。*

+   *从最佳解决方案中选择一个邻域解决方案。*

+   *如果邻域解决方案优于最佳解决方案，则将最佳解决方案设置为等于邻域解决方案。*

+   *重复上述两个步骤，直到邻域解决方案不优于最佳解决方案，或满足其他终止条件，如迭代次数。*

如果这仍然显得有些模糊，不用担心！稍后我们将在 Python 中应用上述算法到一个实际的例子中。

## 类型

爬山算法有多种类型和变体。以下是最常见的几种：

+   ***简单爬山算法：*** *仅考虑最近的邻域。*

+   ***最陡爬升爬山算法：*** *考虑所有邻域并选择最佳方案。*

+   ***随机爬山算法：*** *随机选择一个邻域。*

最陡爬升版本会带来更优的性能，但需要更多的计算资源。

## 优缺点

让我们简要列出爬山算法的主要优缺点：

***优点：***

+   *非常直观，易于向同事、利益相关者等解释。*

+   *可以应用于* [***连续***](https://uwaterloo.ca/combinatorics-and-optimization/research-combinatorics-and-optimization/research-areas/continuous-optimization) *和* [***离散***](https://en.wikipedia.org/wiki/Discrete_optimization) *目标函数和问题。*

+   *能够解决各种不同的问题。*

***缺点：***

+   *可能会陷入* [***局部极小值/极大值***](https://en.wikipedia.org/wiki/Maximum_and_minimum) *，因此不能保证找到最佳的全局解决方案。*

+   *可能出现平坦区域，其中所有邻域具有相同的目标分数。*

还有更复杂的算法，如 [***模拟退火***](https://en.wikipedia.org/wiki/Simulated_annealing) 和 [***禁忌搜索***](https://en.wikipedia.org/wiki/Tabu_search)***，*** 类似于爬山算法，但不会陷入局部极小值，并且能更广泛地探索搜索空间。要了解更多关于这些算法的信息，请查看我之前的帖子：

如何用模拟退火算法解决旅行推销员问题 ## 如何用模拟退火解决旅行推销员问题

### 使用模拟退火优化算法求解旅行推销员问题的最佳解决方案

[towardsdatascience.com ## Tabu Search 简单解释

### 对 Tabu Search 优化算法的直观解释以及如何将其应用于旅行推销员问题……

[towardsdatascience.com

# Python 实现示例

## 旅行推销员问题

我们将编写爬山算法代码来解决旅行推销员问题（TSP）。不过，在此之前，让我们简要说明和解释一下我们在 TSP 中试图解决的问题。

TSP 是优化中的经典问题，并提出了以下问题：

*“访问给定城市列表一次并返回到起点的最短路径是什么？”*

这个问题看起来很简单，但由于[***组合爆炸***](https://en.wikipedia.org/wiki/Combinatorial_explosion)的问题，随着城市数量的增加，暴力解决方法变得计算上不可行。例如，对于 10 个城市，存在[***~300,000 个可能的*** 路径](https://www.lancaster.ac.uk/stor-i-student-sites/libby-daniells/2020/04/21/the-travelling-salesman-problem/)！

> 可行路径的数量作为城市数量**n**的函数是**(n-1)!/2\.** 因此，它的计算复杂度是 O(n!)。

当城市数量约为 20 时，暴力解决方法变得不可行，因为计算所需的时间为[***~2,000*** 年](https://www.sciencedirect.com/topics/earth-and-planetary-sciences/traveling-salesman-problem)！令人惊讶的是，对于 61 个城市，这一时间竟达到惊人的 10⁶⁷年！

## **爬山算法**解决 TSP

让我们简要列出我们将用来实现爬山算法解决 TSP 的伪代码。我们将使用最陡上升版本：

+   *生成一个初始路线并将其设为最佳解决方案。*

+   *通过交换当前最佳解决方案中的两个城市来生成邻域解列表。*

+   *从这些邻域解中获取最佳邻域解（最短距离），并将其设为当前解决方案。*

+   *将当前解决方案与最佳解决方案进行比较。如果当前解决方案更短，则将最佳解决方案设为当前解决方案。*

+   *重复此过程，直到当前解决方案比最佳解决方案更差。*

## Python 代码

以下是我们刚刚讨论的算法的`HillClimb`类的示例代码：

现在让我们运行类并绘制 20 个合成生成城市的初始解决方案和最佳解决方案的结果：

![](img/b00447a0de445beb3ae729a0e71336e5.png)

图表由作者在 Python 中生成。

![](img/0ccc240c2031cb4aecc1e9d78f63efc8.png)

图表由作者在 Python 中生成。

正如我们所见，爬山算法找到的解决方案优于初始解决方案，但显然不是全局最优解。不过，它找到了一个足够的解决方案，而且没有花费 2000 年！

# 总结与进一步思考

在这篇文章中，我们讨论了元启发式局部搜索爬山算法。该算法对最佳解决方案进行小的增量扰动，直到达到一个变化不再带来更好解决方案的点。该算法在旅行推销员问题上表现良好，但却陷入了局部最小值，这是这种优化算法的主要缺点。

本文使用的完整代码可以在我的 GitHub 上找到：

[](https://github.com/egorhowell/Medium-Articles/tree/main/Optimisation/hill-climbing?source=post_page-----dbf1e1e3cf6c--------------------------------) [## Medium-Articles/Optimisation/hill-climbing at main · egorhowell/Medium-Articles

### 当前无法执行该操作。您已在另一个标签或窗口中登录。在另一个标签中注销了…

[github.com](https://github.com/egorhowell/Medium-Articles/tree/main/Optimisation/hill-climbing?source=post_page-----dbf1e1e3cf6c--------------------------------)

# 参考资料与进一步阅读

+   [*优化算法*](https://mitpress.mit.edu/9780262039420/algorithms-for-optimization/)*.* [Mykel J. Kochenderfer](https://mitpress.mit.edu/author/mykel-j-kochenderfer-18773) 和 [Tim A. Wheeler](https://mitpress.mit.edu/author/tim-a-wheeler-28144)。 ***2019***。

# 另一件事！

我有一个免费的通讯，[**Dishing the Data**](https://dishingthedata.substack.com/)，我每周分享成为更好的数据科学家的小贴士。没有“虚华”或“点击诱饵”，只有来自实践数据科学家的纯粹可操作的见解。

[](https://newsletter.egorhowell.com/?source=post_page-----dbf1e1e3cf6c--------------------------------) [## Dishing The Data | Egor Howell | Substack

### 如何成为更好的数据科学家。点击阅读由 Egor Howell 编写的《Dishing The Data》，这是一个 Substack 出版物，包含…

[newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----dbf1e1e3cf6c--------------------------------)

# 与我联系！

+   [**YouTube**](https://www.youtube.com/@egorhowell)

+   [**LinkedIn**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)
