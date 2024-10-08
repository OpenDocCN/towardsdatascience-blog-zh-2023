# 通过强大的五步因果影响框架释放你作为商业分析师的全部潜力

> 原文：[`towardsdatascience.com/unlock-your-full-potential-as-a-business-analyst-with-the-powerful-5-step-causal-impact-framework-8783f8cda4dc?source=collection_archive---------4-----------------------`](https://towardsdatascience.com/unlock-your-full-potential-as-a-business-analyst-with-the-powerful-5-step-causal-impact-framework-8783f8cda4dc?source=collection_archive---------4-----------------------)

## **因果推断可以帮助你成为一名商业分析师明星**

[](https://medium.com/@juanjosemunozp?source=post_page-----8783f8cda4dc--------------------------------)![Juan Jose Munoz](https://medium.com/@juanjosemunozp?source=post_page-----8783f8cda4dc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8783f8cda4dc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8783f8cda4dc--------------------------------) [Juan Jose Munoz](https://medium.com/@juanjosemunozp?source=post_page-----8783f8cda4dc--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8783f8cda4dc--------------------------------) ·阅读时长 7 分钟·2023 年 11 月 22 日

--

**在商业环境中，领导层通常对决策或事件对 KPI 的影响感兴趣。** 作为一名绩效分析师，我的大部分时间都在回答这样的问题：“{新闻、政府公告、特殊事件…} 对国家 X 的表现有何影响？”。直观地，如果我们知道新闻/公告/特殊事件从未发生过的情况下会发生什么，我们就能回答这个问题。

**这就是因果推断的本质，一些非常有才华的人正在努力使因果推断框架对我们可用。**

![](img/2d9ad0b0b476457ea4385e4790bb2aeb.png)

照片由 [Andrew George](https://unsplash.com/@andrewjoegeorge?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Google Causal Impact 库就是其中之一。由 Google 开发，旨在帮助他们做出更好的营销预算决策，这个库可以帮助我们量化任何事件或干预对感兴趣的时间序列的影响。它听起来可能有些吓人，但其实非常直观。

作为商业分析师，我们应该在日常工作中利用这些工具；以下是你可以采取的 5 个简单步骤，以实施你的第一次因果影响分析。

# 步骤 1：安装和导入包

本指南将使用 Python。

我们将从安装 Google Causal Impact 包开始。

```py
>pip install tfcausalimpact
```

你可以在 GitHub 上找到更多关于这个包的信息：[`github.com/WillianFuks/tfcausalimpact`](https://github.com/WillianFuks/tfcausalimpact)

运行因果影响分析时，你只需要 4 个包。

```py
from causalimpact import CausalImpact
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
```

# 第 2 步：导入数据并定义前/后期

**我们可以把因果影响框架看作是一个时间序列问题。**

在特定日期，我们观察一个事件、新闻等，并跟踪我们的关注指标在该事件后相对于某些基准的变化。你可以将基准视为控制组。

要进行因果影响分析，我们需要以下内容：

+   研究事件的日期

+   我们受影响单位的关注变量的时间序列

+   我们受影响单位的关注变量的时间序列

+   未受事件影响的其他多个单位的关注变量的时间序列

对于我的示例，我使用了以下虚构场景：

+   **事件：** 2022 年 7 月 9 日在巴塞罗那举行的全球会议

+   **关注变量**：航空公司的乘客收入

+   **受影响单位：** 巴塞罗那

+   **控制组：** 其他欧洲市场

+   **问题：** 会议对巴塞罗那的乘客收入产生了什么影响？

在考虑时间范围时，应该尽量缩短后期的时间，而前期应该比后期长。

请注意，数据是人为生成的，仅用于说明。

```py
#Define the dates
training_strat = "2022-03-10"
training_end = "2022-06-30"
treatment_start = "2022-07-01"
treatment_end = "2022-07-13"

#import the data- for my example i am using a CSV
data=pd.read_csv("example.csv", index_col='Date', parse_dates=True) 
```

![](img/10c5454cbd2ea71e6fb94de81e38a013.png)

数据框的头部数据

# 第 3 步：创建控制组

此步骤是最关键的部分。

要估计任何事件或处理的影响，我们需要知道我们的关注单位*(例如：巴塞罗那乘客收入)*在处理和未处理情况下的情况。这是因果推断的主要问题。

> 我们永远无法在两种互斥的情况下观察我们的关注单位。解决方案是创建一个反事实场景。

**你可以把反事实看作是一个假设场景，在这个场景中事件/处理并未发生。**

**我们的控制组将帮助我们创建这个场景。** 要决定将哪些市场包括在控制组中，我们需要计算哪些市场具有预测我们关注市场（巴塞罗那）的预测能力。我们将使用相关性来建立这种预测能力。

此步骤仅需使用前期数据完成。

```py
# get the training data only
df_training = data[data.index <= pd.to_datetime(training_end)]
```

在使用时间序列检查相关性时，我们需要数据是平稳的，意味着没有趋势或季节性成分，以避免发现虚假的相关性。

```py
# Check for stationarity
from statsmodels.tsa.stattools import adfuller

test = adfuller(x = df_training['Barcelona'])[1]
print(test)
if test< 0.05:
    print("Data is stationary")
else:
    print("Time series is not stationary")
```

![](img/38628a17b0baca69aa5f4626ef92bf1a.png)

来自 adfuller 测试的结果

**然而，大多数时间序列并非平稳，但我们可以使用称为差分的方法使其平稳。**

```py
# Differening calculates the difference or Pct Difference from the previous date
differencing = df_training.pct_change().dropna(thresh = 1,axis=1).dropna()
differencing.head()
```

![](img/655da767b7c923283bf63b1c80f151b5.png)

每一行表示与前一天的百分比差异。

```py
#Rest on the differenced data
test = adfuller(x = differencing['Barcelona'])[1]
print(test)
if test< 0.05:
    print("Data is stationary")
else:
    print("Time series is not stationary")
```

![](img/7fdb6f7eec170d9658400614ce461658.png)

现在，我们可以使用这个新的时间序列来建立市场之间的相关性。

```py
#Create correlation
market_cor= pd.Series(differencing.corr().abs()['Barcelona'])

#Create a list with markets with correlation coef >=0.3
markets_to_keep = list(market_cor[market_cor >=0.3].index)

#Keep only markets on the above list
final_data = data.drop(columns=[col for col in data if col not in markets_to_keep])
```

![](img/3f2e240aadea11ea2b4eec232b06ed17.png)

我们将用于对照组的市场

# 步骤 4：实施 CausalImpact

现在，我们准备实施 CausalImpact。

**简而言之，CausalImpact 将利用我们的对照组来学习预测巴塞罗那在前期的乘客收入**。模型将用此来预测一个没有会议发生的反事实后期情境。

实际发生的情况与反事实情境之间的差异即为会议的影响。

```py
#Prepare Pre and Post periods
pre_period = [training_strat,training_end]
post_period = [treatment_start,treatment_end]

#Fitting CausalImpact
impact = CausalImpact(data=final_data,
                      pre_period=pre_period,
                      post_period=post_period)
```

# 步骤 5：解释结果与验证

Google CausalImpact 使得结果的可视化和总结变得非常简单。

你可以通过绘制影响图开始。

```py
impact.plot()
```

![](img/f7c4fdf29aaecd2eaf4d7610bd933670.png)

```py
print(impact.summary())
```

```py
Posterior Inference {Causal Impact}
                          Average                Cumulative
Actual                    397631.92              5169215.0
Prediction (s.d.)         177248.77 (6914.43)    2304234.0 (89887.6)
95% CI                    [163527.02, 190631.09] [2125851.11, 2478204.1]

Absolute effect (s.d.)    220383.16 (6914.43)     2864981.0 (89887.6)
95% CI                    [207000.83, 234104.91]  [2691010.9, 3043363.89]

Relative effect (s.d.)    124.34% (3.9%)          124.34% (3.9%)
95% CI                    [116.79%, 132.08%]      [116.79%, 132.08%]

Posterior tail-area probability p: 0.0
Posterior prob. of a causal effect: 100.0%

For more details run the command: print(impact.summary('report'))
```

根据你测量的内容，你可能对平均影响或累计影响感兴趣。在我们的案例中，我们对会议期间的累计影响感兴趣。

根据分析，会议为巴塞罗那的乘客收入贡献了+280 万美元的增益。

为了更全面的总结，你可以使用以下命令。

```py
print(impact.summary('report'))
```

```py
Analysis report {CausalImpact}

During the post-intervention period, the response variable had
an average value of approx. 397631.92\. By contrast, in the absence of an
intervention, we would have expected an average response of 177248.77.
The 95% interval of this counterfactual prediction is [163527.02, 190631.09].
Subtracting this prediction from the observed response yields
an estimate of the causal effect the intervention had on the
response variable. This effect is 220383.16 with a 95% interval of
[207000.83, 234104.91]. For a discussion of the significance of this effect,
see below.

Summing up the individual data points during the post-intervention
period (which can only sometimes be meaningfully interpreted), the
response variable had an overall value of 5169215.0.
By contrast, had the intervention not taken place, we would have expected
a sum of 2304234.0\. The 95% interval of this prediction is [2125851.11, 2478204.1].

The above results are given in terms of absolute numbers. In relative
terms, the response variable showed an increase of +124.34%. The 95%
interval of this percentage is [116.79%, 132.08%].

This means that the positive effect observed during the intervention
period is statistically significant and unlikely to be due to random
fluctuations. It should be noted, however, that the question of whether
this increase also bears substantive significance can only be answered
by comparing the absolute effect (220383.16) to the original goal
of the underlying intervention.

The probability of obtaining this effect by chance is very small
(Bayesian one-sided tail-area probability p = 0.0).
This means the causal effect can be considered statistically
significant.
```

**与机器学习不同，Causal Impact 没有准确度测量，这可能使得验证有点棘手。**

然而，你可以做 3 件事来验证你的结果。

1.  确保你在前期的置信区间不要过于宽泛——这可能表明你的对照组预测能力不足。

1.  确保估计影响的置信区间不包含 0。

1.  使用反驳测试，例如，如果你进行相同的分析但将事件日期更改为前期的任何一天，影响应为 0。

这是我在工作中最常做的分析之一；一旦你熟悉了这 5 步框架，你也可以使用它，并成为一名商业分析高手。

**参考文献**

[1]Brodersen, K. H., Gallusser, F., Koehler, J., Remy, N., & Scott, S. L. (2015). 使用贝叶斯结构时间序列模型推断因果影响。

[2]Molak, A., & Jaokar, A. (2023). 《Python 中的因果推断与发现：揭开现代因果机器学习的秘密，涵盖 DoWhy、EconML、PyTorch 等》 [平装本]。5 月 31 日。

[3][`research.google/pubs/pub41854/`](https://research.google/pubs/pub41854/)[Kay Brodersen 的《使用 CausalImpact 推断事件影响》](https://www.youtube.com/watch?v=GTgZfCltMm8&ab_channel=BigThingsConference)
