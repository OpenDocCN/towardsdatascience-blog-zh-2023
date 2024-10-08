# 对你的预测模型进行测试：回测指南

> 原文：[`towardsdatascience.com/putting-your-forecasting-model-to-the-test-a-guide-to-backtesting-24567d377fb5`](https://towardsdatascience.com/putting-your-forecasting-model-to-the-test-a-guide-to-backtesting-24567d377fb5)

![](img/d92886eef96d6f3b904a3be033fdc241.png)

使用 Midjourney 生成的图像

## 通过回测学习如何正确评估时间序列模型的性能

[](https://eryk-lewinson.medium.com/?source=post_page-----24567d377fb5--------------------------------)![Eryk Lewinson](https://eryk-lewinson.medium.com/?source=post_page-----24567d377fb5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----24567d377fb5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----24567d377fb5--------------------------------) [Eryk Lewinson](https://eryk-lewinson.medium.com/?source=post_page-----24567d377fb5--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----24567d377fb5--------------------------------) ·9 分钟阅读·2023 年 11 月 8 日

--

评估时间序列模型不是一件简单的事。事实上，在评估预测模型时，很容易犯下严重的错误。虽然这些错误可能不会破坏代码或阻止我们获取一些输出数字，但它们可以显著影响这些性能估计的准确性。

在本文中，我们将演示如何正确评估时间序列模型。

# 为什么标准的机器学习方法不适用于时间序列？

评估机器学习模型性能的最简单方法是将数据集拆分为两个子集：训练集和测试集。为了进一步提高性能估计的稳健性，我们可能需要多次拆分数据集。这一过程称为交叉验证。

以下图示表示了最流行的交叉验证类型之一——k 折方法。在 5 折验证的情况下，我们首先将数据集划分为 5 个部分。然后，我们使用其中的 4 个部分训练模型，并在第 5 个部分上评估其性能。这个过程会重复 4 次，每次用不同的部分进行评估。

![](img/639b2f535b64a97ee71a9fda71ed5265.png)

5 折交叉验证

基于图示，你可能会发现使用这种方法进行预测的问题。在大多数情况下，我们会使用在评估集之后按时间顺序出现的数据来训练模型。这会导致*数据泄漏*，我们应当绝对避免。潜在的风险是，模型可能会学习到未来的模式，而这些模式在过去尚未显现出来。这会导致过于乐观的性能估计。

K 折交叉验证以及许多其他方法都假设观察值是独立的。时间序列数据中的时间依赖性显然与这一假设不符，这使得大多数在回归或分类中流行的验证方法变得不可用。因此，我们必须使用专门针对时间序列数据的验证方法。  

*注意：Bergmeir 等人展示了在纯自回归模型的情况下，只要所考虑的模型具有不相关的误差，使用标准的 K 折交叉验证是可能的。你可以* [*在这里*](https://robjhyndman.com/publications/cv-time-series/) *阅读更多相关内容*。  

# 什么是回测？  

**回测**（也称为**后向预测**或**时间序列交叉验证**）是一组旨在满足时间序列特定要求的验证方法。与交叉验证类似，回测的目标是获得模型部署后的可靠性能估计。我们还可以使用这些方法进行超参数调整和特征选择。  

回测的想法是复制一个现实的场景。训练数据应对应于在做出预测时可用于训练模型的数据。验证集应反映我们在部署该模型后可能遇到的数据。  

下面我们展示了一种称为**向前验证**（或**扩展窗口验证**）的方法图示，它遵循了我们刚刚描述的特征。在每个后续时间点，我们有更多的数据来训练我们的模型，相应地，我们的测试集也按相同的时间间隔前移。这种类型的验证保持了时间序列的时间顺序。  

![](img/d6dcf6ac1bf6385fd7c3093c7a4f434d.png)  

向前验证（扩展窗口）  

向前验证是回测中最简单的方法。我们可以考虑一些它的修改，以更好地适应我们的使用案例：  

+   我们假设了一个扩展窗口。然而，我们可能希望仅使用时间序列的最新子集来训练模型。那么，我们应该使用固定大小的滚动窗口。

+   在重新拟合模型时，我们可以采取各种策略。在最简单的情况下，我们在每次回测迭代中重新拟合模型。或者，我们可以只在第一次迭代中拟合模型，然后使用已拟合的模型（可能具有更新的特征）进行预测。或者我们可以每 X 次迭代重新拟合一次。再次强调，我们应该选择一个与模型的实际使用情况紧密契合的解决方案。  

+   我们可以在训练集和验证集之间引入一个间隔，因为验证集的初始部分可能与训练集的最终部分高度相关。通过创建一个间隔（例如，通过移除接近验证集的训练观察值），我们增强了两个数据集之间的独立性。这个过程也称为**清洗**。  

+   处理多个时间序列（如不同产品的销售）可能会带来另一种复杂性。由于数据集中的时间序列可能彼此相关（至少在某种程度上），我们可能希望将每个序列保留在特定的折叠中，以防止信息泄露。有关更多信息，请参阅*参考文献*部分提到的链接。

在文章的下一部分，我们将演示如何使用最简单的前向验证案例创建自定义回测类。我强烈鼓励你自己尝试其他可能性，或使用流行的专注于时间序列预测的 Python 库的回测功能。

![](img/48d652692ffec3c25c17ec57264c2d36.png)

使用 Midjourney 生成的图像

# 实操示例

首先，我们导入所需的库。由于我们要创建一个自定义回测类，因此将使用标准库。

```py
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib as mpl
import seaborn as sns

from sklearn.metrics import mean_absolute_error, mean_squared_error
from sklearn.linear_model import LinearRegression
from sklearn.ensemble import RandomForestRegressor
```

## 生成数据

为了简化起见，我们将生成一个跨度为 4 年的每日时间序列。

稍后，我们将测试几个模型，包括两个机器学习模型。为此，我们将创建按月生成的虚拟变量来考虑季节性。有关这种方法的更多细节，请参阅这篇文章。

## 定义回测器类

自定义回测器类相当长，因此我们将首先查看这里的代码，然后逐部分分析它。

让我们从输入开始：

+   `pred_func` — 一个根据训练数据和所需特征生成预测的函数。我们决定使用这种方法，因为我们希望保持类的灵活性，并允许用户使用他们选择的任何机器学习库/框架。

+   `start_date` — 回测的开始日期。

+   `end_date` — 回测的结束日期。

+   `backtest_freq` — 我们应该多频繁地训练模型并创建预测。例如，提供“7D”会使我们每周从开始日期起创建一组新的预测。

+   `data_freq` — 数据的频率。我们使用此参数来为正确的日期创建预测。

+   `forecast_horizon` — 与`data_freq`结合使用，我们使用这个参数来确保我们为所需的范围创建预测。

在`run_backtest`方法中，我们迭代回测中的每一个预测日期。对于每个日期，我们将训练集（包含进行预测时所有可用信息）与验证集（由预测范围决定）分开。随后，我们生成预测并将预测值与实际值一起存储。在最后一步，我们将所有单独的数据帧合并为一个数据帧，其中包含整个回测期间做出的所有预测。

在`evaluate_backtest`方法中，我们使用先前生成的 DataFrame 来计算各种评估指标。这些指标可以通过提供一个包含指标名称及其计算函数的字典来指定。然后，我们分别计算每个预测期的请求指标，并计算整体结果。

`evaluate_backtest`方法的第一步是检查是否有回测的 DataFrame（它在我们使用`run_backtest`方法后才会变得可用）。如果没有，我们需要先实际运行回测。

## 运行回测

现在是时候运行回测了。我们将比较四个“模型”的表现：

+   朴素预测：在这种方法中，预测等于做出预测时最后已知的值。

+   平均预测：这个预测等于训练集的均值。

+   一个带有月份虚拟变量的线性回归模型。

+   一个带有月份虚拟变量的随机森林模型。

前两个模型将作为简单的基准，而后两个模型旨在真正学习一些东西。然而，这些模型绝不是好的模型。我们仅仅用它们来说明我们类的回测能力。

在以下代码片段中，我们定义了用于获取预测的函数。如前所述，我们选择这种方法以保持灵活性，并能够将任何类型的 ML 模型封装成返回预期预测期的函数。

在以下代码片段中，我们定义了用于运行回测的常量。我们还定义了一个字典，将用来存储评分。

对于指标，我们选择专注于平均绝对误差（MAE）和均方误差（MSE）。我们选择这两个指标是为了展示我们可以在这种方法中使用多个指标。对于实际模型比较，我们将专注于 MAE。

在以下代码片段中，我们回测了朴素模型。因为我们将`verbose`设置为 True，我们还可以检查回测的每个迭代。我们打印了训练集和验证集的范围以及观察值的数量。

此外，我们将 MAE 评分存储在字典中，指明评分来源的方法。我们这样做是为了稍后将它们结合起来进行评估。

类似地，我们对剩下的三个预测函数进行回测。为了简洁起见，我们没有包含所有代码，因为它们非常相似。我们仅提供一个线性模型的示例，因为我们必须将特征列表作为额外参数添加到`run_backtest`方法中。

## 回测结果

通过结合回测结果，我们可以看到 ML 模型在 MAE 方面优于基准模型。

作为回测类的潜在扩展，绘制一些预测与实际值的对比图将有助于进一步评估预测的质量。

# 总结

+   由于时间序列中的时间依赖性，传统的验证方法如 k 折交叉验证不能使用。

+   回测（或时间序列交叉验证）是为了满足时间序列特定要求而设计的验证方法。

+   我们可以使用回测来获得模型部署后的可靠性能估计。此外，我们还可以利用这些方法进行超参数调整和特征选择。

你可以在[这里](https://deepnote.com/workspace/eryks-sandbox-c1f480c2-5a18-4fd6-9cf4-e147f5297b3f/project/Medium-Articles-0fb8b8c3-20f4-42b6-8571-a2968c4d72c2/notebook/introduction_to_backtesting-d9997b101e2741cdbf712f1ce6f0e497)找到本文中使用的代码。如往常一样，任何建设性的反馈都非常欢迎。你可以通过[LinkedIn](https://www.linkedin.com/in/eryklewinson/)、[Twitter](https://twitter.com/erykml1)或评论区与我联系。

*喜欢这篇文章吗？成为 Medium 会员，继续无限制地阅读学习。如果你使用* [*这个链接*](https://eryk-lewinson.medium.com/membership) *成为会员，你将无需额外费用地支持我。提前感谢，期待再见！*

你可能还对以下内容感兴趣：

[](/the-comprehensive-guide-to-moving-averages-in-time-series-analysis-3fb2baa749a?source=post_page-----24567d377fb5--------------------------------) ## 时间序列分析中移动平均的综合指南

### 探索简单移动平均和指数加权移动平均的细微差别

towardsdatascience.com [](/a-comprehensive-guide-on-interaction-terms-in-time-series-forecasting-16bfa468ae?source=post_page-----24567d377fb5--------------------------------) ## 时间序列预测中的交互项综合指南

### 了解如何通过使线性模型对趋势变化更具灵活性来改善其拟合效果

towardsdatascience.com [](/three-approaches-to-feature-engineering-for-time-series-2123069567be?source=post_page-----24567d377fb5--------------------------------) ## 时间序列的三种特征工程方法

### 使用虚拟变量、循环编码和径向基函数

towardsdatascience.com

# 参考文献

+   Bergmeir, Christoph, Mauro Costantini, and José M. Benítez. “关于交叉验证在方向性预测评估中的有用性。”《计算统计与数据分析》76 (2014): 132–143。

+   Bergmeir, C., Hyndman, R. J., & Koo, B. (2018). 关于交叉验证在评估自回归时间序列预测中的有效性的说明。*计算统计与数据分析*，*120*，70–83。

+   Racine, Jeff. “用于依赖数据的一致交叉验证模型选择：hv-block 交叉验证。”《计量经济学杂志》99.1 (2000): 39–61。

+   [`www.kaggle.com/code/jorijnsmit/found-the-holy-grail-grouptimeseriessplit`](https://www.kaggle.com/code/jorijnsmit/found-the-holy-grail-grouptimeseriessplit)

+   [`stackoverflow.com/questions/51963713/cross-validation-for-grouped-time-series-panel-data`](https://stackoverflow.com/questions/51963713/cross-validation-for-grouped-time-series-panel-data)

+   [`datascience.stackexchange.com/questions/77684/time-series-grouped-cross-validation`](https://datascience.stackexchange.com/questions/77684/time-series-grouped-cross-validation)

除非另有说明，否则所有图片均由作者提供。
