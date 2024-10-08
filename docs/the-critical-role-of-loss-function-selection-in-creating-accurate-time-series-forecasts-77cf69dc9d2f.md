# 选择损失函数在创建准确的时间序列预测中的关键作用

> 原文：[`towardsdatascience.com/the-critical-role-of-loss-function-selection-in-creating-accurate-time-series-forecasts-77cf69dc9d2f`](https://towardsdatascience.com/the-critical-role-of-loss-function-selection-in-creating-accurate-time-series-forecasts-77cf69dc9d2f)

## 通过机器学习掌握时间序列预测

## 你的损失函数选择如何影响时间序列预测的准确性

[](https://johnadeojo.medium.com/?source=post_page-----77cf69dc9d2f--------------------------------)![John Adeojo](https://johnadeojo.medium.com/?source=post_page-----77cf69dc9d2f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----77cf69dc9d2f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----77cf69dc9d2f--------------------------------) [John Adeojo](https://johnadeojo.medium.com/?source=post_page-----77cf69dc9d2f--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----77cf69dc9d2f--------------------------------) ·8 分钟阅读·2023 年 3 月 14 日

--

![](img/50c340d8f06526401323807ca45397a4.png)

图片由 [Dan Asaki](https://unsplash.com/@danasaki?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

在这篇文章中，我将向你展示我认为在机器学习中常被忽视的一个重要方面，即损失函数的选择。我将通过介绍我在[Driven Data](https://www.drivendata.org/competitions/44/dengai-predicting-disease-spread/page/80/)主办的登革热竞赛中的方法来实现这一点。

我已经建立了一个岭回归模型作为基准模型，并使用不同损失函数构建了几种“风味”的 XGBoost 回归模型。

*竞争者被要求预测伊基托斯和圣胡安每周时间间隔内的登革热总病例。每位竞争者根据其模型在测试数据集上的平均绝对误差（MAE）进行排名。要了解更多关于挑战、登革热或自己参与竞赛的信息，可以访问挑战的* [*主页*](https://www.drivendata.org/competitions/44/dengai-predicting-disease-spread/page/80/)*。*

## 笔记本与代码库

我已经通过下面的链接提供了我的工作 Jupyter 笔记本和 GitHub 代码库。笔记本可能需要几分钟才能加载，请耐心等待。

📒 [Jupyter 笔记本](https://mybinder.org/v2/gh/john-adeojo/dengue-model/b0424c69bf29f05c0d3eef159f4419536fd770e6?urlpath=lab%2Ftree%2Fnotebooks%2FDengue+Fever+Data+Profilingv3.ipynb)

📁[Github 代码库](https://github.com/john-adeojo/dengue-model)

请随意在笔记本中尝试你自己的损失函数。

# 数据

数据以 csv 格式提供，包括训练特征、标签和测试特征（类似于 Kaggle 比赛格式）。数据特征包括天气预报、气候预报和植被指数。

标签（登革热总病例数）具有长尾特征，极端值较少。这在两个城市中都成立。这些分布的形状应该能给你一些线索，关于什么可能是适合模型的损失函数。

*本项目使用的数据由 Driven Data 策划和提供，按照* [*Driven Data*](https://www.drivendata.org/competitions/44/dengai-predicting-disease-spread/data/)*¹ 的规定可以在比赛之外免费使用。数据的原始来源可以从* [*国家海洋和大气管理局*](https://dengueforecasting.noaa.gov/)*获得，数据公开且可以免费使用，具体根据* [*NOAA 的条款和条件*](https://www.weather.gov/disclaimer#:~:text=Use%20of%20NOAA/NWS%C2%A0Data%20and%20Products)*。*

![](img/5b89fe539632b7df737479760c8514f3.png)

作者提供的图片：登革热总病例数 Iquitos 的分布

![](img/846ffac69405389dcd48a704bcd039ec.png)

作者提供的图片：时间序列 Iquitos

![](img/d18d77b461720201c02d0a12f4ce4c29.png)

作者提供的图片：登革热总病例数 San Juan 的分布

![](img/bcebae54cf4f1eea3eadd96b6b0e9286.png)

作者提供的图片：时间序列 San Juan

# 数据预处理

除了拆分数据外，我进行了三个数据预处理步骤。第一步是填补缺失值。我简单地用这些缺失特征的均值进行填补，这是一种快速且粗略的方法，避免了过多的分布偏移。

第二步是对数据进行标准化。标准化是一种特征缩放的形式，将数值特征的方差设置为 1，均值设置为 0。虽然 XGBoost 不需要缩放，但在拟合 Ridge 回归模型之前缩放特征是有用的。

我应用的第三个操作是独热编码。这将类别变量转换为其数值表示，以便可以用来训练模型。

*注意：为了防止数据泄漏，所有预处理步骤都在训练集、验证集和测试集上分别进行。*

## **特征工程**

我构建了一些滞后特征。这些特征对于提高时间序列模型的预测性能特别有用，因为它们能够利用自相关性。

数据中存在一些限制，这限制了我能够创建的滞后期。首先，测试数据比训练数据提前了 3 年，并且没有标签。这意味着我能够创建的最短滞后期是 3 年，这感觉相当长。接下来，病例数在周级别上记录得不均匀。我不得不通过在月级别上聚合病例数，并基于这些聚合创建滞后特征来克服这个问题。滞后特征被设计为均值、最小值和最大值。

# 机器学习方法

有两个独立的时间序列跟踪登革热的总病例数，按周记录。其中一个时间序列用于 Iquitos (Iq)，另一个用于 San Juan (Sj)，我分别为每个时间序列训练了模型。

这帮助我将机器学习概念化为一个优化问题，在这个问题中，你试图最小化一个目标函数。

> 目标函数 = 损失函数 + 正则化函数

损失函数决定了如何“评分”模型在预测标签方面的整体表现，在这个案例中就是登革热的总病例数。正则化函数惩罚模型复杂性，有助于减轻过拟合。

## 数据拆分

在深入讨论超参数调整之前，我应该简要地讲一下数据拆分。对于时间序列问题，随机拆分是不合适的。随机拆分数据会导致未来的数据点被包含在测试集中，从而给出虚假的（且可能过于乐观的）性能结果。我按日期拆分了数据，以防止任何泄漏，并设立了准确的验证。

+   *对于 Iquitos，训练数据集是截至 2008 年 9 月 30 日的所有观测数据。对于 San Juan，拆分点设置在 2004 年 7 月 30 日。*

+   *为了进一步减少过拟合，我在训练模型时使用了 5 折时间序列交叉验证。* [*阅读更多关于时间序列交叉验证的详细信息*](https://scikit-learn.org/stable/modules/cross_validation.html#time-series-split)*。*

## 超参数调整

我使用了均匀分布上的随机搜索来调整我的正则化参数。我发现这是一个明智的方法，可以快速探索广泛的超参数空间。[阅读更多关于随机搜索的详细信息](https://scikit-learn.org/stable/modules/grid_search.html#randomized-parameter-search)。

*注意，我只调整了 l2 正则化。这是 Ridge 和 XGBoost 的 alpha 和 lambda 参数。*

*如果你想尝试不同的正则化参数，看看它们如何影响模型拟合，可以查看这个* [*我之前制作的 Streamlit 应用*](https://john-adeojo-dengue-model-streamlitdengue-streamlit-x2f4ij.streamlit.app/)*。*

![](img/17cc51925915f00f0ec3bed0c51c8957.png)

作者提供的图像：Iquitos 的 Ridge 模型超参数调整过程示例。平均测试分数为 MAE

# 损失函数

我选择的损失函数包括泊松回归、平均绝对误差 (MAE) 和均方误差 (MSE)。

## 泊松回归

对于登革热传播，独立假设是合理的，因为病例不会从一个人传到另一个人。然而，恒定速率假设可能不适用于本案例研究，我认为登革热病例的发生率会根据多维因素显著变化。然而，泊松分布确实以平均速率作为模型参数。

*当目标变量是遵循泊松分布的计数数据时，使用泊松损失函数。这种分布类型假设事件独立发生并以恒定速率发生。*

## 平均绝对误差

平均绝对误差（MAE）仅仅测量预测值与实际值之间的绝对差异。MAE 对异常值的敏感性低于一些其他损失函数，尤其是 MSE。

> 我认为许多人将损失函数与评分指标混淆，假设最佳的损失函数是 MAE，因为这是比赛评分的依据，你将看到事实并非如此。

## 均方误差

均方误差（MSE）计算模型输出与期望输出之间的平方差。由于平方差异，MSE 对异常值敏感。它对预测值和实际值之间的大差异惩罚比小差异更严重。

# 模型性能

模型性能在验证集上进行评估。评分（与损失函数不同）是基于竞赛中的 MAE 评分和易解释性（简单地衡量登革热病例）。你将从下面的图表中看到选择损失函数如何显著影响每个模型的数据拟合情况。在分析这些图表时，考虑一下选择的损失函数如何导致模型以这种方式拟合数据。

## 岭回归

**伊基托斯**

*伊基托斯模型 MAE：7*

该模型捕捉到了一些季节性变化，但未能有效预测异常值。

![](img/7260d2cefe5e269dd03b0d4082115922.png)

作者提供的图像：伊基托斯的岭回归模型预测值与实际值

**圣胡安**

*圣胡安模型 MAE：23.7*

该模型能够捕捉到一些季节性变化，但未能有效预测异常值。岭回归的预测值通常比实际病例略高。

![](img/1d18ffbceaae3155d8f7569abf3202a8.png)

作者提供的图像：圣胡安的岭回归模型预测值与实际值

## XGBoost 回归器：泊松损失

**伊基托斯**

*伊基托斯模型 MAE：7.8*

该模型捕捉到了一些季节性变化，但未能有效预测异常值。模型预测到的峰值往往被低估。

![](img/5ecdf135fa0b1cbcc85774ca1829582b.png)

作者提供的图像：伊基托斯的泊松 XGBoost 模型预测值与实际值

**圣胡安**

*圣胡安模型 MAE：18.9*

模型似乎能够很好地预测训练数据中的峰值。模型尝试对验证数据进行预测，但可能会出现过度预测。使用泊松损失训练使模型尝试预测峰值。

![](img/3eb785f12f5c7be4d436be654804b2aa.png)

作者提供的图像：Poisson XGBoost 模型预测与 San Juan 实际数据对比

## XGBoost 回归器：MAE 损失

**Iquitos**

*Iquitos 模型 MAE: 7*

尽管 MAE 较低，但该模型似乎只是通过数据绘制了一条直线。它对异常值和季节性完全不敏感。

![](img/8d4f4112fdd705d233092ecf4d012388.png)

作者提供的图像：MAE XGBoost 模型预测与 Iquitos 实际数据对比

**San Juan**

*San Juan 模型 MAE: 16.6*

该模型在预测季节性方面似乎有效，但在预测峰值方面表现不佳，尽管它确实尝试了这方面的预测。

![](img/746fc10e0901e8826a038ac65213c963.png)

作者提供的图像：MAE XGBoost 模型预测与 San Juan 实际数据对比

## XGBoost 回归器：MSE 损失

**Iquitos**

*Iquitos 模型 MAE: 7.3*

该模型能够捕捉到一定的季节性，但在捕捉异常值方面表现不佳。

![](img/ddde1939d5f0c05b12b43bd9e554a52a.png)

作者提供的图像：MSE XGBoost 模型预测与 Iquitos 实际数据对比

**San Juan**

*San Juan 模型 MAE: 18.48*

该模型捕捉了季节性，但对异常值的捕捉效果不是很好。

![](img/c359633b928dab38d877c6c1070d95c3.png)

作者提供的图像：MSE XGBoost 模型预测与 San Juan 实际数据对比

# 提交结果

Poisson 模型在测试数据上获得了最高的整体准确率，MAE 为 27.6，MSE 模型次之，为 27.8，MAE 模型最后，为 29。

*Poisson 损失 XGBoost 在竞争中排名前 27%。对于最小的超参数调整和特征工程，这个成绩还不错。*

# 最后的思考

选择最佳模型完全依赖于预测目标。严格来说，如果我们只是讨论在验证集上最小化 MAE 的模型，那么整体最佳的是具有 MAE 损失的 XGBoost 回归器。然而，表现出最佳建模能力的模型似乎是 Poisson 损失变体。

感谢阅读。

*[1] Bull, P., Slavitt, I. 和 Lipstein, G. (2016). 利用群众的力量增加社会部门数据科学的能力。 [在线] 可用：* [*https://arxiv.org/abs/1606.07781*](https://arxiv.org/abs/1606.07781) *[访问日期：2023 年 3 月 13 日].*

[](https://medium.com/@johnadeojo/membership?source=post_page-----77cf69dc9d2f--------------------------------) [## 通过我的推荐链接加入 Medium - John Adeojo

### 我分享数据科学项目、经验和专业知识，以帮助你在旅程中前行。你可以通过……

medium.com](https://medium.com/@johnadeojo/membership?source=post_page-----77cf69dc9d2f--------------------------------) [](https://www.john-adeojo.com/?source=post_page-----77cf69dc9d2f--------------------------------) [## 主页 | John Adeojo

### 关于我 欢迎来到我的专业作品集！我是一名经验丰富的数据科学家和机器学习（ML）专家……

[www.john-adeojo.com](https://www.john-adeojo.com/?source=post_page-----77cf69dc9d2f--------------------------------)
