# 堆叠时间序列模型以提高准确性

> 原文：[`towardsdatascience.com/stacking-time-series-models-to-improve-accuracy-7977c6667d29`](https://towardsdatascience.com/stacking-time-series-models-to-improve-accuracy-7977c6667d29)

## 从 RNN、ARIMA 和 Prophet 模型中提取信号，以便用 Catboost 进行预测

[](https://mikekeith52.medium.com/?source=post_page-----7977c6667d29--------------------------------)![Michael Keith](https://mikekeith52.medium.com/?source=post_page-----7977c6667d29--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7977c6667d29--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7977c6667d29--------------------------------) [Michael Keith](https://mikekeith52.medium.com/?source=post_page-----7977c6667d29--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7977c6667d29--------------------------------) ·阅读时长 7 分钟·2023 年 2 月 28 日

--

![](img/d4a54a6899fe0dff94fe8914297c3a54.png)

图片由[Robert Sachowski](https://unsplash.com/@rsachowski?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

关于强大的时间序列模型的研究非常丰富。可供选择的选项很多，远远超出了传统的 ARIMA 技术。最近，递归神经网络和 LSTM 模型已成为许多研究人员关注的重点。根据[PePy](https://pepy.tech/project/prophet)上列出的下载数量，Prophet 模型可能是时间序列预测者使用最广泛的模型，因为它的入门门槛较低。对于你的时间序列来说，哪种选项最合适呢？

也许答案是你应该尝试所有这些模型，并结合它们的各种优点。一个常见的技术是堆叠。流行的机器学习库 scikit-learn 提供了一个[StackingRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.StackingRegressor.html)，可以用于时间序列任务。我[之前演示了如何使用它](https://medium.com/towards-data-science/expand-your-time-series-arsenal-with-these-models-10c807d37558)来处理这种情况。

然而，StackingRegressor 有一个限制；它只接受其他 scikit-learn 模型类和 API。因此，像 ARIMA 这样的模型（在 scikit-learn 中不可用）或来自 tensorflow 的深度网络将无法放入堆栈中。这里有一个解决方案。在这篇文章中，我将展示如何使用 [scalecast](https://github.com/mikekeith52/scalecast) 包和一个 Jupyter [notebook](https://scalecast-examples.readthedocs.io/en/latest/misc/stacking/custom_stacking.html) 扩展时间序列的堆叠方法。使用的数据集可以在 GitHub 上 [open access](https://github.com/Mcompetitions/M4-methods/issues/16) 获取。需要以下要求：

```py
pip install --upgrade scalecast
conda install tensorflow
conda install shap
conda install -c conda-forge cmdstanpy
pip install prophet
```

# 数据集概述

数据集是按小时划分的，分为训练集（700 个观测值）和测试集（48 个观测值）。我的 notebook 使用了 H1 系列，但修改为利用 M4 数据集中任何一个小时序列是很直接的。这是如何读取数据并将其存储在 `Forecaster` 对象中的：

```py
import pandas as pd
import numpy as np
from scalecast.Forecaster import Forecaster
from scalecast.util import metrics
import matplotlib.pyplot as plt
import seaborn as sns

def read_data(idx = 'H1', cis = True, metrics = ['smape']):
    info = pd.read_csv(
        'M4-info.csv',
        index_col=0,
        parse_dates=['StartingDate'],
        dayfirst=True,
    )
    train = pd.read_csv(
        f'Hourly-train.csv',
        index_col=0,
    ).loc[idx]
    test = pd.read_csv(
        f'Hourly-test.csv',
        index_col=0,
    ).loc[idx]
    y = train.values
    sd = info.loc[idx,'StartingDate']
    fcst_horizon = info.loc[idx,'Horizon']
    cd = pd.date_range(
        start = sd,
        freq = 'H',
        periods = len(y),
    )
    f = Forecaster(
        y = y, # observed values
        current_dates = cd, # current dates
        future_dates = fcst_horizon, # forecast length
        test_length = fcst_horizon, # test-set length
        cis = cis, # whether to evaluate intervals for each model
        metrics = metrics, # what metrics to evaluate
    )

    return f, test.values

f, test_set = read_data()
f # display the Forecaster object
```

这是该序列的图表：

![](img/f401c9799631034696a0527e7bcc829b.png)

作者提供的图片

# 应用模型

在开始堆叠模型之前，我们需要从它们生成预测。我选择使用简单模型作为 ARIMA、LSTM 和 Prophet 模型的基准。在接下来的部分中，我将解释为什么做出每个选择，但也可以做出其他决策，这些决策同样有趣，甚至更具吸引力。

## 简单基准测试

为了对所有模型进行基准测试，我们可以调用季节性简单估计方法，该方法将给定小时序列中的最后 24 个观测值向前传播。在 scalecast 中，这非常简单。

```py
f.set_estimator('naive')
f.manual_forecast(seasonal=True)
```

## ARIMA

自回归积分滑动平均是一种流行且简单的时间序列技术，它利用序列的滞后值和误差以线性方式预测未来。通过探索性数据分析（您可以在链接的 [notebook](https://scalecast-examples.readthedocs.io/en/latest/misc/stacking/custom_stacking.html) 中看到），我确定序列不是平稳的，并且具有很强的季节性。我最终选择应用了一个季节性 ARIMA 模型，订单为 (5,1,4) x (1,1,1,24)。

```py
f.set_estimator('arima')
f.manual_forecast(
    order = (5,1,4),
    seasonal_order = (1,1,1,24),
    call_me = 'manual_arima',
)
```

## LSTM

如果 ARIMA 属于时间序列模型中的简单方法，[LSTM](https://www.tensorflow.org/api_docs/python/tf/keras/layers/LSTM) 是更先进的方法之一。这是一种深度学习技术，具有许多参数，包括一个注意力机制，可以在序列数据中找到长期和短期的模式，这使其理论上成为时间序列的理想选择。使用 tensorflow 设置这个模型很困难，但在 scalecast 中并不太难（请参见 [这篇文章](https://medium.com/towards-data-science/exploring-the-lstm-neural-network-model-for-time-series-8b7685aa8cf)）。我应用了两个 LSTM 模型：一个使用 Tanh 激活函数，一个使用 ReLu。

```py
f.set_estimator('rnn')
f.manual_forecast(
    lags = 48,
    layers_struct=[
        ('LSTM',{'units':100,'activation':'tanh'}),
        ('LSTM',{'units':100,'activation':'tanh'}),
        ('LSTM',{'units':100,'activation':'tanh'}),
    ],
    optimizer = 'Adam',
    epochs = 15,
    plot_loss = True,
    validation_split=0.2,
    call_me = 'rnn_tanh_activation',
)

f.manual_forecast(
    lags = 48,
    layers_struct=[
        ('LSTM',{'units':100,'activation':'relu'}),
        ('LSTM',{'units':100,'activation':'relu'}),
        ('LSTM',{'units':100,'activation':'relu'}),
    ],
    optimizer = 'Adam',
    epochs = 15,
    plot_loss = True,
    validation_split=0.2,
    call_me = 'rnn_relu_activation',
)
```

## Prophet

尽管 Prophet 模型极受欢迎，但它最近却被[贬低](https://www.microprediction.com/blog/prophet)。有人声称它的准确性令人失望，主要是因为它对趋势的外推过于不现实，并且它没有通过自回归建模考虑局部模式。然而，它确实有一些独特之处。比如，它会自动将节假日效应应用到模型中。它还考虑了几种季节性类型。它以用户需要的最少规格完成这些功能。我喜欢将它作为一个信号使用，即使它不适合生成点预测。

```py
f.set_estimator('prophet')
f.manual_forecast()
```

## 比较结果

现在我们已经为每个模型生成了预测结果，让我们看看它们在验证集上的表现，这些是我们训练集中的最后 48 个观测值（这仍然与之前提到的测试集分开）。

```py
results = f.export(determine_best_by='TestSetSMAPE')
ms = results['model_summaries']
ms[
    [
        'ModelNickname',
        'TestSetLength',
        'TestSetSMAPE',
        'InSampleSMAPE',
    ]
]
```

![](img/2a7e19fb3374c21513b3d8cde09abeee.png)

图片由作者提供

好消息是每个模型都优于朴素方法。ARIMA 模型表现最佳，百分比误差为 4.7%，其次是 Prophet。让我们查看所有预测与验证集的对比图：

```py
f.plot(order_by="TestSetSMAPE",ci=True)
plt.show()
```

![](img/16529891e70fd018f0bfc795c16964f9.png)

图片由作者提供

所有这些模型在这个系列上表现得相当合理，之间没有大的偏差。让我们进行模型叠加！

# 模型叠加

每个叠加模型都需要一个最终估计器，该估计器将筛选其他模型的各种估计值，以创建一组新的预测。我们将用一个叫做[Catboost](https://catboost.ai)的增强树估计器来叠加我们之前探索的结果。Catboost 是一个强大的程序，我们希望它能从每个已经应用的模型中提取最佳信号。

```py
 f.add_signals(
    f.history.keys(), # add signals from all previously evaluated models
)
f.add_ar_terms(48)
f.set_estimator('catboost')
```

上述代码将每个评估模型的预测结果添加到`Forecaster`对象中。它将这些预测称为“信号”。这些信号与存储在同一对象中的其他协变量一样对待。我们还将最后 48 个系列的滞后添加为 Catboost 模型可以用来进行预测的额外回归量。现在我们将调用三个 Catboost 模型：一个使用所有可用的信号和系列滞后，一个仅使用信号，还有一个仅使用滞后。

```py
f.manual_forecast(
    Xvars='all',
    call_me='catboost_all_reg',
    verbose = False,
)
f.manual_forecast(
    Xvars=[x for x in f.get_regressor_names() if x.startswith('AR')], 
    call_me = 'catboost_lags_only',
    verbose = False,
)
f.manual_forecast(
    Xvars=[x for x in f.get_regressor_names() if not x.startswith('AR')], 
    call_me = 'catboost_signals_only',
    verbose = False,
)
```

让我们利用在分析开始时与 Forecaster 对象分开的测试集，比较所有应用模型的结果。这次，我们将关注两个指标：SMAPE 和平均绝对尺度误差（MASE）。这两个指标在实际的 M4 竞赛中被使用。

```py
test_results = pd.DataFrame(index = f.history.keys(),columns = ['smape','mase'])
for k, v in f.history.items():
    test_results.loc[k,['smape','mase']] = [
        metrics.smape(test_set,v['Forecast']),
        metrics.mase(test_set,v['Forecast'],m=24,obs=f.y),
    ]

test_results.sort_values('smape')
```

![](img/c20085add01772635dd715cbd05d4e8d.png)

图片由作者提供

通过结合来自不同类别模型的信号，我们生成了两个优于其他模型的估计器：一个使用所有信号和 48 个系列滞后的 Catboost 模型，以及一个仅使用信号的 Catboost 模型。这两个模型的误差大约为 2.8%。我们可以看到这两个模型与测试集中的实际数据进行对比的图示。

```py
fig, ax = plt.subplots(figsize=(12,6))
f.plot(
    models = ['catboost_all_reg','catboost_signals_only'],
    ci=True,
    ax = ax
)
sns.lineplot(
    x = f.future_dates, 
    y = test_set, 
    ax = ax,
    label = 'held out actuals',
    color = 'darkblue',
    alpha = .75,
)
plt.show()
```

![](img/26cf88caea4e6acf646451780e2bffd3.png)

作者提供的图片

# 哪些信号最为重要？

为了完善分析，我们可以使用 Shapley 分数来确定在这个模型堆叠中哪些信号最为重要。[Shapley 分数](https://christophm.github.io/interpretable-ml-book/shapley.html)被认为是确定输入在给定机器学习模型中的预测能力的最先进方法之一。分数越高，表示这些输入在该模型中越重要。

```py
f.export_feature_importance('catboost_all_reg')
```

![](img/70806022263aa125dea516fb8b33e938.png)

作者提供的图片

上面的截图仅显示了前几个最重要的预测因子，但我们可以从中看出，ARIMA 信号最为重要，其次是系列的第一个滞后项，然后是 Prophet 信号。RNN 模型的得分也高于许多包含的滞后项。如果我们希望将来训练一个更简洁的模型，这可以是一个很好的起点。

# 结论

在这篇文章中，我展示了在时间序列上下文中堆叠模型的威力，以及如何通过使用多样的模型类别提高探索系列的准确性。所有这些都通过 scalecast 包轻松实现。如果你觉得这个分析有趣，请在[GitHub](https://github.com/mikekeith52/scalecast)上给这个包一个星标。感谢你的关注！
