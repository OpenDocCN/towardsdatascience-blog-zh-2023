# 如何解释逻辑回归系数

> 原文：[`towardsdatascience.com/how-to-interpret-logistic-regression-coefficients-db9381379ab3`](https://towardsdatascience.com/how-to-interpret-logistic-regression-coefficients-db9381379ab3)

## 计算逻辑回归系数的均值边际效应

[](https://medium.com/@jarom.hulet?source=post_page-----db9381379ab3--------------------------------)![Jarom Hulet](https://medium.com/@jarom.hulet?source=post_page-----db9381379ab3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----db9381379ab3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----db9381379ab3--------------------------------) [Jarom Hulet](https://medium.com/@jarom.hulet?source=post_page-----db9381379ab3--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----db9381379ab3--------------------------------) ·阅读时间 10 分钟·2023 年 8 月 24 日

--

![](img/6b8de6ded7e16028cbbbdf6fbf4cd355.png)

图片由 Dominika Roseclay 提供，来源于 Pexels.com

你喜欢逻辑回归，却讨厌解释任何形式的对数变换吗？虽然我不能说你和你在一起的人很多，但我可以说你有*我*作伴！

在这篇文章中，我将全面讨论解释逻辑回归系数——以下是大纲：

1.  解释*线性*回归系数

1.  为什么逻辑回归系数的解释具有挑战性

1.  如何解释逻辑回归系数

1.  使用*statsmodels*包计算均值边际效应

1.  结论

**解释线性回归系数**

大多数对统计学有基础知识的人都能完全理解线性回归中系数的解释。如果你属于这种情况，可以考虑跳过文章中讨论逻辑回归系数的部分。

解释线性回归系数非常简单易懂。解释的简单性是线性回归仍然非常受欢迎的原因之一，尽管出现了许多更复杂的算法。

简单线性回归（一个输入变量的线性回归）呈现如下形式：

![](img/5ae797891819951411430121de31ad61.png)

我们主要关注的是解释***B***₁。对于线性回归，这种解释很简单——对于*x*的一个单位变化，我们预期*y*会发生***B***₁的变化。这个关系的另一种说法是“均值边际效应”。

让我们通过模拟来看看如何解释***B***₁。模拟是测试数据科学工具/方法的绝佳工具，因为我们设定基准真相，然后看看我们的方法是否能够识别它。

在下面的代码中，我们模拟了 30,000 行*x*值。我们通过从正态分布中采样来模拟*x*值，参数由我们选择（在此例中均值为 2，标准差为 0.2）。然后，我们通过将*x*乘以我们模拟的影响 0.16 来模拟*y*，接着添加随机误差；也使用来自正态分布的采样（均值=0，标准差=2）。

```py
from sklearn.linear_model import LinearRegression
import pandas as pd
import numpy as np

# simulate linear regression data
def regression_simulation(sim_var, sim_error, sim_coef, size):

    '''
        Simulates data for simple linear regression.

        inputs:
            sim_var (list)    : 2-element list, first element is the mean of a random variable
                                that is being used to simulate a feature in the linear regression, 
                                second is the standard deviation
            sim_error (list)   : 2-element list, first element is the mean of random error being added,
                                 second is the standard deviation
            sim_coef (float)   : impact of the random variable established by sim_var on the target 
                                 variable
            size (int)         : number of units to simulate

        output:
            sim_df (DataFrame) : dataframe with simulated data

    '''

    # create an empty dataframe to populate
    sim_df = pd.DataFrame()

    # create the feature for the linear regression
    sim_df['var'] = np.random.normal(sim_var[0], sim_var[1], size = size)

    # multiply feature by the coef to get a simulated impact
    sim_df['var_impact'] = sim_df['var']*sim_coef

    # create error for the linear regression
    sim_error = np.random.normal(sim_error[0], sim_error[1], size = size)

    # create the target variable
    sim_df['target'] = sim_df['var_impact'] + sim_error

    return sim_df

linear_regression_sim_df = regression_simulation(sim_var = [2, 0.2], 
                                                 sim_error = [0, 2],
                                                 sim_coef = 0.16,
                                                 size = 30000)

lin_reg = LinearRegression()
X = np.array(linear_regression_sim_df['var']).reshape(-1, 1)
y = linear_regression_sim_df['target']
lin_reg.fit(X, y)
```

这是我们打印线性回归创建的系数时看到的结果：

```py
print(lin_reg.coef_)
```

![](img/033a815a601c30ab308aeaa542397daf.png)

很好！这与 0.16 非常接近。如果我们想确保我们的系数估计是无偏的，我们可以多次模拟数据集并查看分布情况。

```py
# run multiple simulations
iters = 1000
ceof_list = []

for i in range(iters):

    reg_sim_df = regression_simulation([2, 0.2], [0, 0.1], 0.16, 5000)

    lin_reg = LinearRegression()
    X = np.array(reg_sim_df['var']).reshape(-1, 1)
    y = reg_sim_df['target']
    lin_reg.fit(X, y)

    coef = lin_reg.coef_[0]

    ceof_list.append(coef)

plt.hist(ceof_list, bins = 20)
```

从直方图中我们可以看到，分布中心在 0.16 左右，这意味着我们的系数估计是无偏的，这相当酷！

![](img/69ec65948da5508fafdfde61ee13f234.png)

图片由作者提供

这个过程对线性回归来说太简单了，让我们挑战自己，开始研究逻辑回归吧！

**为什么逻辑回归系数的解释是具有挑战性的**

逻辑回归是一个基于线性的模型，像线性回归一样，但它进行了一个变换，使预测值*y*保持在 0 和 1 之间。这使得逻辑回归能够预测目标变量属于某个类别的概率。

这是二元逻辑回归的形式：

![](img/b4d0e87f739f14b42d0977f9cbb1016e.png)

虽然这种变换对于将线性回归适应于预测概率非常有效，但它破坏了我们直接解释系数作为平均边际效应的能力！

让我们模拟一些二元数据来进行演示。在下面的代码中，我们遵循与线性回归模拟相同的过程，除了在模拟了*y*之后，我们使用均匀分布进行采样，使*y*变为 1 或 0。 （注意：我们在这里增加了随机性，因为我们通过正态分布手动添加了误差，然后将*y*转换为二元变量的过程也为模拟添加了一些随机噪声）。

```py
from sklearn.linear_model import LogisticRegression
import pandas as pd
import numpy as np

# simulate binary data
def logistic_regression_simulation(sim_var, sim_error, sim_coef, size):

    '''
        Simulates data for simple logistic regression.

        inputs:
            sim_var (list)    : 2-element list, first element is the mean of a random variable
                                that is being used to simulate a feature in the logistic regression, 
                                second is the standard deviation
            sim_error (list)   : 2-element list, first element is the mean of random error being added,
                                 second is the standard deviation
            sim_coef (float)   : impact of the random variable established by sim_var on the target 
                                 variable
            size (int)         : number of units to simulate

        output:
            sim_df (DataFrame) : dataframe with simulated data

    '''

    # create an empty dataframe to populate
    sim_df = pd.DataFrame()

    # create the feature for the linear regression
    sim_df['var'] = np.random.normal(sim_var[0], sim_var[1], size = size)

    # multiply feature by the coef to get a simulated impact
    sim_df['var_impact'] = sim_df['var']*sim_coef

    # create error term
    sim_df['sim_error'] = np.random.normal(sim_error[0], sim_error[1], size = size)

    # add error and impact together
    sim_df['sum_vars_error'] = sim_df['var_impact'] + sim_df['sim_error']

    # create a uniform random variable used to convert sum_vars_error from continuous to binary
    sim_df['uniform_rv'] = np.random.uniform(size = len(sim_df))

    # create the binary target variable using the uniform random variable
    sim_df['binary_target'] = sim_df.apply(lambda x : 1 if x.sum_vars_error > x.uniform_rv else 0, axis = 1)

    return sim_df

log_reg_sim_df = logistic_regression_simulation([2.00, 0.2], [0, 0.1], 0.16, 30000)
```

现在我们已经模拟了数据，让我们运行逻辑回归，看看我们的系数是什么样的。

```py
log_reg = LogisticRegression()
X = np.array(log_reg_sim_df['var']).reshape(-1, 1)
y = log_reg_sim_df['binary_target']
log_reg.fit(X, y)
```

这是输出结果：

```py
print(log_reg.coef_[0])
```

![](img/8d41ac8f0c87235ba69c52b068704979.png)

什么？？？这感觉一点也不好。那个系数远远不是我们创建的 0.16 的正确答案！

但为了确保，让我们运行一千次并查看分布情况。

![](img/1a9c8c60c1246276d4dffdf32c2fff12.png)

图片由作者提供

看起来我们的第一次运行不是异常值。系数的中心距离我们模拟的影响 0.16 相差很远。当然，这是因为逻辑回归系数不能像线性回归系数那样直接解释。

在下一部分，我们将讨论如何解释逻辑回归系数。

**如何解释逻辑回归系数**

首先，让我们谈谈逻辑回归系数的符号。好消息——符号是可解释的！如果符号为正，则对应类别的概率随着 *x* 的增加而增加——负号则相反。这可能非常有帮助。假设你正在开发一个预测客户是否会购买某个产品的模型。你可以通过观察价格的系数是否为负来检查模型的直觉（这意味着随着价格的上涨，客户购买产品的可能性降低）。虽然实际的数值可能是一些对数变换的复杂术语，但至少你可以理解模型是否具有方向上的意义。

那么，我们如何得到逻辑系数的均值边际效应呢？微积分，我的朋友，微积分。

我们想了解 *y* 如何随 *x* 的变化而变化。导数正是这样做的！让我们对我们的逻辑回归函数相对于 *x* 的偏导数进行计算：

![](img/34b288286f0fe5e02271e4c7bfd79b67.png)

这里一个重要的结论是，*x* 出现在其自身的偏导数中。这意味着 *y* 随着 *x* 的单位变化而变化的程度取决于 *x* 本身的水平。

注意：线性回归的均值边际效应以相同的方式计算。我们只是不会像这样思考，因为相对于 *x* 的偏导数只是常数 ***B***₁。

所以，现在我们有了一种理解 *x* 的单位变化如何改变 *y* 的方法，但 *x* 是方程的一部分。我们如何得到均值边际效应？不幸的是，没有参考数据集我们无法得到它。我们将从参考数据集中插入所有 *x* 的值，并计算我们偏导数的平均输出。这将最终得到我们的均值边际效应！如果我们的参考数据集能代表我们的总体，我们可以说我们的计算应该是对逻辑回归模型真实均值边际效应的无偏估计。

有了这些知识，我们再运行一次模拟，但这次使用我们计算出的偏导数来计算均值边际效应。

```py
from math import exp
import numpy as np
import pandas as pd
from matplotlib import pyplot as plt

iters = 1000
mean_marginal_impacts = []
coef_list = []

# create iters number of simulated datasets
for i in range(iters):

    # create simulated data
    log_reg_sim_df = logistic_regression_simulation([2, 0.2], [0, 0.1], 0.16, 20000)

    # run regression and get coefficient and intercept
    log_reg = LogisticRegression()
    X = np.array(log_reg_sim_df['var']).reshape(-1, 1)
    y = log_reg_sim_df['binary_target']
    log_reg.fit(X, y)

    coef = log_reg.coef_[0][0]
    intercept = log_reg.intercept_[0]

    # run the model outputs through the partial derivatives for each simulated observation
    log_reg_sim_df['contribution'] = log_reg_sim_df['var'].apply(lambda x : coef*exp(intercept + (x*coef))/
                                                                         (((exp(intercept + (x*coef)) + 1))**2))

    # calculate the mean of the derivative values
    temp_mean_marginal_impact = log_reg_sim_df['contribution'].mean()

    # save the original coefficient and marginal impact for 
    # this simulation in a list 
    mean_marginal_impacts.append(temp_mean_marginal_impact)
    coef_list.append(coef)

# show the distribution of simulated mean marginal impacts
plt.hist(mean_marginal_impacts, bins = 20)
```

![](img/9a260cd3ef1f459f6386dfc9c2f10b59.png)

图片由作者提供

太棒了！我们看到了那 elusive 0.16 的值，真是令人松了口气！我们现在知道了如何解释逻辑回归系数以及如何手动计算它们！当然，手动计算这些效应并不很实际，尤其是当我们开始将更多的 *x* 添加到模型中时。幸运的是，Python 的 *statsmodels* 包有一个内置方法来计算均值边际效应。我将在下一节中分享一个示例。

**使用 *statsmodels* 包计算均值边际效应**

我们可以使用 *statsmodels* 的 *Logit* 类的 *margeff* 方法来计算均值边际效应。代码如下：

```py
import numpy as np
import pandas as pd
from matplotlib import pyplot as plt
import statsmodels.api as sm

iters = 1000
sm_marginal_effects = []

for i in range(iters):

    # simulate data
    log_reg_sim_df = logistic_regression_simulation([2, 0.2], [0, 0.1], 0.16, 20000)

    # define target and predictor variables
    X = np.array(log_reg_sim_df['var'])
    y = log_reg_sim_df['binary_target']

    # add constant to formula - statsmodels.Logit doesn't automatically include
    # an intercept like sklearn
    X_with_intercept = sm.add_constant(X)
    log_reg_sm = sm.Logit(y, X_with_intercept)
    result = log_reg_sm.fit(disp=False)

    # calculate marginal effects
    marginal_effects = result.get_margeff(at = 'all', method = 'dydx')

    # save mean marginal effects in a list
    sm_marginal_effects.append(np.mean(marginal_effects.margeff)) 
```

让我们来看看 1000 次模拟的平均边际效应：

![](img/32504c846af7ea0952902a193f605863.png)

很好，它们符合我们的预期！让我们快速查看分布：

![](img/5e31f17a4fe8bcb866743a0baf5b5645.png)

图片来源：作者

太棒了！这看起来非常类似（由于模拟过程中的随机性，它会有些不同）于我们手动计算时的结果。这感觉很好！

**结论**

理解逻辑回归系数的意义可能有点棘手。我们可以通过将参考数据集中的所有*x*值代入逻辑回归方程的偏导数来估计它们的平均边际效应。将这些边际效应的平均值就是平均边际效应。*statsmodels.Logit* 类有一个计算平均边际效应的方法，而我们无需计算任何偏导数。在实际操作中，你应该使用*statsmodels*（或任何其他为你计算的包或软件），但现在你确切知道代码在后台做了什么！

希望你已经对逻辑回归及其如何解释单个预测变量的影响有了更深入的理解。

GitHub 仓库链接：[`github.com/jaromhulet/logistic_regression_interpretation`](https://github.com/jaromhulet/logistic_regression_interpretation)
