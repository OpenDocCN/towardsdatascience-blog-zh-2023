# 线性规划：理论与应用

> 原文：[https://towardsdatascience.com/linear-programming-theory-and-applications-c67600591612?source=collection_archive---------3-----------------------#2023-04-05](https://towardsdatascience.com/linear-programming-theory-and-applications-c67600591612?source=collection_archive---------3-----------------------#2023-04-05)

## 线性优化的主要概念及其在 Python 中的实现

[](https://medium.com/@bruscalia12?source=post_page-----c67600591612--------------------------------)[![布鲁诺·斯卡利亚 C. F. 莱特](../Images/1042cd04be047c0811fef79ecd04e69c.png)](https://medium.com/@bruscalia12?source=post_page-----c67600591612--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c67600591612--------------------------------)[![数据科学之道](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c67600591612--------------------------------) [布鲁诺·斯卡利亚 C. F. 莱特](https://medium.com/@bruscalia12?source=post_page-----c67600591612--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3ce9b7482ef0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-programming-theory-and-applications-c67600591612&user=Bruno+Scalia+C.+F.+Leite&userId=3ce9b7482ef0&source=post_page-3ce9b7482ef0----c67600591612---------------------post_header-----------) 发表在 [数据科学之道](https://towardsdatascience.com/?source=post_page-----c67600591612--------------------------------) ·13 分钟阅读·2023年4月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc67600591612&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-programming-theory-and-applications-c67600591612&user=Bruno+Scalia+C.+F.+Leite&userId=3ce9b7482ef0&source=-----c67600591612---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc67600591612&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-programming-theory-and-applications-c67600591612&source=-----c67600591612---------------------bookmark_footer-----------)![](../Images/fffd9350e61f068983fceb5a4c5e2a3d.png)

图片由 [Patrick Fore](https://unsplash.com/@patrickian4?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数值优化是量化决策过程中一个基本工具。为了优化给定的目标，必须在某些规定的情况下选择相互关联的决策变量的值。描述目标和约束条件的数学方程的格式用于区分优化问题的两个主要类别：*线性*和*非线性编程*。

管理科学和运筹学广泛使用线性模型，而非线性编程问题通常自然出现在物理科学和工程学中（Nocedal & Wright, 2006）。在线性问题中，正如名称所示，目标和约束仅由线性函数描述，这将是本文的重点。

在本文中，我们将涵盖线性编程的一些主要理论方面，以及使用 Python 在经典问题中的应用。为此，我们将使用 *scipy* 和 *pyomo* 库（Bynum et al., 2021）。

你可以在这里找到本文本其余部分的简要总结：

+   [问题陈述](#d73a)

+   [可行空间和解决技术](#d77c)

+   [产品组合问题](#71af)

+   [运输问题](#0f73)

+   [进一步阅读](#0b54)

+   [结论](#cd50)

+   [参考文献](#30f1)

你可以在这个 [GIT 仓库](https://github.com/bruscalia/optimization-demo-files/tree/main/convex/linear) 找到完整的代码。现在，尽情享受吧！

# 问题陈述

在制定优化问题时，必须定义一个目标，该目标是向量决策变量 ***x*** 的函数，并可能受到一些等式和不等式约束的限制，这些约束也是 ***x*** 的函数。目标可以定义为 *最小化* 或 *最大化*，尽管前者更为常见。请注意，通过简单地乘以 *最大化* 目标的系数 *cᵢ*，可以将其重新定义为 *最小化* 目标。

在线性问题中，决策变量空间必须以某种方式加以限制。否则，决策变量将根据目标函数趋向于正无穷或负无穷。约束可以通过等式或不等式关系来制定。它们通常在问题矩阵中作为行呈现。让我们区分等式约束矩阵 ***A****_eq* 和不等式约束矩阵 ***A****_ub*。

每个组件 ***x*** 的下界和上界可能在公式中明确规定，这减少了搜索空间。作为惯例，由于解决技术，通常决策变量的下界默认等于零。这导致了如下的一般问题公式。

![](../Images/780119ebdaf4f617198f9c3b300f7ccb.png)

通用形式的线性问题。（作者提供的图片）。

不等式约束可以通过添加非负松弛变量重新公式化为等式约束。如果发生这种情况，并且所有决策变量都声明为非负，我们称线性规划为 *标准形式*。

![](../Images/b628705c02b1b81c3f0b719f6c064edf.png)

通过包含一个非负松弛变量将不等式约束重新公式化为等式。（作者提供的图片）。

然后可以陈述如下。

![](../Images/a6ea008be2f3e2dbeea03875d61bc0fd.png)

标准形式的线性规划问题。（作者提供的图片）。

注意，当不等式的对应松弛变量为零时，不等式将作为等式工作。我们将在下一节中深入探讨这一点，讨论可行空间和解决技术。

# 可行空间和解决技术

让我们引入一个简单的例子来探讨本节的概念。

![](../Images/f647840cb04341e3bb3f37cf65744ff3.png)

示例问题。（图片来源于作者）。

可以在二维空间中表示为如下图示。

![](../Images/4589a4e4dc4d3088bd99ea5debd6fdbc.png)

注意在可行区域的每个顶点（有色的），都有一个约束子集是活跃的。此外，观察到在这个示例中，最优解位于一个顶点。请记住，当不等式活跃时，它们在*标准形式*下的对应松弛变量为零。在线性问题中，我们将关注找到包含最优解的顶点中哪些约束是活跃的。

对于高维问题的推广是，如果存在最优解，则最优点的集合可以是一个单独的顶点（唯一解）、一个边或面，甚至是整个可行集合（多个解）。如果可行集合为空（不可行情况）或目标函数在可行区域上无界（无界情况），则问题没有解。

在正式定义中，假设问题的矩阵***A***在其*标准形式*下具有满行秩。在实践中，通常会进行预处理以去除一些冗余。通过添加松弛变量、盈余变量和人工变量来重新制定问题也是为了强制***A***满足这个条件（Nocedal & Wright, 2006）。如果我们可以识别一个至多具有*m*个非零分量的可行点***x***，并且对应的矩阵***B***（*m* × *m*），由***A***中与***x***中非零索引匹配的列组成，且为非奇异的，那么***x***称为*基本可行点*或*基本可行解*。设置为零的变量称为*非基本*变量，而其余的称为*基本*变量。

回到可行区域……由线性约束定义的可行集合是一个多面体（在提供的两变量示例中，它是一个多边形）。代数上，其所有顶点对应于之前描述的*基本可行点*。因此，我们可以将问题的代数视角与几何视角联系起来，从而深入理解*单纯形*方法的工作原理。

*单纯形*方法的变体可能是线性规划中最广泛使用的解决技术。它们探讨了线性规划的基本定理（Luenberger & Ye, 2008）：

1.  如果存在*可行解*，则存在*基本可行解*。

1.  如果存在*最优可行解*，则存在*最优基本可行解*。

因此，如果存在最优解，则存在一个顶点包含最优解。为了找到这样的顶点，单纯形法的步骤会探索相邻的顶点，其中基本索引的集合在一个分量上有所不同，直到没有进一步改进为止。对获取起始可行点、证明解的最优性以及在两个连续顶点之间迭代感兴趣的读者，可以参考Nocedal & Wright（2006）或Winston & Goldberg（2004）。

内点方法也被广泛使用，特别是对于大型线性规划问题。内点方法具有一些与单纯形法不同的共同特征。每次内点迭代的计算成本较高，但能显著推进解决方案，而单纯形法通常需要更多的低成本迭代（Nocedal & Wright，2006）。一些优化求解器可能还会使用单纯形法的变体来完善由内点方法获得的解决方案。

*单纯形*方法和*内点*方法都在下图中表示。

![](../Images/e6d4a18764a72ed98721583ecae7ea57.png)

单纯形法（左）和内点法（右）的图示表示。（作者提供的图片）。

现在完成了一些重要的理论方面的内容，在下一部分，我们将使用Python实现我们的第一个问题。为了解决这个问题，我们将首先使用Python包*scipy*，它有对开源求解器HiGHS的封装。此外，我们还将使用*pyomo*（Bynum等，2021）实现相同的问题，并用CBC求解器解决它。两个求解器都将利用单纯形法的变体来获得解决方案。

# 产品组合问题

在产品组合问题中，我们必须将有限的*I*资源分配给生产*J*种产品，以最大化预期回报。每种产品*j*具有已知的单位利润率*cⱼ*，并需要已知量的每种资源*fᵢⱼ*。考虑每种资源的可用量*bᵢ*。我们的决策变量表示每种产品应生产的数量*xⱼ*。我们可以将问题表述为如下。

![](../Images/fc6f2d68458a5e3fddd97a099dd637f0.png)

产品组合问题。（作者提供的图片）。

从这个结构出发，我们可以轻松地将问题转换为矩阵形式，其中矩阵*A_ub*在第*i*行和第*j*列的元素是*fᵢⱼ*。

考虑一个例子，其中我们有三种反应物{A, B, C}用于生产三种产品{D, E, F}。每种反应物的可用量分别为8000、3000和2000。每种产品的利润率分别为2.15、1.34、1.72。比例见下表。

```py
+---------------------+--------+-------+------+
| Reactant \ Product  |   D    |   E   |   F  |
+---------------------+--------+-------+------+
| A                   |  7/10  |  1/3  |  1/2 |
| B                   |  1/5   |  2/3  |  1/6 |
| C                   |  1/10  |  0    |  1/3 |
+---------------------+--------+-------+------+
```

让我们通过导入有用的库来开始实现这个问题。

```py
import numpy as np
from scipy.optimize import linprog
```

让我们实例化*numpy*数组来表示我们的参数。

```py
# Pseudo costs
margins = np.array([2.15, 1.34, 1.72])
c = - margins

# Proportions
A = np.array([
    [7/10, 1/3, 1/2],
    [1/5, 2/3, 1/6],
    [1/10, 0.0, 1/3]
])

# Availability
b = np.array([8000.0, 3000.0, 2500.0])
```

我们现在几乎完成了。接下来需要做的就是调用*scipy*函数*linprog*来解决这个问题。

```py
sol = linprog(c, A_ub=A, b_ub=b)
print(sol)
```

它应该返回具有属性*fun*（目标函数）和*x*（决策变量向量）的解决方案。

使用矩阵符号来描述这个问题比使用其他方法更加直观。对于更复杂的问题，代数建模语言（AML）可以非常有帮助。在Python中，一个出色的开源替代方案是*pyomo*（Bynum等，2021）。那么让我们将其应用于产品组合问题。

```py
import pyomo.environ as pyo
```

在*pyomo*中，有两种建模问题的方法：*Abstract*和*Concrete*模型。在第一种方法中，问题的代数表达式是在提供数据值之前定义的，而在第二种方法中，模型实例在定义其元素时立即创建。您可以在[库文档](https://pyomo.readthedocs.io/en/stable/pyomo_overview/abstract_concrete.html)中或在Bynum等（2021）的书中找到更多关于这些方法的信息。在本文中，我们将采用*Concrete*模型形式。

```py
model = pyo.ConcreteModel()
```

在这个问题中有两个集合：

+   资源*I*

+   产品*J*

使用*pyomo*时，我们可以将这些集合创建为建模组件，这样可以实现某些集合操作的好处。具体如下。

```py
model.I = pyo.Set(initialize=["A", "B", "C"])
model.J = pyo.Set(initialize=["D", "E", "F"])
```

由于我们使用*ConcreteModel*方法，因此在实例化时我们必须立即为每个组件提供值。对于集合，这是通过*initialize*关键字参数完成的。

现在让我们创建模型的固定参数。在*pyomo*中，我们可以使用*Param*类来存储参数。参数可以在集合上定义。在这种情况下，我们必须在定义中将给定的集合作为第一个参数传递。至于其他*pyomo*组件，如果定义的元素（变量、参数、表达式或约束）由多个集合索引，可以在此语句中传递多个集合。此定义采用了Python的**args**风格。

```py
# Availability
model.b = pyo.Param(model.I, initialize=dict(zip(model.I, b)))

# Margins (now as a maximization objective)
model.c = pyo.Param(model.J, initialize=dict(zip(model.J, margins)))

# Proportions
proportions = {}
for k, i in enumerate(model.I):
    for l, j in enumerate(model.J):
        proportions[i, j] = A[k, l]

model.f = pyo.Param(model.I, model.J, initialize=proportions)
```

现在，让我们实例化决策变量和约束。这是分别使用*Var*和*Constraint*组件完成的。请注意，我指定了*x*的领域为非负实数。这可以在*pyomo*中通过*within*关键字参数来实现。

```py
# Decision variables
model.x = pyo.Var(model.J, within=pyo.NonNegativeReals)

# Availability constraint
def availability_rule_cstr(model, i):
    return sum(model.f[i, j] * model.x[j] for j in model.J) <= model.b[i]

model.cstr_available = pyo.Constraint(model.I, rule=availability_rule_cstr)
```

请注意，约束是在传递*rule*关键字参数的情况下实例化的。该规则必须是一个可调用的函数，它接收模型作为第一个参数，后面是其对应域的索引。

我们快完成了！让我们定义目标函数。

```py
# Objective function
def obj_func(model):
    return sum(model.x[j] * model.c[j] for j in model.J)

model.obj = pyo.Objective(rule=obj_func, sense=pyo.maximize)
```

最后，让我们实例化一个求解器，并使用它来解决我们的模型。在这个例子中，我使用了开源求解器CBC。您可以从[这个链接](https://www.coin-or.org/download/binary/Cbc/)下载CBC二进制文件。您还可以[在这里](https://github.com/coin-or/Cbc)找到安装教程。因为CBC可执行文件已包含在我的系统的PATH变量中，所以我可以在不指定可执行文件路径的情况下实例化求解器。如果您的系统没有设置，请通过关键字参数“executable”传递可执行文件的路径。

```py
cbc = pyo.SolverFactory("cbc")
sol = cbc.solve(model, tee=False)
```

完成了！现在你可以通过使用 *print* 或 *display* 方法访问任何建模组件并检查其值。正如预期的那样，使用 *pyomo* 和 CBC 的解决方案与之前使用 *scipy* 的结果完全相同。

在接下来的部分中，让我们解决一个稍微复杂一点的问题：运输问题。

# 运输问题

在运输问题中，我们必须将供应商 *I* 的有限容量 *bᵢ* 与客户 *J* 的已知需求 *dⱼ* 匹配。每对 (*i*, *j*) 具有已知的供应成本 *cᵢⱼ*。我们的目标是通过定义每个需求由每个供应商提供的数量来最小化总成本。因此，我们考虑决策变量 *xᵢⱼ* 来表示这些数量。如果我们考虑总供应能力满足总需求的条件，我们可以将问题表述为以下内容。

![](../Images/bf95121652cdd7505efabfa5f004e954.png)

运输问题。（作者提供的图片）。

让我们考虑一个有四个客户 {A, B, C, D} 和三个供应商 {1, 2, 3} 的例子。每个客户的需求分别为 5、15、13 和 17。每个供应商的容量分别为 14、26 和 11。现在考虑以下成本矩阵。

```py
+---+------+------+------+------+
|   |   A  |    B |    C |    D |
+---+------+------+------+------+
| 1 |   10 |    5 |   20 |   12 |
| 2 |   12 |    7 |   12 |   19 |
| 3 |    6 |   12 |   16 |   17 |
+---+------+------+------+------+
```

让我们将这些参数实例化为 Python 对象。

```py
costs = pd.DataFrame({
    "A": [10, 12, 6],
    "B": [5, 7, 12],
    "C": [20, 12, 16],
    "D": [12, 19, 17],
}, index=[1, 2, 3])

availability = {1: 14, 2: 26, 3: 11}
demands = {"A": 5, "B": 15, "C": 13, "D": 17}
```

让我们开始实现我们的 *pyomo* 模型。

```py
model = pyo.ConcreteModel()
```

我们再次有两个集合：

+   供应商 *I*

+   客户 *J*

相应的语法应该与之前的示例非常相似。

```py
model.I = pyo.Set(initialize=[1, 2, 3])
model.J = pyo.Set(initialize=["A", "B", "C", "D"])
```

让我们将这些参数实例化为 *pyomo* 组件。

```py
model.b = pyo.Param(model.I, initialize=availability)

model.d = pyo.Param(model.J, initialize=demands)

c = {(i, j): costs.loc[i, j] for i in costs.index for j in costs.columns}
model.c = pyo.Param(model.I, model.J, initialize=c)
```

我们的决策变量…

```py
model.x = pyo.Var(model.I, model.J, within=pyo.NonNegativeReals)
```

这一次我们应该有等式约束和不等式约束，因为每个需求必须得到满足（等式），同时要尊重供应商的容量（不等式）。

```py
def availability_rule(model, i):
    return sum(model.x[i, j] for j in model.J) <= model.b[i]

model.availability_constr = pyo.Constraint(model.I, rule=availability_rule)

def demand_rule(model, j):
    return sum(model.x[i, j] for i in model.I) == model.d[j]

model.demand_constr = pyo.Constraint(model.J, rule=demand_rule)
```

让我们定义目标函数，再次使用 CBC 来解决问题。注意这次，我使用了 *expr* 关键字参数来定义目标，它直接接收一个 *pyomo* 表达式。

```py
model.obj = pyo.Objective(expr=sum(model.x[i, j] * model.c[i, j] for (i, j) in model.x), sense=pyo.minimize)
sol = cbc.solve(model, tee=False)
```

在这个示例中，我将把结果导出到 *pandas* DataFrame，以便更好地可视化。

```py
results = pd.DataFrame(index=model.I, columns=model.J)
for i, j in model.x:
    results.loc[i, j] = model.x[i, j].value
```

这应该返回类似的结果。

```py
+---+---------+----------+----------+----------+
|   |    A    |      B   |      C   |      D   |
+---+---------+----------+----------+----------+
| 1 |     0.0 |      2.0 |      0.0 |     12.0 |
| 2 |     0.0 |     13.0 |     13.0 |      0.0 |
| 3 |     5.0 |      0.0 |      0.0 |      5.0 |
+---+---------+----------+----------+----------+
```

在这种情况下，所有客户都将以最低的总成本得到服务。但如果我们供应商的可用性少于总需求呢？那么问题将变得不可行，我们需要定义哪些约束应该被违反以返回某种优化结果。在这个问题中，一个简单的建议是引入人工变量来满足那些单位成本高于任何原始供应商的需求。请记住，最佳策略应该是针对实际问题中的每种场景唯一的。

# 进一步阅读

为了更深入地理解线性规划的理论方面，我强烈建议阅读 Luenberger & Ye (2008) 和 Nocedal & Wright (2006) 书中的相关章节。*对偶性*的概念特别有用，因为它涉及灵敏度分析、问题的经济解释和解决技术（仅举几个应用）。

运筹学广泛使用数值优化。Winston & Goldberg (2004) 的书对于那些对优化的更多应用感兴趣的人可以非常有帮助，除了与相关科学的联系。

一些实际问题包括离散变量，以形成析取问题方面和整体性。我写了几篇关于这个主题的中等文章，可能会有所帮助。请参阅这里的 [分支与界限介绍](/a-gentle-introduction-to-branch-bound-d00a4ee1cad)，经典的 [背包问题](https://medium.com/towards-data-science/an-introduction-to-mixed-integer-linear-programming-the-knapsack-problem-1445452a9fe9)，以及 [车间调度问题](https://medium.com/towards-data-science/the-job-shop-scheduling-problem-mixed-integer-programming-models-4bbee83d16ab)。

一些问题还包括*非线性*函数，无论是在目标函数还是约束条件中。如果是这种情况，你可能会想查看这篇其他文章：

[](/nonlinear-programming-theory-and-applications-cfe127b6060c?source=post_page-----c67600591612--------------------------------) [## 非线性编程：理论与应用]

### 基于梯度的线性搜索优化算法详细解释并在 Python 中从零实现。

towardsdatascience.com](/nonlinear-programming-theory-and-applications-cfe127b6060c?source=post_page-----c67600591612--------------------------------)

# 结论

在本文中，详细解释了线性优化的一些最相关的理论方面，并通过两个实际实现示例进行了说明：产品组合问题和运输问题。使用了 Python 库*scipy*来通过矩阵符号解决产品组合问题，并且这两个问题均使用 Python AML *pyomo* 建模，并使用开源求解器进行求解。所用代码完全开放给读者，可以在这个 [GIT 仓库](https://github.com/bruscalia/optimization-demo-files/tree/main/convex/linear)中找到。

# 参考文献

Bynum, M. L. 等, 2021\. *Pyomo：Python中的优化建模.* 施普林格。

Luenberger, D. G. & Ye, Y., 2008\. *线性与非线性编程.* 第三版. 斯坦福: 施普林格。

Nocedal, J. & Wright, S. J., 2006\. *数值优化.* 第二版. 纽约: 施普林格纽约。

Winston, W. L. & Goldberg, J. B., 2004\. *运筹学：应用与算法.* 第四版. 贝尔蒙特，加州: 汤姆森布鲁克斯/科尔贝尔蒙特。
