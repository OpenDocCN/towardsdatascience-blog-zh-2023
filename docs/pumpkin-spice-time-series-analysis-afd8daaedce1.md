# 南瓜香料时间序列分析

> 原文：[https://towardsdatascience.com/pumpkin-spice-time-series-analysis-afd8daaedce1?source=collection_archive---------3-----------------------#2023-10-24](https://towardsdatascience.com/pumpkin-spice-time-series-analysis-afd8daaedce1?source=collection_archive---------3-----------------------#2023-10-24)

## 穿上你最舒适的低保真音乐，拿上宽大的毛衣和你最爱的热饮，让我们开始使用Python吧。

[](https://medium.com/@ls.casanave?source=post_page-----afd8daaedce1--------------------------------)[![Louis Casanave](../Images/ca6ddaa01d86491f05d7e516aeda0baa.png)](https://medium.com/@ls.casanave?source=post_page-----afd8daaedce1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----afd8daaedce1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----afd8daaedce1--------------------------------) [Louis Casanave](https://medium.com/@ls.casanave?source=post_page-----afd8daaedce1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa25bdaa6a5ad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpumpkin-spice-time-series-analysis-afd8daaedce1&user=Louis+Casanave&userId=a25bdaa6a5ad&source=post_page-a25bdaa6a5ad----afd8daaedce1---------------------post_header-----------) 发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----afd8daaedce1--------------------------------) ·8分钟阅读·2023年10月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fafd8daaedce1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpumpkin-spice-time-series-analysis-afd8daaedce1&user=Louis+Casanave&userId=a25bdaa6a5ad&source=-----afd8daaedce1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fafd8daaedce1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpumpkin-spice-time-series-analysis-afd8daaedce1&source=-----afd8daaedce1---------------------bookmark_footer-----------)![](../Images/caaeaeed7ab1087b2fd0f9723b4df342.png)

照片由[Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在北半球，又到了那个时候——苹果、南瓜以及各种肉桂、豆蔻、姜、全香料和丁香的搭配开始出现。随着超市开始为万圣节、感恩节和冬季假期做准备，这是一个重拾我统计建模技能的好时机。抓紧你的调味拿铁，让我们进行一些以函数为导向的季节性建模。[完整代码笔记本可以在这里找到。](https://github.com/casanave/Pumpkin_Spice/tree/main)

## 假设：

“南瓜香料”作为美国的Google搜索词，其流行度会有强烈的季节性，因为它与美国秋季节日和季节性食物有关。

## 零假设：

使用上周或去年数据将更能预测本周“南瓜香料”搜索词的流行程度。

## 数据：

[来自Google趋势的过去5年数据，提取于2023年10月7日。](https://github.com/casanave/Pumpkin_Spice/blob/main/pumpkin_spice_5_years.csv) [1]

# 迭代建模方法：

+   制作一个简单模型，其中上周/去年数据作为本周的预测。具体而言，仅仅让我的最终模型在真空中准确或不准确是不够的。我的最终模型必须优于使用历史数据作为直接预测的表现。

+   训练集和测试集的划分将给我两个数据集，一个用于算法学习，另一个用于测试算法的表现。

+   季节性分解将帮助我大致了解数据的可预测性，通过尝试将年度总体趋势与季节性模式和噪声分离来实现。较小规模的噪声意味着更多的数据可以被算法捕捉到。

+   一系列统计测试以确定数据是否是平稳的。如果数据不是平稳的，我需要进行一次差分（运行时间差函数，其中每个时间间隔的数据仅显示与前一个时间间隔数据的差异。这将迫使数据变得平稳。）

+   制作一些SARIMA模型，利用自相关图的推断来确定移动平均项，利用部分自相关图的推断来确定自回归项。SARIMA是时间序列建模的首选，我将在尝试使用Auto Arima进行强力搜索之前，尝试ACF和PACF推断。

+   尝试使用Auto Arima，它将遍历许多项并选择最佳的项组合。我希望通过实验了解SARIMA模型的参数是否会带来更好的模型性能。

+   尝试ETS模型，利用季节性分解的推断来判断x是随时间的加性还是乘性。ETS模型比SARIMA模型更注重季节性和总体趋势，可能在捕捉南瓜香料与时间的关系时给我带来优势。

## 性能绘图关键绩效指标（KPIs）：

+   尝试使用MAPE评分，因为它在许多工作场所中是行业标准，大家可能已经习惯了。它易于理解。

+   [尝试使用RMSE评分，因为它更有用。](/forecast-kpi-rmse-mae-mape-bias-cdc5703d242d)

+   将预测结果与测试数据绘制在图表上，并通过视觉检查性能。

# 目视检查数据：

![](../Images/2d10979a1d55d85be291f50863337530.png)

图片由作者提供。

从上述图表中可以看出，这些数据展示了强大的季节性建模潜力。每年的下半年有明显的峰值，然后是一个渐弱的趋势，接着在降到基准线之前又有一个峰值。

然而，除了2021年外，每年的主要峰值都在增加，这也是合情合理的，因为在疫情期间，人们可能没有考虑庆祝季节。

# 导入：

注意：这些导入在笔记本中显示得有所不同，因为在笔记本中我依赖于`seasonal_mod.py`，其中包含了许多我的导入。

作者提供的图像。

这些是我用来制作代码笔记本的库。我选择了statsmodels而不是scikit-learn，因为statsmodels的时间序列包更符合我的需求，我对大多数线性回归问题更喜欢statsmodels。

# 基于函数的方法：

我不知道你是否一样，但我不想每次创建新模型时都写几行代码，然后再写更多代码来验证。因此，我制作了一些函数，以保持我的代码简洁并防止我出错。

作者提供的图像。

这三个小函数协同工作，所以我只需用`metrics_graph()`运行`y_true`和`y_preds`作为输入，它会给我一条真实数据的蓝线和一条预测数据的红线，以及MAPE和RMSE。这将节省我的时间和麻烦。

## 使用去年的数据作为成功的基准：

我的零售管理经验促使我尝试使用上周的数据和去年的数据作为对今年数据的直接预测。在零售中，我们通常使用上个季节（1个时间单位前）的数据作为直接预测，以确保例如黑色星期五期间的库存。上周的数据表现不如去年的数据。

![](../Images/5b7fd5c7e2f44c61a58c1c760c5a356f.png)

作者提供的图像。

上周的数据预测本周的数据显示MAPE得分略高于18，RMSE约为11\. 相比之下，去年的数据作为对今年数据的直接预测显示MAPE得分约为12，RMSE约为7。

![](../Images/55ff4865aa6ae49dcb370932e5fa31df.png)

作者提供的图像。

因此，我选择将我构建的所有统计模型与使用去年的数据的简单模型进行比较。这个模型在峰值和下降的时间上比我们的简单周模型更准确，然而，我仍然认为我可以做得更好。建模的下一步是进行季节性分解。

以下函数帮助我运行了季节性分解，我将把它作为可重用的代码用于未来所有的建模工作。

作者提供的图像。

下面展示了我如何使用这个季节性分解。

![](../Images/0181b2dcfdc242a221a18973b75acd4d.png)

作者提供的图像。

加法模型在残差中表现出每年的重复模式，这表明加法模型无法完全分解所有的重复模式。这是尝试乘法模型来处理年度峰值的一个很好的理由。

![](../Images/e300ec40e29fb24857e9c37100d00ae0.png)

作者提供的图像。

现在乘法分解中的残差更有希望。它们变得更随机且规模更小，证明了乘法模型能更好地捕捉数据。残差如此之小——在1.5到-1的范围内，这意味着建模具有很大的潜力。

但现在我想要一个专门运行SARIMA模型的函数，只输入顺序。我还想实验运行`c`、`t`和`ct`版本的SARIMA模型，因为季节性分解更倾向于乘法模型而非加法模型。通过在`trend =`参数中使用`c`、`t`和`ct`，我能够为SARIMA模型添加乘数。

图片由作者提供。

我将跳过描述我查看AFC和PACF图的部分以及我尝试PMD自动ARIMA以找到SARIMA模型中最佳项的部分。[如果你对这些细节感兴趣，请查看我的完整代码笔记本。](https://github.com/casanave/Pumpkin_Spice/blob/main/Pumpkin_Spice.ipynb)

## 我的最佳SARIMA模型：

![](../Images/6077d7a41020e81a621208f399979170.png)

图片由作者提供。

所以，我最好的SARIMA模型的MAPE分数比我的基准模型高，接近29对比12，但RMSE却低了大约一个单位，从接近7降到接近6。使用这个模型最大的问题是它对2023年高峰的预测严重不足，从2023年8月到9月，红线和蓝线之间的误差区域较大。根据你对RMSE和MAPE的看法，可能会有理由更喜欢它或不如年度基准模型。然而，我还没完。我最终的模型确实比我的年度基准模型好。

# 最终模型：

我使用了一个ETS（指数平滑）模型作为最终模型，这让我可以明确使用`seasonal`参数，使其采用乘法方法。

![](../Images/37f625785b389c03c959354c901ca918.png)

图片由作者提供。

现在你可能会想，“但这个模型的MAPE分数比年度基准模型高。” 你说得对，大约高了0.3%。不过，我认为这是一种非常公平的交换，因为我的RMSE从7降低到了大约4.5。虽然这个模型在2022年12月的表现比我最好的SARIMA模型差一点，但它在那个高峰期的误差比2023年秋季的更小，而我更关心的是后者。[你可以在这里找到那个模型。](https://github.com/casanave/Pumpkin_Spice/blob/main/ps_predictor.sav)

# 进一步验证：

我将等到2024年10月7日，再进行一次数据提取，看看模型在去年的数据上的表现如何。

# 结论：

总结一下，我能够证伪零假设，我的最终模型优于朴素的年度模型。我证明了 Google 上的南瓜香料受欢迎程度具有很强的季节性并且可以预测。在朴素、SARMA 模型和 ETS 模型之间，ETS 更好地捕捉了时间与南瓜香料受欢迎程度之间的关系。南瓜香料与时间的乘法关系表明，南瓜香料的受欢迎程度基于除了时间之外的一个或多个独立变量，表达式为 `time * unknown_independant_var = pumpkin_spice_popularity`。

## 我学到了什么以及未来的工作：

我的下一步是使用某种版本的 [Meta’s graph API](https://developers.facebook.com/docs/instagram-api/) 来查找“南瓜香料”在商业文章中的使用情况。我想知道这些数据与我的 Google 趋势数据的相关性如何。我还了解到，当季节性分解指向乘法模型时，我会在过程中更早地选择 ETS。

此外，我对自动化这整个过程很感兴趣。理想情况下，我希望构建一个 Python 模块，其中输入是直接来自 Google Trends 的 CSV 文件，输出可以是一个可用的模型，并且文档足够好，使非技术用户能够创建和测试他们自己的预测模型。对于用户选择难以预测的数据（例如，朴素或随机游走模型可能更适用）的可能性，我希望构建该模块以向用户解释这一点。然后，我可以利用该模块从应用程序中收集数据，以展示未测试数据中的季节性发现。

请关注明年南瓜香料季节的那个应用程序！

[1] Google Trends, N/A ([https://www.google.com/trends](http://www.google.com/trends))
