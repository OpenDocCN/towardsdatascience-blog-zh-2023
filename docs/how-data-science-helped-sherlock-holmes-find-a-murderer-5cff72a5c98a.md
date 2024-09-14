# 数据科学如何帮助福尔摩斯找到凶手

> 原文：[`towardsdatascience.com/how-data-science-helped-sherlock-holmes-find-a-murderer-5cff72a5c98a`](https://towardsdatascience.com/how-data-science-helped-sherlock-holmes-find-a-murderer-5cff72a5c98a)

## 使用图论、约束编程和混合整数线性规划解决“谁杀了丹斯莫尔公爵”的谜团

[](https://medium.com/@bernardovf?source=post_page-----5cff72a5c98a--------------------------------)![伯纳多·弗图多](https://medium.com/@bernardovf?source=post_page-----5cff72a5c98a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5cff72a5c98a--------------------------------)![数据科学的前沿](https://towardsdatascience.com/?source=post_page-----5cff72a5c98a--------------------------------) [伯纳多·弗图多](https://medium.com/@bernardovf?source=post_page-----5cff72a5c98a--------------------------------)

·发表于 [数据科学的前沿](https://towardsdatascience.com/?source=post_page-----5cff72a5c98a--------------------------------) ·14 分钟阅读·2023 年 4 月 28 日

--

![](img/dec485cff44fa5de03d7ae2f2233fda4.png)

图片由 [弗拉基米尔·赫里申科](https://unsplash.com/@lunarts?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/ZT9gjcJog6U?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

库存管理、投资组合优化、机器调度、车辆路线以及许多其他现实生活中的问题都是数据科学和分析技术应用的绝佳例子。这些问题在大学里通常是我们最早学习的内容。然而，我对可以被表示为抽象实体的众多其他问题感到着迷，这些实体可以是图、计划或一组数学方程，并利用适当的技术加以解决。一旦你掌握了建模技能，将这些技术应用于供应链问题或简单的谜题都是很直接的。

在本文中，我将展示如何使用 ***图论***、***约束编程*** 和 ***混合整数线性规划*** 解决丹斯莫尔公爵谋杀案的谜团。

克劳德·贝尔热的《*谁杀了丹斯莫尔公爵*》是一部悬疑小说，专注于找出在怀特岛的丹斯莫尔城堡安放炸弹导致公爵死亡的人。尽管过去了十年，这个案件仍然未能破解，直到福尔摩斯和华生决定深入探讨此事。

当邓斯摩尔公爵因爆炸事件在怀特岛的城堡中遇难时，他的六位前妻——*安*、*贝蒂*、*夏洛特*、*伊迪丝*、*费利西亚*、*乔治亚*和*海伦*——成为了主要嫌疑人。问题进一步复杂化的是公爵的遗嘱在爆炸中被毁，据说其中对某位前妻的态度非常严厉。然而，揭示了所有六位女性在悲剧发生前都曾收到过城堡的邀请。

他所有的前妻都发誓她们只去过一次城堡。她们也不记得自己在那里待了多久，但知道在访问期间遇见了谁。

+   *安*遇见了*贝蒂*、*夏洛特*、*费利西亚*和*乔治亚*。

+   *贝蒂*遇见了*安*、*夏洛特*、*伊迪丝*、*费利西亚*和*海伦*。

+   *夏洛特*遇见了*安*、*贝蒂*和*伊迪丝*。

+   *伊迪丝*遇见了*贝蒂*、*夏洛特*和*费利西亚*。

+   *费利西亚*遇见了*安*、*贝蒂*、*伊迪丝*和*海伦*。

+   *乔治亚*遇见了*安*和*海伦*。

+   *海伦*遇见了*贝蒂*、*费利西亚*和*乔治亚*。

根据提供的信息，福尔摩斯和华生认为凶手多次访问了城堡，这与前妻们只访问过一次的说法相矛盾。侦探怀疑其中一位前妻在谎报她访问城堡的次数。确定哪位前妻对她们访问的频率撒了谎，是解开谜团的关键。

我将展示三种解决这个难题的方法：使用图论、约束编程和混合整数线性编程。

## 图论

这就是贝尔热最终解决谜团的方法，这似乎是最直接的办法。每个前妻在城堡的停留时间都是特定的，有明确的到达和离开时间。虽然我们不知道她们的确切停留时间以及到达和离开的顺序，但从审问中我们知道她们的某些访问时间是重叠的。

为了简化问题，我们集中关注*海伦*及其在城堡访问期间与*贝蒂*、*费利西亚*和*乔治亚*的互动。为了说明她们的访问，我们可以使用水平线，端点表示她们的到达和离开时间，如下所示：

![](img/6c6732ac05bd52ce0db5a35c7da9762d.png)

时间段和重叠示例

如果两个时间段有重叠，这意味着对应的人在城堡里同时出现过，因此一定见过面。我们可以将时间段表示为图，其中每个节点对应一个人。如果两个时间段相交，我们就在相应的节点之间画一条边。对于*海伦*、*贝蒂*、*费利西亚*和*乔治亚*，最终得到的图如下所示：

![](img/a1ac57886d42b44e08d7396642043aec.png)

时间段图示例

这种图称为*时间段图*，可以用来建模任何时间段集合。然而，并非所有图都可以表示为时间段。例如，考虑下图：

![](img/f470b3082f555fe561899a7572afed8d.png)

周期图 C4

它不能被分解为区间，因为无法找到四个区间，使得 *A-B*, *B-C*, *C-D* 和 *D-A* 相交。这个图称为 *C4*。如果一个图有 *C4* 作为子图，肯定不是一个区间图。

因此，如果图不是一个区间图，我们知道至少有一个人撒谎关于他们是否访问过城堡。我们可以利用这一点通过从审问中构建图并检查它是否为区间图来识别真正的罪犯。

![](img/2e0a64d264e7c9ed6ffd54d9434dc762.png)

基于审问的图。如果两个人见过面，它们之间就有一条边。

让我们在 Python 中使用库[*networkx*](https://networkx.org/)并创建图：

```py
people = ['Ann', 'Betty', 'Charlotte', 'Edith', 'Felicia', 'Georgia', 'Helen']
people_met = {'Ann': ['Betty', 'Charlotte', 'Felicia', 'Georgia'],
              'Betty': ['Ann', 'Charlotte', 'Edith', 'Felicia', 'Helen'],
              'Charlotte': ['Ann', 'Betty', 'Edith'],
              'Edith': ['Betty', 'Charlotte', 'Felicia'],
              'Felicia': ['Ann', 'Betty', 'Edith', 'Helen'],
              'Georgia': ['Ann', 'Helen'],
              'Helen': ['Betty', 'Felicia', 'Georgia']}

def create_graph(plot_graph=False):
    """
    Generate graph based on interrogation, and draw it it plot_graph = True
    :param plot_graph: True if graph is plotted
    :return: Graph
    """
    G = nx.Graph()
    for person in people:
        G.add_node(person)
    for p1 in people_met.keys():
        for p2 in people_met[p1]:
            G.add_edge(p1, p2)
    if plot_graph:
        nx.draw_spectral(G, node_size=500, node_color=['red', 'green', 'pink', 'brown', 'yellow', 'orange', 'skyblue'],
                         with_labels=True)
        plt.show()
    return G
```

既然图已经定义好了，接下来需要进行以下两个步骤：

+   使用 *networkx* 的 *simple_cycles* 函数生成图的所有简单周期（见文档[此处](https://networkx.org/documentation/stable/reference/algorithms/generated/networkx.algorithms.cycles.simple_cycles.html)）。将生成所有长度的基本周期。

+   对于所有长度为 4 的周期，使用 *subgraph* [函数](https://networkx.org/documentation/stable/reference/generated/networkx.classes.function.subgraph.html#subgraph)生成子图。并检查它是否不是[*chordal*](https://networkx.org/documentation/stable/reference/algorithms/generated/networkx.algorithms.chordal.is_chordal.html#is-chordal)。换句话说，检查该周期是否为 *C4* —— 一个长度为 4 的周期，其中所有节点的度数均为 2（见[此处](https://en.wikipedia.org/wiki/Cycle_graph)）。

```py
def check_cycles(G):
    """
    Print all the chordless cycles with length = 4
    :param G: Input graph
    """
    list_cycles = list(sorted(nx.simple_cycles(G.to_directed())))
    for l in list_cycles:
        if len(l) == 4:
            H = G.subgraph(l)
            if not nx.is_chordal(H):
                print(l, len(l))

G = create_graph(False)
check_cycles(G)
```

*C4* 子图是：

+   *安*, *夏洛特*, *伊迪斯*, *费莉西亚*

+   *安*, *费莉西亚*, *伊迪斯*, *夏洛特*

+   *乔治亚*, *海伦*, *贝蒂*, *安*

+   *乔治亚*, *安*, *费莉西亚*, *海伦*

+   *乔治亚*, *海伦*, *贝蒂*, *安*

+   *乔治亚*, *海伦*, *费莉西亚*, *安*

***安*** 出现在所有周期中，如果将她从集合中去除，可以完美地生成一个区间图，这表明她可能在撒谎，并可能被认为是凶手。

## 约束编程

约束编程（CP）是一种解决问题的方法，旨在通过关注寻找满足给定约束的可行解来解决组合和逻辑问题。CP 算法通过逐步减少可行解集来实现收敛。由于其处理复杂逻辑语句和约束的能力，CP 在解决当前问题时特别有效。

为了解决我们的问题，我们可以使用一个 CP 模型，其中 *N* 是人们的集合，*s_i* 是个体 *i* 到达城堡的时间，*d_i* 是他们访问的持续时间，*e_i* 是他们访问的结束时间。我们还可以引入一个逻辑变量 *v_i*，如果个体 *i* 不能被包含在访问计划中，则 *v_i* 为 *TRUE*，否则为 *FALSE*。此外，*met_i* 是个体 *i* 在访问期间遇到的人的集合。整数变量 *s*、*d* 和 *e* 的上限很高（1000），并且必须是非负的。

我们将使用 [*ortools*](https://developers.google.com/optimization/cp/cp_solver)库来解决这个问题：

```py
from ortools.sat.python import cp_model
import pandas as pd
from matplotlib import pyplot as plt

people = ['Ann', 'Betty', 'Charlotte', 'Edith', 'Felicia', 'Georgia', 'Helen']
people_met = {'Ann': ['Betty', 'Charlotte', 'Felicia', 'Georgia'],
           'Betty': ['Ann', 'Charlotte', 'Edith', 'Felicia', 'Helen'],
           'Charlotte': ['Ann', 'Betty', 'Edith'],
           'Edith': ['Betty', 'Charlotte', 'Felicia'],
           'Felicia': ['Ann', 'Betty', 'Edith', 'Helen'],
           'Georgia': ['Ann', 'Helen'],
           'Helen': ['Betty', 'Felicia', 'Georgia']}

model = cp_model.CpModel()
num_vals = 1000

# Arrival time of person i in the castle
s = {i: model.NewIntVar(0, num_vals, 'start_' + i) for i in people}
# Duration of the visit of person i in the castle
e = {i: model.NewIntVar(0, num_vals, 'end_' + i) for i in people}
# Departure time of person i from the castle
d = {i: model.NewIntVar(0, num_vals, 'duration_' + i) for i in people}
# Logic variable that is True (1) is person cannot be included in the visit schedule. False otherwise
v = {i: model.NewBoolVar('v_' + i) for i in people}
```

初始步骤是确定两个个体是否重叠，这意味着验证区间 *(s_i, e_i)* 和 *(s_j, e_j)* 是否在任何时刻相交。这可以通过满足以下两个条件之一来实现：

+   *s_j < e_i* AND *s_i < e_j*

+   *s_i < e_j* AND *s_i > s_j*

还需要定义当两个区间不重叠时的情况。它应满足以下两个条件之一：

+   *e_i ≤ s_j*

+   *s_i ≥ e_j*

本质上，为了让两个人见面，它们的区间必须重叠，否则它们必须不重叠。然而，如果一个人不能被包含在计划中呢？这就是变量 *v_i* 发挥作用的地方。

最后，对于每一对人 *i* 和 *j*：

![](img/9519ea042495bb5cefa968635b851b6d.png)

当两个个体 *i* 和 *j* 见面时，它们必须遵守以下两个重叠条件之一：(*s_j < e_i* AND *s_i < e_j*) 或者 (*s_i < e_j* AND *s_i > s_j*)。如果它们未能遵守其中一个条件，那么一个或两个个体将不会被包含在计划中 (*v_i = True* 或者 *v_j = True*)。

另一方面，如果两个个体 *i* 和 *j* 不见面，它们应遵循非重叠条件 (*e_i ≤ s_j* 或者 *s_i ≥ e_j*)。如果它们不遵守这个条件，一个或两个个体将不会被包含在计划中 (*v_i = True* 或者 *v_j = True*)。

为了表示这些条件，我们将使用布尔变量 *b1*、*b2*、*b3* 和 *b4*。

+   *b1 =* *s_j < e_i* AND *s_i < e_j*

+   *b2 =* *s_i < e_j* AND *s_i > s_j*

+   *b3 =* *e_i ≤ s_j*

+   *b4 =* *s_i ≥ e_j*

```py
# Auxiliary Boolean variables
b1 = {(p, p2): model.NewBoolVar(name='b1_' + p + "_" + p2) for p in people for p2 in people if p != p2}
b2 = {(p, p2): model.NewBoolVar(name='b2_' + p + "_" + p2) for p in people for p2 in people if p != p2}
b3 = {(p, p2): model.NewBoolVar(name='b3_' + p + "_" + p2) for p in people for p2 in people if p != p2}
b4 = {(p, p2): model.NewBoolVar(name='b4_' + p + "_" + p2) for p in people for p2 in people if p != p2}

for i in people:
    for j in people:
        if i != j:
            if j in people_met[i]:
                # Overlap condition 1
                model.Add(e[i] > s[j]).OnlyEnforceIf(b1[(i, j)])
                model.Add(s[i] < e[j]).OnlyEnforceIf(b1[(i, j)])
                # Overlap condition 2
                model.Add(s[i] < e[j]).OnlyEnforceIf(b2[(i, j)])
                model.Add(s[i] > s[j]).OnlyEnforceIf(b2[(i, j)])
                # At least one of the overlap conditions
                model.AddAtLeastOne([b1[(i, j)], b2[(i, j)], v[i], v[j]])
            else:
                # No overlap condition 1
                model.Add(e[i] <= s[j]).OnlyEnforceIf(b3[(i, j)])
                # No overlap condition 2
                model.Add(s[i] >= e[j]).OnlyEnforceIf(b4[(i, j)])
                # At least one of the non overlap conditions
                model.AddAtLeastOne([b3[(i, j)], b4[(i, j)], v[i], v[j]])
```

如果一个人的访问持续时间 (*d_i*) 为 *0*，则 *v_i* 的值为 True。这意味着该人不被包含。如果 *v_i* 为 False，则 *d_i* 必须是严格正的。

![](img/86dd247aa1ace7721166888002808855.png)

```py
# Duration of person (d_i) is 0 is value v_i is True. If v_i is False, d_i must be strictly positive.
for i in people:
    model.Add(d[i] == 0).OnlyEnforceIf(v[i])
    model.Add(d[i] > 0).OnlyEnforceIf(v[i].Not())
```

最后，最后一个约束确保每个人的到达时间加上持续时间等于结束时间

![](img/3a32ebeed9f8ad7d339cf934484e4b01.png)

```py
# Departure time is equal to arrival time plus duration
for i in people:
    model.Add(s[i] + d[i] == e[i])
```

我们的目标函数是最大化包含的人员数量，这等同于最小化变量 *v_i* 的总和。

![](img/7aece2dd6b2948820732e23a23c23d0f.png)

```py
# Objective function is maximizing the number of people included - minimizing the sum of variable v_i.
var_obj = 0
for i in people:
    var_obj += v[i]
model.Minimize(var_obj)Running the model we can plot the gantt chart generated:
```

通过解决模型并绘制甘特图，我们得到如下结果：

```py
 solver = cp_model.CpSolver()
status = solver.Solve(model)

if status == cp_model.OPTIMAL:
   for p in people:
      print(p, solver.Value(s[p]), solver.Value(e[p]), solver.Value(d[p]))
gantt_result_dict = {'Person': [], 'Start': [], 'End': [], 'Duration': []}
for i in people:
   gantt_result_dict['Person'].append(i)
   gantt_result_dict['Start'].append(solver.Value(s[i]))
   gantt_result_dict['End'].append(solver.Value(e[i]))
   gantt_result_dict['Duration'].append(solver.Value(d[i]))
gantt_result_df = pd.DataFrame.from_dict(gantt_result_dict)
print(gantt_result_df)
fig, ax = plt.subplots(1, figsize=(16, 6))
ax.barh(gantt_result_df['Person'], gantt_result_df['Duration'], left=gantt_result_df['Start'])
plt.grid()
plt.show()
```

![](img/ba27d1d442240f376c648cb9ff69e4de.png)

运行 CP 模型生成的甘特图

谁是那个不能被包含且持续时间等于 0 的人？**安**！

## 混合整数线性规划

虽然这可能不是最简单的方法，但我最喜欢的解决这个难题的方法是使用 *MILP*。要了解更多关于如何建模和解决 *MILP* 的信息，可以参考我下面的文章：

> [`medium.com/towards-data-science/heuristics-as-warm-start-for-mixed-integer-programming-mip-models-9046781dd21f`](https://medium.com/towards-data-science/heuristics-as-warm-start-for-mixed-integer-programming-mip-models-9046781dd21f)

我们仍然有集合 *N*，表示参与的人，以及 *met_i*，表示人员 *i* 遇到的人。在这种情况下，我们将采用时间索引方法，事先定义一组时间段 *T*（假设 *T = 6*）。变量如下：

+   *y_i*：二进制。若人员 *i* 被包含，则为 1。否则为 0

+   *x_(i, t)*：二进制。若人员 *i* 在 *t* 在城堡中，则为 1。否则为 0

+   *z_(i, j, t)*：二进制。若人员 *i* 和 *j* 在 *t* 同时在城堡中，则为 1。否则为 0

+   *w_(i, t)*：二进制。若人员 *i* 在 *t* 到达城堡，则为 1。否则为 0

让我们还使用 [*ortools*](https://developers.google.com/optimization/mip) 来解决它：

```py
import pandas as pd
from matplotlib import pyplot as plt
from ortools.linear_solver import pywraplp

people = ['Ann', 'Betty', 'Charlotte', 'Edith', 'Felicia', 'Georgia', 'Helen']
people_met = {'Ann': ['Betty', 'Charlotte', 'Felicia', 'Georgia'],
              'Betty': ['Ann', 'Charlotte', 'Edith', 'Felicia', 'Helen'],
              'Charlotte': ['Ann', 'Betty', 'Edith'],
              'Edith': ['Betty', 'Charlotte', 'Felicia'],
              'Felicia': ['Ann', 'Betty', 'Edith', 'Helen'],
              'Georgia': ['Ann', 'Helen'],
              'Helen': ['Betty', 'Felicia', 'Georgia']}

periods = 6
T = [p for p in range(0, periods)]
solver = pywraplp.Solver('puzzle', pywraplp.Solver.CBC_MIXED_INTEGER_PROGRAMMING)

# 1 if person i is included. 0 otherwise
y = {(i): solver.IntVar(0, 1, "y_{0}".format(i)) for i in people}

# 1 if person i is in the castle at t. 0 otherwise
x = {(i, t): solver.IntVar(0, 1, "x_{0}_{1}".format(i, t)) for i in people for t in T}

# 1 if person i and j are in the castle simultaneously at t. 0 otherwise
z = {(i, j, t): solver.IntVar(0, 1, "z_{0}_{1}_{2}".format(i, j, t))
for i in people for j in people for t in T if i != j}

# 1 if person i arrives in the castle at t. 0 otherwise
w = {(i, t): solver.IntVar(0, 1, "w_{0}_{1}".format(i, t)) for i in people for t in range(0, periods - 1)}
```

约束条件如下所示：

![](img/8e400b4a70435d240dbf7ad91ae5e996.png)

*(1)* 保证任何包含在计划中的个人必须至少在城堡中出现过一次。*(2)* 确保如果一个人被包含，他们必须与其他他们遇到的个体在同一时间在城堡中。*(3)* 和 *(4)* 确保两个个体仅当且仅当他们都被包含且在某时刻都在城堡中时才同时存在。*(5)* 确保未见过的两个人不能同时在城堡中。*(6)* 定义每个人的到达时间。*(7)* 不允许任何人在时间段 0 和 T（为了简单起见）期间在城堡中。最后，*(8)* 保证任何包含在计划中的人只有一个记录的到达时间。

```py
# If a person is included she must have been in the castle at least once
c1 = {i: solver.Add(solver.Sum(x[(i, t)] for t in T) >= y[i]) for i in people}

# If a person is included she must have been in the castle simultaneously with other people she met at least once
c2 = {(i, j): solver.Add(solver.Sum(z[(i, j, t)] for t in T) >= y[i])
      for i in people for j in people_met[i]}

# Two people were simultaneously at t in the castle only if they are both there at t
c3 = {(i, j, t): solver.Add((z[(i, j, t)] >= x[(i, t)] + x[(j, t)] - 1))
      for i in people for j in people for t in T if i != j}

# Two people were simultaneously at t in the castle only if they are both there at t and if they are included
c4 = {(i, j, t): solver.Add(
    (x[(i, t)] + x[(j, t)] + periods * (1 - z[(i, j, t)]) >= y[i] + y[j]))
      for i in people for j in people for t in T if i != j}

# Two people that did not meet cannot be in the castle simultaneously
c5 = {(i, j): solver.Add(solver.Sum(z[(i, j, t)] for t in T) == 0)
      for i in people for j in people if i != j and j not in people_met[i]}

# Defines the arrival time of person
c6 = {(i, t): solver.Add((w[(i, t)] >= x[(i, t + 1)] - x[(i, t)]))
      for i in people for t in range(0, periods - 1)}

# Person cannot be in the castle in period 0 and period T
c7 = {(i): solver.Add(x[(i, 0)] + x[(i, periods - 1)] == 0)
      for i in people}

# If a person was included it has just one arrival time
c8 = {(i): solver.Add(solver.Sum(w[(i, t)] for t in range(0, periods - 1)) == y[i])
      for i in people}
```

目标函数是最大化包含的人数

![](img/3b32dcc928520003da0bf3f373147262.png)

```py
number_of_people = solver.Sum(y[i] for i in people)
solver.Maximize(number_of_people)
```

解决模型并绘制甘特图，我们得到如下结果：

```py
gantt_result_dict = {'Person': [], 'Start': [], 'End': [], 'Duration': []}
duration = {i: 0 for i in people}
start = {i: 0 for i in people}
end = {i: 0 for i in people}
for i in people:
    for t in T:
        if x[(i, t)].solution_value() >= 0.1:
            duration[i] += 1
            if x[(i, t - 1)].solution_value() == 0:
                start[i] = t
    end[i] = start[i] + duration[i]
    gantt_result_dict['Person'].append(i)
    gantt_result_dict['Start'].append(start[i])
    gantt_result_dict['End'].append(end[i])
    gantt_result_dict['Duration'].append(duration[i])
gantt_result_df = pd.DataFrame.from_dict(gantt_result_dict)
print(gantt_result_df)
fig, ax = plt.subplots(1, figsize=(16, 6))
ax.barh(gantt_result_df['Person'], gantt_result_df['Duration'], left=gantt_result_df['Start'])
plt.grid()
plt.show()
```

![](img/4d23949d49ee818e426b056d0a6a6db2.png)

通过运行 MIP 模型生成的甘特图

再次没有包括在内的是谁？**安**！

这个例子演示了如何使用不同技术以各种方式处理和解决问题，但所有方法都产生相同的结果。你自己尝试解决问题时，可能会开发出替代的方法或模型。

尽管在考虑这个特定问题时，***图论*** 似乎是最简单的方法，但由于它是一种特定的方法，因此不能普遍适用于所有问题。***数学规划***（MILP）与 ***约束编程*** 的差异可能是整篇文章或甚至一本书的主题。不过，主要的差异是：

+   ***数学编程*** 模型支持离散和连续决策变量，而 ***约束编程*** 仅支持离散决策变量（整数或布尔值）。

+   ***约束编程*** 支持逻辑约束以及算术表达式，包括模运算、整数除法、最小值、最大值和一个通过决策变量索引值数组的表达式。它还可以利用专门的约束，例如 *全不同* 和 *无重叠* 约束，这可以简化建模，而 ***数学编程*** 仅支持 *≥*、*≤* 和 *=* 约束。

参考文献

[1] B. Claude 和 W. Ian, 《谁杀了丹斯莫尔公爵》（1994），Biblioteque Oulipienne #67

除非另有说明，所有图片均为作者提供
