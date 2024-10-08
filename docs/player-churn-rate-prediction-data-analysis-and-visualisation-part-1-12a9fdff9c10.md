# 如何预测玩家流失，借助 ChatGPT 的一些帮助

> 原文：[`towardsdatascience.com/player-churn-rate-prediction-data-analysis-and-visualisation-part-1-12a9fdff9c10`](https://towardsdatascience.com/player-churn-rate-prediction-data-analysis-and-visualisation-part-1-12a9fdff9c10)

## 使用低代码机器学习平台的数据科学 | Actable AI

## 使用低代码平台分析数据和训练模型以预测玩家流失

[](https://medium.com/@chrisgalea?source=post_page-----12a9fdff9c10--------------------------------)![Christian Galea](https://medium.com/@chrisgalea?source=post_page-----12a9fdff9c10--------------------------------)[](https://towardsdatascience.com/?source=post_page-----12a9fdff9c10--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----12a9fdff9c10--------------------------------) [Christian Galea](https://medium.com/@chrisgalea?source=post_page-----12a9fdff9c10--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----12a9fdff9c10--------------------------------) ·阅读时长 22 分钟·2023 年 6 月 20 日

--

![](img/b1907bde077d48abe4a7127f8a720a6c.png)

图片由 [Tima Miroshnichenko 在 Pexels](https://www.pexels.com/photo/man-in-white-dress-shirt-with-eyeglasses-feeling-exhausted-7567442/) 提供

# 介绍

在游戏世界中，公司不仅致力于吸引玩家，还希望尽可能长时间地保留他们，特别是在依赖游戏内微交易的免费游戏中。这些微交易通常涉及购买游戏内货币，使玩家能够获取用于进阶或定制的物品，同时为游戏的开发提供资金。监控 *流失率* 是至关重要的，因为高流失率意味着收入显著损失，这反过来会导致开发者和管理者的压力增加。

本文探讨了基于从移动应用程序获取的数据的真实世界数据集的使用，特别关注用户所玩过的关卡。利用 *机器学习*，这已成为技术领域的一个重要部分，并构成了人工智能 (AI) 的基础，企业可以从他们的数据中提取有价值的洞察。

然而，构建机器学习模型通常需要编码和数据科学专业知识，这使得许多个人和缺乏资源的小公司无法接触到此技术，因为他们没有足够的资金来聘请数据科学家或强大的硬件来处理复杂的算法。

为了应对这些挑战，低代码和无代码机器学习平台应运而生，旨在简化机器学习和数据科学过程，从而减少对广泛编码知识的需求。这样的平台包括[Einblick](https://www.einblick.ai/)、[KNIME](https://www.knime.com/)、[Dataiku](https://www.dataiku.com/)、[Alteryx](https://www.alteryx.com/)和[Akkio](https://www.akkio.com/)。

本文使用一个低代码机器学习平台来训练一个能够预测用户是否会停止玩游戏的模型。此外，还探讨了结果解释和可以用来提高模型性能的技术。

本文的其余部分组织如下：

1.  平台

1.  数据集

1.  探索性数据分析

1.  训练分类模型

1.  提高模型性能

1.  创建新特征

1.  训练一个新的（希望得到改进的）分类模型

1.  生产中的模型部署

1.  结论

# 平台

完整披露——在撰写本文时，我是一名在[Actable AI](https://www.actable.ai/)工作的数据科学家，因此本文中将使用该平台。我还参与了 ML 库中新功能的实施和维护，因此我很好奇这个平台在实际问题中的表现。

该平台提供了一个 Web 应用程序，包含多个流行的机器学习方法，适用于传统的分类、回归和分割应用。还提供了一些不那么常见的工具，如时间序列预测、情感分析和因果推断。缺失的数据也可以被填补，数据集的统计信息可以被计算（例如特征之间的相关性、方差分析（ANOVA）等），同时数据可以通过条形图、直方图和词云等工具进行可视化。

还有一个[Google Sheets 插件](https://www.actable.ai/resources/gsheets-add-on)，可以在电子表格中直接进行分析和模型训练。不过，请注意，这个插件中可能不包含最新的功能。

![](img/2a0725ee07bdf56af89ca82dfffe0293.png)

[Actable AI 的 Google Sheets 插件](https://www.actable.ai/resources/gsheets-add-on)。图像由作者提供。

核心库是开源的，托管在[GitHub](https://github.com/Actable-AI/actableai-lib)上，由几个知名且值得信赖的框架组成，例如[AutoGluon](https://github.com/autogluon/autogluon)和[scikit-learn](https://scikit-learn.org/stable/)，这些框架也是开源且免费提供的。这与其他相关平台类似，这些平台也利用了现有的开源解决方案。

然而，这引出了一个问题：如果大多数工具已经可用且免费，那么你为何还要使用这些平台？

主要原因是这些工具需要编程语言如 Python 的知识，因此任何可能对编程不熟悉的人可能会发现难以或无法使用。因此，这些平台旨在以图形用户界面（GUI）的形式提供所有功能，而不是一组编程命令。

更有经验的专业人士也可能通过易于使用的图形界面节省时间，这种界面可能还会提供可用工具和技术的信息描述。一些平台还可能呈现你可能不熟悉的工具，或在处理数据时提供可能有用的警告（如[数据泄露](https://machinelearningmastery.com/data-leakage-machine-learning/)的存在，即模型访问了在生产环境中不可用的特征）。

使用这些平台的另一个原因是提供了运行模型的硬件。因此，无需购买和维护自己的计算机和组件，如图形处理单元（GPU）。

# 数据集

数据集由使用该平台的游戏公司提供，可以[在这里](https://app.actable.ai/r/ze61cu96uA3ZKApZgUCztA)查看，并附有[CC BY-SA-4 许可证](https://creativecommons.org/licenses/by-sa/4.0/)，允许在提供适当信用的情况下进行共享和改编。共有 789,879 行（样本），这相当可观，应有助于减少如模型[过拟合](https://en.wikipedia.org/wiki/Overfitting)等效果。

数据集包含了一个人在移动应用中玩过的每个等级的信息。例如，包含了游戏时间、玩家是否赢得了等级、等级编号等信息。

用户 ID 也已包括在内，但已被匿名化，以免透露原始玩家的身份。一些字段也已被删除。然而，这应该为检验本文章中考虑的 ML 平台提供的工具是否在预测玩家是否会流失方面有用提供了一个坚实的基础。

每个特征的意义如下：

+   `Churn`：如果玩家在两周以上没有玩游戏，则为‘1’，否则为‘0’

+   `ServerTime`：等级播放时服务器的时间戳

+   `EndType`：等级结束的原因（如果玩家赢得游戏则主要为‘Win’，如果玩家输掉游戏则为‘Lose’）

+   `LevelType`：等级类型

+   `Level`：等级编号

+   `SubLevel`：子等级编号

+   `Variant`：等级变体

+   `Levelversion`：等级版本

+   `NextCar`：未使用（包括在内是为了观察平台如何处理只有一个标签的特征，后文将讨论）

+   `AddMoves`：可用的额外移动

+   `DoubleMana`：未使用（包括在内是为了观察平台如何处理只有一个标签的特征，后文将讨论）

+   `StartMoves`: 关卡开始时可用的移动次数

+   `ExtraMoves`: 购买的额外移动次数

+   `UsedMoves`: 玩家使用的移动次数

+   `UsedChangeCar`: 未使用（包括在内是为了观察平台如何处理只有一个标签的特征，稍后将讨论）

+   `WatchedVideo`: 是否观看了视频，以获得额外的移动机会

+   `BuyMoreMoves`: 玩家购买更多移动的次数

+   `PlayTime`: 玩这个关卡的时间

+   `Scores`: 玩家获得的分数

+   `UsedCoins`: 在关卡中使用的总金币

+   `MaxLevel`: 玩家达到的最高关卡

+   `Platform`: 设备类型

+   `UserID`: 玩家 ID

+   `RollingLosses`: 玩家连续失败的次数

# 探索性数据分析

训练前的第一步是通过探索性数据分析（EDA）了解数据。EDA 是一种数据分析方法，涉及总结、可视化和理解数据集的主要特征。其目标是深入了解数据，识别任何模式、趋势、异常或存在的问题（例如缺失值），这些都可以帮助指导所使用的特征和模型。

让我们首先查看关卡结束的主要原因：

![](img/576f1a4434eb9416e890a1b7dc9843fa.png)

数据集中某些列的统计信息。图片来源：作者。

在上图中，我们可以看到关卡结束的主要原因（由 `EndType` 表示）是玩家输掉了游戏（63.6%），而获胜的玩家占 35.2%。我们还可以看到，`UsedChangeCar` 列似乎没有用，因为它在所有行中包含相同的值。

一个非常重要的观察是，我们的目标值高度不平衡，在前 10,000 行中只有 63 个样本（即 0.6%的数据）具有流失值 1（即玩家已流失）。这一点需要记住，因为我们的模型可能很容易被偏向仅预测 `Churn` 的值为 0。原因是模型可以在一些指标上取得非常好的值；在这种情况下，如果使用一个简单的模型选择最常见的类别，它将 99.4%的时间是正确的！我邀请你阅读 Baptiste Rocca 和[Jason Brownlee](https://machinelearningmastery.com/failure-of-accuracy-for-imbalanced-class-distributions/)的两篇精彩文章，以了解更多相关信息。

不幸的是，Actable AI 目前尚不提供处理不平衡数据的方法，例如通过[合成少数类过采样技术 (SMOTE)](https://machinelearningmastery.com/smote-oversampling-for-imbalanced-classification/)或使用类权重或不同的采样策略。这意味着在选择优化指标时需要特别小心。如上所述，准确率可能不是最佳选择，因为即使某一类样本从未被正确标记，高准确率也可以实现。

另一种有用的分析方法是特征之间的相关性，特别是预测特征与目标特征之间的相关性。这可以使用‘相关性分析’工具完成，结果可以直接在 Actable AI 平台上 [查看](https://app.actable.ai/r/QZ-BLTgqb8ggJIiRVh3zpQ)：

![](img/f2eab9f0bb3310d5a62c3bd42cf9738f.png)

正/负图表。图片由作者提供。

在上面的图表中，蓝色条形图表示当值为 1 时，特征与 `Churn` 的正相关，而橙色条形图表示负相关。需要注意的是，相关性介于 -1 和 1 之间，其中正值表示两个特征趋向于相同方向变化（例如，两者都增加或都减少），而负相关仅表示一个特征增加或减少时，另一个特征则相反。因此，相关性的大小（忽略负号）可能是最重要的注意点。

有一些结论，例如，失去一关的玩家更容易流失（最上面的蓝色条形图），而赢得一关的玩家往往继续游戏（第三个橙色条形图）。但是，也需要注意的是，这些值相当低，表明这些特征与目标的相关性较弱。这意味着可能需要进行 *特征工程*，通过现有特征创建新的特征，以捕捉更显著的信息，从而使模型能够进行更准确的预测。特征工程将在本文后续部分详细讨论。

然而，在创建新特征之前，值得查看仅使用数据集中原始特征可以实现什么样的性能。因此，下一步可能会更令人兴奋——训练一个模型以查看可以获得什么样的性能。

# 训练分类模型

由于我们希望预测用户是否会停止游戏，这是一个 *分类* 问题，需要选择多个标签中的一个。在我们的情况下，问题涉及分配两个标签中的一个（‘1’对应于‘Churn’，‘0’对应于‘No Churn’），这进一步使其成为一个 *二分类* 问题。

这个过程主要通过 [AutoGluon](https://github.com/autogluon/autogluon) 库完成，该库自动训练多个模型，然后选择性能最佳的模型。这避免了手动训练单个模型并比较其性能的麻烦。

在 Actable AI 平台上需要设置多个参数，以下是我的选择：

![](img/18e2c7d418273b8712c517c9fce3bd00.png)

在 Actable AI 网络应用中选择的选项。图片由作者提供。

也可以选择用于模型优化的指标。我使用了[接收者操作特征曲线下的面积 (AUC ROC) ](https://developers.google.com/machine-learning/glossary#auc-area-under-the-roc-curve)，因为它对之前讨论的类别不平衡问题的敏感性要小得多。数值范围从 0 到 1（后者为完美分数）。

一段时间后，结果被生成并显示，也可以在[这里](https://app.actable.ai/r/C3FLuD7jpJY2NaV2VLdKDg)查看。计算了一些不同的指标，这不仅是良好的实践，而且如果我们真正想了解我们的模型的话，几乎是必要的，因为每个指标关注模型性能的某些方面。

显示的第一个指标是优化指标，其值为 0.675：

![](img/e4ea302c903c1803fca04dcbc95536be.png)

评估指标。图片由作者提供。

这不是很好，但请记住，在我们的 EDA 过程中，特征与目标的相关性相当弱，因此性能不佳并不令人惊讶。

这一结果还突出了理解结果的重要性；我们通常会对 0.997（即 99.7%）的准确率感到非常满意。然而，这主要是由于数据集的不平衡性质，如前所述，因此不应过于重视。同时，像精确度和召回率这样的分数默认基于 0.5 的阈值，这可能不是最适合我们应用的阈值。

[ROC](https://developers.google.com/machine-learning/glossary#roc-receiver-operating-characteristic-curve)和[精确度-召回率曲线](https://machinelearningmastery.com/roc-curves-and-precision-recall-curves-for-classification-in-python/)也显示出来，这再次清晰地表明性能略显不足：

![](img/bc830ba4048cf3377a7acf9e78b53c53.png)![](img/f460be3a37ecf272e79f0ccdd8038ba6.png)

训练模型的 ROC 曲线（左）和精确度-召回率曲线（右）。图片由作者提供。

这些曲线还对确定我们在最终应用中可以使用的阈值非常有用。例如，如果希望最小化假阳性数量，则可以选择一个模型获得更高精确度的阈值，并检查相应的召回率。

最佳模型获得的每个特征的重要性也可以查看，这可能是更有趣的结果之一。这是使用[AutoGluon 的排列重要性](https://auto.gluon.ai/stable/api/autogluon.tabular.TabularPredictor.feature_importance.html)计算的。[P 值](https://en.wikipedia.org/wiki/P-value)也显示出来，以确定结果的可靠性：

![](img/c3d1e484aadbd41848dfe25df6bce665.png)

特征重要性表。图片由作者提供。

也许并不意外，最重要的特征是`EndType`（显示导致等级结束的原因，如胜利或失败），其次是`MaxLevel`（用户玩过的最高等级，较高的数字表示玩家在游戏中非常活跃）。

另一方面，`UsedMoves`（玩家执行的动作次数）几乎没有用，而`StartMoves`（玩家可用的动作次数）实际上可能会损害性能。这也很有道理，因为单独的动作次数和可用动作次数并没有提供太多信息；它们之间的比较可能会更加有用。

我们还可以查看每个类别（在这种情况下是 1 或 0）的估计概率，这些概率用于推导预测类别（默认情况下，概率最高的类别被指定为预测类别）：

![](img/cddb98bd5cec2315e136b03ddb02abe1.png)

包含原始值、Shapley 值和预测值的表格。图片来源于作者。

[可解释的 AI](https://cloud.google.com/explainable-ai#:~:text=Explainable%20AI%20is%20a%20set,others%20understand%20your%20models'%20behavior.) 正变得越来越重要，以理解模型行为，这也是像[Shapley 值](https://docs.actable.ai/glossary.html#shapley-values)这样的工具越来越受欢迎的原因。这些值表示特征对预测类别概率的贡献。例如，在第一行中，我们可以看到 `RollingLosses` 值为 36 会降低该玩家的预测类别（类别 0，即该人将继续玩游戏）的概率。

相反，这意味着其他类别（类别 1，即玩家流失的概率）增加了。这是有道理的，因为较高的`RollingLosses`值表示玩家连续失去了很多等级，因此更可能由于挫折而停止玩游戏。另一方面，较低的`RollingLosses`值通常会提高负类别的概率（即玩家不会停止玩游戏）。

如前所述，训练并评估了多个模型，然后选择最佳模型。值得注意的是，在这种情况下，最佳模型是[LightGBM](https://en.wikipedia.org/wiki/LightGBM)，它也是最快的之一。

![](img/31ff24f446b7cef2af787163f068bb36.png)

关于训练模型的信息。图片来源于作者。

# 提高模型性能

现在，我们可以尝试提高模型性能。也许最简单的方法之一是选择“优化质量”选项，看看我们能走多远。这个选项配置了几个通常能提高性能的参数，但可能会导致训练时间变慢。以下是获得的结果（你也可以[在这里](https://app.actable.ai/r/ze61cu96uA3ZKApZgUCztA)查看）：

![](img/e37ee8499c3922291c91c72cc3931a29.png)

使用“优化质量”选项时的评估指标。图像由作者提供。

再次关注 ROC AUC 指标，性能从 0.675 提高到 0.709。这对于如此简单的更改来说是相当不错的提升，尽管仍远未理想。我们还能做些什么来进一步提高性能？

# 创建新特征

如前所述，我们可以通过*特征工程*来做到这一点。这涉及从现有特征中创建新特征，这些新特征能够捕捉更强的模式，并与待预测变量有更高的相关性。

在我们的例子中，数据集中特征的范围相当狭窄，因为这些值只涉及到一个记录（即用户玩过的一个关卡的信息）。因此，通过总结一段时间内的记录，获得更全球性的视角可能会非常有用。这样，模型将了解用户的历史趋势。

例如，我们可以确定玩家使用了多少额外的动作，从而提供一个难度的衡量指标；如果需要的额外动作很少，那么关卡可能太简单；另一方面，较高的数字可能意味着关卡太难。

还可以通过检查玩家在过去几天内的游戏时间，来检查用户是否沉浸并投入于游戏。如果玩家玩得很少，这可能意味着他们失去了兴趣，可能很快就会停止游戏。

有用的特征在不同领域之间有所不同，因此重要的是要尝试找到与当前任务相关的信息。例如，你可以查找和阅读研究论文、案例研究和文章，或寻求在该领域有经验的公司或专业人士的建议，他们对最常见的特征、它们之间的关系、潜在的陷阱以及最有可能有用的新特征都有深入了解。这些方法有助于减少试错过程，加快特征工程过程。

考虑到近期在大型语言模型（LLMs）方面的进展（例如，你可能听说过[ChatGPT](https://chat.openai.com/)……），以及考虑到特征工程对经验不足的用户可能有些令人生畏，我很好奇 LLMs 是否可以在提供创建特征的想法上有所帮助。我正是这样做的，结果如下：

![](img/3293709148c6044e8bb2af656d7b9f17.png)

当询问关于如何创建新特征以更准确地预测玩家流失时，ChatGPT 的回答实际上是非常有用的。图像由作者提供。

ChatGPT 的回答实际上相当不错，并且也指出了上述讨论的许多基于时间的特征。当然，请记住，如果所需的信息不可用，我们可能无法实现所有建议的功能。此外，众所周知，它[容易产生幻觉](https://spectrum.ieee.org/ai-hallucination)，因此可能无法提供完全准确的答案。

我们可以从 ChatGPT 获得更相关的回答，例如通过指定我们正在使用的功能或使用[提示](https://github.com/f/awesome-chatgpt-prompts)，但这超出了本文的范围，留给读者作为练习。不过，LLMs 可以被视为一个起始步骤，尽管仍强烈建议从论文、专业人士等处寻求更可靠的信息。

在 Actable AI 平台上，可以使用相当知名的[SQL](https://en.wikipedia.org/wiki/SQL)编程语言创建新功能。对于不太熟悉 SQL 的用户，利用[ChatGPT](https://chat.openai.com/)自动生成查询的方法可能会很有帮助。然而，根据我的有限实验，这种方法的可靠性可能会有些不稳定。

为确保输出结果的准确计算，建议手动检查部分结果以验证是否正确计算了预期的输出。这可以通过在 SQL Lab 中运行查询后查看显示的表格轻松完成，SQL Lab 是 Actable AI 用来编写和运行 SQL 代码的界面。

以下是我用来生成新列的 SQL 代码，如果你想创建其他功能，这些代码可以帮助你入门：

```py
SELECT 
    *,
    SUM("PlayTime") OVER UserLevelWindow AS "time_spent_on_level",
    (a."Max_Level" - a."Min_Level") AS "levels_completed_in_last_7_days",
    COALESCE(CAST("total_wins_in_last_14_days" AS DECIMAL)/NULLIF("total_losses_in_last_14_days", 0), 0.0) AS "win_to_lose_ratio_in_last_14_days",
    COALESCE(SUM("UsedCoins") OVER User1DayWindow, 0) AS "UsedCoins_in_last_1_days",
    COALESCE(SUM("UsedCoins") OVER User7DayWindow, 0) AS "UsedCoins_in_last_7_days",
    COALESCE(SUM("UsedCoins") OVER User14DayWindow, 0) AS "UsedCoins_in_last_14_days",
    COALESCE(SUM("ExtraMoves") OVER User1DayWindow, 0) AS "ExtraMoves_in_last_1_days",
    COALESCE(SUM("ExtraMoves") OVER User7DayWindow, 0) AS "ExtraMoves_in_last_7_days",
    COALESCE(SUM("ExtraMoves") OVER User14DayWindow, 0) AS "ExtraMoves_in_last_14_days",
    AVG("RollingLosses") OVER User7DayWindow AS "RollingLosses_mean_last_7_days",
    AVG("MaxLevel") OVER PastWindow AS "MaxLevel_mean"
FROM (
    SELECT
        *,
        MAX("Level") OVER User7DayWindow AS "Max_Level",
        MIN("Level") OVER User7DayWindow AS "Min_Level",
        SUM(CASE WHEN "EndType" = 'Lose' THEN 1 ELSE 0 END) OVER User14DayWindow AS "total_losses_in_last_14_days",
        SUM(CASE WHEN "EndType" = 'Win' THEN 1 ELSE 0 END) OVER User14DayWindow AS "total_wins_in_last_14_days",
        SUM("PlayTime") OVER User7DayWindow AS "PlayTime_cumul_7_days",
        SUM("RollingLosses") OVER User7DayWindow AS "RollingLosses_cumul_7_days",
        SUM("PlayTime") OVER UserPastWindow AS "PlayTime_cumul"
    FROM "game_data_levels"
    WINDOW
        User7DayWindow AS (
            PARTITION BY "UserID"
            ORDER BY "ServerTime"
            RANGE BETWEEN INTERVAL '7' DAY PRECEDING AND CURRENT ROW
        ),
        User14DayWindow AS (
            PARTITION BY "UserID"
            ORDER BY "ServerTime"
            RANGE BETWEEN INTERVAL '14' DAY PRECEDING AND CURRENT ROW
        ),
        UserPastWindow AS (
        PARTITION BY "UserID"
        ORDER BY "ServerTime"
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
        )
) AS a
WINDOW
    UserLevelWindow AS (
        PARTITION BY "UserID", "Level"
        ORDER BY "ServerTime"
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ),
    PastWindow AS (
        ORDER BY "ServerTime"
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ),
    User1DayWindow AS (
        PARTITION BY "UserID" 
        ORDER BY "ServerTime" 
        RANGE BETWEEN INTERVAL '1' DAY PRECEDING AND CURRENT ROW
    ),
    User7DayWindow AS (
        PARTITION BY "UserID"
        ORDER BY "ServerTime"
        RANGE BETWEEN INTERVAL '7' DAY PRECEDING AND CURRENT ROW
    ),
    User14DayWindow AS (
        PARTITION BY "UserID"
        ORDER BY "ServerTime"
        RANGE BETWEEN INTERVAL '14' DAY PRECEDING AND CURRENT ROW
    )
ORDER BY "ServerTime";
```

在这段代码中，“窗口”被创建以定义需要考虑的时间范围，如过去一天、过去一周或过去两周。落在该范围内的记录将用于功能计算，这主要是为了提供一些关于玩家在游戏中的历程的历史背景。功能的完整列表如下：

+   `time_spend_on_level`：用户在某一关卡上花费的时间。这反映了关卡的难度。

+   `levels_completed_in_last_7_days`：用户在过去 7 天内完成的关卡数量（1 周）。这反映了关卡的难度、毅力和游戏沉浸感。

+   `total_wins_in_last_14_days`：用户在过去 14 天内赢得关卡的总次数

+   `total_losses_in_last_14_days`：用户在过去 14 天内失败的关卡总次数

+   `win_to_lose_ratio_in_last_14_days`：过去 14 天内获胜次数与失败次数的比例（`total_wins_in_last_14_days/total_losses_in_last_14_days`）

+   `UsedCoins_in_last_1_days`：前一天使用的金币数量。这反映了游戏难度水平和玩家花费游戏货币的意愿。

+   `UsedCoins_in_last_7_days`：过去 7 天内使用的金币数量（1 周）

+   `UsedCoins_in_last_14_days`：过去 14 天（2 周）内使用的硬币数量

+   `ExtraMoves_in_last_1_days`：用户在前一天内使用的额外移动次数。表示了等级的困难程度。

+   `ExtraMoves_in_last_7_days`：用户在过去 7 天（1 周）内使用的额外移动次数

+   `ExtraMoves_in_last_14_days`：用户在过去 14 天（2 周）内使用的额外移动次数

+   `RollingLosses_mean_last_7_days`：用户在过去 7 天（1 周）内的累计损失平均数。表示了等级的困难程度。

+   `MaxLevel_mean`：所有用户达到的最高等级的平均值。

+   `Max_Level`：玩家在过去 7 天（1 周）内达到的最高等级。结合`MaxLevel_mean`，可以指示玩家相对于其他玩家的进展。

+   `Min_Level`：用户在过去 7 天（1 周）内玩的最低等级

+   `PlayTime_cumul_7_days`：用户在过去 7 天（1 周）内玩的总时间。表示玩家在游戏中的沉浸度。

+   `PlayTime_cumul`：用户玩的总时间（自第一个可用记录以来）

+   `RollingLosses_cumul_7_days`：过去 7 天（1 周）内的累计滚动损失总数。这表示了困难程度。

计算某一行新特征的值时，重要的是只使用过去的记录。换句话说，必须避免使用未来的观测数据，因为在生产环境中模型显然无法访问任何未来的值。

一旦对创建的特征感到满意，我们可以将表格保存为新的数据集，并运行一个新的模型，应该（希望）达到更好的性能。

# 训练一个新的（希望有所改进的）分类模型

现在是时候看看新列是否有用。我们可以重复之前的步骤，唯一的不同是现在使用包含额外特征的新数据集。使用相同的设置以确保与原始模型的公平比较，结果如下（也可以在[这里](https://app.actable.ai/r/GDfT0fXaMmYO-5cmt-XxZA)查看）：

![](img/cc9aff0dd7f3d14e64986ab9a92e2925.png)

使用新列的评估指标。图像由作者提供。

ROC AUC 值为 0.918，相比原始值 0.675 有了显著改善。它甚至比为质量优化的模型（0.709）还要好！这证明了理解数据和创建能够提供更丰富信息的新特征的重要性。

现在，了解哪些新特征实际最有用会很有趣；我们可以再次查看特征重要性表：

![](img/16410615270256f5f7c7ca1aa6a8826b.png)

新模型的特征重要性表。图像由作者提供。

看起来过去两周的总损失数相当重要，这很有意义，因为一个玩家输掉游戏的次数越多，他们变得沮丧并停止游戏的可能性就越大。

所有用户的平均最大水平似乎也很重要，这再次是有意义的，因为它可以用来确定一个玩家与大多数其他玩家的差距——远高于平均水平表明一个玩家对游戏非常投入，而远低于平均水平的值可能表明玩家的动机还不够强烈。

这些只是我们可以创建的一些简单特性。还有其他特性可以创建，可能进一步提高性能。我将把这留给读者作为练习，看看还可以创建哪些其他特性。

在相同时间限制下，训练一个优化质量的模型并没有提高性能。然而，这也许可以理解，因为使用的特性更多，因此可能需要更多时间进行优化。如[这里](https://app.actable.ai/r/G8frdBHCjbl9twVSTKuP8w)所示，将时间限制增加到 6 小时确实将性能提升到 0.923（以 AUC 为标准）：

![](img/beaf8b4e003c1323f94db9f1c4fad341.png)

使用新特性并优化质量时的评估指标结果。图像由作者提供。

还应该注意到一些指标，如精准率和召回率，仍然相当差。这是因为假设了 0.5 的分类阈值，而这可能并非最佳。尽管可以通过点击曲线来更改阈值，AUC 是阈值无关的，可以提供更全面的性能图像。如前所述，AUC 在训练不平衡数据集时作为优化指标尤为有用，如此处所示。

经过训练的模型的 AUC 性能总结如下：

```py
┌─────────────────────────────────────────────────────────┬───────────┐
│                         Model                           │ AUC (ROC) │
├─────────────────────────────────────────────────────────┼───────────┤
│ [Original features](https://app.actable.ai/r/C3FLuD7jpJY2NaV2VLdKDg)                                       │     0.675 │
│ [Original features + optim. for quality](https://app.actable.ai/r/ze61cu96uA3ZKApZgUCztA)                  │     0.709 │
│ [Engineered features](https://app.actable.ai/r/GDfT0fXaMmYO-5cmt-XxZA)                                     │     0.918 │
│ [Engineered features + optim. for quality + longer time](https://app.actable.ai/r/G8frdBHCjbl9twVSTKuP8w)  │     0.923 │
└─────────────────────────────────────────────────────────┴───────────┘
```

# 生产环境中的模型部署

如果我们不能在新数据上实际使用一个好的模型，那么它也没有什么用处。机器学习平台可能提供在训练模型的基础上生成未来未见数据预测的能力。例如，Actable AI 平台允许使用一个[API](https://en.wikipedia.org/wiki/API)，使得模型可以在平台外的数据上使用，比如导出模型或插入原始值以获得即时预测。

然而，定期在未来数据上测试模型是至关重要的，以确定其是否仍按预期运行。确实，可能需要用更新的数据重新训练模型。这是因为特征（例如特征分布）可能随时间变化，从而影响模型的准确性。

例如，一家公司可能会引入一项新政策，从而影响客户行为（无论是积极的还是消极的），但如果模型无法访问反映新变化的特征，则可能无法考虑到这一新政策。如果出现这种剧烈变化但没有可以告知模型的特征，那么可以考虑使用两个模型：一个用于训练和处理旧数据，另一个用于训练和处理新数据。这将确保模型专门适用于具有不同特征的数据，而这些特征可能很难被单一模型捕捉。

# 结论

在本文中，使用了包含用户在移动应用中每个级别信息的真实数据集，训练了一个分类模型，该模型可以预测玩家是否会在两周内停止玩游戏。

整个处理流程都被考虑在内，从探索性数据分析（EDA）到模型训练，再到特征工程。提供了关于结果解释和如何改进的讨论，将值从 0.675 提高到 0.923（其中 1.0 为最大值）。

新创建的特征相对简单，当然还存在许多可以考虑的特征。此外，还可以考虑像特征归一化和标准化这样的技术。一些有用的资源可以在这里和[这里](https://machinelearningmastery.com/how-to-improve-neural-network-stability-and-modeling-performance-with-data-scaling/)找到。

关于 Actable AI 平台，当然我可能有些偏见，但我确实认为它有助于简化一些数据科学家和机器学习专家需要完成的繁琐过程，具备以下期望的方面：

+   核心 ML 库是开源的，因此任何具有良好编程知识的人都可以验证其安全性。任何了解 Python 的人也可以使用它。

+   对于那些不了解 Python 或不熟悉编程的人来说，图形用户界面（GUI）提供了一种使用多种分析和可视化的方式，操作起来相对简单。

+   开始使用该平台并不困难（它不会用过多的技术信息让用户感到困惑，从而劝退知识较少的人）。

+   免费层允许对公开的数据集进行分析

+   提供了大量工具（除了本文讨论的分类问题之外）

尽管如此，仍然存在一些缺点，并且有几个方面可以改进，例如：

+   免费层不允许在私有数据上运行 ML 模型

+   用户界面看起来有些过时

+   一些可视化可能不够清晰，有时难以解释。

+   应用程序有时响应较慢

+   不支持不平衡数据

+   仍然需要一些数据科学和机器学习的知识才能从平台中获得最大的收益（虽然其他平台可能也是如此）

在未来的文章中，我将考虑使用其他平台，以确定它们的优缺点，从而确定哪些使用场景最适合每个平台。

直到那时，希望这篇文章对你来说是一次有趣的阅读！请随时留下你的反馈或问题！

*你对这篇文章有什么想法吗？请随时在* [*LinkedIn*](https://www.linkedin.com/in/cgalea) *上发留言、评论，或直接给我发消息！*

*此外，请确保* [***关注***](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa9be78db0c9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplayer-churn-rate-prediction-data-analysis-and-visualisation-part-1-12a9fdff9c10&user=Christian+Galea&userId=a9be78db0c9b) *我，以确保在未来文章发布时能够及时收到通知。*

![](img/9b800d9a965cfbb1a33f23e4d154787b.png)

图片来源于 [Pixabay](https://www.pexels.com/photo/application-blur-business-code-270408/)

*作者在撰写本文时是 Actable AI 的数据科学家。*
