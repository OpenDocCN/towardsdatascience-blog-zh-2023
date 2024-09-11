# 逻辑回归：对决与概念理解

> 原文：[https://towardsdatascience.com/logistic-regression-faceoff-67560de4f492?source=collection_archive---------5-----------------------#2023-05-18](https://towardsdatascience.com/logistic-regression-faceoff-67560de4f492?source=collection_archive---------5-----------------------#2023-05-18)

## 逻辑损失和完美分离的数据与冰球棒有什么关系？

[](https://medium.com/@igor-s?source=post_page-----67560de4f492--------------------------------)[![Igor Šegota](../Images/17c592b71fef9526a0679d47937837f6.png)](https://medium.com/@igor-s?source=post_page-----67560de4f492--------------------------------)[](https://towardsdatascience.com/?source=post_page-----67560de4f492--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----67560de4f492--------------------------------) [Igor Šegota](https://medium.com/@igor-s?source=post_page-----67560de4f492--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe5f8ebca4ad8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flogistic-regression-faceoff-67560de4f492&user=Igor+%C5%A0egota&userId=e5f8ebca4ad8&source=post_page-e5f8ebca4ad8----67560de4f492---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----67560de4f492--------------------------------) ·7分钟阅读·2023年5月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F67560de4f492&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flogistic-regression-faceoff-67560de4f492&user=Igor+%C5%A0egota&userId=e5f8ebca4ad8&source=-----67560de4f492---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F67560de4f492&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flogistic-regression-faceoff-67560de4f492&source=-----67560de4f492---------------------bookmark_footer-----------)![](../Images/91a7b0a99da3d1740a217fe617880118.png)

图片由 [Jerry Yu](https://unsplash.com/@jerryyu?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 谁下的这个订单？

截至撰写本文时，谷歌搜索“逻辑回归教程”显示大约有1120万条结果。为什么还要在这堆信息中再添加一份？

阅读了大量文章、书籍和指南后，我意识到大多数缺乏对逻辑回归工作原理的清晰直观解释。相反，它们通常要么致力于展示如何运行模型的实用性，要么尽可能地数学全面，因此基本概念被埋在矩阵代数的森林中。

我们将从澄清看似常见的误解开始。逻辑回归**不是**：

+   线性回归，但用 sigmoid 曲线代替直线

+   分类算法（但可以用于此）

+   sigmoid 曲线“拟合”了x-y平面中分隔两类点的决策边界

# 什么是逻辑回归？

逻辑回归是一种回归模型，返回二元结果（0或1）的概率，假设赔率的对数是一个或多个输入的线性组合。赔率是结果发生的概率（* p*）与结果不发生的概率（*1-p*）之间的比率。当我们只有一个输入或预测变量时，这一初始假设数学上表示为：

![](../Images/685c506d6b3786c9ed389a77a810331b.png)

逻辑回归的目标是建模当输入将结果概率从0逐渐转移到1的情况。结果为1的概率 * p* 可以从先前的方程中推导出来，并表示为输入的函数：

![](../Images/eca13451561745c8a572f7c35592f11c.png)

在最后一部分，我们从参数* β₁* 和* β₀* 转换为 * k* 和 * x₀*。使用 * k* 和 * x₀* 将使我们在继续过程中对模型有更清晰的了解。我们还将坚持使用单一预测变量*x*，而不是引入一大堆矩阵，这样我们可以更容易地可视化逻辑拟合。

# 逻辑曲线

我们将开始绘制逻辑曲线，参数为 * x₀ = 2.5* 和 * k = 3*，在区间 * x* 从0到5：

```py
import numpy as np
import pandas as pd
import plotnine as p9
from scipy.stats import uniform, bernoulli

# From https://github.com/igor-sb/blog/blob/main/posts/logistic/plots.py
from logistic.plots import plot_naive_logistic_fit

def logistic(x, k, x0):
    return 1 / (1 + np.exp(-k*(x - x0)))

def create_smooth_logistic_curve_data(k, x0, n_points=100):
    df = pd.DataFrame({'x': np.linspace(0, 5, n_points)})
    df['p_x'] = logistic(df['x'], k, x0)
    return df

def create_sample_data(k, x0, n_points, seed=1):
    np.random.seed(seed)
    df = pd.DataFrame({
        'x': uniform.rvs(loc=0, scale=5, size=n_points)
    }).sort_values('x', ignore_index=True)
    p_x = logistic(df['x'], k, x0)
    df['y'] = bernoulli.rvs(p_x)
    return df

sample_df = create_sample_data(k=3, x0=2.5, n_points=30)
smooth_px_df = create_smooth_logistic_curve_data(k=3, x0=2.5)

plot_naive_logistic_fit(sample_df, smooth_px_df)
```

![](../Images/d11aca277e68ed608888dac0fb31053d.png)

点（红色表示 y=0，青绿色表示 y=1）和 p(x)（黑色）用于逻辑回归。

这个逻辑曲线*p(x)*由两个参数描述：

+   * x₀*是预测变量*x*的值，此时概率为0.5（中点）：* p(x = x₀) = 0.5*，所以告诉我们中点的位置。

+   * k* 与中点的概率斜率有关：*(dp/dx){x = x₀} = k/4*，因此告诉我们该中点处曲线的陡峭程度。* k* 越大，中间的曲线越陡。

如果我们天真地使用普通最小二乘法来拟合曲线*p(x)*到这些点上，我们会发现所有残差都小于1，并且大多数在中点“错误一侧”的点的残差接近1。将更大的成本分配给那些大的离群点会更有意义。

# 对数损失拟合

与其尝试让普通最小二乘法拟合*p(x)*到点上，逻辑回归的处理方式不同：

+   对于* y = 1*的青绿色点，我们将拟合 * -log p(x)* 而不是 * p(x)*。负对数使 * -log p(x)* 随着 * p(x)* 接近零而逐渐增大。

+   对于* y = 0*的红色点，我们可以通过使用结果为零的概率来进行相同的处理，* -log[1-p(x)]*。

我们称这些为“log-loss”。如果我们将所有点都折叠到*y = 0*，那么对于每个点，这两个log-loss代表该点的成本，因为它们与log-loss曲线有一定的差距。为了利用`numpy`的向量化，我们将这两个一起编码为单个log-loss函数（这种组合log-loss也被称为“交叉熵”）。

```py
def log_loss(p_x, y):
    return -y * np.log(p_x) - (1 - y) * np.log(1 - p_x)
```

逻辑回归的一种理解方式是同时适合于*y = 1*的*-log p(x)*和*y = 0*的*-log[1-p(x)]*。

这两个log-loss曲线看起来如何？

为了可视化它们，我们将在前面的图中绘制相同的数据，但现在使用log-loss替代概率。

```py
def create_smooth_logloss_data(k, x0, n_points=100):
    x = np.linspace(0, 5, n_points)
    p_x = logistic(x, k, x0)
    return pd.DataFrame({
        'x': np.concatenate((x, x)),
        'y': np.concatenate(([0] * len(x), [1] * len(x))),
        'log_loss': np.concatenate((log_loss(p_x, 0), log_loss(p_x, 1))),
    })

def add_logloss(df, k, x0):
    p_x = logistic(df['x'], k, x0)
    return df.assign(log_loss = log_loss(p_x, df['y']))

def fit_data_to_logloss(sample_df, k, x0):
    sample_fit_df = add_logloss(sample_df, k, x0)
    logloss_df = create_smooth_logloss_data(k, x0)
    return (sample_fit_df, logloss_df)
```

```py
from logistic.plots import plot_logistic_fit

sample_fit_df, logloss_df = fit_data_to_logloss(sample_df, k=3, x0=2.5)

plot_logistic_fit(sample_fit_df, logloss_df)
```

![](../Images/c076cb0e9f2619ade776975a758f6cb9.png)

我们将所有点都折叠到*y = 0*，但使用颜色作为*y*标签，因为log-loss值本身代表成本。红色点（*y = 0*）适合于红色曲线：*-log[1-p(x)]*。蓝绿色点（*y = 1*）适合于蓝绿色曲线：*-log p(x)*。垂直虚线的总和表示需要最小化的总log-loss，适用于各种*k*和*x₀*。

与概率不同，log-loss曲线具有按比例惩罚大离群值的特性，并且它们没有在1处截断的残差。

# 寻找最小的log-loss

更改*k*和*x₀*如何影响这种拟合？要回答这个问题，我们可以运行具有各种*k*和*x₀*组合的拟合。

```py
def fit_parameter_combinations(sample_df, combinations):
    sample_df_list = []
    logloss_df_list = []
    for k, x0 in combinations:
        sample_fit_df, logloss_df = fit_data_to_logloss(sample_df, k, x0)
        sample_fit_df['k'] = logloss_df['k'] = k
        sample_fit_df['x0'] = logloss_df['x0'] = x0
        sample_df_list.append(sample_fit_df)
        logloss_df_list.append(logloss_df)

    return (
        pd.concat(sample_df_list, ignore_index=True),
        pd.concat(logloss_df_list, ignore_index=True)
    )
```

更改*x₀*会使交点向侧面移动：

```py
# From https://github.com/igor-sb/blog/blob/main/posts/logistic/plots.py
from logistic.plots import plot_logistic_fit_panel

x0_dfs, x0_logloss_dfs = fit_parameter_combinations(
    sample_df,
    [(3, 1.5), (3, 2.5), (3, 3.5)]
)
plot_logistic_fit_panel(x0_dfs, x0_logloss_dfs, '~x0')
```

![](../Images/679f7ce48de4018c7a378a456b070d2f.png)

如果*x₀*选择离最优点较远，则log-loss会增加，因为越来越多的点适合于hockey stick曲线的上升部分。

更改*k*会影响log-loss曲线的陡度（注意不同的y轴）：

```py
k_dfs, k_logloss_dfs = fit_parameter_combinations(
    sample_df,
    [(0.5, 2.5), (3, 2.5), (7, 2.5)]
)
plot_logistic_fit_panel(k_dfs, k_logloss_dfs, '~k')
```

![](../Images/fbd25b1c48f6adda8fd7b9973317a959.png)

如果*k*太低（0.5），大多数点会对总log-loss增加一些小但显著的贡献。如果*k*太高（7.0），只有“错误方向”的点会对总log-loss有显著贡献。在这种情况下，是在*x₀ = 2.5*中点左侧的两个蓝绿色点。

这带来一个问题：如果中点没有“错误方向”的点，例如数据完全分离时会发生什么？

# 完全分离的数据

结果表明，逻辑模型无法拟合完全分离的数据！😮 我们可以应用我们早前学到的有关拟合log-loss的知识来理解为什么。我们首先创建完全分离的数据（使用*k = 3*和*x₀ = 2.5*）：

```py
perfect_df = create_sample_data(k=3, x0=2.5, n_points=30, seed=112)

perfect_sep_df, perfect_sep_logloss_df = fit_data_to_logloss(
    perfect_df,
    k=3,
    x0=2.5
)

print("Total log-loss: ", perfect_sep_df['log_loss'].sum())

plot_logistic_fit(perfect_sep_df, perfect_sep_logloss_df)

#> Total log-loss:  1.4776889859527733
```

![](../Images/7f62036d309316e52dcff76ef7a35e73.png)

记得改变*k*如何影响这些hockey stick log-loss曲线吗？当我们增加*k*时，总log-loss的主要贡献来自于中点“错误方向”的点。现在，所有点都在中点的“正确方向”。

这意味着我们可以通过持续增加*k*来创建任意好的适合度。这里是我们设置*k = 13*时适合度的样子：

```py
perfect_sep_df, perfect_sep_logloss_df = fit_data_to_logloss(
    perfect_df,
    k=13,
    x0=2.5
)

print("Total log-loss: ", perfect_sep_df['log_loss'].sum())

plot_logistic_fit(perfect_sep_df, perfect_sep_logloss_df)

#> Total log-loss:  0.050774866143719344
```

![](../Images/84fb7ceac71bc3223536de55616f4355.png)

完美分离的数据需要对数损失（log-losses）具有90度角，并且拟合概率在中点处具有无限斜率。因此，没有参数*k*使总对数损失达到最小值。在实际中，数值算法在若干步骤后停止，可能会返回最后一步的*k*值或一个错误。

# 待续

这篇文章涵盖了逻辑回归概念上的主要部分。在下一部分中，我们将探讨参数*k*作为对数赔率比的有些不直观的含义，并展示如何在Python库`statsmodels`和`scikit-learn`中运行和破解逻辑模型。

[逻辑回归：表面无缺陷的陷阱](/logistic-regression-deceptively-flawed-2c3e7f77eac9?source=post_page-----67560de4f492--------------------------------)

### 何时大赔率比和完美分离的数据会给你带来麻烦？

[towardsdatascience.com](/logistic-regression-deceptively-flawed-2c3e7f77eac9?source=post_page-----67560de4f492--------------------------------)
