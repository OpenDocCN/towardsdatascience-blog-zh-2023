# 使用 InterpretML 分析 NYC Yellow Taxi 乘车记录

> 原文：[`towardsdatascience.com/analysing-nyc-yellow-taxi-trip-records-with-interpretml-d4421bff32ac`](https://towardsdatascience.com/analysing-nyc-yellow-taxi-trip-records-with-interpretml-d4421bff32ac)

## 回归分析和反事实解释

[](https://mgcodesandstats.medium.com/?source=post_page-----d4421bff32ac--------------------------------)![Michael Grogan](https://mgcodesandstats.medium.com/?source=post_page-----d4421bff32ac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d4421bff32ac--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d4421bff32ac--------------------------------) [Michael Grogan](https://mgcodesandstats.medium.com/?source=post_page-----d4421bff32ac--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d4421bff32ac--------------------------------) ·9 分钟阅读·2023 年 1 月 13 日

--

![](img/64c4442ade0c015dd072fde8ac9621d0.png)

来源：[StockSnap](https://pixabay.com/users/stocksnap-894430/) 提供的照片，来自 [Pixabay](https://pixabay.com/photos/coding-programming-working-macbook-924920/)

[InterpretML](https://interpret.ml/) 是一个由微软设计的 **可解释机器学习** 库，旨在使机器学习模型更易于理解并开放给人类解释。

在与业务利益相关者沟通发现结果时，这尤其重要，因为在许多情况下，业务利益相关者是非技术人员，他们希望理解机器学习模型所产生的发现对业务的影响。

本文的目的是说明如何通过可解释机器学习和反事实分析，更好地理解数据集中的潜在趋势，以及 InterpretML 如何以直观的方式传达这些发现。

本示例使用的数据集是 **NYC Taxi & Limousine Commission — yellow taxi trip records** 数据集。该数据集通过 [Azure Open Data](https://learn.microsoft.com/en-us/azure/open-datasets/dataset-taxi-yellow?tabs=azureml-opendatasets) 获取，该平台的数据来源于 [nyc.gov](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page) 网站，并受到 nyc.gov [使用条款](https://www.nyc.gov/home/terms-of-use.page) 的约束。数据集由 NYC Open Data 提供，该公司在其 [Kaggle](https://www.kaggle.com/datasets/nycopendata/new-york) 账户下以 CC0: 公众领域许可的方式发布数据。

请注意，**Python 3.8.0** 被用于进行以下分析。

# 数据集和预处理

前述数据集包含了关于纽约市黄色出租车行程的诸多方面的数据点，包括总收费金额、行程距离、小费金额和通行费金额。

该数据集还提供了许多其他变量，例如接送时间和地点、乘客人数以及收费类型。

然而，为了确定对**总收费金额**（下文分析中的因变量）的主要影响因素——**行程距离**、**小费金额**和**通行费金额**被选为本次分析的自变量（特征变量）。

本次分析使用了一个月的数据（2018 年 5 月 6 日至 2018 年 6 月 6 日）用于建模目的。

```py
import numpy as np
from azureml.opendatasets import NycTlcYellow
from datetime import datetime
from dateutil import parser

end_date = parser.parse('2018-06-06')
start_date = parser.parse('2018-05-06')

nyc_tlc = NycTlcYellow(start_date=start_date, end_date=end_date)
nyc_tlc_df = nyc_tlc.to_pandas_dataframe()
nyc_tlc_df
```

分析中产生了超过 900 万行的数据——这里是数据的一部分：

```py
>>> nyc_tlc_df
       vendorID  tpepPickupDateTime tpepDropoffDateTime  passengerCount  tripDistance puLocationId  ... extra  mtaTax  improvementSurcharge  tipAmount  tollsAmount  totalAmount
0             2 2018-05-27 17:50:34 2018-05-27 17:56:41               3          0.82          161  ...   0.0     0.5                   0.3       0.00          0.0         6.80
1             2 2018-05-23 08:20:41 2018-05-23 08:37:06               1          1.69          142  ...   0.0     0.5                   0.3       3.08          0.0        15.38
3             2 2018-05-23 09:02:54 2018-05-23 09:17:59               2          6.64          140  ...   0.0     0.5                   0.3       0.00          0.0        20.30
5             2 2018-05-23 13:28:48 2018-05-23 13:35:15               1          0.61          170  ...   0.0     0.5                   0.3       1.00          0.0         7.80
7             2 2018-05-23 07:05:50 2018-05-23 07:07:40               2          0.48           48  ...   0.0     0.5                   0.3       0.00          0.0         4.30
...         ...                 ...                 ...             ...           ...          ...  ...   ...     ...                   ...        ...          ...          ...
339945        2 2018-06-04 14:03:37 2018-06-04 14:17:11               1          1.95          262  ...   0.0     0.5                   0.3       2.00          0.0        13.30
339946        2 2018-06-04 17:15:23 2018-06-04 17:16:38               1          0.55          262  ...   1.0     0.5                   0.3       0.00          0.0         5.30
339947        2 2018-06-04 16:59:23 2018-06-04 18:24:02               6         16.95           88  ...   1.0     0.5                   0.3       0.00          0.0        62.30
339948        2 2018-06-04 10:34:44 2018-06-04 10:40:46               1          1.16          229  ...   0.0     0.5                   0.3       0.00          0.0         6.80
339949        1 2018-06-04 12:35:57 2018-06-04 12:58:32               1          2.80          231  ...   0.0     0.5                   0.3       0.00          0.0        17.30

[9066744 rows x 21 columns]
```

值得注意的是，原始数据集中包含一些虚假的数据，这需要在预处理阶段处理。例如，查看相关变量的描述性统计数据时，发现如*tipAmount*这类变量包含负值——显然支付“负小费”是不可能的。

```py
>>> nyc_tlc_df['totalAmount'].describe()
count    9.066744e+06
mean     1.676839e+01
std      1.502198e+01
min     -4.003000e+02
25%      8.750000e+00
50%      1.209000e+01
75%      1.830000e+01
max      8.019600e+03
Name: totalAmount, dtype: float64

>>> nyc_tlc_df['tipAmount'].describe()
count    9.066744e+06
mean     1.912497e+00
std      2.658866e+00
min     -1.010000e+02
25%      0.000000e+00
50%      1.410000e+00
75%      2.460000e+00
max      4.000000e+02
Name: tipAmount, dtype: float64

>>> nyc_tlc_df['tollsAmount'].describe()
count    9.066744e+06
mean     3.693462e-01
std      1.883414e+00
min     -1.800000e+01
25%      0.000000e+00
50%      0.000000e+00
75%      0.000000e+00
max      1.650000e+03
Name: tollsAmount, dtype: float64

>>> nyc_tlc_df['tripDistance'].describe()
count    9.066744e+06
mean     3.022766e+00
std      3.905009e+00
min      0.000000e+00
25%      1.000000e+00
50%      1.650000e+00
75%      3.100000e+00
max      9.108000e+02
Name: tripDistance, dtype: float64
```

为了处理这个问题，负值被替换为 0 值在相关变量中：

```py
y=nyc_tlc_df['totalAmount']
y[y < 0] = 0

tripDistance=nyc_tlc_df['tripDistance']
tripDistance[tripDistance < 0] = 0

tipAmount=nyc_tlc_df['tipAmount']
tipAmount[tipAmount < 0] = 0

tollsAmount=nyc_tlc_df['tollsAmount']
tollsAmount[tollsAmount < 0] = 0
```

现在，检查这些变量的最小值会得到最小值为 0——这正是我们所期望的。

```py
>>> np.min(tollsAmount)
0.0
>>> np.min(tipAmount)
0.0
>>> np.min(tripDistance)
0.0
>>> np.min(y)
0.0
```

注意到原始数据集的规模非常庞大——截至 2018 年有 15 亿行——超过 50 GB。此外，该数据集的数据自 2009 年开始收集。在这方面，这 900 万行的数据分析实例仍然只是冰山一角。

例如，冬季几个月的交通模式可能与五月和六月的情况大相径庭，而且没有保证在特定月份得出的发现会适用于其他时间段。

话虽如此，本次的目标是使用 InterpretML 来更好地理解数据——鉴于分析的这一数据片段，这应该是可以实现的。

# InterpretML：回归分析

进行分析时，首先导入相关库并对数据集进行训练-测试拆分：

```py
from interpret.glassbox import LinearRegression
from interpret import show
from sklearn.model_selection import train_test_split
seed = 1

X = np.column_stack((tripDistance, tipAmount, tollsAmount))
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.20, random_state=seed)
X_train
y_train
```

现在，在训练数据上运行回归分析：

```py
lr = LinearRegression(random_state=seed)
lr
lr.fit(X_train, y_train)
lr_global = lr.explain_global()
show(lr_global)
lr_local = lr.explain_local(X_test[:5], y_test[:5])
show(lr_local)
```

从上述代码中，你会注意到模型生成了**全局**和**局部**解释。

根据微软白皮书[“InterpretML：理解机器学习模型的工具包”](https://www.microsoft.com/en-us/research/uploads/prod/2020/05/InterpretML-Whitepaper.pdf)：

+   **全局**解释允许用户更好地理解模型的整体行为。

+   **局部**解释有助于更好地理解单个预测。

在上述模型中——全局解释说明了特征与因变量之间的总体关系，而局部解释则说明了在测试集中的五个独立观察中的关系。

![](img/cdc3900cc0096f1374db54a86070ba69.png)

来源：通过 Plotly.js 生成的 InterpretML 库图表

查看上述图表的全球解释，我们可以看到 feature_0001（或*tripDistance*）被排为最重要的特征，其次是*tipAmount*和*tollsAmount*。

让我们来看看测试集中的五个观测值的局部解释。*y_test[:5]* 变量包含以下值：

```py
>>> y_test[:5]
451746     9.95
161571    15.30
72007     20.16
115597    21.36
37697     22.77
Name: totalAmount, dtype: float64
```

生成了一个类似于前面的图表——但目的是预测上述每个值。

例如，对于实际的 y_test 值 9.95，预测值为 12.2，其中*tripDistance*特征的重要性最大，*tipAmount*作为次要重要特征。

![](img/6a04696b8a67a7b499724975271b38f5.png)

来源：通过 Plotly.js 生成的 InterpretML 库图表

然而，当预测实际 y_test 值 15.3 的值为 13.3 时，我们可以看到只有*tripDistance*被排为重要特征——其他两个特征没有被包括在内：

![](img/2f64acd80435d1a12a040f797fd69abc.png)

来源：通过 Plotly.js 生成的 InterpretML 库图表

这是预测值 22.8 对实际值 19.1 的图表。

![](img/22a6201a5eee804d552f846fb342ce27.png)

来源：通过 Plotly.js 生成的 InterpretML 库图表

# 反事实解释

反事实解释的目的是评估某个特征度量的变化如何影响结果变量。

为了本示例的目的，totalAmount 变量被转换为分类变量——任何 totalAmount 值高于 $10 被视为较高的费用，并赋值为**1**。任何 totalAmount 值低于 $10 被视为低费用，并赋值为**0**。

这是我们希望回答的问题：

> 在什么情况下，trip distance 和 tip amount 的变化会导致 1 值变为 0，反之亦然？

进行分析时，使用了 dice_ml 库——定义了连续特征和结果变量：

```py
import dice_ml
from dice_ml.utils import helpers  # helper functions
d = dice_ml.Data(dataframe=nyc_tlc_df, continuous_features=['tripDistance', 'tipAmount'], outcome_name='totalAmount')
```

数据被划分为训练集和测试集，定义了数值变量和分类变量，并创建了数值和分类数据的预处理管道——然后使用随机森林分类器训练模型。实现这些功能的完整代码可以在 DiCE [仓库](https://github.com/interpretml/DiCE/blob/main/docs/source/notebooks/DiCE_model_agnostic_CFs.ipynb) 中找到。

以下是 dice-ml 提供的反事实结果：

```py
>>> # generate counterfactuals
>>> dice_exp_random.visualize_as_dataframe(show_only_changes=True)
Query instance (original outcome : 1)
   tripDistance  tipAmount  totalAmount
0           1.8       1.85            1

Diverse Counterfactual set (new outcome: 0.0)
  tripDistance tipAmount totalAmount
0          0.4       1.0         0.0
1          0.3       1.0         0.0
Query instance (original outcome : 1)
   tripDistance  tipAmount  totalAmount
0           2.3        2.0            1

Diverse Counterfactual set (new outcome: 0.0)
  tripDistance tipAmount totalAmount
0          1.0         -         0.0
1          0.6         -         0.0
2          0.1         -         0.0
3          0.4         -         0.0
```

查看这些反事实结果提供了一些有趣的见解。

首先，我们可以看到，当 tipAmount 变量高于 1 时，totalAmount 变量也显示为 1——即表示收费总额高于 $10。

当观察到结果变量变为 0（费用低于 $10）时，我们可以看到 tipAmount 变量在所有实例中都不超过 1.0，且旅行距离都短于 1.0。

这可能表明在距离较长的旅程中更可能支付小费。也可能表明小费是出租车司机在特定旅程中总收入的重要来源——可能会有一些情况，其中一次旅行虽然距离较短，但支付的小费使总金额高于距离较长但未支付小费的旅程。

# 结论

在本文中，我们探讨了：

+   分析和预处理大数据集的方法

+   如何使用 InterpretML 进行回归分析

+   InterpretML 模型中的全局解释与局部解释之间的差异

+   使用 DICE-ML 生成反事实解释和从这一技术中获得的见解

如果你愿意，你也可以尝试在不同时间段运行上述模型，看看你得到什么结果。希望你喜欢这篇文章，并欢迎任何问题或反馈！

# 参考文献

+   [DiCE: 使用任何机器学习模型生成反事实解释](https://github.com/interpretml/DiCE/blob/main/docs/source/notebooks/DiCE_model_agnostic_CFs.ipynb)

+   [geeksforgeeks.org: 在 Pandas DataFrame 中用零替换负数](https://www.geeksforgeeks.org/replace-negative-number-by-zeros-in-pandas-dataframe/)

+   [InterpretML: 用于理解机器学习模型的工具包](https://www.microsoft.com/en-us/research/uploads/prod/2020/05/InterpretML-Whitepaper.pdf)

+   [InterpretML: 线性模型](https://interpret.ml/docs/lr.html)

+   [learn.microsoft.com: 纽约市出租车与豪华车委员会——黄出租车旅行记录](https://learn.microsoft.com/en-us/azure/open-datasets/dataset-taxi-yellow?tabs=azureml-opendatasets)

+   [nyc.gov](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

+   [Python 数据科学：描述性统计](https://www.pythonfordatascience.org/descriptive-statistics-python/)

*免责声明：本文基于“现状”撰写，不提供任何担保。本文旨在提供数据科学概念的概述，不应视为专业建议。本文中的发现和解释仅代表作者个人观点，并未得到或与本文提及的任何第三方认可或关联。作者与文中提到的任何第三方没有任何关系。*
