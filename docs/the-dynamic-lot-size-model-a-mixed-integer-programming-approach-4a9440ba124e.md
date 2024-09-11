# 动态批量模型：一种混合整数编程方法

> 原文：[https://towardsdatascience.com/the-dynamic-lot-size-model-a-mixed-integer-programming-approach-4a9440ba124e?source=collection_archive---------8-----------------------#2023-02-01](https://towardsdatascience.com/the-dynamic-lot-size-model-a-mixed-integer-programming-approach-4a9440ba124e?source=collection_archive---------8-----------------------#2023-02-01)

## 经典库存管理优化问题，使用 Python 和 Pyomo 解决

[](https://medium.com/@bruscalia12?source=post_page-----4a9440ba124e--------------------------------)[![布鲁诺·斯卡利亚 C. F. 莱特](../Images/1042cd04be047c0811fef79ecd04e69c.png)](https://medium.com/@bruscalia12?source=post_page-----4a9440ba124e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4a9440ba124e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4a9440ba124e--------------------------------) [布鲁诺·斯卡利亚 C. F. 莱特](https://medium.com/@bruscalia12?source=post_page-----4a9440ba124e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3ce9b7482ef0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-dynamic-lot-size-model-a-mixed-integer-programming-approach-4a9440ba124e&user=Bruno+Scalia+C.+F.+Leite&userId=3ce9b7482ef0&source=post_page-3ce9b7482ef0----4a9440ba124e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4a9440ba124e--------------------------------) · 7 分钟阅读 · 2023年2月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4a9440ba124e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-dynamic-lot-size-model-a-mixed-integer-programming-approach-4a9440ba124e&user=Bruno+Scalia+C.+F.+Leite&userId=3ce9b7482ef0&source=-----4a9440ba124e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4a9440ba124e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-dynamic-lot-size-model-a-mixed-integer-programming-approach-4a9440ba124e&source=-----4a9440ba124e---------------------bookmark_footer-----------)![](../Images/180753602572ed69a63534bd608ac8c3.png)

图片由 [CHUTTERSNAP](https://unsplash.com/ko/@chuttersnap?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

批量定制问题是具有生产批次之间设置的生产计划问题。由于这些设置，通常在每个周期内生产给定产品的成本过高（Suwondo & Yuliando, 2012）。相比之下，较少的设置与更高的持有库存成本相关。因此，为了获得最佳成本，需要平衡这些操作方面。

在本文中，Wagner & Whitin（1958）提出的问题将使用混合整数规划方法进行实现。为此，将使用Python库[*pyomo*](http://www.pyomo.org/)（Bynum等，2021）和[CBC求解器](https://github.com/coin-or/Cbc)。个人而言，我发现这个问题对于介绍离散规划中的库存管理概念非常有用，这在更复杂的问题中随处可见。请注意，在他们的原始工作中，Wagner & Whitin（1958）开发了一种求解该问题的精确算法，这与这里采用的方法有所不同。

你可以在[这里](https://github.com/bruscalia/optimization-demo-files/blob/main/mip/dynamic_lot_size/notebooks/dynamic_lot_size.ipynb)找到包含问题完整实现的笔记本。

如果你对数值优化和/或混合整数规划不熟悉，你可能需要先查看这个关于该主题的介绍。

[](/an-introduction-to-mixed-integer-linear-programming-the-knapsack-problem-1445452a9fe9?source=post_page-----4a9440ba124e--------------------------------) [## 混合整数线性规划简介：背包问题

### 学习如何使用scipy和pyomo在Python中解决优化问题

towardsdatascience.com](/an-introduction-to-mixed-integer-linear-programming-the-knapsack-problem-1445452a9fe9?source=post_page-----4a9440ba124e--------------------------------)

# 问题陈述

假设在给定的时间范围*T*内，你可以预测每个时间点*t*的给定产品的需求*d*。此外，假设你可以预测每个时间点的设置成本*s*和库存持有成本*h*。如果在某个时间点*t*生产了任何数量的产品，则应发生相应的设置成本*s*。此外，在每个周期结束时，应产生持有库存成本，对应于*h*乘以*t*的期末库存。

如果问题是非限制性的，如Wagner & Whitin（1958）所考虑的，可以认为每个需求都应得到满足。此外，假设单位生产成本与时间无关，尽管问题结构的微小变化可能会轻易引入这些成本。你的目标是确定每一时刻生产多少单位的产品，以便最小化设置和持有库存成本。

我将在下一节中详细描述每个建模组件，但可以通过以下方程总结所考虑的完整优化问题。

![](../Images/8727d6deb1bb984be18f52f7da79c238.png)

动态批量模型作为混合整数规划问题。 (图片由作者提供)。

# 建模

首先，让我们导入所使用的 Python 库：

```py
import numpy as np
import pandas as pd
import pyomo.environ as pyo
import matplotlib.pyplot as plt
```

现在，让我们按照原始文章（Wagner & Whitin, 1958）中的示例来实例化数据集。

```py
months = np.arange(12, dtype=int) + 1
setup_cost = np.array([85, 102, 102, 101, 98, 114, 105, 86, 119, 110, 98, 114])
demand = np.array([69, 29, 36, 61, 61, 26, 34, 67, 45, 67, 79, 56])
inventory = np.ones(12)

dataset = pd.DataFrame(
    {"setup_cost": setup_cost, "inventory_cost": inventory, "demand": demand},
    index=months,
)
```

由于我们在这个示例中使用了 Pyomo，我们必须首先实例化 *pyomo* 模型。在这个示例中，我选择使用 *ConcreteModel* 方法，因为我发现它对于简单问题更直接。或者，也可以使用 *AbstractModel*。这两种方法在[这里](https://pyomo.readthedocs.io/en/stable/pyomo_overview/abstract_concrete.html)有更详细的描述。

```py
model = pyo.ConcreteModel()
```

在这个问题中，唯一被考虑的*集合*是计划时间范围 *T*。让我们将其实例化为一个 pyomo 对象。

```py
model.T = pyo.Set(initialize=list(dataset.index))
```

接下来，我们将实例化从问题数据集中导入的模型参数。它们如下：

+   *d*：每个时间 *t* 的需求

+   *s*：每个时间 *t* 的设定成本

+   *h*：每个时间 *t* 的持有库存成本

```py
model.d = pyo.Param(model.T, initialize=dataset.demand)
model.s = pyo.Param(model.T, initialize=dataset.setup_cost)
model.h = pyo.Param(model.T, initialize=dataset.inventory_cost)
```

以及决策变量：

+   *x*：每个时间 *t* 生产的产品数量

+   *y*：二进制变量，标记每个时间 *t* 的设定成本

+   *I*：每个时间 *t* 的最终库存

```py
model.x = pyo.Var(model.T, within=pyo.NonNegativeReals)
model.y = pyo.Var(model.T, within=pyo.Binary)
model.I = pyo.Var(model.T, within=pyo.NonNegativeReals)
```

库存平衡约束表示在某一时期 *t* 的库存等于前一时期末的库存加上生产量减去该时期的需求。我们在这里假设初始库存为零。

![](../Images/e7a4cf4a6f1a54b0f2d61417bdeb512d.png)

库存平衡约束。 (图片由作者提供)。

```py
def inventory_rule(model, t):
    if t == model.T.first():
        return model.I[t] == model.x[t] - model.d[t]
    else:
        return model.I[t] == model.I[model.T.prev(t)] + model.x[t] - model.d[t]

model.inventory_rule = pyo.Constraint(model.T, rule=inventory_rule)
```

现在让我们制定 *big M* 约束以识别具有设定成本的时期。我们的目的是，如果在时间 *t* 生产了任何数量的产品，则 *y* 应为 1，否则为 0。

![](../Images/030ad1874dbcc877a86b657446f09037.png)

大 M 约束。 (图片由作者提供)。

通常，你可能会想选择一个足够大的 *M* 值，以避免从整数决策空间中去除可行解。这样可以在 *分支定界算法* 中产生更好的 *best-bound* 值，而不是选择任意大的值。

在这个问题中，只有在持有库存成本从时间 *t-n* 到 *t* 小于或等于 *t* 时的设定成本时，才有意义在时间 *t-n* 生产一定需求量 *d*。因此，我们可以通过以下几行计算某一时刻 *t* 的“最大智能生产”：

```py
def get_max_antecip(t, dataset=dataset):
    """
    Returns the first instant in which it might be
    intelligent to produce a certain demand
    """

    total_inv = 0
    d = dataset.demand[t]
    s = dataset.setup_cost[t]
    out = t

    for i in range(1, t - dataset.index[0] + 1):
        h = dataset.inventory_cost[t - i]
        if total_inv + h * d <= s:
            total_inv = total_inv + h * d
            out = t - i
        else:
            break

    return out

def get_max_prod(t, dataset=dataset):
    df = dataset.query(f"max_antecip <= {t} & index >= {t}")
    return df.demand.sum()

dataset["max_antecip"] = [get_max_antecip(t, dataset=dataset) for t in dataset.index]
dataset["max_prod"] = [get_max_prod(t, dataset=dataset) for t in dataset.index]
```

现在我们可以为每个时间段定义一个合适的 *M* 值，并实例化 *big M* 约束。

```py
model.M = pyo.Param(model.T, initialize=dataset.max_prod)

def active_prod(model, t):
    return model.x[t] <= model.M[t] * model.y[t]

model.active_prod = pyo.Constraint(model.T, rule=active_prod)
```

最后，目标函数将计划时间范围内所有持有库存和设定成本相加。

![](../Images/8637397fe41ec00c915e6c8d32f25b2d.png)

目标函数。 (图片由作者提供)。

```py
def total_cost(model):
    holding = sum(model.h[t] * model.I[t] for t in model.T)
    setup = sum(model.s[t] * model.y[t] for t in model.T)
    return setup + holding

model.obj = pyo.Objective(rule=total_cost, sense=pyo.minimize)
```

为了解决模型，我使用了开源求解器 CBC。你可以从 [AMPL](https://ampl.com/products/solvers/open-source-solvers/#cbc) 或 [这个链接](https://www.coin-or.org/download/binary/Cbc/) 下载 CBC 二进制文件。你还可以在 [这里](https://github.com/coin-or/Cbc) 找到安装教程。由于 CBC 可执行文件已包含在我的系统 PATH 变量中，我可以在不指定可执行文件路径的情况下实例化求解器。如果你的系统没有包含，可通过关键字参数“executable”解析到你的可执行文件路径。

```py
# executable=YOUR_PATH
solver = pyo.SolverFactory("cbc")
```

另外，也可以使用 GLPK 来解决这个问题（或任何其他与*pyomo*兼容的求解器）。最新的 GLPK 版本可以在 [这里](http://ftp.gnu.org/gnu/glpk/) 找到，Windows 可执行文件可以在 [这里](https://sourceforge.net/projects/winglpk/) 找到。

```py
solver.solve(model, tee=True)
```

完成了！结果已存储在我们的模型中，并将在下一节中详细分析。

# 结果

首先，让我们打印简单场景的成本，其中每个需求都应该在其对应的瞬时生产。

```py
# Obtain the maximum cost for comparison
max_cost = dataset.setup_cost.sum()
print(f"Maximum cost: {max_cost:.1f}")
```

这应该返回 1234.0。

接下来，让我们从目标函数中获取新的成本并比较结果：

```py
opt_value = model.obj()
print(f"Best cost {opt_value}")
print(f"% savings {100 * (1 - opt_value / max_cost) :.2f}")
```

哇！新的成本是 864.0，节省了近 30%！

让我们在数据集中添加新列，并绘制生产与需求的关系，以便更好地可视化结果：

```py
# Include production as a column
dataset["production"] = [model.x[t].value for t in dataset.index]

# Plot figure
fig, ax = plt.subplots(figsize=[6, 3], dpi=100)
x = dataset.index
width = 0.35
ax.bar(x - width/2, dataset.production, width, color="darkgreen", label="production")
ax.bar(x + width/2, dataset.demand, width, color="navy", label="demand")
ax.set_xticks(x)
ax.set_ylabel("Qtd")
ax.set_xlabel("t")
ax.legend()
fig.tight_layout()
plt.show()
```

![](../Images/f0c7b2c6d90e6b5a2f35c81f75a2e86e.png)

计划期结果。（图像由作者提供）。

可以注意到，除了瞬时 10（其只有一个需求）之外，其他需求被组合在同一生产瞬时，以显著减少整体设置成本。

# 结论

在这篇文章中，动态批量模型（Wagner & Whitin, 1958）被形式化并解决为一个混合整数规划问题。与简单的解决方案相比，优化结果可以将总体成本减少30%。在这种情况下，库存持有和设置成本得到了合理平衡。完整的解决方案可以在这个 [git 仓库](https://github.com/bruscalia/optimization-demo-files/blob/main/mip/dynamic_lot_size/notebooks/dynamic_lot_size.ipynb) 中找到。

# 进一步阅读

对整数规划有更多兴趣的人可以参考 Wolsey (2020)；有关运筹学的内容，可以参考 Winston & Goldberg (2004)。

分支限界算法是解决整数和混合整数问题中最常用的算法。有兴趣了解其机制的读者可以参考我之前的 [Medium 文章](/a-gentle-introduction-to-branch-bound-d00a4ee1cad)。

# 参考文献

Bynum, M. L. 等，2021。*Pyomo-optimization modeling in python.* Springer。

Suwondo, E., & Yuliando, H. (2012). 动态批量问题：关于模型和高效算法的综述。*农业工业期刊*, *1*(1), 36。

Wagner, H. M., & Whitin, T. M. (1958). 动态版本的经济批量模型。*管理科学*, *5*(1), 89–96。

Winston, W. L. & Goldberg, J. B., 2004\. *运筹学：应用与算法.* 第4版. 加州贝尔蒙特：汤姆森·布鲁克斯/科尔贝尔蒙特。

Wolsey, L. A., 2020\. *整数规划.* 第2版. 约翰·威利与儿子公司。
