# 利用运筹学提升呼叫中心效率

> 原文：[https://towardsdatascience.com/leveraging-operations-research-for-call-center-efficiency-c625f062c34d?source=collection_archive---------5-----------------------#2023-05-25](https://towardsdatascience.com/leveraging-operations-research-for-call-center-efficiency-c625f062c34d?source=collection_archive---------5-----------------------#2023-05-25)

## 利用高效的劳动力优化来推动组织卓越

[](https://saifalikheraj.medium.com/?source=post_page-----c625f062c34d--------------------------------)[![Saif Ali Kheraj](../Images/7b4fbd0444f5945ec7385b82a8276f27.png)](https://saifalikheraj.medium.com/?source=post_page-----c625f062c34d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c625f062c34d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c625f062c34d--------------------------------) [Saif Ali Kheraj](https://saifalikheraj.medium.com/?source=post_page-----c625f062c34d--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9feeca234e74&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleveraging-operations-research-for-call-center-efficiency-c625f062c34d&user=Saif+Ali+Kheraj&userId=9feeca234e74&source=post_page-9feeca234e74----c625f062c34d---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----c625f062c34d--------------------------------) ·11 分钟阅读·2023年5月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc625f062c34d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleveraging-operations-research-for-call-center-efficiency-c625f062c34d&user=Saif+Ali+Kheraj&userId=9feeca234e74&source=-----c625f062c34d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc625f062c34d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleveraging-operations-research-for-call-center-efficiency-c625f062c34d&source=-----c625f062c34d---------------------bookmark_footer-----------)

在当今的数据驱动世界中，组织可以通过利用数据的力量和应用先进的优化模型，高效配置资源、降低成本并提供卓越的客户体验。

当预测分析与处方分析相结合时，它使得管理者能够在公司内做出关键决策。我们将通过利用预测分析的输出 —— 每小时呼叫中心服务代表的需求 —— 来观察这一点，以通过优化来有效分配客户服务代表，最终将导致客户满意度的提高和整体业务费用的降低。

![](../Images/6c6e3804b9868f5b1ce3447df1f133f6.png)

图1：分析金字塔及处方分析的重要性（图由作者提供）

**概述**

呼叫中心公司面临的关键挑战之一是低接听率，这可能导致客户不满，并可能导致客户流失。例如，如果呼叫中心公司的接听率仅为70%，这意味着30%的客户无法联系到公司，这可能会导致挫败感和不满。

在这篇文章中，我们将通过使用带有方程和问题解决的示例场景，解决一个优化问题，以便更好地安排呼叫中心的排班。这将帮助你详细理解一切。我们将在优化过程中施加一些约束，最小化每天和每小时呼叫中心服务代表的短缺。然后你可以更深入地提出一些有趣的软约束，例如几周后的可能休假等，以提高代理的积极性。

![](../Images/09a49e26fec7acbda8352e2e1a8ccdf6.png)

图2：[Freepik](https://www.freepik.com/free-vector/call-center-agent-concept-flat-style_3148705.htm#page=2&query=call%20centre&position=42&from_view=search&track=ais)

# 示例场景：

我们有十名呼叫中心代理必须每天分配到特定的班次。

## **班次和排班类型**

一天中可能有几个班次，中间有休息时间。让我们看看公司管理层如何设计各种可能的班次。

![](../Images/34a62b6c106f316e073941f6c3ef5a05.png)

图3 — 概念模型1：带有预定义休息时间和工作时间的班次矩阵（由作者提供）

所以我们有公司管理层设定的总共16个班次。例如，如果一名呼叫中心代理被分配到班次5，则该代理将基本上从4点到12点工作，中间有一个定义好的休息时间。因此，代理在总共9小时的工作时间中，有一个休息时间。

在这种情况下，我们用变量K定义班次，用变量T定义小时。

设A为班次矩阵，如果班次k覆盖了时间段t，则Akt为1，否则为0。

![](../Images/f637e7ba46721c85a5457c668815a1b4.png)

方程1：班次矩阵分配（由作者提供）

我们希望将一个客户服务代表（CSR）分配到每天的一个班次。这些班次将本质上告诉员工在优化后应何时到岗、离岗以及休息。这本质上是我们模型的第一个组成部分。

## **轮班分配矩阵（决策变量）**

![](../Images/85d39297a4329dfb3f22a875a43ddcb5.png)

图 4— 概念模型 2：轮班分配矩阵 — CSR 是否被分配到特定轮班（由作者提供）

然后是轮班分配矩阵，我们可以用I和J来表示客户服务代表和日期。K已经定义为轮班，我们在前面的部分中定义了总共16个轮班。

这个矩阵简单地指示客户代表 I 是否在特定的轮班 K 被分配到日期 J。这基本上是我们的决策变量，我们感兴趣的是将CSR分配到正确的轮班，以便在每小时最小化短缺。

**决策变量（数学符号）**

![](../Images/3f931556a12a917206dc39620b063ae5.png)

方程 2：轮班矩阵分配（由作者提供）

## 短缺

现在让我们来研究短缺的概念。

![](../Images/56a67e3d9b0e3579086923f77241b389.png)

图 5— 概念模型 3：获取总分配、需求和短缺（由作者提供）

需求已经通过预测模型提供给我们，一旦我们知道每小时分配的客户服务代表总数，我们就可以计算总分配代表数。一旦我们定义了轮班分配矩阵(*图 4 — 概念模型 2*) 和轮班时间表矩阵(*图 3 — 概念模型 1*)，我们就可以快速构建此表格。我们可以简单地对10 x 16(*概念模型 2*)和16 x 24(*概念模型 1*)进行点积，得到10 x 24，其中10是呼叫中心服务代表的总数，24是小时数。因此，通过将每个CSR分配到每小时，我们可以轻松实现上述目标(*图 5 — * *概念模型 3*)。这使我们能够轻松计算每一天和每小时的总分配CSR。为了简便起见，我们假设需求为三，但在现实世界中，这将来自基于历史数据的预测模型。

预测模型将基于不同的因素和变量，如历史呼叫量、客户人口统计、季节性、产品/服务趋势。

一旦我们有了总分配和需求，我们可以轻松计算短缺，这正是我们需要最小化的，并将作为我们的目标函数的基础。这就是我们的第三个概念表的总结。

# **目标函数**

现在我们来定义短缺的概念，因为我们需要最小化它。让 y[j,t] 代表第 j 天的时间段 t 中 CSR 的短缺。目标是最小化所有时间段和日期中的CSR总短缺，如下面的方程 3 所示。

y[j,t] 是一个派生的决策变量，因为它是基于轮班调度优化模型中的其他决策变量和参数计算得出的。

你可以简单地将短缺视为在*概念模型表格 3*（上述）中对所有天数进行红色突出显示的总和。

![](../Images/0bd52f799c852334c9aec3f6b5145db3.png)

方程 3：目标函数以最小化短缺（图由作者提供）

我们将在约束部分进一步探讨短缺的概念。

# 约束条件

## **硬性约束：**

1.  对于每一天和每个时间段，短缺应大于或等于零。短缺是需求和分配的 CSR（供应）之间的差值。如果分配的 CSR 超过了需求，短缺被视为零。我们必须能够表达需求和供应，以便定义短缺。Djt（第 j 天 t 时段所需 CSR 的需求）。让我们将 D[j,t] 定义为第 j 天 t 时段的 CSR 需求。

我们可以简单地使用班次安排矩阵（*图 3 — 概念模型 1 图*）和班次分配矩阵（*图 4 — 概念模型 2 图*）来计算每一天特定小时的供应或总分配 CSR。让我通过计算第 0 小时分配的 CSR 数量来演示。

让我们选择我们的班次矩阵（*图 3 — 概念模型 1*）的第一列（第 0 小时）和班次分配矩阵（*图 4 — 概念模型 2*）的第一行（CSR 1）。

![](../Images/5dfe65fd108d749e5b64e76781ef0eff.png)

图 6— 点积运算以计算第 0 小时分配的总 CSR（作者提供）

这是一个直接的点积运算。我们选择了特定一天（星期一）的第 0 小时作为示例。我们对班次矩阵（第 0 小时的图 3）和班次分配（CSR 1 的图 4）进行点积运算。由于 CSR 1 被分配到班次 # 5，该班次没有第 0 小时作为工作时间，因此结果将为零。我们将计算第 0 小时的班次矩阵与所有 CSR 1–16 的乘积，并将其相加以获得第 0 小时的总分配 CSR。我们可以简单地对每个 CSR（I）的所有班次 K 进行求和，并对所有 I 和 J 进行此操作。这是直观的理解，下面的方程与我描述的内容相对应。

![](../Images/d05ead274b017945fc55ca6161e68721.png)

方程 4：定义短缺—非线性函数（图由作者提供）

为计算短缺，只需取需求和供应之间的差值（需求 — 供应），如果值为负，表示过剩，我们将其视为 0。通过使用 max 函数来实现这一点。然而，这将不再是线性程序。我们希望能够将此方程转换为线性形式，以便使用线性规划来解决。让我们现在为此定义线性方程。

![](../Images/7d07319b5ab83f126c616913eb7e2077.png)

方程 5：定义短缺—线性函数（图由作者提供）

因此，我们已经成功将非线性方程转换为线性方程。

2\. 每天仅一班次约束：每位CSR每天只能被分配到一个班次，以防工作负荷过重。

![](../Images/c175e2abee7f5954b40f8b36c2fcf93b.png)

方程 6：每个人每天应只参加一个班次（图由作者提供）

对于每个CSR I 在每一天 J，人员应该正好选择一个班次，所以这里我们只是对该CSR在第J天的班次K进行求和。直观地，你可以把它看作是在班次分配矩阵中对特定CSR的求和，并确保它小于等于1。换句话说，班次分配矩阵中某一天的某行（CSR）必须小于等于1。

如果人员休假，他应该选择班次0，你可以自己定义。

## **软约束**

1.  最大额外CSR约束通过设定最大限制来控制额外在岗CSRs的数量。这是为了减轻负担并确保积极的积压。我们不会实现这一部分，但让我们讨论一下方程。

![](../Images/f0022aca104162f3ef5dbb332ba9fb02.png)

方程7：在岗额外CSRs的最大数量 — 可选（作者提供的图示）

软约束涉及到额外在岗CSRs的最大数量，这意味着超过满足客户需求所需的最低数量的额外CSRs。我们希望尽量减少额外的CSRs数量，但确保目标函数值，记作W1，至少等于每一天每个时间段的额外CSRs总数。

为了实现这一点，我们引入了一个软约束，规定了额外CSRs的上限。这个约束确保通过从供应中减去对CSRs的需求得到的额外CSRs数量不超过指定的最大值。

为了包括这个软约束，我们需要通过添加加权组件q来稍微修改我们的目标函数。

![](../Images/f3107fd72fd2345189b37eb175fa78e4.png)

方程8：修改后的目标函数 — 可选（作者提供的图示）

目前这超出了范围，但这是为了理解。

# 实施前的最终方程

![](../Images/d8756fc5ddbe3b094942f617d19c7848.png)

最终方程9（作者提供的图示）

# 在Python中的实现（Pyomo包）

让我们选择之前讨论的相同场景：10个CSRs，每天16个班次，24小时，1天。

```py
 shift_matrix_array = np.array(
    # Shift 1
    [[1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
     [0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
     [0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
     [0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
     [0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
     [0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
     [0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0],
     [0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0],
     [0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0],
     [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0],
     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0],
     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0],
     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0],
     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0],
     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0],
     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1],
    ])

num_csrs=10
num_days=1
num_shifts=16
num_times=24
shift_matrix_array.shape
shift_matrix_dict=dict()

for row in range(shift_matrix_array.shape[0]):
    #print(row)
    for column in range(shift_matrix_array.shape[1]):
        shift_matrix_dict[row,column]=shift_matrix_array[row,column]

model = ConcreteModel()

# Define the sets
model.I = Set(initialize=range(num_csrs))
model.J = Set(initialize=range(num_days))
model.K = Set(initialize=range(num_shifts))
model.T = Set(initialize=range(num_times))

model.shift_matrix = Param(model.K, model.T, initialize=lambda model, k, t: shift_matrix_array[k][t])

demand = {(j, t): 3 for j in range(num_days) for t in range(num_times)}

model.shift_demand = Param(model.J, model.T, initialize=demand)

# decision variables
model.x = Var(model.I, model.J, model.K, domain=Binary)
model.y = Var(model.J, model.T, within=NonNegativeReals)

model.demand_constraint = ConstraintList()
for j in model.J:
    for t in model.T:
        model.demand_constraint.add(
            model.y[j, t] >= model.shift_demand[j, t] - sum(model.x[i, j, k] * model.shift_matrix[k,t] for k in model.K for i in model.I)
        )

model.one_shift_per_day = ConstraintList()
for i in model.I:
    for j in model.J:
        model.one_shift_per_day.add(sum(model.x[i,j,k] for k in model.K) <= 1)

model.obj = Objective(
    expr=sum(model.y[j, t] for j in model.J for t in model.T) ,
    sense=minimize
)

solver = SolverFactory('glpk')
solver.solve(model)
```

## 输出：

```py
print("Day | Hour | Assigned CSRs")
for j in model.J:
    for t in model.T:
        assigned_csrs = []
        for i in model.I:
            for k in model.K:
                if model.x[i, j, k].value == 1 and model.shift_matrix[k, t] == 1:
                    assigned_csrs.append(i)
        print(f"{j}   | {t:02}  | {assigned_csrs}")
```

![](../Images/60f0fc88bbd09688f1b845a46d742c1c.png)

图7— 每小时在特定日期分配的CSRs（作者提供的输出）

```py
print("CSR | Assigned Shift")
for i in model.I:
    assigned_shift = []
    for j in model.J:
        for k in model.K:
            if model.x[i, j, k].value == 1:
                assigned_shift.append(k)
                #break
        if assigned_shift is not None:
            break
    print(f"{i}   | {assigned_shift}")
```

![](../Images/e350f6bd4242f69456e023a82e99b6fc.png)

图9— 分配给每个CSR的班次（作者提供的输出）

```py
total_shortage = sum(max(model.shift_demand[j, t] - sum(model.x[i, j, k].value * model.shift_matrix[k, t] for i in range(num_csrs) for k in range(num_shifts)), 0) for j in range(num_days) for t in range(num_times))

print("Total shortage:", total_shortage)
```

![](../Images/2864712d57e441abeb2f39c0f0438342.png)

图10— 优化后的总短缺（作者提供的输出）

如我们所见，尽管仍然缺少4个CSRs，但这是经过优化的结果，我们可能需要招聘额外的CSRs。因此，这也指导了招聘决策过程，并且我们可能节省招聘额外CSRs的成本。

# 结论：

总之，尽管呼叫中心排班的代码看起来很简单，但在实施优化模型之前，必须有一个扎实的概念模型，并对各种因素进行深思熟虑。通过结合软约束，如考虑额外值班客服代表的最大数量以及解决短缺问题，我们可以有效应对呼叫中心排班相关的挑战。

无论是医疗保健人员配置、零售劳动力管理，还是物流规划，数据科学和优化的应用都能帮助解决资源分配和短缺的类似挑战。通过利用这些方法，组织可以优化运营、提高效率，并增强客户满意度。COVID-19大流行突显了跨行业高效资源管理的重要性。服务需求和有效排班的需求比以往任何时候都更加突出。

# 参考文献

[1] 市场研究 — [https://www.reportlinker.com/p087286/World-Call-Centers-Markets.html](https://www.reportlinker.com/p087286/World-Call-Centers-Markets.html)

[2] 一种混合人员排班模型用于员工排班问题 — [https://www.mdpi.com/2227-7390/8/10/1702](https://www.mdpi.com/2227-7390/8/10/1702)

[3] 人员排班: [https://www.coursera.org/learn/operations-research-modeling](https://www.coursera.org/learn/operations-research-modeling)
