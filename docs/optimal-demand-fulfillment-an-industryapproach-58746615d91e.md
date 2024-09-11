# **优化需求满足：行业方法**

> 原文：[https://towardsdatascience.com/optimal-demand-fulfillment-an-industryapproach-58746615d91e?source=collection_archive---------15-----------------------#2023-02-06](https://towardsdatascience.com/optimal-demand-fulfillment-an-industryapproach-58746615d91e?source=collection_archive---------15-----------------------#2023-02-06)

[](https://saifalikheraj.medium.com/?source=post_page-----58746615d91e--------------------------------)[![Saif Ali Kheraj](../Images/7b4fbd0444f5945ec7385b82a8276f27.png)](https://saifalikheraj.medium.com/?source=post_page-----58746615d91e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----58746615d91e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----58746615d91e--------------------------------) [Saif Ali Kheraj](https://saifalikheraj.medium.com/?source=post_page-----58746615d91e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9feeca234e74&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimal-demand-fulfillment-an-industryapproach-58746615d91e&user=Saif+Ali+Kheraj&userId=9feeca234e74&source=post_page-9feeca234e74----58746615d91e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----58746615d91e--------------------------------) · 8分钟阅读 · 2023年2月6日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F58746615d91e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimal-demand-fulfillment-an-industryapproach-58746615d91e&user=Saif+Ali+Kheraj&userId=9feeca234e74&source=-----58746615d91e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F58746615d91e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimal-demand-fulfillment-an-industryapproach-58746615d91e&source=-----58746615d91e---------------------bookmark_footer-----------)

在电信领域，数据科学有许多应用，涵盖从站点规划到预算和管理会计决策、客户生命周期管理以及市场营销。在这篇文章中，我们将讨论一个这样的用例，即网络站点规划。我们将从两个场景开始，以便您可以理解它们。

**场景 1：** 最近，电信决策者发现由于未规划的站点规划导致了巨大的开销。网络站点当然有各种成本，包括固定建设成本、可变成本和其他可变开销。公司的管理者发现了导致低效的各种差异。现在，管理者想要重新规划一切，关闭不必要的站点，同时确保剩余的站点完全覆盖所有需求/区域。这就是一个用例。目标是减少基站的总数量，同时仍然覆盖所有区域。

**场景 2：** 另一个用例是当管理者想要限制用于覆盖最大区域数量的站点数量，这可能是由于财务不足或某个季度或年份的预算限制。因此，在这种情况下，我们希望在选择不超过 p 个基站（上限）的情况下最大化我们的覆盖区域。在这种情况下，可能无法覆盖所有区域。

# **概述**

让我们看看如何使用优化技术来轻松解决这两个用例。需求并不总是指用户或区域的数量，但可以从整体上考虑多个因素，这超出了本文的范围。

# **场景 1**

让我们从第一个用例开始，即在满足所有需求的情况下，尽可能少地选择基站。我们将使用一个简单的例子来演示这些概念；然而，在现实世界中，你可能会有不同类型的数据集，包含成千上万的基站，并且每个基站的邻域或覆盖范围可以通过一定的距离阈值来检查。

让我们从一个例子开始。我们有四个基站，区域被分为四个区域。大面积通常会在地图上分成不同的网格。我们不会深入讨论这一点。例如，基站 #1 可以覆盖区域 1 和 2，而基站 #2 可以覆盖区域 3。基站 #2 可以覆盖区域 1、2 和 3。基站 #3 可以覆盖区域 3 和 4。基站 #4 可以覆盖区域 2 和 4。下图展示了这一点。

![](../Images/dbbeb2b254c4fd912bba43e8f38d26f0.png)

图 1：区域与网络塔的连接性（作者图）

在这种情况下，我们想要找到满足所有要求的最少数量的基站。例如，我们可能不需要基站 #4，因为需求已经由基站 #1、#2 和 #3 满足。因此，我们可能要排除基站 #4。这正是我们想要完成的：找到最佳的基站组合，以满足所有需求，同时消除不必要的基站。

## 方程

让我们用J表示位置/基站，用I表示需求节点/区域。我们还定义aij，如果需求节点i被位置节点j覆盖，则aij等于1。aij可以使用某个距离阈值来定义。

![](../Images/abe8cd616ee03c0ee4c282bc3626d897.png)

表1：场景1的样本问题表述和变量符号（作者绘制）

每个基站位置由一个二进制变量xj表示，该变量指示该位置是否被选中。如果基站被选中，则xj=1，否则xj=0。

目标函数是通过最小化选定基站的总和来进行优化。

![](../Images/57dc31020fc54fd9813b70b33eec14b5.png)

方程1：目标函数——场景1中最小化选定基站的总和（作者绘制）

约束条件是确保每个区域至少被一个选定的基站覆盖。因此，我们希望每个需求节点i（在我们案例中是区域）至少由一个位置j（在我们案例中是基站）服务。

![](../Images/ed3b25b1e5d99e7a50d3ef92ed34089f.png)

方程2：约束条件——场景1中每个区域必须由至少一个基站/位置覆盖（作者绘制）

因此，对于每个区域i，我们查看所有基站，看看这些基站是否覆盖区域i。每个区域应该至少由一个基站覆盖。请注意，在这个特定模型中，区域可以由多个基站服务。

通过解决这个优化问题，电信公司可以在最小化所需基站数量的同时确定最佳的基站位置，以覆盖整个国家/城市/区域。

## `Code`

```py
from pyomo.environ import *

model = ConcreteModel()

# Define parameters regions and cell sites
model.I = Set(initialize=[1,2,3,4]) # regions/demands
model.J = Set(initialize=[1,2,3,4]) # cell sites
model.a = Param(model.I, model.J, initialize={(1,1):1, (1,2):1, (1,3):0, (1,4):0, (2,1):1,(2,2):1,(2,3):1, (2,4):0, (3,1):0,(3,2):0,(3,3):1,(3,4):1,(4,1):0,(4,2):1,(4,3):0, (4,4):1})

# Define variables
model.x = Var(model.J, within=Binary)

# Define objective function
def objective(model):
    return sum(model.x[j] for j in model.J)

model.obj = Objective(rule=objective, sense=minimize)

# Define constraints (sumproduct), for each Region i, we look at all cell sites, and see whether this cell site covers region i or not. sum must be greator than equal to 1
def demand_rule(model, i):
    return sum(model.a[i,j]*model.x[j] for j in model.J) >= 1

model.demand_constraint = Constraint(model.I, rule=demand_rule)

# Solve the model
solver = SolverFactory('glpk')
results = solver.solve(model)

# Print the solution
print("Optimal solution:")
for j in model.J:
    if model.x[j].value == 1:
        print(f"Location {j} is open")
print(f"Number of open locations: {model.obj()}")
```

![](../Images/3e5b39b1ae1c809e6291ca291a7031a3.png)

图1：场景1的结果（作者绘制）

正如我们所见，我们可以用两个基站覆盖所有需求点/区域，你可以使用上面的图表自行验证结果。

# 场景2

现在我们进入第二个用例，讨论如何对可用的站点位置设置上限，以满足最大需求。考虑到可用的基站数量有限，我们希望最大化覆盖范围。例如，公司可能决定设置一个最大为两个基站的上限，并关闭其他基站以节省费用。现在的问题是我们能用两个基站覆盖多少个区域。当然，不同的组合是可能的；例如，如果我们只使用基站1和2，并关闭基站3和4，那么我们实际上会在区域3失去覆盖。也许有某些两个基站的组合可以覆盖整个区域，这将通过我们的优化方程来解决。可能存在不止一个可行的解决方案。

## 方程

![](../Images/651c0996d8e5acdd04ac6e60e4f0ef8f.png)

表2：场景2的样本问题表述和变量符号（作者绘制）

在这种情况下，我们希望基本上最大化我们的覆盖范围，我们用 y 变量定义覆盖的概念。在这种情况下， yi 表示我们的区域是否会被选中。如果被选中，则 yi 将为1，否则为0。我们基本上希望最大化这个总和，也就是说，我们希望每个区域都被覆盖。这可以通过以下方程表示。

![](../Images/27ac29b475025dcd365306b4135d4c96.png)

方程 3：目标函数——最大化场景1的覆盖/区域（图由作者提供）

然而，我们有一些限制条件，因此可能无法覆盖所有区域。让我们从第一个限制条件开始，即覆盖区域的基站数量必须大于等于 yi。例如，如果某个区域被选中，则 yi=1，那么我们基本上希望覆盖该区域的基站数量大于等于 yi。

![](../Images/8901760ed5cfa6b333b0140943d15a9b.png)

方程 4：限制条件——每个选定的区域（yi）需要至少由1个基站/位置覆盖，用于场景2（图由作者提供）

方程与第一部分相同，只是将1替换为 yi。因此，对于每个区域 I，我们查看它是否被基站覆盖，并将其加起来以确保它大于等于 yi。

除此之外，最重要的限制条件是公司决定将我们限制在最多两个基站。我们基本上希望基站总数小于等于 p。（在我们的情况下是2）。

![](../Images/3ae149a469e29792feea2ca278c3c1b5.png)

方程 5：限制条件——场景2的 p 个基站上限（图由作者提供）

现在，让我们将所有方程组合在一起，再次写出完整的内容。

![](../Images/730e1b537f2ced3310cdf65017c9a2ef.png)

方程 6：结合上述所有内容的完整方程，用于场景2（图由作者提供）

## 代码

```py
from pyomo.environ import *

model = ConcreteModel()
# Define parameters regions and cell sites
model.I = Set(initialize=[1,2,3,4]) # regions/demands
model.J = Set(initialize=[1,2,3,4]) # cell sites
model.a = Param(model.I, model.J, initialize={(1,1):1, (1,2):1, (1,3):0, (1,4):0, (2,1):1,(2,2):1,(2,3):1, (2,4):0, (3,1):0,(3,2):0,(3,3):1,(3,4):1,(4,1):0,(4,2):1,(4,3):0, (4,4):1})

model.p = Param(initialize=1)
model.x = Var(model.J, within=Binary)
model.y = Var(model.I, within=Binary)

def objective(model):
    return sum(model.y[i] for i in model.I)

model.obj = Objective(rule=objective, sense=maximize)

##constraint
def coverage_rule(model, i):
    return sum(model.a[i,j]*model.x[j] for j in model.J) >= model.y[i]

model.coverage_constraint = Constraint(model.I, rule=coverage_rule)

def cellsitelocation_rule(model):
    return sum(model.x[j] for j in model.J) <= model.p

model.cellsite_constraint = Constraint(rule=cellsitelocation_rule)

opt = SolverFactory("glpk")
results = opt.solve(model)

print("Optimal solution:")
for j in model.J:
    if model.x[j].value == 1:
        print(f"Cellsite {j} is on")

print("Optimal solution:")
for i in model.I:
    if model.y[i].value == 1:
        print(f"Region {i} is covered")
print(f"Number of regions covered: {model.obj()}")
```

![](../Images/4879794e00704a493f3c8f3d0c350b27.png)

图 2：场景2的结果（图由作者提供）

我们仍然可以用有限数量的基站覆盖所有需求/区域。你现在可以尝试 p=1 来查看可能会遗漏哪些区域。重要的是要注意，使用上限时，你可能会遗漏一些区域，但仍能最大化覆盖范围。

# 结论

这些是我们在本文中探讨的非常简单的问题。然而，当涉及到这样的问题时，这可能是一个非常强大的工具。我们可以有一个更高级的方程，还带有加权部分，现在由你进一步思考。对于这些类型的问题还有许多其他变体，例如最小化从位置到需求点的运输成本。这些类型的问题有许多有趣的变体。

# 参考文献

[1] [https://optimization.cbe.cornell.edu/index.php?title=Set_covering_problem](https://optimization.cbe.cornell.edu/index.php?title=Set_covering_problem)

[2] [http://www.im.ntu.edu.tw/~lckung/courses/OR15/slides/OR-Sp15_09_IPapplication.pdf](http://www.im.ntu.edu.tw/~lckung/courses/OR15/slides/OR-Sp15_09_IPapplication.pdf)

[3] [http://www.pyomo.org/documentation](http://www.pyomo.org/documentation)
