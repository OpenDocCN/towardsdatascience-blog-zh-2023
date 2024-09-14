# 蚁群优化算法的实际应用

> 原文：[`towardsdatascience.com/ant-colony-optimization-in-action-6d9106de60af`](https://towardsdatascience.com/ant-colony-optimization-in-action-6d9106de60af)

![](img/a404a01ac29f0af038d7f4bdf990bd20.png)

一只滑雪的蚂蚁。图片由作者使用 Dall·E 创建。

## 使用 ACO 在 Python 中解决优化问题并提升结果

[](https://hennie-de-harder.medium.com/?source=post_page-----6d9106de60af--------------------------------)![Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----6d9106de60af--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6d9106de60af--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6d9106de60af--------------------------------) [Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----6d9106de60af--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----6d9106de60af--------------------------------) ·10 分钟阅读·2023 年 9 月 20 日

--

**欢迎回来！在我的** **上一篇文章****中，我介绍了蚁群优化（ACO）的基本原理。在这一部分，我们将深入探讨如何从头开始实现 ACO 算法，以解决两种不同类型的问题。**

我们将要解决的问题是旅行商问题（TSP）和二次分配问题（QAP）。为什么是这两个问题？因为 TSP 是一个经典挑战，而 ACO 恰好是一种有效的算法，用于找到图中最具成本效益的路径。另一方面，二次分配问题代表了与优化物品排列相关的不同问题类别，在这篇文章中，我旨在展示 ACO 也可以成为解决此类分配相关问题的有价值工具。这种多功能性使得 ACO 算法适用于广泛的问题。最后，我将分享一些更快速获得改进解决方案的技巧。

# 旅行商问题

TSP（旅行商问题）描述起来很简单，但在寻找解决方案时可能会面临重大挑战。基本定义是：你的任务是发现一条访问图中所有节点的最短路径。这个问题属于[NP 难题](https://medium.com/towards-data-science/np-what-complexity-types-of-optimization-problems-explained-558d43276044)的范畴，这意味着如果你尝试探索所有可能的路径，找到解决方案可能需要不切实际的时间。相反，更有效的方法是寻求在合理时间内的高质量解决方案，这正是我们通过 ACO（蚁群优化算法）要实现的目标。

## 问题定义

使用以下代码，我们可以创建一个具有给定节点数量的 TSP 实例：

```py
import itertools
import math
import random
from typing import Tuple

import networkx as nx
import networkx.algorithms.shortest_paths.dense as nxalg

class TSP:
    """
    Creates a TSP problem with a certain number of nodes
    """
    def __init__(self, nodes: int = 30, dimensions: Tuple[int, int] = (1000, 1000), seed: int = 5):
        if seed:
            random.seed(seed)

        graph = nx.Graph()
        nodes_dict = dict()

        for i in range(nodes):
            nodes_dict[i] = (random.randint(0, dimensions[0]), random.randint(0, dimensions[1]))
            graph.add_node(i)

        for i, j in itertools.permutations(range(nodes), 2):
            graph.add_edge(i, j, weight=self.calculate_distance(nodes_dict[i], nodes_dict[j]))

        self.graph = graph
        self.nodes = nodes_dict
        self.distance_matrix = nxalg.floyd_warshall_numpy(graph)

    @staticmethod
    def calculate_distance(i, j):
        """
        Calculate the Euclidian distance between two nodes
        """
        return int(math.sqrt((i[0] - j[0])**2 + (i[1] - j[1])**2))
```

我们将用来演示 ACO 的 TSP 示例是以下的（默认设置）：

![](img/787d3843040210ad149e99c98c79184a.png)

访问所有节点并返回起始节点。图片由作者提供。

对于这个问题的**最优解**（用混合整数规划计算）如下：

![](img/20b510a5325964c37e3c89136353326c.png)

旅行推销员问题的**最优解**。图片由作者提供。

这条路径的距离是 4897。

## 用 ACO 解决 TSP

下一步是用蚁群优化解决这个问题，看看我们能接近**最优解**到什么程度。如果你不熟悉 ACO，想了解算法如何工作，可以阅读我之前的文章。然后，你可以回到这里查看 ACO 的实际应用。

ACO 的代码：

```py
import datetime
import json
import logging
import random 
import time

import matplotlib.pyplot as plt
import numpy as np

from problem import TSP

class AntColonyOptimization:
    """
    Ant colony optimization algorithm for finding the shortest route in a graph.

    Parameters:
        m = number of ants
        k_max = number of iterations
        alpha = pheromone importance
        beta = distance importance
        rho = pheromone evaporation rate
        Q = pheromone deposit
        tau = pheromone
        eta = distance
    """
    def __init__(self, problem, **kwargs):
        self.graph = problem.graph
        self.nodes = list(problem.nodes)
        self.coordinates = list(problem.nodes.values())
        self.n = len(self.nodes)
        self.distance_matrix = problem.distance_matrix

        self.m = kwargs.get("m", 100)
        self.k_max = kwargs.get("k_max", 50)
        self.alpha = kwargs.get("alpha", 1)
        self.beta = kwargs.get("beta", 5)
        self.rho = kwargs.get("rho", 0.9)
        self.Q = kwargs.get("Q", 1)
        self.time_limit = kwargs.get("time_limit", 5)

        # initialization of tau and eta
        self.tau = np.full(self.distance_matrix.shape, 0.1)
        self.eta = 1 / (self.distance_matrix + 1e-10)

        self.history = []

    def ant_colony_optimization(self):
        """
        Ant colony optimization algorithm
        """
        start_time = time.time()
        x_best, y_best = [], float("inf")
        for _ in range(self.k_max):
            self.edge_attractiveness()
            self.tau *= (1-self.rho)
            for _ in range(self.m):
                x_best, y_best = self.ant_walk(x_best, y_best)
                if time.time() - start_time > self.time_limit:
                    logging.info("Time limit reached. Stopping ACO.")
                    return x_best, y_best
        return x_best, y_best

    def edge_attractiveness(self, plot: bool = False):
        """
        Calculate edge attractiveness
        tau = pheromone
        eta = distance
        alpha = pheromone importance
        beta = distance importance
        """
        self.A = (self.tau ** self.alpha) * (self.eta ** self.beta)

    def ant_walk(self, x_best, y_best, plot: bool = True):
        """
        Ant walk
        """
        x = [0]  # Start at first node
        while len(x) < self.n:
            i = x[-1]
            neighbors = [j for j in range(self.n) if j not in x and self.distance_matrix[i][j] > 0]
            if len(neighbors) == 0:
                return x_best, y_best
            p = [self.A[(i, j)] for j in neighbors]
            sampled_neighbor = random.choices(neighbors, weights=p)[0]
            x.append(sampled_neighbor)
        x.append(0)
        y = self.score(x)
        self.history.append(y)
        for i in range(1, self.n):
            self.tau[(x[i-1], x[i])] += self.Q / y
        if y < y_best:
            logging.info("Better ACO solution found. Score: %.2f", y)
            return x, y
        return x_best, y_best

    def score(self, x):
        """
        Score a solution
        """
        y = 0
        for i in range(len(x) - 1):
            y += self.distance_matrix[x[i]][x[i + 1]]
        return y
```

让我们一步步分析代码中的关键部分：

+   第一步是初始化`__init__`。我们定义问题，这里是一个 TSP 实例，并根据需要提供超参数。

+   `ant_colony_optimization`部分包含核心执行。在指定的迭代次数`k_max`内，算法致力于提升当前的最佳解。它涉及部署多个蚂蚁`m`，每只蚂蚁在图中移动。

+   `ant_walk`提供了单只蚂蚁旅程的模拟。在 while 循环中，蚂蚁的路径通过选择基于其吸引力`A`的下一条边来构建。边的吸引力使用`edge_attractiveness`方法计算，考虑了信息素矩阵`tau`、alpha、距离矩阵和 beta。每只蚂蚁走完后，信息素矩阵会更新。

运行算法以处理问题实例，只需执行以下操作：

```py
problem = TSP()
aco = AntColonyOptimization(problem)
best_solution, best_score = aco.ant_colony_optimization()
```

现在，ACO 与**最优解**相比表现如何？你可以通过 GIF 可视化进展，GIF 中的每个图像显示当前的最佳路径，让你观察到随时间的改进。

![](img/8510da8a471b8d5b652e2bff352c7721.png)

路径的改进。GIF 由作者提供。

最终解决方案的得分是 4944，非常接近**最优解**（误差小于 1%）！观察解决过程也很有趣：

![](img/7ace6f53e7b43891bc769613aeeeaae3.png)

解决过程。每个点表示一次蚂蚁行走。图片由作者提供。

在这个图表中，x 轴表示蚂蚁的数量，而 y 轴表示蚂蚁旅程中覆盖的距离。水平红线表示最优解决方案的分数，红点象征蚂蚁发现了新的改进解决方案。值得注意的是，通常需要几只蚂蚁才能发现更好的解决方案。然而，最后一个红点非常接近最优解决方案。还有一些策略可以提高 ACO 的性能，我将在接下来的部分中详细说明。

# 分配问题

TSP 是一个路径规划问题，而 ACO 最初是为了应对路径规划问题而设计的。虽然这已经是很久以前的事了，但在此期间，人们发现了使用 ACO 解决不同类型挑战的方法。其中一个值得强调的例子是其在分配问题中的应用。

分配问题是将“某物”分配到“其他物”的问题。一个例子是*二次分配问题*（QAP）。假设你有一组位置，并且你想将一组设施分配到这些位置。目标是确定最佳分配，以最小化总成本。将设施*f*分配到位置*l*的成本由以下公式决定：

+   设施之间的流量：设施之间有一定的流量或互动。这表示在设施之间转移了多少“物品”或“活动”。

+   位置之间的距离：每对位置都有一个对应的距离，代表在这些位置之间运输或操作所需的成本或努力。

将特定设施分配到特定位置的成本由设施之间的流量和位置之间的距离决定。具体来说，计算一对设施的成本是将它们的流量与它们被分配到的地点之间的距离相乘。

要找到特定分配的总成本，你需要将所有可能的设施对的成本加总，这些成本基于它们的位置信息。

![](img/c7698c67956a5043e5cf862832edbd12.png)

一个五个设施放置在五个位置上的最优解决方案的示例。蓝色表示流量。点击放大。图片由作者提供。

当你想将 ACO 应用于分配问题而不是路径规划问题时，问题的表述会发生怎样的变化？我会把编码留给你，但我将从蚂蚁的视角提供对路径规划问题和分配问题之间差异的直观理解。

TSP（旅行商问题）主要是找到访问各个地点的最佳顺序。另一方面，QAP（设施布局问题）则将焦点转向决定物品或设施的放置位置。在 TSP 的 ACO（蚁群优化）中，蚂蚁学会偏向于特定的访问序列。相比之下，在处理 QAP 时，蚂蚁则倾向于选择特定的设施放置于特定的位置。在这种情况下，信息素轨迹表示将设施分配到特定位置的期望程度。

你可以这样想象：每只蚂蚁都有一个可用位置（QAP 中的位置）列表，在这些位置上分配物品（QAP 中的设施）。蚂蚁不断重复这些步骤，直到确定所有物品的最佳安排。

简而言之，可以将其视为蚂蚁协作确定最有效的物品分配方式，依据他们所获得的关于物品到位置分配的最佳效果的知识。

# 提高解决方案质量

有几种策略可以在更短的时间内获得更好的解决方案。以下是三条宝贵的建议，它们可以显著影响你的结果：

## 超参数搜索

如果你处理的是多个相同类型的问题，强烈建议进行超参数搜索。诸如迭代次数、蚂蚁数量、alpha、beta、rho 和 Q 等参数可以对算法性能产生重大影响。

让我们看一个例子。在下面的图表中，我们测试了两个不同的 alpha 值，而其他参数保持不变。线形图显示了 100 次蚂蚁运行的移动平均值，而点表示某次运行找到的最佳解决方案。

![](img/e8a42b6f94a2eaf556217bfd50c04ddb.png)

使用橙色设置（alpha = 2）的算法不仅发现了一个优越的解决方案（由橙点表示），而且比使用蓝色设置（alpha = 1）的算法更快速地完成了这一任务。

为了进一步强调超参数调整的影响，考虑一个包含 100 个节点的 TSP 问题。如果我们在超参数上进行随机搜索（10 次迭代）并绘制移动平均图，我们会观察到性能的显著变化。

![](img/e4ad6c2532bd5f1e1d46f2736abf4ad0.png)

在顶部，橙色和黄色线条产生了次优结果，而其他线条则接近于最佳解决方案。作为背景，需要指出的是，OR 工具求解器花费了 5 分钟找到最优解，而 ACO 算法则在最多 5 秒内完成运行。

## 预热程序

在让蚂蚁开始之前，你可以选择对信息素矩阵进行*预热*。这个准备步骤通常减少了找到最佳解决方案所需的迭代次数。在[这篇论文](https://www.mdpi.com/1999-4893/14/10/295)中，提出了一种预热程序，并比较了其效果。

## 探索与开发

类似于许多算法，如果它们的探索不够充分，可能会遇到停滞。为了解决这个问题，你可以使用[最大-最小蚂蚁系统](https://www.sciencedirect.com/science/article/abs/pii/S0167739X00000431)。MMAS 通过将高信息素值分配给未探索的路径，鼓励蚂蚁探索这些路径。当停滞发生时，这些路径的轨迹会被重置为这个较高的值。与原始算法相比，MMAS 的一个额外优势是只有全局最佳路径或迭代中的最佳路径才能增强其信息素轨迹。这些调整有助于在探索和利用之间实现更有效的平衡。

![](img/49cf5f5fb375e39a075c6ce8717a3a35.png)

照片由[Shardar Tarikul Islam](https://unsplash.com/@tarikul_islam?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 结论

蚁群优化算法是一个有趣的算法，它能够在问题规模增加的情况下仍然找到高质量的解决方案。这使它成为一个强大的算法。

为了进一步提升算法的性能并发现更优解，考虑融入一些关键策略，如超参数搜索、预热过程以及平衡探索和利用的技术。这些调整可以显著改善你的结果。

如果你对 ACO 产生了兴趣并想进一步阅读，我推荐[这篇文章](https://ds.amu.edu.et/xmlui/bitstream/handle/123456789/10303/ant%20colony%20optimization%20%282004%29.pdf?sequence=1&isAllowed=y)。其中一位作者是 1992 年提出 ACO 的人。它提供了对这一卓越优化技术的宝贵见解和全面理解。

## 相关内容

[## 优化连接：图中的数学优化](https://towardsdatascience.com/optimizing-connections-mathematical-optimization-within-graphs-7364e082a984?source=post_page-----6d9106de60af--------------------------------)

### 图论及其应用简介

[## 介绍一种强大的优化技术：模拟退火](https://towardsdatascience.com/optimizing-connections-mathematical-optimization-within-graphs-7364e082a984?source=post_page-----6d9106de60af--------------------------------)

### 解释、参数、优点、缺点及使用案例

[## 元启发式算法解释：蚁群优化](https://towardsdatascience.com/an-introduction-to-a-powerful-optimization-technique-simulated-annealing-87fd1e3676dd?source=post_page-----6d9106de60af--------------------------------)

### 基于蚂蚁行为的较少为人知的启发式算法简介

towardsdatascience.com
