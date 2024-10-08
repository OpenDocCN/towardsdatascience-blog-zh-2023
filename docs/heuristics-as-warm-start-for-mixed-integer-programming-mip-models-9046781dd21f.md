# 混合整数规划（MIP）模型的启发式方法

> 原文：[`towardsdatascience.com/heuristics-as-warm-start-for-mixed-integer-programming-mip-models-9046781dd21f`](https://towardsdatascience.com/heuristics-as-warm-start-for-mixed-integer-programming-mip-models-9046781dd21f)

## 在 MIP 模型中设置起始解：一个调度应用程序

[](https://medium.com/@bernardovf?source=post_page-----9046781dd21f--------------------------------)![Bernardo Furtado](https://medium.com/@bernardovf?source=post_page-----9046781dd21f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9046781dd21f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9046781dd21f--------------------------------) [Bernardo Furtado](https://medium.com/@bernardovf?source=post_page-----9046781dd21f--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9046781dd21f--------------------------------) ·阅读时间 10 分钟·2023 年 4 月 6 日

--

![](img/dddb71a353bfd16b1c89773175b62f3d.png)

图片由 [Nils Geldner](https://unsplash.com/@n_geldner?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/nZf1_8WPIQk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在计算机科学中，启发式方法是用于寻找给定问题的可行解的技术，通常比精确方法快，但不保证最优性。另一方面，精确方法计算成本高，但可以保证最优解。

将问题建模为混合整数规划（*MIP*）并使用求解器求解可能会给出最终解决方案。通常，这些求解器在后台使用分支定界算法。分支定界（*B&B*）被认为是一种解决优化问题的精确方法。

正如名字所示，它将解枚举为树状结构（分支）。*B&B* 和通常称为蛮力的穷举搜索的主要区别在于界限阶段。在过程中，每个节点（解）与解的下界和上界进行比较。如果分支被证明不能比已找到的最佳解更好，则会被剪枝，算法将转向另一个分支。

本文的目标不是详细介绍分支定界算法；更深入的解释可以在 [这里](https://www.gurobi.com/resources/mixed-integer-programming-mip-a-primer-on-the-basics/) 找到。但我们需要定义几个概念：

+   *现有解*：在算法的每一步找到的最佳可行解。如果这是一个最小化问题，它是上界。

+   *最佳界限*：解的有效下界。

+   *Gap (%)*：最佳上界与现有解之间的差异。如果现有解等于最佳上界，则差距为 0，表明已找到最优解。

先进的求解器有不同的方法来计算任意 *MIP* 的现有解和最佳上界。但对于求解器而言，我们的 *MIP* 只是一个方程组和目标函数。既然我们知道问题的确切结构，那么开发一个专门算法并将结果作为现有解提供会不会更有帮助？是的，这就是启发式方法和热启动发挥作用的地方。

排列流车间调度问题是文献中最经典的优化问题之一。它可以简要描述为：给定一组机器 *m* 和一组作业 *n*，如何处理这些作业，使得每个作业必须按照相同的顺序经过所有机器。目标是最小化最后一个作业的完成时间。

假设有 *3* 台机器（*M1*、*M2* 和 *M3*）和 *3* 个作业（*J1*、*J2* 和 *J3*）。作业必须按照给定的顺序进行。下图显示了一个可能的可行解，形式为甘特图：

![](img/7bcaaf563561dc00b248db36fd137a92.png)

排列流车间的可行解

这个调度的完成时间，或称 Makespan，为 *34*，解决方案/顺序是 *J1*、*J2*、*J3*。

这个问题可以被表述为 *MIP* 模型。我们称 *m* 为机器集合，*n* 为作业集合。作业必须按照相同的机器顺序 *{0,1,…,m}* 进行。作业 *j* 在机器 *i* 上的处理时间由 p_ij 给出。我们来用 Python 创建这些参数。我们将从 *15* 个作业和 *5* 台机器开始。处理时间在 *1* 和 *100* 之间随机生成。*M* 是解的上界。一个好的上界是所有作业在所有机器上的处理时间之和。

```py
import random
import numpy as np

random.seed(10)

# Number of jobs
n = 15
# Number of machines
m = 5
# Max time for random input generator
max_time = 100

# Generate processing times randomly
times = np.zeros((m, n))
tot_time_job = []
for i in range(m):
    for j in range(n):
        times[i][j] = random.randint(1, max_time)
# Total processing time per job - sum across machines
tot_processing_job = np.sum(times, axis=0)

# Upper bound of the solution - sum of transit matrix
M = sum(times[j][i] for i in range(n) for j in range(m))
```

变量有两组。变量 *x_ij* 是连续变量，表示作业 *j* 在机器 *i* 上的开始时间。变量 *y_jk* 是二进制变量，如果作业 *j* 在 *k* 之前执行，则等于 *1*，否则为 *0*。*Cmax* 是调度的 makespan。现在我们用 *gurobipy* 来定义这些。

```py
opt_model = grb.Model(name="Flow shop scheduling")

# Start time of job j at in machine i
x = {(j, i): opt_model.addVar(vtype=grb.GRB.CONTINUOUS, lb=0, name="x_{0}_{1}".format(j, i))
     for j in range(n) for i in range(m)}

# 1 if job j is executed before job k. 0 otherwise
y = {(j, k): opt_model.addVar(vtype=grb.GRB.BINARY, name="y_{0}_{1}".format(j, k))
     for j in range(n) for k in range(n) if j != k}

# Makespan - Completion time of last job in last machine
c = opt_model.addVar(vtype=grb.GRB.CONTINUOUS, lb=0, name="c")
```

约束如下所示：

![](img/15343bbeae0ca682b7b3e88f41d2f3af.png)

***(1)*** 确保作业 *j* 在机器 *i* 上的处理只能在其在机器 *i-1* 上完成后开始。***(2)*** 和 ***(3)*** 是选择约束——如果作业 *j* 在作业 *k* 之后执行，则在给定的机器上应该在其完成后开始。***(4)*** 定义了 makespan，即在机器 *m*（最后一台机器）上最后一个作业的完成时间。

```py
# Job j in machine i can start only when it is finished in machine i-1
c1 = {(j, i): opt_model.addConstr(x[j, i] - x[j, i - 1] >= times[i - 1][j],
                                  name="c1_{0}_{1}".format(j, i))
      for j in range(n) for i in range(1, m)}

# Disjunctive constraints - if job j is after k, it should start after its completion
c2 = {(j, k, i): opt_model.addConstr(x[j, i] - x[k, i] + M * y[j, k] >= times[i][k],
                                     name="c2_{0}_{1}_{2}".format(j, k, i))
      for j in range(n) for k in range(n) for i in range(m) if k != j}

c3 = {(j, k, i): opt_model.addConstr(-x[j, i] + x[k, i] - M * y[j, k] >= times[i][j] - M,
                                     name="c3_{0}_{1}_{2}".format(j, k, i))
      for j in range(n) for k in range(n) for i in range(m) if k != j}

# Makespan is the completion time of last job in last machine
c4 = {j: opt_model.addConstr(c >= x[j, m - 1] + times[m - 1][j],
                             name="c4_{0}".format(j))
      for j in range(n)}
```

目标函数是最小化 makespan：

```py
opt_model.ModelSense = grb.GRB.MINIMIZE
opt_model.setObjective(c)
opt_model.setParam('MIPGap', 0.018)
opt_model.optimize()
```

如果我们将 *MIP* Gap 设置为 *1.8%*，最佳目标是 *832*。分支定界算法的收敛情况见下图：

![](img/62581d485f313261ae8d6a11665c5744.png)

排列流车间示例的收敛曲线

现有解开始时约为 *1,200*（Gurobi 能找到的第一个解），并收敛到 *832*。如果我们能找到一个更好的初始解并将其作为现有解会怎样？

*NEH* 启发式算法是流车间调度问题中最著名的启发式算法之一。它以其作者（[Nawaz, Enscore, 和 Ham](https://www.sciencedirect.com/science/article/abs/pii/0305048383900889)）命名为 *NEH*。它是一种构造性启发式算法，因此，它从一个空解开始，并迭代地构建调度，直到所有任务都被分配。

排列流车间问题的解的表示是一个由 *n* 个元素组成的列表。列表按开始时间排序——列表中的第一个任务是最早执行的任务，而最后一个任务是最晚的任务。第一步是创建一个函数，该函数接收有序列表（一个解）作为输入，并返回与之相关的总工时。

```py
def get_makespan(solution):
    ''' Calculate the makespan of a sequence of integer (jobs).
        - A job can start only after the previous operation of the same job in machine j-1 ends and
            machine is not processing any other job
        - Finish time of a job in a given machine is its start time plus processing time in current machine
    '''
    end_time = np.zeros((m, len(solution) + 1))
    for j in range(1, len(solution) + 1):
        end_time[0][j] = end_time[0][j - 1] + times[0][solution[j - 1]]
    for i in range(1, m):
        for j in range(1, len(solution) + 1):
            end_time[i][j] = max(end_time[i - 1][j], end_time[i][j - 1]) + times[i][solution[j - 1]]
    return end_time
```

*NEH* 从按处理时间递减的顺序对任务进行排序开始（所有机器的处理时间之和）。第一次迭代将处理时间最长的任务添加到调度的开头。对于剩余的任务，它尝试将其插入当前解中的所有可能位置，比较每个位置的（部分）总工时，并存储找到的最佳解。这个过程会重复，直到所有任务都被分配。函数如下：

```py
def neh():
    ''' Heuristic NEH (Nawaz, Enscore & Ham) for flow shop scheduling
        1 - Start from an empty schedule
        2 - Add first the job with highest sum of processing time
        3 - Go through the list of the unassigned jobs, test all in all possible positions in the current solutions
        4 - Assign the best job at the best position (with lowest makespan) at the final solution
        5 - Repeat (3) and (4) until there are no job unassigned
    '''
    initial_solution = np.argsort(-tot_processing_job)
    current_solution = [initial_solution[0]]
    for i in range(1, n):
        best_cmax = 99999999
        for j in range(0, i + 1):
            temp_solution = current_solution[:]
            temp_solution.insert(j, initial_solution[i])
            temp_cmax = get_makespan(temp_solution)[m - 1][len(temp_solution)]
            if best_cmax > temp_cmax:
                best_seq = temp_solution
                best_cmax = temp_cmax
        current_solution = best_seq
    return current_solution, get_makespan(current_solution)[m - 1][n]
```

*NEH* 找到的结果的总工时为 *891*，比 Gurobi 作为第一个现有解找到的 *1,200* 要好得多。

如果我们想从现有解开始，Gurobi 的 *B&B* 方法可以跳过许多解较高于 *891* 的分支。如下面的图表所示：

![](img/881071d8ffa91adf1187133707ec5150.png)

对比 NEH 解的排列流车间示例的收敛曲线

在 Gurobi 中使用热启动相对容易。唯一的挑战是将启发式返回的数据结构转换为 *MIP* 变量的值。对于我们的问题，我们必须将有序列表和总工时值转换为 *y*、*x* 和 *c* 的变量值。

*cmax_heuristic* 和 *c* 变量具有相同的含义，因此唯一的任务是分配变量的初始值。

```py
c.Start = cmax_heuristic
```

将有序列表（*sequence_heuristic*）转换为变量 *y* 也很简单。需要遍历列表，如果任务 *j* 在列表中排在任务 *k* 之前，则 *y_jk = 1*。否则为 *0*。

```py
for j in range(n):
   for k in range(n):
       if j != k:
           y[j, k].Start = 0
for i in range(0, len(sequence_heuristic) - 1):
   for j in range(i + 1, len(sequence_heuristic)):
       j1 = sequence_heuristic[i]
       j2 = sequence_heuristic[j]
       y[j1, j2].Start = 1
```

我们不需要分配变量 *x* 的值。Gurobi 可以从约束条件中推断出它们的值。一旦模型执行，用户将能够看到以下消息：

![](img/ab0cefcc8e354885050eacb1d7a244c8.png)

这意味着解是可行的，其目标是 *891*，并将作为 *MIP* 开始。那如果我们尝试插入一个不可行的 *MIP* 开始呢？例如，如果我们尝试将所有 *i* 和所有 *j* 的 *y_ij = 0* 作为开始。当然，这是不可行的，因为所有任务都必须在调度中。在这种情况下，用户将看到以下消息：

![](img/602ec4cace045ac441a4b9fb7c23a4d1.png)

具有一个能为*MIP*找到良好起点的启发式方法并不是*MIP*启动功能的唯一用途。还有其他几个用途：

+   在实际问题中，用户可以用现实中发生的场景来为模型提供数据。在这种情况下，基准将是模型的起始点。例如，如果一家公司想要为其路线过程创建一个模型，并且有历史数据可用，那么一天运营所采取的路线可以作为*MIP*启动点，前提是它符合数学模型中给定的约束条件。这也是一个很好的练习，用于检查设计的约束在现实中是否得到遵守。

+   正如已解释的，对于求解器而言，我们的模型只是一些方程。如果用户不想花时间开发启发式方法，使用非常简单/天真的方法也可能有帮助。例如，按索引排序的作业列表是一个可行解（*[1,2,3,..,n]*），如果求解器在寻找初始点时遇到困难，这可能会很有用。

+   如果有一个非常好的已知解，并且想要证明它是否最优，或者离最优解有多远。

最后但同样重要的是，不能保证即使*MIP*启动比求解器的初始解更好，也会改善收敛性和/或运行时间。这是因为求解器会走一条与原始不同的路径。我们之前展示的案例就是一个很好的例子。使用*MIP*启动的*MIP*收敛曲线如下所示：

![](img/fb9777c7bbe498eacd4fcff084553835.png)

使用 NEH 作为起点的排列流车间示例的收敛曲线

它确实从一个更好的解开始，但会卡在接近*891*的值附近。

最后，*MIP*启动是一个非常有用的资源，几乎所有商业求解器都有这一功能。但它的价值在很大程度上取决于问题、数据集以及开发一个好的启发式方法的时间。

除非另有说明，所有图片均由作者提供。完整代码可在[GitHub](https://github.com/bernardoor/Permutation-Flow-Shop-Warm-Start)获取。
