# 任何时间序列模型的动态符合区间

> 原文：[`towardsdatascience.com/dynamic-conformal-intervals-for-any-time-series-model-d1638aa48527`](https://towardsdatascience.com/dynamic-conformal-intervals-for-any-time-series-model-d1638aa48527)

## 使用回测应用和动态扩展一个区间

[](https://mikekeith52.medium.com/?source=post_page-----d1638aa48527--------------------------------)![Michael Keith](https://mikekeith52.medium.com/?source=post_page-----d1638aa48527--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d1638aa48527--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d1638aa48527--------------------------------) [Michael Keith](https://mikekeith52.medium.com/?source=post_page-----d1638aa48527--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d1638aa48527--------------------------------) ·8 分钟阅读·2023 年 4 月 17 日

--

![](img/c0838f13c2dd273cf20f2025a6148207.png)

照片由 [Léonard Cotte](https://unsplash.com/@ettocl?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

根据生成预测的目的，评估准确的置信区间可能是一个关键任务。大多数经典计量经济模型基于对预测和残差分布的假设，内置了这样的机制。当转向机器学习进行时间序列分析时，例如使用 XGBoost 或递归神经网络，情况可能会更复杂。一种流行的技术是符合区间——一种量化不确定性的无假设预测分布的方法。

# 简单的符合区间

这种方法的最简单实现是训练一个模型并保留一个测试集。如果这个测试集至少有 20 个观察值（假设我们希望 95%的确定性），我们可以通过在任何点预测上添加一个正负值来构建一个区间，该正负值代表测试集残差绝对值的第 95 百分位数。然后，我们在整个序列上重新拟合模型，并将这个正负值应用于未知范围内的所有点预测。这可以被看作是一个简单的符合区间。

[Scalecast](https://github.com/mikekeith52/scalecast) 是一个 Python 预测库，如果你想在应用优化的机器学习或深度学习模型之前对序列进行转换，然后轻松恢复结果，它表现良好。虽然其他库为 ML 模型提供了灵活和动态的区间，我不确定它们是否能够有效处理需要转换然后恢复的数据，特别是涉及差分的情况。如果我错了，请纠正我。

我专门为这个目的创建了 scalecast。使用该包变换和逆变换系列非常简单，包括使用[交叉验证来寻找最佳变换组合的选项](https://scalecast-examples.readthedocs.io/en/latest/transforming/medium_code.html#Function-to-Automatically-Find-Optimal-Transformation)。然而，在差分水平上应用置信区间（任何区间）在映射到系列的原始水平时变得复杂。如果你只是以与点预测相同的方式逆差分区间，它很可能会过于宽泛。避免这种情况的建议是使用不需要平稳数据的模型——如 ARIMA、指数平滑等。但如果你真的想比较 ML 模型，而你的数据不是平稳的，那就没那么有趣了。

Scalecast 使用的解决方案是上述简单一致性区间。如果对系列进行第一次、第二次或季节性差分，然后进行逆操作，再计算测试集残差并在其上应用百分位函数是很简单的。我使用一种称为 MSIS 的度量评估了该区间的有效性，详细信息见[过去的帖子](https://medium.com/towards-data-science/easy-distribution-free-conformal-intervals-for-time-series-665137e4d907)。

```py
pip install --upgrade scalecast
```

但这还可以更好。在时间序列中，当误差积累时，人们直观地认为，对于一个在时间上远离基准真实值的点预测，其区间将进一步扩展。预测我明天会做什么比预测一个月后的事情更容易。这种直观的概念已被纳入计量经济学方法中，但在简单区间中却不存在。

我们可以通过几种方式来解决这个问题，其中之一是[一致性分位数回归](https://arxiv.org/abs/1905.03222)，例如由[Neural Prophet](https://neuralprophet.com/code/forecaster.html)使用。这种方法可能有一天会被纳入 scalecast。但我将在这里概述的方法涉及使用回溯测试和根据每次回溯测试的残差应用百分位数。与采用假设不同，该方法将一切基于一些观察到的经验真相——实施模型与其实施的时间序列之间的真实关系。

# 回溯一致性区间

为此，我们需要将数据拆分成多个训练集和测试集。每个测试集需要与我们期望的预测范围长度相同，分割的数量应至少等于 1 除以 alpha，其中 alpha 是 1 减去期望的置信水平。同样，这将导致 95%置信度区间的 20 次迭代。考虑到我们需要通过整个预测范围的长度迭代 20 次或更多次，较短的序列可能在此过程完成之前就用尽观测值。一个缓解的方法是允许测试集重叠。只要测试集之间至少有一个观测值的差异，并且没有数据从任何训练集中泄漏，这样应该是可以的。这可能会使区间偏向于较新的观测值，但如果序列包含足够的观测值，可以选择在训练集之间增加更多空间。我解释的过程称为回测，但也可以视为修改版时间序列交叉验证，这是一种常见的方式来促进更准确的符合区间。Scalecast 通过管道和三个实用函数使获得这个区间的过程变得简单。

## 构建完整模型管道

首先我们构建一个管道。假设我们需要差分数据并使用 XGBoost 模型进行预测，管道可以是：

```py
transformer = Transformer(['DiffTransform'])
reverter = Reverter(['DiffRevert'],base_transformer=transformer)

def forecaster(f):
    f.add_ar_terms(100)
    f.add_seasonal_regressors('month')
    f.set_estimator('xgboost')
    f.manual_forecast()

pipeline = Pipeline(
    steps = [
        ('Transform',transformer),
        ('Forecast',forecaster),
        ('Revert',reverter)
    ]
)
```

重要的是要注意，这个框架也可以应用于深度学习模型、经典计量经济模型、RNN，甚至是简单模型。对于你想要应用于时间序列的任何模型，这都适用。

接下来，我们使用`fit_predict()`方法生成 24 个未来观测值：

```py
f = Forecaster(
    y=starts, # an array of observed values
    current_dates=starts.index, # an array of dates
    future_dates=24, # 24-length forecast horizon
    test_length=24, # 24-length test-set for confidence intervals
    cis=True, # generate naive intervals for comparison with the end result
)

f = pipeline.fit_predict(f)
```

## 回测管道并构建残差矩阵

现在，我们进行回测。对于 95%区间，这意味着至少需要 20 次训练/测试分割，迭代地向后移动通过最新观测。这是过程中的计算最昂贵部分，具体取决于我们想通过管道发送多少模型（我们可以通过扩展`forecaster()`函数来增加更多），是否想优化每个模型的超参数，以及是否使用多变量过程，可能会花费一些时间。在我的 Macbook 上，这个简单的管道在 20 次迭代下回测时间略超过一分钟。

```py
backtest_results = backtest_for_resid_matrix(
    f, # one or more positional Forecaster objects can go here
    pipeline=pipeline, # both univariate and multivariate pipelines supported
    alpha = 0.05, # 0.05 for 95% cis (default) 
    bt_n_iter = None, # by default uses the minimum required: 20 for 95% cis, 10 for 90%, etc.
    jump_back = 1, # space between training sets, default 1
)
```

从这个函数得到的回测结果可以用于多种目的。我们可以用它们来报告模型的平均误差，或者从中获取许多样本外预测的见解，或者用它们来生成区间。要生成区间，我们需要：

```py
backtest_resid_matrix = get_backtest_resid_matrix(backtest_results)
```

这会为每个评估的模型创建一个矩阵，其中一行代表每次回测迭代，一列代表每个预测步骤。每个单元格中的值是预测误差（残差）。

![](img/a22a4e7061ab75241cb885dea3f1b280.png)

作者提供的图片

通过应用列级百分位函数，我们可以生成上下值，以找到每个预测步骤的绝对残差的第 95 百分位数。平均来说，随着预测的延伸，这个值应该更大。在我们的例子中，第 1 步的上下值为 15，第 4 步为 16，第 24 步（最后一步）为 46。并非所有值都比上一个值大，但通常是的。

![](img/f36e78cbb17f5d8e6f3e79be8587c894.png)

图片由作者提供

## 构建回测区间

然后我们用新的动态区间覆盖了过时的简单区间。

```py
overwrite_forecast_intervals(
    f, # one or more positional Forecaster objects can go here
    backtest_resid_matrix=backtest_resid_matrix,
    models=None, # if more than one models are in the matrix, subset down here
    alpha = .05, # 0.05 for 95% cis (default)
)
```

看！我们为时间序列模型构建了一个无需假设的动态拟合区间。

这个区间比默认区间好多少？使用[MSIS](https://scalecast.readthedocs.io/en/latest/Forecaster/Util.html#src.scalecast.util.metrics.msis)，一种不为很多人所知或使用的度量方法，我们可以评估每个获得的区间在此过程前后的效果。我们还可以使用每个区间的覆盖率（实际观察值落在区间内的百分比）。我们为此目的预留了一部分与之前评估的测试集不重叠的数据。简单区间如下：

![](img/d81ab6dba30729ab44037cc203d4eefc.png)

图片由作者提供

结果是一个准确的预测，具有紧凑的区间。它包含了 100%的实际观察值，MSIS 得分为 4.03。从我对 MSIS 的有限使用经验来看，低于 5 通常是相当不错的。我们应用动态区间，得到如下结果：

![](img/e60be2eb6b1348a93c817e6902295b27.png)

图片由作者提供

这很好。我们有一个扩展的区间，其平均值比默认区间更紧凑。MSIS 得分略微提高至 3.92。坏消息是：24 个测试集观察值中有 3 个超出了这个新区间的范围，覆盖率为 87.5%。对于 95%的区间来说，这可能并不理想。

# 结论

当然，这只是一个例子，我们应谨慎得出过于宽泛的结论。我相信回测区间几乎总会扩展，这使得它比默认区间更直观。它可能在平均上也更准确，只是获得它需要更多的计算能力。

除了获得新的区间，我们还获得了回测信息。在 20 次迭代中，我们观察到了以下误差指标：

![](img/456f04d77caf1d852be5e3aae2b0f3e9.png)

图片由作者提供

相比仅使用一个测试集的误差，我们对报告这些结果感到更有信心。

感谢您的关注！如果您喜欢这些内容，请在 GitHub 上为[scalecast](https://github.com/mikekeith52/scalecast)点个赞，并查看与本文相关的[完整笔记本](https://scalecast-examples.readthedocs.io/en/latest/misc/cis-bt/cis-bt.html)。使用的数据可以通过[FRED](https://fred.stlouisfed.org/series/HOUSTNSA)公开获取。

[## GitHub - mikekeith52/scalecast: 实践者的预测库](https://github.com/mikekeith52/scalecast?source=post_page-----d1638aa48527--------------------------------)

### Scalecast 帮助你预测时间序列。以下是如何初始化其主要对象：Uniform ML 建模（包括模型…

[github.com](https://github.com/mikekeith52/scalecast?source=post_page-----d1638aa48527--------------------------------)
