# 时间序列转换（及还原）变得简单

> 原文：[`towardsdatascience.com/time-series-transformations-and-reverting-made-easy-f4f768c18f63`](https://towardsdatascience.com/time-series-transformations-and-reverting-made-easy-f4f768c18f63)

## 探索时间序列的转换及如何在 Python 中使用 scalecast 进行还原

[](https://mikekeith52.medium.com/?source=post_page-----f4f768c18f63--------------------------------)![迈克尔·基思](https://mikekeith52.medium.com/?source=post_page-----f4f768c18f63--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f4f768c18f63--------------------------------)![数据科学之道](https://towardsdatascience.com/?source=post_page-----f4f768c18f63--------------------------------) [迈克尔·基思](https://mikekeith52.medium.com/?source=post_page-----f4f768c18f63--------------------------------)

·发布于 [数据科学之道](https://towardsdatascience.com/?source=post_page-----f4f768c18f63--------------------------------) ·5 分钟阅读·2023 年 1 月 26 日

--

![](img/6ce1d41146609f8ca174df859a6cde91.png)

图片来源：作者

在预测时间序列数据时，平稳性是一个重要因素。某些模型——如 ARIMA、Holt-Winters、指数平滑等——专门用于时间序列，并不一定要求数据是平稳的。序列的平稳性指的是其随时间回到均值的倾向。非平稳序列会在数据集中引入趋势，这可能导致过度依赖这些趋势的函数，从而产生虚假的结果（看起来过于理想的结果）。

指数平滑模型平滑最近的趋势以应对非平稳性，而 ARIMA 会在积分因子（标准 ARIMA 顺序中的中间项）设置为大于 0 时对数据进行差分。其他时间序列模型以不同的方式处理平稳性。

然而，像 XGBoost 这样流行的机器学习模型并不考虑序列的平稳性，因为它并未开发来处理时间序列数据的特殊性。这并不一定意味着你不应该用它进行预测，但在这样做之前，处理潜在的非平稳性是一个好主意。这通常意味着应用一种转换——以某种方式操控原始值——以剥离趋势并使其更适合模型进行预测。有些人对进行转换持保留态度，因为这增加了数据准备的步骤，并使还原结果（以便预测产生可用的信息）变得困难。但情况不一定是这样的。

今天，我将概述如何在 Python 中轻松进行时间序列的转换、预测和还原。以下安装是必需的：

```py
pip install --upgrade scalecast
```

# 基本语法

使用 scalecast 包应用转换的语法很简单。首先，我们导入必要的包并提取时间序列数据。我使用的是 [Kaggle](https://www.kaggle.com/rakannimer/air-passengers) 上的航空公司数据集，具有开放数据库许可证。

```py
import pandas as pd
import matplotlib.pyplot as plt
from scalecast.Forecaster import Forecaster
from scalecast.SeriesTransformer import SeriesTransformer

data = pd.read_csv('AirPassengers.csv')
```

然后我们创建一个 Forecaster 对象：

```py
f = Forecaster(
    current_dates = data['Month'],
    y = data['#Passengers'],
    future_dates = 24,
)
```

使用 Forecaster 对象来提供一个 SeriesTransformer 对象：

```py
transformer = SeriesTransformer(f)
```

就这样。现在，许多不同的转换，包括自定义转换，已经可供使用。这些转换包括一阶差分、二阶差分及以上、季节性差分、线性去趋势、多项式去趋势、对数去趋势、缩放、boxcox 转换等等。所有转换都是可堆叠且完全可还原的。例如，如果我想先应用季节性差分，然后去趋势我的系列，我可以这样做：

```py
f = transformer.DiffTransform(12) # 12 periods is one seasonal difference for monthly data
f = transformer.DetrendTransform()

# plot results
f.plot();
```

![](img/85473e41c82b55e1c674b2a2fceb758c.png)

作者提供的图片

然后我会使用 XGBoost 调用一个预测模型：

```py
f.set_estimator('xgboost')
f.add_ar_terms(12)
f.manual_forecast(n_estimators=100,gamma=2)
f.plot();
```

![](img/27f705adb8533953294b6c60fbafcdf4.png)

要撤销已应用的转换，我会按照其转换对应函数的相反顺序调用还原函数：

```py
f = transformer.DetrendRevert()
f = transformer.DiffRevert(12)
f.plot();
```

![](img/d7f3742153c3de24c748b816823e70aa.png)

作者提供的图片

# 自动转换

有这么多转换选项，如果能基于可能最大化预测准确性的自定义推荐，岂不是很好？这也是 scalecast 的一个功能 ([doc](https://scalecast.readthedocs.io/en/latest/Forecaster/Util.html#find-optimal-transformation))：

```py
from scalecast.util import find_optimal_transformation
# default args below
transformer, reverter = find_optimal_transformation(
    f, # Forecaster object to try the transformations on
    estimator=None, # model used to evaluate each transformation, default mlr
    monitor='rmse', # out-of-sample metric to monitor
    test_length = None, # default is the fcst horizon in the Forecaster object
    train_length = None, # default is the max available
    num_test_sets = 1, # number of test sets to iterate through, final transformation based on best avg. metric
    space_between_sets = 1, # space between consectutive train sets
    lags='auto', # uses the length of the inferred seasonality
    try_order = ['detrend','seasonal_adj','boxcox','first_diff','first_seasonal_diff','scale'], # order of transformations to try
    boxcox_lambdas = [-0.5,0,0.5], # box-cox lambas
    detrend_kwargs = [{'loess': True},{'poly_order':1},{'poly_order':2}], # detrender transform kwargs (tries as many detrenders as the length of this list)
    scale_type = ['Scale','MinMax','RobustScale'], # scale transformers to try
    m = 'auto', # the seasonal length to try for the seasonal adjusters, accepts multiple
    model = 'add', # the model to use when seasonally adjusting
    # specific model kwargs also accepted
)
# see what it chose
reverter
```

这个函数按照给定的顺序循环许多可能的转换，并返回它认为能够提供最佳预测机会的转换集合，通过监控一个样本外度量来做出这个判断。从输出中我们可以看到，它选择了 boxcox 转换，随后是一次差分和一次季节性差分。

```py
Reverter(
  reverters = [
    ('DiffRevert', 12),
    ('DiffRevert', 1),
    ('Revert', <function find_optimal_transformation.<locals>.boxcox_re at 0x0000029FA6900EE0>, {'lmbda': -0.5})
  ],
  base_transformer = Transformer(
  transformers = [
    ('Transform', <function find_optimal_transformation.<locals>.boxcox_tr at 0x0000029FA6E579D0>, {'lmbda': -0.5}),
    ('DiffTransform', 1),
    ('DiffTransform', 12)
  ]
)
)
```

返回的对象可以被输入到一个管道中，在那里应用其他自动机器学习方法：

```py
from scalecast.Pipeline import Pipeline
from scalecast import GridGenerator

GridGenerator.get_example_grids()

transformer, reverter = find_optimal_transformation(f)
def forecaster(f):
    f.auto_Xvar_select()
    f.tune_test_forecast(
        ['elasticnet','xgboost'],
        cross_validate=True,
        limit_grid_size = .2,
    )

pipeline = Pipeline(
    steps = [
        ('Transform',transformer),
        ('Forecast',forecaster),
        ('Revert',reverter),
    ],
)

f = pipeline.fit_predict(f)
f.plot()
plt.title('Automated Forecasting with Transformations');
```

![](img/672bc6e8d6e4adf694dd74dc72910715.png)

作者提供的图片

生成的代码易于阅读和维护。还可以进行更多的自定义，或者使用自动方法通常会得到不错的结果。

# 结论

今天，我概述了使用 scalecast 进行 Python 时间序列转换的方法。该方法扩展了自动机器学习技术，转换以及相应的还原函数被放入管道中以简化应用。感谢您的关注！确保在 GitHub 上为 [scalecast](https://github.com/mikekeith52/scalecast) 点赞。这里是本发布中使用的完整 [代码](https://github.com/mikekeith52/scalecast-examples/blob/main/transforming/medium_code.ipynb)。

[](https://github.com/mikekeith52/scalecast?source=post_page-----f4f768c18f63--------------------------------) [## GitHub - mikekeith52/scalecast: 实践者的预测库

### Scalecast 是一个轻量级的时间序列预测程序、封装器和结果容器，专为应用而构建。

[github.com](https://github.com/mikekeith52/scalecast?source=post_page-----f4f768c18f63--------------------------------)
