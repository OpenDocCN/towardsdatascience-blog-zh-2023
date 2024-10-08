# 用 Facebook 的 Prophet 进行时间序列预测，10 分钟 — 第二部分

> 原文：[`towardsdatascience.com/time-series-forecasting-with-facebooks-prophet-in-10-minutes-part-2-1f558ccc3e83`](https://towardsdatascience.com/time-series-forecasting-with-facebooks-prophet-in-10-minutes-part-2-1f558ccc3e83)

## 模型性能和超参数微调

[](https://guillaume-weingertner.medium.com/?source=post_page-----1f558ccc3e83--------------------------------)![Guillaume Weingertner](https://guillaume-weingertner.medium.com/?source=post_page-----1f558ccc3e83--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1f558ccc3e83--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1f558ccc3e83--------------------------------) [Guillaume Weingertner](https://guillaume-weingertner.medium.com/?source=post_page-----1f558ccc3e83--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1f558ccc3e83--------------------------------) ·9 分钟阅读·2023 年 4 月 25 日

--

![](img/5229e96a7d81abdd74c0b1b14ccefc02.png)

Prophet 的输出 — 作者图片

# #1 在上一集…

在第一部分中，我们看到如何仅用 6 行代码快速建立一个工作的模型。请参阅下面的链接：

[](/time-series-forecasting-with-facebooks-prophet-in-10-minutes-958bd1caff3f?source=post_page-----1f558ccc3e83--------------------------------) ## 用 Facebook 的 Prophet 进行时间序列预测，10 分钟 — 第一部分

### 用 6 行代码建立一个工作的模型

towardsdatascience.com

但是我们真的可以信任这个模型输出的结果吗？

如果是这样，它的表现如何？我们能让它更好吗？

让我们看看如何评估模型的性能，添加假期/特殊事件（这些可能是时间序列的重要部分）到模型中，并调整其超参数以使其更准确、更可靠。

# #2 训练-测试拆分

为了评估我们模型的性能，我们可以等待未来的值发生，并将其与模型预测的结果进行比较，或者我们可以使用一种非常常见的模型验证方法，即将历史数据拆分成两个不同的数据集：

+   一个用于拟合模型的**训练集**

+   一个用于评估模型的**测试集**

我们将 2023 年 11 月和 12 月作为测试数据，其余部分（即训练集）用于训练模型。

```py
train = df.iloc[:1400,:]
test = df.iloc[1400:,:]
```

![](img/8fe919db71887506a39dc8e65d219e65.png)

训练-测试拆分 — 作者图片

# #3 性能指标

在这一部分中，让我们快速介绍一下我们将用来评估模型性能的指标。

我们先从几个快速定义开始：

+   yᵢ 是测试集中的实际数据点

+   ŷᵢ 是模型估计的点

+   ȳ 是时间序列测试集的均值

这三种指标（MAE、MSE 和 R 方）的组合应该能为我们提供对模型性能的足够估计。

**1/ 平均绝对误差 (MAE):**

我们计算观察值与预测值之间的平均绝对差。

![](img/4593cb47069417a5308fd0b598d574f6.png)

平均绝对误差 — 图片作者

MAE 非常简单易于解释，并告诉你“平均而言模型偏离了 x 数量”。

与其他误差指标相比，MAE 对异常值的敏感度较低，这意味着异常值受到的惩罚较轻。

**2/ 均方误差 (MSE):**

我们计算观察值与预测值之间的平均平方差。

![](img/1c440070e08141dbb913d3ed1f6b0e1b.png)

均方误差 — 图片作者

MSE 的单位与被预测数据的单位不同，因为它涉及误差的平方，使其比 MAE 更难解释。

然而，它对较大的误差给予更重的惩罚（由于平方操作），使得它对异常值或偏离实际值的预测更加敏感。

**3/ R 方**

决定系数是性能指标而非误差指标。它衡量回归模型中预测变量解释响应变量方差的比例。

![](img/80f437ef6fcb6ed5a8a8100765ab647b.png)

R 方 — 图片作者

R 方值范围从 0 到 1，较高的值表示模型与数据的拟合更好。值为 1 表示回归模型解释了响应变量的所有变异，而值为 0 表示模型没有解释任何变异。

# #4 模型的初步性能

正如我们在第一部分中所做的那样，让我们创建一个基本的、现成的模型。然而这次，我们将在训练集上拟合它，并在未见过的数据（即测试集）上测试其性能。

```py
# Creating an instance of the Prophet class and training the model
m = Prophet() #instantiating a new Prophet object
model = m.fit(train) #build the model with the historical data

future = model.make_future_dataframe(periods=len(test), freq='D') #build the dataframe containing the predictions 
forecast = model.predict(future) #dataframe including a column yhat with the forecast
```

让我们在同一图表上绘制基本模型和测试集的实际值，以了解发生了什么：

![](img/d728060cc209ad78107dc8c8fc7e45d1.png)

基本模型（橙色）与实际值（蓝色）— 图片作者

对于一个 6 行模型，它的视觉效果还不错！然而，我们可以清楚地看到模型没有很好地捕捉到一些点（图上的异常值/峰值）。

为了计算 MAE、MSE 和 R 方以评估模型的性能，我们可以简单地从 scikit-learn 库中导入这些指标。另一种方法是从头计算，但我们这里可以偷懒。

```py
from sklearn import metrics

r2_score = metrics.r2_score(list(test['y']), list(forecast.loc[1400:,'yhat']))
mae = metrics.mean_absolute_error(list(test['y']), list(forecast.loc[1400:,'yhat']))
mse = metrics.mean_squared_error(list(test['y']), list(forecast.loc[1400:,'yhat']))

print(f'r2_score : {r2_score}')
print(f'mae : {mae}')
print(f'mse : {mse}')
```

这是第一个基本模型的指标：

![](img/8635c18c19a409323e004015f7a579a6.png)

基本模型性能 — 图片作者

我们可以看出，这第一个模型在每次预测时平均偏差 $123（如 MAE 所示），R2 分数为 0.52。

目前 MSE 很难解释，但在与其他模型比较时会发挥作用。

# #5 添加假期

根据业务案例，将假期纳入模型可能是相关的。在我们虚构公司的情况下，我们可以假设假期可能会对销售产生影响。

Prophet 文档清楚地解释了如何输入特殊假期以便模型能够考虑。

让我们将黑色星期五和圣诞前夕作为特殊事件添加进来……

```py
black_friday = pd.DataFrame({
  'holiday': 'black_friday',
  'ds': pd.to_datetime(['2019-11-29', '2020-11-27', '2021-11-26', '2022-11-25']),
  'lower_window': -10,
  'upper_window': 1,
})
xmas_eve = pd.DataFrame({
  'holiday': 'xmas_eve',
  'ds': pd.to_datetime(['2019-12-24', '2020-12-24', '2021-12-24', '2022-12-24']),
  'lower_window': -5,
  'upper_window': 1,
})
holidays = pd.concat((black_friday, xmas_eve))
```

……以及将常规美国假期纳入我们的模型中。

并对这个第二个模型进行与之前相同的训练阶段。

```py
# Creating an instance of the Prophet class and training the model
m2 = Prophet(holidays=holidays)
m2.add_country_holidays(country_name='US') #adding US holidays
model2 = m2.fit(train)

future2 = model2.make_future_dataframe(periods=len(test), freq='D')
forecast2 = model2.predict(future2)
```

让我们绘制这两个模型，以了解发生了什么：

![](img/3a847f6454c69b7775984a874bb62d5c.png)

模型比较 — 作者提供的图像

从视觉上我们可以看出，将假期纳入模型中产生了积极影响，因为异常值被考虑在内。让我们来验证一下。

```py
r2_score = metrics.r2_score(list(test['y']), list(forecast2.loc[1400:,'yhat']))
mae = metrics.mean_absolute_error(list(test['y']), list(forecast2.loc[1400:,'yhat']))
mse = metrics.mean_squared_error(list(test['y']), list(forecast2.loc[1400:,'yhat']))

print(f'r2_score : {r2_score}')
print(f'mae : {mae}')
print(f'mse : {mse}')
```

![](img/9784a61b25447f68f82445f9c6920a1b.png)

基本模型 + 假期表现 — 作者提供的图像

将假期纳入我们的模型中是一个巨大的改进。

+   R2 分数从 0.52 上升到 0.69

+   MAE 从 $123 降至 $109，意味着每次预测的平均误差为 $109

+   由于一些异常值很可能是节假日，现在模型对这些异常值的建模更为准确，因此 MSE 显著下降。

# #6 微调模型的超参数

Prophet 的文档在解释我们应该调整哪些参数和哪些参数应该保持不变方面非常有帮助。

根据 Prophet 的开发人员[1]，让我们关注最重要的 5 个参数：

1/ **changepoint_prior_scale** — 默认值 0.05

“它决定了趋势的灵活性，特别是趋势在趋势变化点的变化程度”

2/ **seasonality_prior_scale** — 默认值 10

“此参数控制季节性的灵活性”

3/ **holidays_prior_scale** — 默认值 10

“这控制了假期效应的灵活性”

4/ **seasonality_mode** — 默认值 ‘additive’

“选项有‘additive’ 或 ‘multiplicative’。默认值是 ‘additive’，但许多业务时间序列将具有乘法季节性。”

5/ **changepoint_range** — 默认值 0.8

“这是趋势允许变化的历史比例。”

这些参数对模型的输出有很大影响。问题是，它们是否设置在最佳值上——即会产生最低 MAE 或 MSE 和最接近 1 的 R-squared 的值？

这一部分最有趣的是我们如何调整这些参数。我们将使用网格搜索，这是一种在机器学习中非常常见的技术，用于微调模型的超参数。

思路很简单：我们创建一个包含广泛超参数组合的“网格”，为每个组合训练一个模型，并测试每一个以查看哪个模型表现最佳。

```py
# Create the grid
changepoint_prior_scale = [0.01, 0.03, 0.05, 0.07] # default 0.05
seasonality_prior_scale = [1, 5, 10, 15] # default 10
holidays_prior_scale = [1, 5, 10, 15] # default 10
seasonality_mode = ['additive', 'multiplicative']
changepoint_range = [0.6, 0.7, 0.8, 0.9] # default 0.8

# Compute the total number of iterations
total_iter = len(changepoint_prior_scale)*len(seasonality_prior_scale)*len(holidays_prior_scale)*len(seasonality_mode)*len(changepoint_range)
print(f'Number of iterations : {total_iter}')

# Loop over the parameters, build and assess the models
grid_search_results = []
iteration = 1
for cps in changepoint_prior_scale:
    for sps in seasonality_prior_scale:
        for hps in holidays_prior_scale:
            for sm in seasonality_mode:
                for cr in changepoint_range:
                    m = Prophet(holidays=holidays, 
                                 changepoint_prior_scale = cps, 
                                 seasonality_prior_scale = sps, 
                                 holidays_prior_scale = hps, 
                                 seasonality_mode = sm, 
                                 changepoint_range = cr)
                    m.add_country_holidays(country_name='US')
                    model = m.fit(train)
                    future = model.make_future_dataframe(periods=len(test), freq='D')
                    forecast = model.predict(future) 

                    r2_score = metrics.r2_score(list(test['y']), list(forecast.loc[1400:,'yhat']))
                    mae = metrics.mean_absolute_error(list(test['y']), list(forecast.loc[1400:,'yhat']))
                    mse = metrics.mean_squared_error(list(test['y']), list(forecast.loc[1400:,'yhat']))

                    print(f'iteration : {iteration} / {total_iter} ')
                    print(f'r2_score : {r2_score}')
                    print(f'mae : {mae}')
                    print(f'mse : {mse}')

                    grid_search_results.append([iteration, cps, sps, hps, sm, cr, r2_score, mae, mse])
                    iteration += 1

# Store the results in a dataframe
grid_search_df = pd.DataFrame(grid_search_results, columns = ['iteration', 'cps', 'sps', 'hps', 'sm', 'cr', 'r2_score', 'mae', 'mse'])
```

当这一步完成后，我们需要做的就是选择具有最低 MAE、最低 MSE 或最接近 1 的 R-squared 的模型，具体取决于哪种标准最适合我们的业务问题。

对我来说，最重要的是在任何一天都不要出现较大的误差，而每天出现小的误差是可以接受的。这就是为什么我选择了具有最低均方误差（MSE）的模型。

```py
grid_search_df.sort_values('mse', ascending = True).head()
```

![](img/3e5c79550bb366dff916a12129a71e52.png)

超参数选择 — 作者图片

现在，让我们使用这些参数来构建我们最后的优化模型：

```py
changepoint_prior_scale = 0.05 # default 0.05
seasonality_prior_scale = 1 # default 10
holidays_prior_scale = 5 # default 10
seasonality_mode = 'additive'
changepoint_range = 0.6 # default 0.8

# Creating an instance of the Prophet class and training the model
m3 = Prophet(holidays=holidays, 
            changepoint_prior_scale = changepoint_prior_scale,
            seasonality_prior_scale = seasonality_prior_scale,
            holidays_prior_scale = holidays_prior_scale,
            seasonality_mode = seasonality_mode,
            changepoint_range = changepoint_range)
m3.add_country_holidays(country_name='US')
model3 = m3.fit(train)

future3 = model3.make_future_dataframe(periods=len(test), freq='D')
forecast3 = model3.predict(future3)
```

下面图表中的绿色线显示了优化模型，验证了上面表格所强调的内容：这个最新模型的测试数据预测更加准确。

![](img/4bb2e14f3a7fd93f2d06958ae553efb3.png)

所有模型的比较 — 作者图片

# #7 结论

该方法论（训练-测试分割、添加特殊事件、微调超参数）提供了一个基本框架，用于提升模型的准确性，如下表所示：

![](img/1325f8a7bab0ee44ba023a2fb18c7aab.png)

我们检查的 3 个指标上每个模型改进的总结

为了简化，我们没有进行任何交叉验证，这意味着我们只在一个测试数据集上评估了我们的模型。下一步是将交叉验证集成到我们的模型评估过程中。

另一个有趣的方面是将额外的回归变量添加到 Prophet 中——这是模型可以考虑的一个附加变量，以便进行预测。

# 有用的资源

[1] 官方 Prophet 文档：[`facebook.github.io/prophet/`](https://facebook.github.io/prophet/)

[2] Joos Korstanje 的《使用最先进模型（包括 LSTM、Facebook 的 Prophet 和 Amazon 的 DeepAR）进行高级预测》

[3] Aileen Nielsen 的《实用时间序列分析：统计和机器学习预测》

[4] 解释时间序列预测并简要介绍 Prophet 方程的有用网站：[`otexts.com/fpp3/prophet.html`](https://otexts.com/fpp3/prophet.html)

*感谢你阅读到文章的最后。

关注获取更多内容！

如果你有任何问题或意见，请随时在下方留言，或通过* [*LinkedIn*](https://www.linkedin.com/in/guillaume-weingertner-a4a27972/) *与我联系！*

[](https://guillaume-weingertner.medium.com/membership?source=post_page-----1f558ccc3e83--------------------------------) [## 通过我的推荐链接加入 Medium - Guillaume Weingertner

### 阅读 Guillaume Weingertner 和其他成千上万的 Medium 作者的每个故事。你的会员费直接...

guillaume-weingertner.medium.com](https://guillaume-weingertner.medium.com/membership?source=post_page-----1f558ccc3e83--------------------------------)
