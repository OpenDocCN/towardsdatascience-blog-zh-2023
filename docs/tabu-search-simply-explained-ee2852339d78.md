# 什么是禁忌搜索？

> 原文：[`towardsdatascience.com/tabu-search-simply-explained-ee2852339d78`](https://towardsdatascience.com/tabu-search-simply-explained-ee2852339d78)

## 禁忌搜索优化算法的直观解释及其在旅行商问题中的应用

[](https://medium.com/@egorhowell?source=post_page-----ee2852339d78--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----ee2852339d78--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ee2852339d78--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ee2852339d78--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----ee2852339d78--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ee2852339d78--------------------------------) ·5 分钟阅读·2023 年 3 月 13 日

--

![](img/6d13a379bededdf19a70772ac5ff5521.png)

图片由[Clint Adair](https://unsplash.com/@clintadair?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

在我的[上一篇文章](https://medium.com/towards-data-science/how-to-solve-travelling-salesman-problem-with-simulated-annealing-c248447a8bcd)中，我们讨论了[***元启发式***](https://en.wikipedia.org/wiki/Metaheuristic)优化算法[***模拟退火***](https://medium.com/towards-data-science/how-to-solve-travelling-salesman-problem-with-simulated-annealing-c248447a8bcd)***。*** 这是一种随机搜索算法，用于尝试在[***组合优化***](https://en.wikipedia.org/wiki/Combinatorial_optimization)问题中找到[***全局最优解***](https://en.wikipedia.org/wiki/Maximum_and_minimum)，如著名的[***旅行商问题 (TSP)***](https://en.wikipedia.org/wiki/Travelling_salesman_problem)和[***背包问题***](https://en.wikipedia.org/wiki/Knapsack_problem)***。***

另一种类似的算法称为[***禁忌搜索***](https://en.wikipedia.org/wiki/Tabu_search)***，*** 它可以被视为模拟退火算法的推广。在这篇文章中，我想讨论和解释禁忌搜索，回顾旅行商问题（TSP），然后在 Python 中实现禁忌搜索来解决 TSP。

# 禁忌搜索

## 概述

Tabu Search 是一种元启发式优化算法，[由 Fred Glover 在 1980 年代末期构思](https://www.sciencedirect.com/science/article/abs/pii/0305054886900481?via%3Dihub=)。类似于模拟退火，Tabu Search 使用 [***局部搜索***](https://en.wikipedia.org/wiki/Local_search_%28optimization%29)，但可以接受更差的解以避免陷入 ***局部最小值***。它的另一个主要关键成分是它使用 *记忆结构* 防止算法访问之前观察过的解，从而更广泛地探索搜索空间。换句话说，它有一个‘TABU’列表！

Tabu Search 算法可以用来解决各种各样的问题：

+   [***资源分配***](https://en.wikipedia.org/wiki/Resource_allocation)

+   [***调度***](https://optimoroute.com/schedule-optimization/)

+   [***供应链优化***](https://en.wikipedia.org/wiki/Supply_chain_optimization)

因此，学习和理解它是值得的，因为它可以应用于多种情况。

## 记忆结构、任期和 Tabu List

如上所述，Tabu Search 记录之前访问过的解，这被称为 ***Tabu List***，并在特定时间内将它们保存在记忆中，***任期***，以防止解的回收并更好地 *探索* 搜索空间。

一般来说，Tabu Search 使用两种类型的 ***记忆结构***：

+   ***短期：*** 这通常是一定数量的之前访问过的解，我们不应回到这些解上。

+   ***长期：*** 这是为了在搜索遇到困境时提供帮助，并帮助扩展搜索范围。

实际上，没有要求一定要有一个或两个，甚至两个都不需要。主要的思想是我们跟踪算法正在做什么，并帮助其探索更广泛的可能解。

## 算法概述与愿望准则

Tabu Search 的一般流程如下：

+   生成一个初始有效解。

+   使用从当前解出发的局部搜索获取可能的邻域解集。

+   从这些邻域解中，获取一个不在 Tabu List 上的最佳候选解。

+   将这个最佳候选解与迄今为止找到的最佳解进行比较，并根据需要进行分配。

+   使用最佳候选解更新 tabu 列表。

+   使用最佳候选解重复步骤 2-5 以生成新的邻域，直到满足某些停止条件。

另一个规则是，如果我们发现一个在 Tabu List 上的解但其目标函数比当前最佳解更好，我们仍然接受这个解。这被称为 ***愿望准则***。

就这样！如果你对这个过程还有疑问，请继续阅读，我们将用 Python 实现这个理论以使其更为具体。

# 旅行推销员问题

在使用 Tabu Search 解决旅行推销员问题（TSP）之前，值得快速讨论一下什么是 TSP。

TSP 可能是最著名且最易于理解的组合优化问题。问题很简单：“给定一组城市，什么是访问每个城市一次并返回原始城市的最短路线？”

这个问题难以解决的原因是它是[***NP 难***](https://en.wikipedia.org/wiki/NP-hardness)的，随着我们需要访问的城市数量增加，可能路线的数量会[***组合爆炸***](https://en.wikipedia.org/wiki/Combinatorial_explosion)。例如，[对 20 个城市进行穷举所有解需要约 2000 年！](https://www.sciencedirect.com/topics/earth-and-planetary-sciences/traveling-salesman-problem)

> 可能路线的数量按**(n-1)!/2** 计算

由于 TSP 在特定数量城市下的难解性，我们需要 resort to heuristics such as Tabu Search and Simulated Annealing to provide sufficient solutions in a reasonable amount of time.

# 用 Python 实现 TSP 的禁忌搜索

## 算法设计

首先列出一些伪代码，说明我们如何实现 TSP 的禁忌搜索：

+   生成一个初始路线并用这个初始路线更新禁忌列表

+   从这个初始路线中，通过交换当前路线中的相邻城市来生成邻域

+   从这个邻域中获取最短的最佳邻域路线，该路线不在禁忌列表上

+   将最佳邻域路线与找到的最佳总体路线进行比较，并根据需要更新

+   使用当前最佳邻域路线重复步骤 1–3，以产生新的邻域

这是一个相当基础的禁忌搜索算法，因为它仅包含短期记忆结构。

## Python 代码

以下是实现上述算法的通用类。该类只需要一个`initial_solution`，即城市的某个顺序列表，以及一个将城市映射到其坐标的字典`cities`。

作者的 Github Gist。

现在让我们对一些合成生成的数据集运行这个类：

![](img/f34fbeaa968e2025b685dda27054fc95.png)

由作者在 Python 中生成的图。

![](img/404d30d8c01e955c659ea2238b9e6f1c.png)

由作者在 Python 中生成的图。

最佳解决方案看起来是一个相当合理的结果，而且没有花费我们数千年的时间来计算！

# 总结与进一步思考

在这篇文章中，我们解释了元启发式禁忌搜索算法。这个优化算法使用局部搜索技术，但仍可以通过接受更差的解决方案来逃避局部最小值。它还利用了禁忌列表，阻止它过渡到之前访问过的解决方案，并更大程度地探索搜索空间。应用于旅行推销员问题时，这个算法取得了很好的结果。

完整代码可以在我的 GitHub 上找到：

[## Medium-Articles/Optimisation/tabu-search at main · egorhowell/Medium-Articles](https://github.com/egorhowell/Medium-Articles/tree/main/Optimisation/tabu-search?source=post_page-----ee2852339d78--------------------------------)

### 目前无法执行该操作。您已在另一个标签页或窗口中登录。您在另一个标签页或…

[github.com](https://github.com/egorhowell/Medium-Articles/tree/main/Optimisation/tabu-search?source=post_page-----ee2852339d78--------------------------------)

# 另一个事项！

我有一个免费的新闻通讯，[**Dishing the Data**](https://dishingthedata.substack.com/)，每周分享成为更好的数据科学家的技巧。没有“废话”或“点击诱饵”，只有来自实践数据科学家的纯粹可操作见解。

[## Dishing The Data | Egor Howell | Substack](https://newsletter.egorhowell.com/?source=post_page-----ee2852339d78--------------------------------)

### 如何成为更好的数据科学家。点击阅读《Dishing The Data》，由 Egor Howell 编写，是 Substack 出版物，包含…

[newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----ee2852339d78--------------------------------)

# 连接与我！

+   [**YouTube**](https://www.youtube.com/@egorhowell)

+   [**LinkedIn**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)

# 参考文献与进一步阅读

+   [*优化算法*](https://mitpress.mit.edu/9780262039420/algorithms-for-optimization/)**。** [Mykel J. Kochenderfer](https://mitpress.mit.edu/author/mykel-j-kochenderfer-18773) 和 [Tim A. Wheeler](https://mitpress.mit.edu/author/tim-a-wheeler-28144)。***2019***。
