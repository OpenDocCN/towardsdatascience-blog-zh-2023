# 图着色问题：精确解和启发式解

> 原文：[https://towardsdatascience.com/the-graph-coloring-problem-exact-and-heuristic-solutions-169dce4d88ab?source=collection_archive---------3-----------------------#2023-11-13](https://towardsdatascience.com/the-graph-coloring-problem-exact-and-heuristic-solutions-169dce4d88ab?source=collection_archive---------3-----------------------#2023-11-13)

## 通过自定义构造启发式方法和整数规划在 Python 中探索经典的离散优化问题

[](https://medium.com/@bruscalia12?source=post_page-----169dce4d88ab--------------------------------)[![Bruno Scalia C. F. Leite](../Images/1042cd04be047c0811fef79ecd04e69c.png)](https://medium.com/@bruscalia12?source=post_page-----169dce4d88ab--------------------------------)[](https://towardsdatascience.com/?source=post_page-----169dce4d88ab--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----169dce4d88ab--------------------------------) [Bruno Scalia C. F. Leite](https://medium.com/@bruscalia12?source=post_page-----169dce4d88ab--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3ce9b7482ef0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-graph-coloring-problem-exact-and-heuristic-solutions-169dce4d88ab&user=Bruno+Scalia+C.+F.+Leite&userId=3ce9b7482ef0&source=post_page-3ce9b7482ef0----169dce4d88ab---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----169dce4d88ab--------------------------------) ·10分钟阅读·2023年11月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F169dce4d88ab&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-graph-coloring-problem-exact-and-heuristic-solutions-169dce4d88ab&user=Bruno+Scalia+C.+F.+Leite&userId=3ce9b7482ef0&source=-----169dce4d88ab---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F169dce4d88ab&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-graph-coloring-problem-exact-and-heuristic-solutions-169dce4d88ab&source=-----169dce4d88ab---------------------bookmark_footer-----------)![](../Images/4509b47d7da81cc86ecf9f75d39735f1.png)

32 节点实例的图着色启发式解。（图片由作者提供）。

图着色理论在离散数学中占据核心地位。它出现在许多看似与着色无关的地方。它处理的是将一组对象根据某些规则划分为类别的基本问题（Jensen & Toft, 1995）。

我们可以将问题总结如下：给定一个无向图*G*(*V*, *E*)，为每个节点（顶点）分配颜色，使得相邻节点不共享相同颜色，并且使用的颜色数量最小化。尽管问题陈述简洁明了，但这个问题以其计算复杂性而臭名昭著，属于NP难类问题。

由于其组合复杂性，即使使用最佳可用求解器，整数线性规划（ILP）的精确方法可能无法解决大型实例。在这种情况下，启发式算法和元启发式算法可以是有趣的替代方法。虽然无法证明最优性，但这些方法可以提供快速和高质量的解决方案。

在本文中，我们将使用构造启发式算法*DSatur*（Brélaz, 1979）和整数线性规划模型使用*pyomo*（Bynum等，2021），使用HiGHS求解器解决图着色问题。至于其他故事，你可以在我的[code repository](https://github.com/bruscalia/optimization-demo-files/tree/1fa7a3825421d0b166195d890f2629c576cfbfda/graph-coloring)中找到完整的代码。

如果您还不熟悉*线性规划*，我建议您在继续之前阅读我以前的文章以获得基础知识。

[](/linear-programming-theory-and-applications-c67600591612?source=post_page-----169dce4d88ab--------------------------------) [## 线性规划：理论与应用

### Python中的线性优化的主要概念和实现

towardsdatascience.com](/linear-programming-theory-and-applications-c67600591612?source=post_page-----169dce4d88ab--------------------------------)

现在让我们动手创建像这样的令人惊叹的解决方案。

![](../Images/e6311f0269b445fe63d866bd5e512c61.png)

32个节点实例的图着色启发式解决方案。（作者动画）

# 构造启发式算法 — DSatur

让我们回顾一下我们的问题定义：

+   考虑一个无向图*G*(*V*, *E*)。

+   为每个节点分配颜色，使得相邻节点不共享相同的颜色。

+   最小化使用的颜色数量。

一个朴素的构造性解决方案可以是依次选择一个空节点，并为其分配一个颜色，确保其邻居中尚未使用该颜色。这可能不是可能的最佳策略，但肯定可以为所需颜色数量提供一个上界。然而，*DSatur*（Brélaz, 1979）算法包括选择下一个节点和改进该启发式的新元素。

+   *度数*：连接到给定节点的边的数量。

+   *饱和度*：在邻居中使用的不同颜色数量。

*DSatur*的算法从一个未着色节点的队列开始，迭代选择一个饱和度度数最大的节点，将其从队列中移除，并为其分配颜色。如果饱和度相等，Brélaz（1979）建议选择任何一个饱和度最大的节点。我们将采用稍微不同的方法，通过选择具有最大整体度数的节点来打破平局。分配给给定节点的颜色应为可用的最低索引颜色。下面给出了伪代码。设*N*为节点集合，*Q*为未着色节点队列，引用*N*本身的相同内存位置，*C*为已使用颜色集合。

```py
dsatur(N)
  Q = [&n for n in N]
  C = {}
  while |Q| > 0:
    sort(Q) // descending order by saturation and degree
    n = Q.pop(0)
    assign_color(n, C)
```

函数`assign_color`应该验证最低索引可用的颜色，如果当前集合中的颜色不可行，则包含一个新的备选颜色并增加集合。

让我们把这部分代码写成Python代码。首先是包的导入。我们的启发式算法将用纯Python编写，只需导入一些类型提示。

```py
from typing import List, Tuple
```

现在，让我们定义我们的基本建模元素：*Colors*和*Nodes*。类*Color*定义为一个可变的占位符，用于相应实例的索引。在Python中，这可能是内存高效的，因为多个变量可以引用相同的内存位置。每次用给定颜色给新节点上色时，都应该调用`add_node`方法。

```py
class Color:

    index: int
    n_nodes: int

    def __init__(self, index) -> None:
        self.index = index
        self.n_nodes = 0

    def __repr__(self):
        return f"C{self.index}"

    def add_node(self):
        self.n_nodes = self.n_nodes + 1
```

现在谈谈*Node*类。每个*Node*实例都有一个属性`neighbors`，这是一个节点（指针）的列表。因此，每次修改节点时，我们无需更改其邻居的任何信息。我们可以使用`add_neighbor`方法添加一个邻居，使用`set_color`方法设置其颜色，并访问依赖于其邻居当前状态的属性`neighbor_colors`、`saturation`和`degree`。

```py
class Node:

    neighbors: List['Node']
    index: int
    color: Color

    def __init__(self, index):
        self.index = index
        self.neighbors = []
        self.color = None

    def __repr__(self) -> str:
        return f"N{self.index}|{self.color}"

    def add_neighbor(self, node: 'Node'):
        if node not in self.neighbors:
            self.neighbors.append(node)

    def set_color(self, color: Color):
        self.color = color
        color.add_node()

    @property
    def neighbor_colors(self):
        return [n.color for n in self.neighbors if n.color is not None]

    @property
    def saturation(self):
        return len(set((n.color for n in self.neighbors if n.color is not None)))

    @property
    def degree(self):
        return len(self.neighbors)
```

现在进入*DSatur*算法。为了实现它，将创建一个同名的类。新创建的*DSatur*实例需要图的节点数`n_nodes`和`edges`作为参数。然后，它会实例化节点并根据边设置它们的邻居。

```py
class DSatur:

    N: List[Node]
    C: List[Color]
    history: List[Node]

    def __init__(self, nodes: List[int], edges: List[Tuple[int, int]]):
        N = [Node(i) for i in nodes]
        for e in edges:
            i, j = e
            N[i].add_neighbor(N[j])
            N[j].add_neighbor(N[i])
        self.N = N
        self.C = []
        self.history = []

    def find_next_color(self, node: Node) -> Color:
        next_color = None
        for c in self.C:
            if c not in node.neighbor_colors:
                next_color = c
                break
        if next_color is None:
            next_color = Color(len(self.C) + 1)
            self.C.append(next_color)
        return next_color

    def solve(self, save_history=False):
        Q = [n for n in self.N]  # Pool of uncolored nodes
        while len(Q) > 0:
            Q.sort(key=lambda x: (x.saturation, x.degree), reverse=True)
            n: Node = Q.pop(0)
            next_color = self.find_next_color(n)
            n.set_color(next_color)
            if save_history:
                self.history.append(n)
        self.C.sort(key=lambda x: x.n_nodes, reverse=True)

    @property
    def cost(self):
        return len(self.C)
```

现在，让我们使用它来解决一些具有挑战性的问题！你可以在[OR Library](http://people.brunel.ac.uk/~mastjjb/jeb/orlib/files/)中找到几个实例。在那里，它们被标记为“gcolX”。我们可以使用以下代码从这些文件中的一个提取节点和边的数量。

```py
# Open and read file
with open($YOUR_FILE_PATH, mode="r") as file:
    lines = file.readlines()
    header = lines[0].strip().split()
    n_nodes = int(header[2])
    edges = []
    node_set = set()
    for line in lines[1:]:
        if line.startswith('e'):
            _, i, j = line.strip().split()
            edges.append((int(i), int(j)))
            node_set.add(int(i))
            node_set.add(int(j))
nodes = sorted(node_set)
assert len(nodes) == n_nodes, "Wrong number of nodes specified"
```

然后，我们将这些颜色解析到一个新的*DSatur*实例中，解决问题（或者至少得到一个高质量的近似解）。

```py
dsatur = DSatur(nodes, edges)
dsatur.solve()
```

创建介绍中结果的有趣可视化的代码可能过于冗长，不便在这里包含，但你可以在我的[代码库](https://github.com/bruscalia/optimization-demo-files/tree/1fa7a3825421d0b166195d890f2629c576cfbfda/graph-coloring)中找到。它可以传达处理多个节点时难度的总体概念…

![](../Images/e51268bd9c6951ed7eb7116e12f481c0.png)

图着色启发式解决方案针对100个节点实例。（作者动画）。

现在让我们看看如何通过精确方法处理它。

# 整数线性规划

为了优化我们使用启发式方法得到的解决方案，并尝试证明解决方案的最优性，让我们将图着色问题表述为一个整数线性规划模型。请记住，尽管如此，它可能无法处理大型实例。本节介绍的模型以及其他确切算法均在Lewis（2021）的*第三章*中介绍。

让我们定义在这种方法中考虑的*Sets*：

+   *C*：颜色

+   *N*：节点（或顶点）

+   *E*：边

第一个问题已经出现：“我们应该考虑多少种颜色？”在最坏的情况下，所有节点都连接在一起，因此应该考虑与*N*相同大小的*C*。然而，这种方法可能通过不必要增加决策变量的数量并恶化模型的线性放松，使我们的解决方案变得更加困难。一个好的替代方法是使用启发式方法（如*DSatur*）来获取颜色数量的上限。

在这个问题中，我们有两组决策变量：

+   *x_*{*i*, *c*}：是二进制变量，指示节点*i*使用颜色*c*着色。

+   *y_*{*c*}：是二进制变量，指示使用颜色*c*。

我们还必须创建约束条件以确保：

+   每个节点必须着色。

+   如果任何一条边上的节点有颜色，请确保使用该颜色。

+   每条边上最多只能用一个颜色着色。

+   打破对称（这不是必须的，但可能有帮助）

最终，我们的目标是最小化使用的总颜色数，这是*y*的总和。总结我们有以下方程。

![](../Images/3a8f414140d6685082f65ad6ca27e856.png)

图着色整数规划模型。（作者提供的图片）。

毫不拖延，让我们为整数编程模型导入*pyomo*。

```py
import pyomo.environ as pyo
```

*pyomo*中建模问题有两种方法：*抽象*和*具体*模型。在第一种方法中，问题的代数表达式在提供某些数据值之前定义，而在第二种方法中，模型实例在其元素定义后立即创建。您可以在[库文档](https://pyomo.readthedocs.io/en/stable/pyomo_overview/abstract_concrete.html)或Bynum等人（2021）的书中了解更多关于这些方法的信息。在本文中，我们将采用*具体*模型的形式。

```py
model = pyo.ConcreteModel()
```

接下来，让我们实例化我们的*Sets*。直接从`dsatur`属性`N`和`C`中解析可迭代对象是有效的，因此可以在`initialize`关键字参数中使用它们。或者，我将从输入数据中传递原始*nodes*和*edges*，并从*DSatur*创建一个范围作为我们颜色的初始化。

```py
model.C = pyo.Set(initialize=range(len(dsatur.C)))  # Colors
model.N = pyo.Set(initialize=nodes)  # Nodes
model.E = pyo.Set(initialize=edges])  # Edges
```

接下来，我们实例化我们的决策变量。

```py
model.x = pyo.Var(model.N, model.C, within=pyo.Binary)
model.y = pyo.Var(model.C, within=pyo.Binary)
```

然后是我们的约束条件。

```py
# Fill every node with some color
def fill_cstr(model, i):
    return sum(model.x[i, :]) == 1

# Do not repeat colors on edges and indicator that color is used
def edge_cstr(model, i, j, c):
    return model.x[i, c] + model.x[j, c] <= model.y[c]

# Break symmetry by setting a preference order (not required)
def break_symmetry(model, c):
    if model.C.first() == c:
        return 0 <= model.y[c]
    else:
        c_prev = model.C.prev(c)
        return model.y[c] <= model.y[c_prev]

model.fill_cstr = pyo.Constraint(model.N, rule=fill_cstr)
model.edge_cstr = pyo.Constraint(model.E, model.C, rule=edge_cstr)
model.break_symmetry = pyo.Constraint(model.C, rule=break_symmetry)
```

你可以尝试包括其他对称破缺约束，并查看哪个在你的可用求解器中效果更好。在我进行的一些实验中，包括使用总节点数的优先级顺序的效果不如忽略它。这可能是由于求解器的本地对称破缺策略。

```py
# Break symmetry by setting a preference order with total assignments
def break_symmetry_agg(model, c):
    if model.C.first() == c:
        return 0 <= sum(model.x[:, c])
    else:
        c_prev = model.C.prev(c)
        return sum(model.x[:, c]) <= sum(model.x[:, c_prev])
```

最终，目标…

```py
# Total number of colors used
def obj(model):
    return sum(model.y[:])

model.obj = pyo.Objective(rule=obj)
```

我们的模型已经准备好解决了！为此，我使用了HiGHS持久求解器，该求解器在*pyomo*中可用，前提是你的Python环境中还安装了*highspy*。

```py
solver = pyo.SolverFactory("appsi_highs")
solver.highs_options["time_limit"] = 120
res = solver.solve(model)
print(res)
```

对于大型实例，我们的求解器可能很难改善启发式解。然而，对于在[代码仓库](https://github.com/bruscalia/optimization-demo-files/tree/1fa7a3825421d0b166195d890f2629c576cfbfda/graph-coloring)中提供的32节点实例，求解器能够将使用的颜色数量从9减少到8。值得一提的是，完成执行花费了24秒，而对于相同实例的*DSatur*算法仅花费了6毫秒（使用纯Python，这是一个解释型语言）。

![](../Images/262d463424737ab0f54c1ddb5159a143.png)

32节点实例的图着色整数规划解。（图片由作者提供）。

# 进一步阅读

尽管*DSatur*是一种直观的启发式方法，能提供快速的高质量解，但其他方法可能会带来更好的结果，尤其是在复杂实例上。图着色问题中最著名的元启发式算法之一是*Tabucol*（Hertz & Werra, 1987）。作者提出了一种从初始解开始的方法，初始解具有*k*种颜色，并可能存在连接相同颜色节点的边。然后，执行局部移动，改变给定节点的颜色，以最小化冲突，使用禁忌表逃避局部最优。

比本文中所示的图着色问题的更高效的精确方法依赖于*Column Generation*，使用一种称为*Branch & Price*的算法。对这个主题感兴趣的读者可以在我的其他Medium故事中找到简洁的概述。

[](/column-generation-in-linear-programming-and-the-cutting-stock-problem-3c697caf4e2b?source=post_page-----169dce4d88ab--------------------------------) [## 线性规划中的列生成和切料问题

### 如何使用Python示例解决具有大量决策变量的线性问题

towardsdatascience.com](/column-generation-in-linear-programming-and-the-cutting-stock-problem-3c697caf4e2b?source=post_page-----169dce4d88ab--------------------------------)

# 结论

在这篇文章中，介绍了两种解决图着色问题的方法：构造性启发式*DSatur*（Brélaz, 1979）和整数线性规划（ILP）模型。启发式方法能够为中等规模的实例提供快速的高质量解，之后的解通过ILP模型进一步改进。实现的代码可在公共仓库中进一步使用。

# 参考文献

Brélaz, D., 1979\. 图的顶点着色的新方法。*ACM通讯*，*22*(4)，251–256。

Bynum, M. L. et al., 2021\. *Pyomo-优化建模在Python中*。Springer。

Hertz, A., & Werra, D. D., 1987\. 使用禁忌搜索技术进行图着色。*计算*，*39*(4)，345–351。

Jensen, T. R., & Toft, B., 1995\. *图着色问题*。John Wiley & Sons。

Lewis, R.M.R., 2021\. 图着色的高级技术。在：*图着色指南*。计算机科学文本。Springer，Cham。[https://doi.org/10.1007/978-3-030-81054-2_4](https://doi.org/10.1007/978-3-030-81054-2_4)
