# 修复 Prophet 的预测问题

> 原文：[`towardsdatascience.com/fixing-prophets-forecasting-issue-b473afe2cc70`](https://towardsdatascience.com/fixing-prophets-forecasting-issue-b473afe2cc70)

## 第一步：限制疯狂的趋势

[](https://medium.com/@tylerblume?source=post_page-----b473afe2cc70--------------------------------)![Tyler Blume](https://medium.com/@tylerblume?source=post_page-----b473afe2cc70--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b473afe2cc70--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b473afe2cc70--------------------------------) [Tyler Blume](https://medium.com/@tylerblume?source=post_page-----b473afe2cc70--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----b473afe2cc70--------------------------------) ·10 分钟阅读·2023 年 1 月 24 日

--

![](img/8d63f95cc7d070338a256c75364f8771.png)

图片由[Hunter Haley](https://unsplash.com/@hnhmarketing?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/s/photos/nails-in-wood?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

目前，Prophet 预测准确性问题已不是秘密。它在多个基准和预测竞赛中一再交付糟糕的结果。然而，它仍然是最常用的预测算法之一……

所以…是时候通过一些小的调整来解决困扰它的问题，并（希望）提高它的预测准确性了。

[TSUtilities 项目](https://github.com/tblume1992/TSUtilities)

# 预言者的趋势问题

奇怪的是，Prophet 的一个主要吸引力也是其核心弱点之一。它确实提供了一个引人注目的趋势，包含了变更点和线性段，分析起来非常易于消化。**但是**，有时，这种拟合趋势的方式既过度拟合又不足拟合——在序列末尾的一个简单水平偏移可能会让趋势在预测范围内无限延伸，同时在残差中留下**大量**有用信号。

这篇文章旨在解决第一个问题：**不受限制的疯狂趋势**。

为了说明这个问题，我们将使用 M4 数据集。这些数据集都是开源的，可以在 M-competitions 的[github](https://github.com/Mcompetitions/M4-methods/tree/master/Dataset)上找到。

首先，让我们看一下每周数据集，特别是这个数据集中第 52 个时间序列。

![](img/a9f2ce4df4b9434d5ca6c30d5a1bae75.png)

图片由作者提供

![](img/69a721150d02aee8d64ae6703906464a.png)

图片由作者提供

如果你之前使用过 Prophet，这些图表应该很熟悉。

在这里，我们对 100 的时间范围进行预测——远远超出了 M4 竞赛中对该系列的处理。但这展示了 Prophet 可能会如何失控，趋势从 7000 骤降至约 5500。这意味着时间序列在几年的过程中完全崩溃。从视觉上来看，我明白模型捕捉到的内容，并且在预测范围的近期，遵循这一点可能是***可以的***。然而，从长期来看，这种趋势可能会导致灾难性的结果。这个想法很重要，所以我会用**粗体**文字重新写一下：

**短期内跟随趋势，长期内控制趋势。**

在我们进入详细内容之前，让我们澄清一些术语。我见过‘dampen’、‘dampened’、‘dampening’和‘damped’这些词在不同来源和时代中交替使用。我知道有一个‘正确的’词，我曾经谷歌过，但我会根据自己的感觉使用它们。只要知道它们在本文中意义相同，如果你有偏好，务必告诉我！

现在开始修复问题。

# 修复趋势

通常，在时间序列背景下，‘damped’趋势出现时讨论的是双指数平滑。对我们来说，我们将采用更简单的方法，更符合‘经验法则’，而不是严格的统计方法。比如——指数衰减。

使用指数衰减，我们将从原始趋势非常接近开始，但在进入时间范围的过程中会逐渐远离它。同时，我们将接近一个‘目标’，该目标将作为一个连贯的参数提供。

让我们看一个简单的例子，首先是一个漂亮的曲线：

```py
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np

sns.set_style('darkgrid')
y = np.linspace(0, 100, 100)
plt.plot(y)
plt.show()
```

![](img/2b9bbf2fedb5df094ca972de2b7dbc34.png)

作者提供的图片

真是漂亮的一条线！

接下来，我们将其分解为在处理时间序列时预期的不同部分。这里我们假设趋势等于实际信号，没有噪音或季节性。

```py
y_train = y[:80]
future_y = y[80:]
future_trend = future_y
```

现在我们将使用‘future_trend’来抑制这个信号。为此，我们将安装一个名为‘TSUtilities’的包，它是我在其他包[ThymeBoost](https://github.com/tblume1992/ThymeBoost)、[LazyProphet](https://github.com/tblume1992/LazyProphet)和[TimeMurmur](https://github.com/tblume1992/TimeMurmur)中使用的各种工具的集合，我希望将其集中管理。

```py
pip install TSUtilities
```

仍在整理和结构化，但 TrendDampen 在这里，并可以像这样导入：

```py
from TSUtilities.TSTrend.trend_dampen import TrendDampen
```

创建类时传递的两个参数是：

**damp_factor**：一个介于 0 和 1 之间的浮点数，其中 0 表示‘完全抑制’，1 表示‘不抑制’。

**damp_style**：如何‘抑制’趋势，通过平滑来‘平滑’地抑制趋势，使用指数衰减。

```py
dampener = TrendDampen(damp_factor=.7,
                       damp_style='smooth')
dampened_trend = dampener.dampen(future_trend)
```

![](img/b7ee05865104b24c8459af1c81cc5dd6.png)

作者提供的图片

在这里我们使用 0.7 的 damp_factor，这意味着新趋势实现了旧趋势大约 70%的效果。我们在这里唯一需要传递给`dampen`方法的是预测的趋势组件。

正如前面提到的，我们从接近原始趋势开始，但会越来越远。

现在这对我们帮助不大，因为我们传入了一个硬编码的 `damp_factor` 值，但我们可以利用这个方法中内置的一些进一步逻辑。如果我们传入拟合的组件：

1.  **训练实际值**

1.  **拟合的趋势成分**

1.  **拟合的季节性成分**

（注：所有这些都是由 Prophet 提供的）

并将 `damp_factor` 改为‘auto’——方法将为你选择一个合适的参数。它通过评估拟合趋势的强度来实现这一点——这个趋势越强，我们就越愿意在长远中信任它。

**注：这里的强度是通过 Wang, Smith, & Hyndman¹定义的趋势强度度量来确定的。**

我们使用的这个‘经验法则’将根据算法的不同而有所不同。如果我们将逻辑应用于 ETS 方法，那么按照这些定义，趋势将会异常强烈，我们会过度信任它。但对于像 Prophet 这样的方法，或任何其他有某种确定性趋势的方法，它在实践中效果良好。

所以让我们看看这个方法如何处理我们简单的线。首先，我们需要一些拟合趋势和季节性的度量。

```py
trend = y_train
seasonality = np.zeros(len(y_train))
```

那么我们将把这些值传递给 `dampen` 方法。

```py
dampener = TrendDampen(damp_factor='auto',
                       damp_style='smooth')
dampened_trend = dampener.dampen(future_trend,
                                 y=y_train,
                                 trend_component=trend,
                                 seasonality_component=seasonality)
plt.plot(future_trend, color='black', alpha=.5,label='Actual Trend', linestyle='dotted')
plt.plot(dampened_trend, label='Damped Trend', alpha=.7)
plt.legend()
plt.show()
```

![](img/b94ff07b610314488ef8d9476b3e88c3.png)

图片由作者提供

正如你所见，‘auto’决定完全不抑制趋势！

这是一个很好的检查——虽然我相信你可以很容易地打破这个逻辑。

让我们看看这个序列的各种 damp_factors：

```py
for damp_factor in [.1, .3, .5, .7, .9, 'auto']:
    dampener = TrendDampen(damp_factor=damp_factor,
                           damp_style='smooth')
    dampened_trend = dampener.dampen(future_trend,
                                     y=y_train,
                                     trend_component=trend,
                                     seasonality_component=seasonality)
    plt.plot(dampened_trend, label=damp_factor, alpha=.7)

plt.plot(future_trend, color='black', alpha=.5,label='Actual Trend', linestyle='dotted')
plt.legend()
plt.show()
```

![](img/12cf47d91aff2a613878aa28247bfcc1.png)

图片由作者提供

很酷！

再次，我们有一个很好的、易于理解的参数，并且有一些‘auto’逻辑看起来还不错。

**现在进入主要内容：修复 Prophet 的趋势。**

在这个示例中，我使用了 M4 的每周数据集中第 52 个时间序列，并将预测 100 期。这个预测期虽然较长，但能够很好地展示问题。此外，我将使用一个函数，该函数接收 Prophet 的输出并抑制趋势，以返回这些修正后的预测。这个函数看起来像这样：

```py
def dampen_prophet(y, fit_df, forecast_df):
    """
    A function that takes in the forecasted dataframe output of Prophet and
    constrains the trend based on it's percieved strength'

    Parameters
    ----------
    y : pd.Series
        The single time series of actuals that are fitted with Prophet.
    fit_df : pd.DataFrame
        The Fitted DataFrame from Prophet.
    forecast_df : pd.DataFrame
        The future forecast dataframe from prophet which includes the predicted trend.

    Returns
    -------
    forecasts_damped : np.array
        The damped trend forecast.

    """
    predictions = forecast_df.tail(len(forecast_df) - len(fit_df))
    predicted_trend = predictions['trend'].values
    trend_component = fit_df['trend'].values
    if 'multiplicative_terms' in forecast_df.columns:
        seasonality_component = fit_df['trend'].values * \
                                fit_df['multiplicative_terms'].values
        dampener = TrendDampen(damp_factor='auto',
                                damp_style='smooth')
        dampened_trend = dampener.dampen(predicted_trend,
                                         y=y,
                                         trend_component=trend_component,
                                         seasonality_component=seasonality_component)
        forecasts_damped = predictions['additive_terms'].values + \
                           dampened_trend + \
                           (dampened_trend * \
                           predictions['multiplicative_terms'].values)
    else:
        seasonality_component = fit_df['additive_terms'].values
        dampener = TrendDampen(damp_factor='auto',
                                damp_style='smooth')
        dampened_trend = dampener.dampen(predicted_trend,
                                         y=y,
                                         trend_component=trend_component,
                                         seasonality_component=seasonality_component)
        forecasts_damped = predictions['additive_terms'].values + dampened_trend
    return forecasts_damped
```

但这个函数也在 TSUtilities 中，我们可以直接使用以下方法导入：

```py
from TSUtilities.functions import dampen_prophet
```

现在让我们看看正常的 Prophet 输出和抑制后的输出有什么不同：

![](img/97e593b65e997f434e5c3a9d95075100.png)

图片由作者提供

我们可以看到，Prophet 在预测范围内的最近趋势变化点远低于任何先前的值，而抑制后的趋势则显得更加保守。

从趋势的角度来看，我们可以看到究竟发生了什么，以及方法改变了什么。

![](img/06deba63f1971897915056ed096f9c36.png)

图片由作者提供

注意，我们的方法并没有改变拟合的值。我们仅仅是旨在收敛未来的趋势成分。

最后，我们看到两种方法的 SMAPE——使用衰减趋势的准确性有了相当显著的提高：

![](img/e31d1068d628faea8735b90fe1154bcb.png)

图片由作者提供

# 在 M4 上的基准测试

到目前为止，我们只看了一个单一的例子，但现在让我们看看 M4 的两个数据集——**每周**和**每日**数据集。提醒一下，这些数据集都是开源的，并且作为*还可以*的基准，适合在时间序列领域进行尝试。

让我们继续导入每周的数据集：

```py
import matplotlib.pyplot as plt
import numpy as np
from tqdm import tqdm
import pandas as pd
from prophet import Prophet
import seaborn as sns
sns.set_style('darkgrid')

train_df = pd.read_csv(r'm4-weekly-train.csv')
test_df = pd.read_csv(r'm4-weekly-test.csv')
train_df.index = train_df['V1']
train_df = train_df.drop('V1', axis = 1)
test_df.index = test_df['V1']
test_df = test_df.drop('V1', axis = 1)
```

接下来，让我们定义用来评估预测的 SMAPE 函数：

```py
def smape(A, F):
    return 100/len(A) * np.sum(2 * np.abs(F - A) / (np.abs(A) + np.abs(F)))
```

现在我们已经定义了数据和度量指标，让我们遍历数据集，使用 Prophet 生成预测——一个标准预测和一个带有衰减趋势的预测。

```py
seasonality = 52
no_damp_smapes = []
damp_smapes = []
naive_smape = []
j = tqdm(range(len(train_df)))
for row in j:
    y = train_df.iloc[row, :].dropna()
    y = y.iloc[-(3*seasonality):]
    y_test = test_df.iloc[row, :].dropna()
    #create a random datetime index to pass to Prophet
    ds = pd.date_range(start='01-01-2000',
                       periods=len(y) + len(y_test),
                       freq='W')
    ts = y.to_frame()
    ts.columns = ['y']
    ts['ds'] = ds[:len(y)]
    j.set_description(f'{np.mean(no_damp_smapes)}, {np.mean(damp_smapes)}')
    prophet = Prophet()
    prophet.fit(ts)
    fitted = prophet.predict()

    # create a future data frame
    future = prophet.make_future_dataframe(freq='W',periods=len(y_test))
    forecast = prophet.predict(future)

    #get predictions and required data inputs for auto-damping
    predictions = forecast.tail(len(y_test))
    predicted_trend = predictions['trend'].values
    trend_component = fitted['trend'].values
    seasonality_component = fitted['additive_terms'].values
    forecasts_no_dampen = predictions['yhat'].values
    forecasts_damped = dampen_prophet(y=y.values,
                                      fit_df=fitted,
                                      forecast_df=forecast)

    #append smape for each method
    no_damp_smapes.append(smape(y_test.values, forecasts_no_dampen))
    damp_smapes.append(smape(y_test.values, forecasts_damped))
    naive_smape.append(smape(y_test.values, np.tile(y.iloc[-1], len(y_test))))
print(f'Standard Prophet {np.mean(no_damp_smapes)}')
print(f'Damped Prophet {np.mean(damp_smapes)}')
print(f'Naive {np.mean(naive_smape)}')
```

结果如何？

![](img/d8a8a76d123d7f0e3b888a7294040bb9.png)

图片由作者提供

衰减方法有效！但 SMAPE 仅减少了约 2.5%。虽然不足以大书特书，但还是一个不错的提升。值得记住的是，我们并没有真正失去 Prophet，只是改变了它的趋势。如果 Prophet 是你预测过程中的一个重要部分，那么这可以看作是锦上添花。

不过，对于每日数据集，结果确实有所改善。重新运行过程得到：

![](img/7f5c01664f134472bf328cad5a1fa45a.png)

图片由作者提供

*瞧*，SMAPE 减少了 7.8%！

与其他在 M4 上进行基准测试的模型比较这些结果时，我们发现***即使有***这些改进，Prophet 仍然无论如何都产生较差的结果。我想一个理由是我们没有调整超参数。不管怎样——我不会使用 Prophet。

我不会在集成模型中使用 Prophet。

我不会使用 Prophet 进行多变量预测。

我绝对不会输入：*import Prophet*

但这一系列的重点是尝试*增强*Prophet。下次——如果有下次的话——我们会有另一个技巧来实现这个目标。

# 结论

我们展示了基于经验法则的趋势衰减过程在提高 Prophet 预测准确性方面的有效性。该过程非常灵活，可以与任何输出一起使用（尽管 dampen_prophet 函数本身不灵活）。我们在 M4 系列的两个数据集上观察到 SMAPE 减少了约 2.4%和约 7.8%。

如果你喜欢这篇文章，你可能会喜欢我其他的一些文章！

[](/lazyprophet-time-series-forecasting-with-lightgbm-3745bafe5ce5?source=post_page-----b473afe2cc70--------------------------------) ## LazyProphet: Time Series Forecasting with LightGBM

### 一切都与特征有关

[towardsdatascience.com [](/thymeboost-a0529353bf34?source=post_page-----b473afe2cc70--------------------------------) ## Time Series Forecasting with ThymeBoost

### 一种梯度提升时间序列分解方法

towardsdatascience.com [](/gradient-boosted-arima-for-time-series-forecasting-e093f80772f6?source=post_page-----b473afe2cc70--------------------------------) ## 梯度增强 ARIMA 用于时间序列预测

### 提升 PmdArima 的 Auto-Arima 性能

towardsdatascience.com

## ****剧透警告****

本系列的终点将完全摆脱 Prophet —— 思路类似于 Nixtla 使用的 [StatForecast 包](https://github.com/Nixtla/statsforecast/tree/main/experiments/arima_prophet_adapter)。与 Nixtla 不同的是，我们将保留相同的时间序列分解和易于理解的线性变点趋势。这将通过 [ThymeBoost](https://github.com/tblume1992/ThymeBoost) 实现。

当然，您也可以通过我的推荐链接注册 Medium：

[](https://medium.com/@tylerblume/membership?source=post_page-----b473afe2cc70--------------------------------) [## 通过我的推荐链接加入 Medium - Tyler Blume

### 阅读 Tyler Blume 的每一个故事（以及 Medium 上成千上万其他作家的故事）。您的会员费将直接支持……

medium.com](https://medium.com/@tylerblume/membership?source=post_page-----b473afe2cc70--------------------------------)

**参考文献**

1.  Wang, X., Smith, K. A., & Hyndman, R. J. (2006). 基于特征的时间序列数据聚类。*数据挖掘与知识发现*, *13*(3), 335–364.
