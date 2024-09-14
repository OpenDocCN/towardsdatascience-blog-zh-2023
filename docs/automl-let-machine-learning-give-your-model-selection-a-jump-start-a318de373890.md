# AutoML — 让机器学习为您的模型选择加速

> 原文：[`towardsdatascience.com/automl-let-machine-learning-give-your-model-selection-a-jump-start-a318de373890`](https://towardsdatascience.com/automl-let-machine-learning-give-your-model-selection-a-jump-start-a318de373890)

## 利用 AutoML 提高生产力

[](https://medium.com/@fmnobar?source=post_page-----a318de373890--------------------------------)![Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----a318de373890--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a318de373890--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a318de373890--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----a318de373890--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a318de373890--------------------------------) ·6 分钟阅读·2023 年 2 月 13 日

--

![](img/70f880341c0148024182fe0ad6b008b3.png)

人与机器，图由 [DALL.E 2](https://openai.com/dall-e-2/)

我们每天使用机器学习（ML）来寻找问题的解决方案并做出预测，这通常包括通过探索性分析了解数据，随后进行数据清理，根据我们的最佳判断决定使用哪些 ML 模型来解决问题，然后进行超参数优化和迭代。但如果我们可以使用 ML 来解决所有这些步骤的更高级问题，甚至选择最佳模型，而不是我们手动完成这些重复且繁琐的步骤呢？AutoML 来满足这一需求！

在这篇文章中，我将展示如何仅用 3 行代码，AutoML 在不到 14 秒的时间内超越了我个人开发的预测 ML 模型（用于之前的文章）。

本文的目标不是提议我们不再需要科学家和机器学习从业者，因为我们有了 AutoML，而是要展示如何利用 AutoML 使我们的模型选择过程更高效，从而提高整体生产力。一旦 AutoML 为我们提供了各种机器学习模型家族的性能比较，我们可以继续任务并进一步微调模型以获得更好的结果。

让我们开始吧！

*(除非另有说明，否则所有图像均由作者提供。)*

[](https://medium.com/@fmnobar/membership?source=post_page-----a318de373890--------------------------------) [## 通过我的推荐链接加入 Medium - Farzad Mahmoodinobar

### 阅读 Farzad（以及 Medium 上的其他作者）每一篇文章。您的会员费用直接支持 Farzad 和其他…

[medium.com](https://medium.com/@fmnobar/membership?source=post_page-----a318de373890--------------------------------)

# 什么是 AutoML？

-   自动化机器学习或 AutoML 是自动化数据清理、模型选择、训练、超参数优化，甚至有时模型部署的机器学习工作流的过程。AutoML 最初旨在使机器学习对非技术用户更易于访问，并且随着时间的推移，它已发展成一个即使对于经验丰富的机器学习从业者也可靠的生产力工具。

-   现在我们了解了 AutoML 是什么，让我们继续看看它的实际应用。

# -   实施

-   我们将首先快速实施 AutoML，使用[AutoGluon](https://auto.gluon.ai/stable/index.html)，然后将结果与我在关于[线性回归](https://medium.com/towards-data-science/linear-regression-occams-razor-of-predictive-machine-learning-modeling-f2ba5b144a2b)（下方链接）中开发的模型进行比较，以便我们可以将 AutoML 的结果与我的结果进行比较。

-   [](/linear-regression-occams-razor-of-predictive-machine-learning-modeling-f2ba5b144a2b?source=post_page-----a318de373890--------------------------------) ## 线性回归 — 预测机器学习建模的奥卡姆剃刀

### -   使用 Python 进行线性回归的机器学习建模

-   towardsdatascience.com

-   为了使比较有意义，我们将使用来自[UCI 机器学习库](https://archive-beta.ics.uci.edu/dataset/10/automobile)（CC BY 4.0）的相同汽车价格数据集。您可以从[此链接](https://gist.github.com/fmnobar/c9b4029e08e97978a9a53f4eb034b16f)下载清理后的数据，并按照代码逐步操作。

-   如果这是你第一次使用 AutoGluon，你可能需要在你的环境中安装它。我为 Mac（Python 3.8）使用 CPU 遵循的安装步骤如下（如果你使用的是不同的操作系统，请访问[这里](https://auto.gluon.ai/stable/install.html)获取简单的说明）：

```py
pip3 install -U pip
pip3 install -U setuptools wheel
pip3 install torch==1.12.1+cpu torchvision==0.13.1+cpu torchtext==0.13.1 -f https://download.pytorch.org/whl/cpu/torch_stable.html
pip3 install autogluon
```

-   现在 AutoGluon 已准备好使用，让我们导入我们将使用的库。

```py
# Import libraries
import pandas as pd
from sklearn.model_selection import train_test_split
from autogluon.tabular import TabularDataset, TabularPredictor

# Show all columns/rows of the dataframe
pd.set_option("display.max_columns", None)
pd.set_option("display.max_rows", None)
```

-   接下来，我们将数据集读入 Pandas 数据框。

```py
# Load the data into a dataframe
df = pd.read_csv('auto-cleaned.csv')
```

-   然后，我们将数据分为训练集和测试集。我们将使用 30%的数据作为测试集，其余部分将作为训练集。在此阶段，为了可比性，我将确保我们使用与我在其他关于线性回归的帖子中使用的相同的`random_state = 1234`，这样我们这里创建的训练集和测试集将与我在那篇帖子中创建的相同。

```py
# Split the data into train and test set
df_train, df_test = train_test_split(df, test_size=0.3, random_state=1234)

print(f"Data includes {df.shape[0]} rows (and {df.shape[1]} columns), broken down into {df_train.shape[0]} rows for training and the balance {df_test.shape[0]} rows for testing.")
```

-   运行上述代码的结果是：

![](img/e2aec8bebaf77c45f940bde9150b3591.png)

-   正如我们所见，数据包括 25 列中的 193 行。一列是“价格”，这是我们希望预测的目标变量，其余是用于预测目标变量的自变量。

-   让我们查看数据的前五行，以便理解数据的样貌。

```py
# Return top five rows of the data frame
df.head()
```

-   结果：

![](img/60c29c1ee4427653ca7f0d35128252ce.png)

接下来，我们详细讨论 AutoGluon。首先，我们将创建一个字典，其中包含希望 AutoGluon 在本次练习中使用和比较的模型。以下是这些模型的列表：

+   GBM: LightGBM

+   CAT: CatBoost

+   XGB: XGBoost

+   RF: 随机森林

+   XT: 极端随机树

+   KNN: K 最近邻

+   LR: 线性回归

然后，我们进入我承诺的三行代码。这些代码将完成并对应以下步骤：

1.  训练（或拟合）模型到训练集

1.  使用训练好的模型为测试集创建预测

1.  创建模型评估结果的排行榜

让我们编写代码。

```py
# Run AutoGluon

# Create a dictionary of hyperparameters for the models to be included
hyperparameters_dict = {
    'GBM':{}, 
    'CAT':{},
    'XGB':{},
    'RF':{}, 
    'XT':{}, 
    'KNN':{},
    'LR':{},
    }

# 1\. Fit/train the models
autogluon_predictor = TabularPredictor(label="price").fit(train_data=df_train, presets='best_quality', hyperparameters=hyperparameters_dict)

# 2\. Create predictions
predictions = autogluon_predictor.predict(df_test)

# 3\. Create the leaderboard
autogluon_predictor.leaderboard(silent=True)
```

结果：

![](img/9243881eeea7abcb847d2a9d03ee51e7.png)

比较各种机器学习模型评估结果的排行榜

就这样！

让我们更详细地查看排行榜。

在最终结果中，名为“model”的列显示了我们在模型字典中包含的模型名称，共有八个（注意行号范围为 0 到 7，总计 8）。名为“score_val”的列是均方根误差（RMSE）乘以 -1（AutoGluon 通过乘以 -1 使得较高的数值表示更好）。模型按从最佳到最差的顺序排列在表格中。换句话说，“WeightedEnsemble_L2”是本次练习中最好的模型，RMSE 约为 2,142。

现在，让我们看看这个数字与我在关于 [线性回归](https://medium.com/towards-data-science/linear-regression-occams-razor-of-predictive-machine-learning-modeling-f2ba5b144a2b) 的文章中创建的机器学习模型评估结果相比如何。如果你访问那篇文章并搜索 MSE，你会发现 MSE 约为 6,725,127，相当于 RMSE 约为 2,593（RMSE 只是 MSE 的平方根）。将这个数字与排行榜中的“score_val”列进行比较，显示我的模型比 AutoGluon 尝试的 4 个模型要好，但比前 4 个模型差！记住，我花了相当多的时间进行特征工程和创建那个模型，而 AutoGluon 仅用 3 行代码在 13 秒多一点的时间内找到了 4 个更好的模型！这就是 AutoML 在实践中的力量。

# 实现笔记本

我在创建这篇文章时使用的笔记本如下提供。欢迎下载、复制并进行尝试。

# 结论

在这篇文章中，我们讨论了什么是 AutoML 以及它如何帮助有或没有机器学习建模背景的用户。对于不熟悉机器学习的非技术用户，AutoML 降低了入门门槛，使这些用户能够创建强大的机器学习模型。另一方面，科学家和机器学习从业者等技术用户可以利用 AutoML 提供的功能，通过在短时间内尝试各种模型来提高生产力。然后，技术用户可以花时间对 AutoML 算法的最佳推荐进行微调或改进。

# 谢谢阅读！

如果你觉得这篇文章有帮助，请关注我在 Medium 上的账号并订阅以获取我的最新文章！
