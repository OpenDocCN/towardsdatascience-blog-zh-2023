# Python 中的数学优化介绍

> 原文：[`towardsdatascience.com/introduction-to-mathematical-optimisation-in-python-b88e6556f9e`](https://towardsdatascience.com/introduction-to-mathematical-optimisation-in-python-b88e6556f9e)

## 数据科学基础

## Python 中离散优化的初学者实用指南

[](https://zluvsand.medium.com/?source=post_page-----b88e6556f9e--------------------------------)![Zolzaya Luvsandorj](https://zluvsand.medium.com/?source=post_page-----b88e6556f9e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b88e6556f9e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b88e6556f9e--------------------------------) [Zolzaya Luvsandorj](https://zluvsand.medium.com/?source=post_page-----b88e6556f9e--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b88e6556f9e--------------------------------) ·10 min 阅读·2023 年 12 月 2 日

--

数据科学家使用数据和各种技术解决广泛的实际问题。数学优化是一种强大的技术，可以应用于许多领域的广泛问题，是数据科学家工具箱中的一项重要投资。在这篇实用的入门文章中，我们将熟悉 Python 中的三个流行优化库：Google 的 [OR-Tools](https://developers.google.com/optimization)、IBM 的 [DOcplex](https://www.ibm.com/docs/en/icos/12.9.0?topic=docplex-python-modeling-api) 和 COIN-OR 基金会的 [PuLP](https://coin-or.github.io/pulp/)

![](img/8110e850abb6f32c794b985530fffe78.png)

图片由 [Akhilesh Sharma](https://unsplash.com/@fotonium?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 📍 概述

数学优化是关于在*预定义的范围*内为*定量问题*找到*最优选择*。它有三个组成部分：

+   **目标函数：** 告诉我们一个解决方案的好坏，并允许我们比较不同的解决方案。一个最优解决方案是根据使用情况最大化或最小化目标函数的解决方案。

    ▶ ️在某些情况下，可能会有多个目标函数。这会增加确定最优解的复杂性。

    ▶ ️在某些情况下，可能没有目标函数。这种优化问题称为可行性问题。

+   **决策变量：** 代表我们想要找出的值或值的集合，即我们在定量问题中寻找的答案。根据决策变量的类型，优化可以分为两种：

    ▶️ *离散优化：* 决策变量是离散的。分配时间表和寻找两个地点之间的最短旅行路径是一些离散优化的例子。如果你想了解更多关于离散优化的信息，[这个课程](https://www.coursera.org/learn/discrete-optimization)和/或[这个指南](https://www.math.uwaterloo.ca/~bico/papers/comp_chapter1.pdf#:~:text=Discrete%20optimization%20is%20the%20study%20of%20problems%20that,the%20best%20alternative%20from%20a%20%EF%AC%81eld%20of%20possibilities.) 可能会对你有兴趣。

    ▶️ *连续优化：* 决策变量是连续的。你可能已经听说过机器学习中的优化术语。机器学习是使用连续优化的一个例子。如果你想了解更多关于连续优化的信息，你可以参考[这个教程](https://www.youtube.com/watch?v=PoTpmjvHxlg)。

+   **约束条件：** 定义了决策变量的可行解范围。

    ▶️ 在一些连续优化问题中，可能没有约束条件。这被称为无约束优化。

在本教程中，我们将专注于具有单一目标的离散优化。在这种类型的优化问题中，我们希望找到决策变量的可行值组合，以使其在满足约束条件的情况下最小化或最大化目标函数。

# 📍 示例

在了解了基本知识后，让我们看一个例子，然后再深入了解 Python。我们将研究一个足够简单的业务案例，以便手动找到最佳解决方案。这样，我们可以在不感到过于困惑的情况下熟悉这些库。

## 📌 背景

▶️ 假设我们运营一家时尚工厂，生产三种物品：围巾、帽子和手套。每种物品有一个设计和一个尺寸。假设我们知道每月的市场需求：

![](img/1a5002f89b2e51aa775314a6795630e8.png)

作者提供的图片

▶️ 我们拥有有限数量的员工和机器。下表显示了生产一个单位物品所需的劳动（员工）和生产（机器）时间。例如，生产一条围巾需要 0.4 小时的劳动时间和 0.1 小时的生产时间。表中还显示了我们一个月内可用的总劳动和生产时间。最后，我们可以在最后一行看到每件物品的利润。

![](img/2b44d74a769a513a590abebee38f35fb.png)

作者提供的图片

▶️ 我们的业务目标是确定生产多少围巾、帽子和手套，以最大化我们的利润。

现在让我们把这个业务问题分解为三个组件。

▶️ **决策变量：** 所有决策变量必须是整数。

![](img/c21eec6f121e01a30cfcf24839d50d11.png)

作者提供的图片

▶️ **目标函数：** 我们有一个目标函数。

![](img/5944cb92d128e3eb465d4ae1444fad84.png)

作者提供的图片

▶️ **约束条件：** 虽然没有明确说明，我们不希望生产任何超过市场需求的物品。否则，这将导致我们有未售出的物品。我们还需要确保生产所需的资源在劳动和生产限制范围内，换句话说，即可行。

![](img/e34bfd3a2b6fa623df9351a324e9fea4.png)

作者提供的图片

## 📌 手动解决方案

一种解决这个问题的幼稚方法是从需求范围内选择随机数字，检查它们是否可行并比较它们的利润。例如，5,000 条围巾、4,000 顶帽子和 6,000 双手套。然而，以这种方式比较少量解决方案可能会导致利润不尽如人意。为了彻底，我们可以使用暴力方法遍历所有组合（7,001 x 9,001 x 8,001 ≈ 5000 亿）。然而，这种方法似乎效率低下、繁琐且计算成本高。肯定还有更聪明的方法。

相反，我们可以通过应用简单的逻辑/常识直观地解决这个问题。我们将找到带来最高利润的物品。

![](img/6fca2be46c66ac914bff3f3946ae10a6.png)

作者提供的图片

上表显示手套是最有利可图的物品。用 1.2 小时的劳动时间和 0.3 小时的生产时间，我们从手套中赚取$48，从围巾中赚取$45，从帽子中赚取$0。由于资源充足，我们将生产最大数量的手套（8,000）。在剩余资源下，我们将比较两种选择：优先生产*围巾*还是*帽子*。

![](img/91a77cb2ee1d9ee2b65dafd2de2ad21e.png)

作者提供的图片

我们发现优先生产围巾更有利可图。因此，我们将生产 7,000 条围巾、5,000 顶帽子和 8,000 双手套以最大化利润。

# 📍 Python 中的优化

现在，我们将使用优化算法在 Python 中利用三种不同的优化库高效地找到*最佳*解决方案。在深入研究这些库之前，让我们将关键信息保存到字典中：

```py
demand = {'scarves': 7000, 'hats': 9000, 'gloves': 8000}
labour = {'scarves': 0.4, 'hats': 0.3, 'gloves': 0.2, 'limit': 6000}
production = {'scarves': 0.1, 'hats': 0.18, 'gloves': 0.05, 'limit': 2000}
profit = {'scarves': 15, 'hats': 12, 'gloves': 8}
```

现在是时候了解一下*求解器*这个术语了，因为我们将经常使用这个术语。求解器是通过应用优化算法来解决优化问题的工具。现在，让我们深入了解一下。

## 📌 OR-Tools

我们要熟悉的第一个库是：谷歌的 OR-Tools。这里，OR 代表[运筹学](https://research.google/teams/operations-research/)。这个优化工具是开源软件。我们将首先使用他们的 CP-SAT 求解器来找出我们的决策变量。这个求解器是一个[使用 SAT（可满足性）方法的约束编程求解器](https://developers.google.com/optimization/cp/#:~:text=Tools%20Google%20provides%20few%20ways%20to%20solve%20CP,constraint%20programming%20solver%20that%20uses%20SAT%20%28satisfiability%29%20methods.)。要了解更多关于求解器的信息，请查看[这个大师班](https://www.youtube.com/watch?v=lmy1ddn4cyw)。

```py
# Install library with: pip install ortools
# Load library
from ortools.sat.python import cp_model

# Initialise CP-SAT model
model = cp_model.CpModel()

# Set decision variables
scarves = model.NewIntVar(0, demand['scarves'], 'scarves')
hats = model.NewIntVar(0, demand['hats'], 'hats')
gloves = model.NewIntVar(0, demand['gloves'], 'gloves')

# Add constraints
model.Add(int(10*labour['scarves'])*scarves + 
          int(10*labour['hats'])*hats + 
          int(10*labour['gloves'])*gloves <= 10*labour['limit'])

model.Add(int(100*production['scarves'])*scarves + 
          int(100*production['hats'])*hats + 
          int(100*production['gloves'])*gloves <= 100*production['limit'])

# Set objective
model.Maximize(profit['scarves']*scarves + 
               profit['hats']*hats + 
               profit['gloves']*gloves)

# Solve model
solver = cp_model.CpSolver()
solver.Solve(model)

# Inspect output
print(f"Profit: ${solver.ObjectiveValue():,.2f}\n")

labour_cost = 0
production_cost = 0
for var in [scarves, hats, gloves]:
    value = solver.Value(var)
    name = var.Name()
    labour_cost += value * labour[name]
    production_cost += value * production[name]
    print(f"{value:.0f} {name}")

print(f"\nLabour cost: ${labour_cost:,.2f}")
print(f"Production cost: ${production_cost:,.2f}")
```

![](img/19a728eb5f2e44e3bc080495ecbbdda5.png)

作者提供的图片

在这里，我们确保在设置决策变量和约束时使用整数。我们还通过将劳动和生产成本乘以常数并转换其类型，同时保持其比例，来将其转换为整数。

现在，让我们使用线性求解器看看语法的对比：

```py
from ortools.linear_solver import pywraplp

# Initialise linear solver with the SCIP backend
model = pywraplp.Solver.CreateSolver('SCIP')

# Set decision variables
scarves = model.IntVar(0, demand['scarves'], 'scarves')
hats = model.IntVar(0, demand['hats'], 'hats')
gloves = model.IntVar(0, demand['gloves'], 'gloves')

# Add constraints
model.Add(labour['scarves']*scarves + 
          labour['hats']*hats + 
          labour['gloves']*gloves <= labour['limit'])

model.Add(production['scarves']*scarves + 
          production['hats']*hats + 
          production['gloves']*gloves <= production['limit'])

# Set objective
model.Maximize(profit['scarves']*scarves + 
               profit['hats']*hats + 
               profit['gloves']*gloves)
model.Solve()

# Inspect output
print(f"Profit: ${model.Objective().Value():,.2f}\n")

labour_cost = 0
production_cost = 0
for var in [scarves, hats, gloves]:
    value = var.solution_value()
    name = var.name()
    labour_cost += value * labour[name]
    production_cost += value * production[name]
    print(f"{value:.0f} {name}")

print(f"\nLabour cost: ${labour_cost:,.2f}")
print(f"Production cost: ${production_cost:,.2f}")
```

![](img/f428266eca4687c6bfd47bf0e8b2a6c0.png)

作者提供的图片

这种语法看起来与之前的语法非常相似。在使用线性求解器时，我们能够直接使用劳动和生产成本，而无需将其转换为整数。

这里有一种替代的语法可以实现相同的功能。主要的区别在于我们如何设置约束条件和目标函数。

```py
# Initialise linear solver with the SCIP backend
model = pywraplp.Solver.CreateSolver('SCIP')

# Set decision variables
scarves = model.IntVar(0, demand['scarves'], 'scarves')
hats = model.IntVar(0, demand['hats'], 'hats')
gloves = model.IntVar(0, demand['gloves'], 'gloves')

# Add constraints
labour_constraint = model.Constraint(0, labour['limit'], 'labour_constraint')
labour_constraint.SetCoefficient(scarves, labour['scarves'])
labour_constraint.SetCoefficient(hats, labour['hats'])
labour_constraint.SetCoefficient(gloves, labour['gloves'])

production_constraint = model.Constraint(0, production['limit'], 'production_constraint')
production_constraint.SetCoefficient(scarves, production['scarves'])
production_constraint.SetCoefficient(hats, production['hats'])
production_constraint.SetCoefficient(gloves, production['gloves'])

# Set objective
objective = model.Objective()
objective.SetCoefficient(scarves, profit['scarves'])
objective.SetCoefficient(hats, profit['hats'])
objective.SetCoefficient(gloves, profit['gloves'])
objective.SetMaximization()

model.Solve()

print(f"Profit: ${model.Objective().Value():,.2f}\n")

labour_cost = 0
production_cost = 0
for var in [scarves, hats, gloves]:
    value = var.solution_value()
    name = var.name()
    labour_cost += value * labour[name]
    production_cost += value * production[name]
    print(f"{value:.0f} {name}")

print(f"\nLabour cost: ${labour_cost:,.2f}")
print(f"Production cost: ${production_cost:,.2f}")
```

这就是 OR-Tools 的全部内容！希望现在你对语法的基本工作原理有了基本的了解。

🔗 要了解更多信息，请查看以下附加资源：◼️ [OR-Tools Python 示例 GitHub](https://github.com/google/or-tools/tree/stable/examples/python)

◼️ [OR-Tools 示例 | Google 开发者](https://developers.google.com/optimization/examples)

## 📌 DOcplex

我们接下来要熟悉的库是 DOcplex，这是 IBM 开发的一个库。这个工具是商业产品，因此免费版本的求解能力有限。下面是我们如何使用其约束编程求解器解决示例问题的方法：

```py
# Install library with: pip install docplex cplex
# Load library
from docplex.cp.model import CpoModel

# Initialise model
model = CpoModel()

# Set decision variables
scarves = model.integer_var(0, demand['scarves'], 'scarves')
hats = model.integer_var(0, demand['hats'], 'hats')
gloves = model.integer_var(0, demand['gloves'], 'gloves')

# Add constraints
model.add(labour['scarves']*scarves + 
          labour['hats']*hats + 
          labour['gloves']*gloves <= labour['limit'])

model.add(production['scarves']*scarves + 
          production['hats']*hats + 
          production['gloves']*gloves <= production['limit'])

# Set objective
model.maximize(profit['scarves']*scarves + 
               profit['hats']*hats + 
               profit['gloves']*gloves)
solution = model.solve()

# Inspect output
print(f"Profit: ${solution.get_objective_value():,.2f}\n")

labour_cost = 0
production_cost = 0
for name in ['scarves', 'hats', 'gloves']:
    value = solution[name]
    labour_cost += value * labour[name]
    production_cost += value * production[name]
    print(f"{value:.0f} {name}")

print(f"\nLabour cost: ${labour_cost:,.2f}")
print(f"Production cost: ${production_cost:,.2f}")
```

![](img/82abc0b4d2bc465843064291ca64c954.png)

作者提供的图片 | 仅部分输出

我们可以从输出中看到很多关于解决方案的细节。

这种语法与 OR-Tool 的语法非常相似，不是吗？我们可以通过修改前两行来使用其数学编程求解器解决这个例子：

```py
from docplex.mp.model import Model

# Initialise model
model = Model()
```

值得注意的是，以下关于`name`和`value`的代码片段

```py
for name in ['scarves', 'hats', 'gloves']:
    value = solution[name]
```

也可以通过这种方式获得：

```py
for var in [scarves, hats, gloves]:
    value = var.solution_value
    name = var.name
```

这就是 DOcplex 的基础内容！

🔗 要了解更多信息，请查看以下附加资源：◼️ [DOcplex 约束编程 Jupyter Notebook 示例](https://github.com/IBMDecisionOptimization/docplex-examples/tree/master/examples/cp/jupyter)

◼️ [DOcplex 数学编程 Jupyter Notebook 示例](https://github.com/IBMDecisionOptimization/docplex-examples/tree/master/examples/cp/jupyter)

◼️ [数学规划与约束编程](http://ibmdecisionoptimization.github.io/docplex-doc/mp_vs_cp.html)

## 📌 PuLP

最后但同样重要的是，我们将熟悉 PuLP，这是一个提供线性编程求解器的库。PuLP 也是开源软件。下面是我们可以使用其线性求解器找到示例问题最优解的一种方法：

```py
# Install library with: pip install pulp
# Load library
from pulp import LpProblem, LpMaximize, LpVariable, LpStatus

model = LpProblem(sense=LpMaximize)

# Set decision variables
scarves = LpVariable(name='scarves', lowBound=0, upBound=demand['scarves'], cat='Integer')
hats = LpVariable(name='hats', lowBound=0, upBound=demand['hats'], cat='Integer')
gloves = LpVariable(name='gloves', lowBound=0, upBound=demand['gloves'], cat='Integer')

# Add constraints
model += (labour['scarves']*scarves + 
          labour['hats']*hats + 
          labour['gloves']*gloves <= labour['limit'], 'labour_constraint')

model += (production['scarves']*scarves + 
          production['hats']*hats + 
          production['gloves']*gloves <= production['limit'], 'production_constraint')

# Set objective
model += profit['scarves']*scarves + profit['hats']*hats + profit['gloves']*gloves
model.solve()

# Inspect output
print(f"Profit: ${model.objective.value():,.2f}\n")    

labour_cost = 0
production_cost = 0
for var in model.variables():
    value = var.value()
    name = var.name
    labour_cost += value * labour[name]
    production_cost += value * production[name]
    print(f"{value:.0f} {name}")

print(f"\nLabour cost: ${labour_cost:,.2f}")
print(f"Production cost: ${production_cost:,.2f}") 
```

![](img/f97a4fe1ef0fedc70dc825a537290f32.png)

作者提供的图片 | 仅部分输出

值得一提的是，你会在 PuLP 的官方文档中看到`from pulp import *`。然而，我没有以这种方式导入库，因为我想明确地显示像`LpProblem`这样的函数的来源。

🔗 要了解更多信息，请查看以下附加资源：◼️ [PuLP 案例研究](https://coin-or.github.io/pulp/CaseStudies/index.html)

![](img/a26bc6fc729166e9294c494ad9863b5f.png)

图片由 [Michael Dziedzic](https://unsplash.com/@lazycreekimages?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

就是这样，这篇文章到此为止！优化是一个广泛的领域，在各种领域中都有许多有趣的应用。我希望这篇文章能为你介绍 Python 中的三个流行优化库，并扩展你的技巧库。这里有一些额外的资源，可能有助于加深你对优化的理解：

🔗 [数学优化导论 (stanford.edu)](https://web.stanford.edu/group/sisl/k12/optimization/MO-unit1-pdfs/1.1optimization.pdf)

🔗 [离散优化的实用指南](https://www.math.uwaterloo.ca/~bico/papers/comp_chapter1.pdf#:~:text=Discrete%20optimization%20is%20the%20study%20of%20problems%20that,the%20best%20alternative%20from%20a%20%EF%AC%81eld%20of%20possibilities.) (第一章)

🔗 [基本优化概念](https://sboyles.github.io/teaching/ce377k/introduction.pdf)

感谢你阅读这篇文章。如果你感兴趣，以下是我其他一些文章的链接：

◼️ ️️倾向评分匹配

◼️ 通过这些技巧丰富你的 GitHub 个人资料

◼️️ 从 ML 模型到 ML 管道

◼️️ [在 Python 中创建合成数据集的简单方法](https://medium.com/towards-data-science/simple-ways-to-create-synthetic-dataset-in-python-76a8e9a2f35c)

◼️️ 使用 SHAP 解释 Scikit-learn 模型

◼️️ 在 Python 中绘制多个图表的 4 个简单技巧

◼️ 美化 pandas DataFrames

◼ 你会发现有用的 Python 简单数据可视化️

◼️ 在 Seaborn (Python) 中创建更美观和定制的图表的 6 个简单技巧

暂时告别 🏃 💨
