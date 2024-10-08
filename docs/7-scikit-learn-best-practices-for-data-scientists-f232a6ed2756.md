# 7 条数据科学家的 Scikit-Learn 最佳实践

> 原文：[`towardsdatascience.com/7-scikit-learn-best-practices-for-data-scientists-f232a6ed2756`](https://towardsdatascience.com/7-scikit-learn-best-practices-for-data-scientists-f232a6ed2756)

## 充分利用此机器学习包的技巧

[](https://medium.com/@aashishnair?source=post_page-----f232a6ed2756--------------------------------)![Aashish Nair](https://medium.com/@aashishnair?source=post_page-----f232a6ed2756--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f232a6ed2756--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f232a6ed2756--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----f232a6ed2756--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f232a6ed2756--------------------------------) ·阅读时长 5 分钟·2023 年 1 月 10 日

--

![](img/0ac004ef554c3720bab10cad7e2a3756.png)

图片由 [John Schnobrich](https://unsplash.com/@johnschno?utm_source=medium&utm_medium=referral) 拍摄，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Scikit Learn 是机器学习领域的常用库，很容易理解原因。这个包由简单但有效的工具组成，并配有非常详尽的文档。

然而，尽管它易于使用，但如果不遵循某些实践，容易犯错，尤其是对于初学者而言。我自己也常常在看到我以前工作中使用该模块时的明显错误时想要捂脸。

最终，即使你严格遵循文档，也很容易错误地遗漏某些关键特性或做出次优决策。

因此，我借鉴了过去的经验，深入探讨了 7 条 Scikit Learn 最佳实践，以有效进行预测数据分析。

## 1\. 使用 Scikit Learn（而不是 Pandas）进行特征工程

Scikit Learn 专为机器学习任务设计，这当然包括特征工程。然而，有些人习惯使用 Pandas 进行某些操作（例如，独热编码），因为这是大多数人最先学习的包。

虽然 Pandas 库在进行探索性数据分析方面表现优秀，但在机器学习领域中，它无法与 Scikit Learn 相比。

Scikit Learn 中的变换器是为机器学习应用设计的。它们可以高效地准备训练集和测试集，同时避免数据泄露（如果操作得当）。

将 Pandas 函数硬塞到与其他 Scikit Learn 工具的数据管道中，必然会导致低效的流程，容易出错。

更好的做法是主要依赖 Scikit Learn 进行与特征工程相关的操作。

## 2\. 在分类任务中使用分层拆分

当感兴趣的数据表现出数据不平衡时，分类任务可能会具有挑战性，即一个或多个类别被低估。

幸运的是，通过分层抽样，用户可以在原始数据的每个子集中保持所有类别的存在。

在将数据集拆分为训练集和测试集时，用户可以使用 `stratify` 参数。

在将训练数据拆分为多个折叠进行交叉验证时，用户可以使用 [StratifiedKFold](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.StratifiedKFold.html) 类。

## 3\. 使用 n_jobs 参数加速超参数调整

超参数调整可能是数据建模阶段中最耗时的部分之一。逐个评估多个超参数组合自然是一个缓慢的过程。

幸运的是，用户可以通过利用 `n_jobs` 参数来加速超参数调整方法，如网格搜索和随机搜索，该参数决定了并行运行的作业数量。默认情况下，`n_jobs` 值设置为 1，但用户可以通过将 `n_jobs` 设置为 -1 来更快地获得结果，这样可以利用所有可用的处理器并行运行作业。

## 4\. 分配 random_state 值以获得可重复的结果

一些特征工程过程和机器学习算法包含随机性。然而，利用纯随机性的程序将无法重现其结果，这使得进行实验变得困难。

用户可以通过为随机数生成器设置种子来获得可重复的结果。对于 Scikit Learn 工具，可以通过在适用时将值分配给 `random_state` 参数来完成。这确保了程序将产生可重复的结果。

用户在执行如下操作时可以设置 `random_state` 值：

+   将数据集拆分为训练集和测试集

+   配置机器学习分类器对象

+   超参数调整

> 注意：分配给 `random_state` 参数的数字并不重要，只要在实验过程中不改变它。

## 5\. 在超参数调整中指定评分参数

超参数调整方法通过不同的超参数组合评估模型。这种技术的目的是识别出能够产生最佳性能的超参数。

然而，当模型使用错误的指标进行评估时，如何能保证其表现良好？当你在 Scikit Learn 模块的 GridSearchCV 和 RandomizedSearchCV 对象中使用 `scoring` 参数的默认值时，这是一种非常可能的结果。

默认情况下，网格搜索和随机搜索通过准确性评估分类模型的超参数。不幸的是，这很少是机器学习应用的合适指标。

为了避免这种情况，请确定最适合目标模型的评估指标，并将其分配给评分参数。可以在包的[文档](https://scikit-learn.org/stable/modules/model_evaluation.html)中找到可用指标的列表。

如果提供的指标都不合适，也可以使用 [make_scorer](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.make_scorer.html) 函数创建自定义指标。当用户偏好某种错误类型时，这是一个有用的功能。

## 6\. 使用管道转换数据

刚开始使用 Scikit Learn 的初学者可能习惯一次进行一个变换。这种方法需要在训练集和测试集上多次使用 `fit` 和 `transform` 方法。

以这种方式转换数据需要多行代码，并且容易出现错误（例如，在测试集上使用 `fit`）。因此，你会很高兴知道，Scikit Learn 提供了一个工具，可以更轻松地完成这些操作：管道。

Scikit Learn 管道是一个工具，它将一系列变换和估算器串联在一起，使用户能够执行更易于编写、阅读和调试的操作。

我始终倡导使用管道，并将在此再次提及；它们实在太好用了。

例如，我们可以使用管道对象来执行之前的操作：

与之前的代码片段相比，这里的代码更加可读，容易理解工作流程中的所有步骤。

此外，管道对象中的所有变换和建模都可以通过一个 `fit` 方法在训练集上执行。此外，相同的变换也可以在生成测试集预测之前通过一个 `predict` 方法应用。

## 7\. 熟悉与 Scikit Learn 兼容的其他包

最终，Scikit Learn 包的广泛工具范围无法涵盖所有可能的情况。

因此，熟悉与 Scikit Learn 兼容的其他包是值得的。这些包包含可以与 Scikit Learn 一起用于特征工程和数据建模的工具。

两个值得注意的例子是 [feature_engine](https://feature-engine.readthedocs.io/en/latest/) 和 [XGBoost](https://xgboost.readthedocs.io/en/stable/python/index.html) 包，它们拥有自己独特的变换器和机器学习算法，可以与其他 Scikit Learn 工具一起使用。

## 结论

![](img/5072bf85b045aebe810284adb869eee1.png)

照片由 [Prateek Katyal](https://unsplash.com/@prateekkatyal?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Scikit Learn 包具有巨大的实用性，可以用于解决各种机器学习问题。

然而，用户必须学习遵循某些最佳实践，以从包中获得最大收益。

如果你发现这个简短的概述有帮助，你也可以考虑阅读这些文章：

[](/k-fold-cross-validation-are-you-doing-it-right-e98cdf3e6690?source=post_page-----f232a6ed2756--------------------------------) ## K-Fold 交叉验证：你做对了吗？

### 讨论在数据集上执行 k-fold 交叉验证的正确（和不正确）方法

towardsdatascience.com [](/why-you-should-use-scikit-learn-pipelines-8754b4d1e375?source=post_page-----f232a6ed2756--------------------------------) ## 为什么你应该使用 Scikit-Learn 管道

### 这个工具将你的代码提升到一个新水平

towardsdatascience.com [](/harnessing-randomness-in-machine-learning-59e26e82fdfc?source=post_page-----f232a6ed2756--------------------------------) ## 在机器学习中利用随机性

### “随机”应该有多随机？

towardsdatascience.com

祝你在数据科学的探索中好运！
