# 从 Python 调用 R 进行动态预测组合

> 原文：[`towardsdatascience.com/dynamic-forecast-combination-using-r-from-python-afcdf6adf85b`](https://towardsdatascience.com/dynamic-forecast-combination-using-r-from-python-afcdf6adf85b)

## 探索 rpy2 从 Python 调用 R 方法

[](https://vcerq.medium.com/?source=post_page-----afcdf6adf85b--------------------------------)![Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----afcdf6adf85b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----afcdf6adf85b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----afcdf6adf85b--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----afcdf6adf85b--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----afcdf6adf85b--------------------------------) ·6 分钟阅读·2023 年 1 月 25 日

--

![](img/bd1918e4b14c3cdaf4f5ae5c2e4a0479.png)

图片由 [Louis Hansel](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，你将学习如何使用 rpy2 库从 Python 调用 R 方法。

我们将介绍一个与预测相关的示例。我们将定义并运行 R 函数，这些函数组合由基于 Python 的模型生成的预测结果。

# 介绍

即使 Python 是你首选的编程语言，R 有时仍然会有用。

我不想参与 Python 与 R 的辩论。现在我主要使用 Python。但是，许多优秀的方法仅在 R 中可用。重新从头实现这些方法实在是麻烦。

库 rpy2 满足了我们的需求。它允许你在 Python 中运行 R 代码。R 数据结构，如 *matrix* 或 *data.frame*，会被转换为 numpy 或 pandas 对象。将自定义 R 函数集成到你的 Python 工作流程中也很简单。

那么，rpy2 是如何工作的呢？

# 使用 Opera 的示例

我们将重点使用 R 包 opera。你可以使用这个包来组合预测结果。

在深入了解 rpy2 之前，让我们先回顾一下我们要解决的问题。

## 预测集成入门

通过结合多种不同的模型，集成方法提高了预测性能。

最常见的组合方式是使用简单的平均值。集成中的每个模型在最终预测中具有相同的重要性。但，一种更好的组合预测的方法是使用动态权重。因此，每个模型的权重会适应时间序列的变化。

## Opera

![](img/19d089c4d2f1452cda78122027cb6c43.png)

图片由 [Gwen King](https://unsplash.com/@gwenking?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

动态预测组合的方法有很多种。你可以查看[之前的一篇文章以获取不同方法的列表](https://medium.com/towards-data-science/how-to-combine-the-forecasts-of-an-ensemble-11022e5cac25)。

opera 有什么特别之处？

Opera 代表专家聚合的在线预测。一些最好的预测组合方法仅在这个 R 包中可用。它们包含有关预测组合最坏情况的有趣理论属性。这些属性对开发稳健的预测模型非常有价值。

你可以在[这里](https://cran.r-project.org/web/packages/opera/vignettes/opera-vignette.html)找到 opera 如何工作的完整示例。

在本文的其余部分，我们将使用 opera 来组合 Python 模型的预测。

# 案例研究

像在上一篇文章中一样，我们将以能源需求时间序列作为案例研究。

这个示例包括三个步骤：

+   建立集合；

+   创建我们需要运行的 R 函数；

+   使用这些函数进行动态预测组合。

让我们逐步深入每一个步骤。

## 建立集合

首先，我们使用 Python 的 *scikit-learn* 方法建立一个集合。

下面是你可以这样做的方法：

```py
import pandas as pd

from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestRegressor
from sklearn.neighbors import KNeighborsRegressor
from sklearn.linear_model import Lasso, Ridge, ElasticNetCV

from pmdarima.datasets import load_taylor

# src module available here: https://github.com/vcerqueira/blog
from src.tde import time_delay_embedding

series = load_taylor(as_series=True)
series.index = pd.date_range(end=pd.Timestamp(day=27, month=8, year=2000), periods=len(series), freq='30min')
series.name = 'Series'
series.index.name = 'Index'

# train test split
train, test = train_test_split(series, test_size=0.1, shuffle=False)

# ts for supervised learning
train_df = time_delay_embedding(train, n_lags=10, horizon=1).dropna()
test_df = time_delay_embedding(test, n_lags=10, horizon=1).dropna()

# creating the predictors and target variables
X_train, y_train = train_df.drop('Series(t+1)', axis=1), train_df['Series(t+1)']
X_test, y_test = test_df.drop('Series(t+1)', axis=1), test_df['Series(t+1)']

# defining four models composing the ensemble
models = {
    'RF': RandomForestRegressor(),
    'KNN': KNeighborsRegressor(),
    'LASSO': Lasso(),
    'EN': ElasticNetCV(),
    'Ridge': Ridge(),
}

# training and getting predictions
test_forecasts = {}
for k in models:
    models[k].fit(X_train, y_train)
    test_forecasts[k] = models[k].predict(X_test)

# predictions as pandas dataframe
forecasts_df = pd.DataFrame(test_forecasts, index=y_test.index)
```

我们创建了五个模型：一个随机森林，一个 K-最近邻，以及三个线性模型（Ridge、LASSO 和 ElasticNet）。这些模型[以自回归方式训练](https://medium.com/towards-data-science/machine-learning-for-forecasting-transformations-and-feature-extraction-bbbea9de0ac2)。

这是它们预测的一个示例：

![](img/531daa0ec526106731c839937e4ee97e.png)

几个模型的预测。作者提供的图像。

现在，让我们使用 R 的 *opera* 来结合这些预测，使用 rpy2。我们将介绍这个库的两个有用的方面：

+   如何在 Python 中定义和使用 R 函数；

+   如何在这两种语言之间转换数据结构。

## 在 Python 中定义 R 函数

你可以在 Python 多行字符串中定义一个 R 函数：

```py
import rpy2.robjects as ro

# polynomially weighted average
method = 'MLpol'

# defining the R function in a Python multi-line string
ro.r(
    """
    define_mixture_r <-
      function(model) {
        library(opera)

        opera_model <- mixture(model = model, loss.type = 'square')

        return(opera_model)
      }
    """
)

# storing the function in the global environment
define_mixture_func = ro.globalenv['define_mixture_r']

# using the function
opera_model = define_mixture_func(method)
```

包含函数的字符串传递给 *rpy2.robjects* 模块。然后，*globalenv* 方法使其在 Python 中可用。

你可以定义任何你想要的函数。注意，为了使其工作，系统中需要安装 R 及任何所需的 R 包。

关于上面示例中的函数。它用于创建一个 opera 对象（称为 *mixture*）。所需的参数是用于组合预测的方法。我们使用 *MLpol*，这是基于多项式加权平均的。

这里还有一些其他有用的替代方案：

+   EWA: 指数加权平均；

+   OGD: 在线梯度下降；

+   FTRL: 跟随正则化的领导者；

+   Ridge: 在线 Ridge 回归。

## 将数据从 pandas 转换为 R，反之亦然

这是我们需要的另一个函数：

```py
from rpy2.robjects import pandas2ri

ro.r(
    """
    update_mixture_r <-
      function(opera_model, predictions,trues) {
        library(opera)
        for (i in 1:length(trues)) {
            opera_model <- predict(opera_model, newexperts = predictions[i, ], newY = trues[i])
        }
        return(opera_model)
      }
    """
)

update_mixture_func = ro.globalenv['update_mixture_r']
# activating automatic data conversions
pandas2ri.activate()

# using the function above
## predictions is a pandas DataFrame and trues is a pandas Series
## opera_model is a rpy2 object that represents a R data structure
new_opera_model = update_mixture_func(opera_model, predictions, trues)

# deactivating automatic data conversions
pandas2ri.deactivate()
```

函数定义与之前类似。但这个函数除了需要 *opera_model*（我们上面定义的）外，还需要额外的输入。我们需要传递一个 R *data.frame*（预测结果）和一个 *vector*（真实值）作为输入。

你可以使用 [*pandas2ri*](https://pandas.pydata.org/pandas-docs/version/0.22/r_interface.html) 在 Python 和 R 之间转换数据结构。这样，你可以传递一个 pd.DataFrame（预测结果）和一个 pd.Series（真实值）。rpy2 会自动进行转换。函数应用后，rpy2 会将结果转换回 Python 数据结构。

## 汇总

最后，让我们回到我们的案例研究中。

我将上述函数封装在一个名为 Opera 的 Python 类中。 [你可以在我的 Github 上查看其代码](https://github.com/vcerqueira/blog/blob/main/src/ensembles/opera_r.py)。

以下是如何使用它的方法：

```py
# https://github.com/vcerqueira/blog/blob/main/src/ensembles/opera_r.py
from src.ensembles.opera_r import Opera

opera = Opera('MLpol')
opera.compute_weights(forecasts_df, y_test)

ensemble = (opera.weights.values * forecasts_df).sum(axis=1)
```

下面是分配给每个模型的权重分布：

![](img/55b0fd7c3c27e0f7fd203d0ecc662cd4.png)

每个模型在集合中的权重分布。图片由作者提供。

这些权重随着时间变化以适应时间序列的动态：

![](img/e67f99cc52f2140930b5d09d475ec391.png)

随着时间推移，每个模型在集合中的权重。图片由作者提供。

# 关键要点

本文涉及两个主题：

+   在 Python 中使用 rpy2 库运行 R 代码；

+   使用 opera R 包进行动态预测组合。

我们使用 rpy2 在 Python 中定义和运行了多个 R 函数。我们专注于一个名为 opera 的特定包。不过，你可以定义和运行任何你想要的函数。

rpy2 还有很多其他内容。以下是文档链接：

+   [`rpy2.github.io/doc/v3.5.x/html/`](https://rpy2.github.io/doc/v3.5.x/html/)

opera 包对动态预测组合非常有用。其方法高效且提供了宝贵的预测性能理论保障。

感谢阅读，我们下次故事见！

## 相关文章

+   预测组合简介

+   如何组合预测结果

## 进一步阅读

[1] rpy2 文档: [`rpy2.github.io/doc/v3.5.x/html/`](https://rpy2.github.io/doc/v3.5.x/html/)

[2] Opera 文档: [`cran.r-project.org/web/packages/opera/vignettes/opera-vignette.html`](https://cran.r-project.org/web/packages/opera/vignettes/opera-vignette.html)
