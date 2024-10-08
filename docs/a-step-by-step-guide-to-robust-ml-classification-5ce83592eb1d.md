# 逐步指南：稳健的机器学习分类

> 原文：[`towardsdatascience.com/a-step-by-step-guide-to-robust-ml-classification-5ce83592eb1d`](https://towardsdatascience.com/a-step-by-step-guide-to-robust-ml-classification-5ce83592eb1d)

## 如何避免常见的陷阱并深入探讨我们的模型

[](https://ryancburke8.medium.com/?source=post_page-----5ce83592eb1d--------------------------------)![Ryan Burke](https://ryancburke8.medium.com/?source=post_page-----5ce83592eb1d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5ce83592eb1d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ce83592eb1d--------------------------------) [Ryan Burke](https://ryancburke8.medium.com/?source=post_page-----5ce83592eb1d--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ce83592eb1d--------------------------------) ·阅读时间 17 分钟·2023 年 3 月 3 日

--

![](img/f1856f0dfbd79cecda67d8bfcb71783b.png)

图片由 [Luca Bravo](https://unsplash.com/@lucabravo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/wallpapers/nature/forest?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在之前的文章中，我主要介绍了我认为有趣的单一算法。在这里，我将介绍一个完整的机器学习分类项目。目标是讨论一些机器学习项目中的常见陷阱，并向读者描述如何避免这些陷阱。我还将演示如何通过分析模型错误进一步挖掘，以获得通常未被发现的重要见解。

如果你想查看完整的笔记本，请点击这里 → [这里](https://github.com/ryancburke/forest_cover) ←

# 库

下面是我今天分析中使用的库的列表。它们包括标准的数据科学工具包以及所需的 sklearn 库。

```py
import sys
import os
import pandas as pd
import numpy as np

import matplotlib.pyplot as plt
import seaborn as sns
from IPython.display import display
%matplotlib inline

import plotly.offline as py
import plotly.graph_objs as go
import plotly.tools as tls
py.init_notebook_mode(connected=True)

import warnings
warnings.filterwarnings('ignore')

from pandas import set_option
from pandas.plotting import scatter_matrix
from sklearn.preprocessing import StandardScaler, MinMaxScaler, QuantileTransformer, RobustScaler
from sklearn.model_selection import train_test_split, KFold, StratifiedKFold, cross_val_score, GridSearchCV
from sklearn.feature_selection import RFECV, SelectFromModel, SelectKBest, f_classif
from sklearn.metrics import classification_report, confusion_matrix, balanced_accuracy_score, ConfusionMatrixDisplay, f1_score
from sklearn.pipeline import Pipeline
from sklearn.neighbors import KNeighborsClassifier
from sklearn.ensemble import RandomForestClassifier, ExtraTreesClassifier, VotingClassifier
from scipy.stats import uniform

from imblearn.over_sampling import ADASYN

import swifter

# Always good to set a seed for reproducibility
SEED = 8
np.random.seed(SEED)
```

# 数据

今天的数据集包括可以直接用在 sklearn 中的森林覆盖数据。以下是来自 sklearn 网站的描述。

## **数据集特点：**

> 该数据集中的样本对应于美国 30×30 米的森林区域，收集用于预测每个区域的覆盖类型，即主要树种。共有七种覆盖类型，这使得这是一个多类别分类问题。每个样本有 54 个特征，特征在数据集主页上有描述。部分特征为布尔指标，其他则为离散或连续测量值。

实例数量：581 012

## 特征信息（名称 / 数据类型 / 测量 / 描述**）

+   高度 / 定量 / 米 / 以米为单位的高度

+   方面 / 定量 / 方位角 / 以度为单位的方位角

+   坡度 / 定量 / 度 / 以度为单位的坡度

+   Horizontal_Distance_To_Hydrology / 定量 / 米 / 到最近水体的水平距离

+   Vertical_Distance_To_Hydrology / 定量 / 米 / 到最近水体的垂直距离

+   Horizontal_Distance_To_Roadways / 定量 / 米 / 到最近道路的水平距离

+   Hillshade_9am / 定量 / 0 到 255 指数 / 早上 9 点的阴影指数，夏至

+   Hillshade_Noon / 定量 / 0 到 255 指数 / 中午的阴影指数，夏至

+   Hillshade_3pm / 定量 / 0 到 255 指数 / 下午 3 点的阴影指数，夏至

+   Horizontal_Distance_To_Fire_Points / 定量 / 米 / 到最近火灾点的水平距离

+   Wilderness_Area (4 个二元列) / 定性 / 0（缺失）或 1（存在） / 荒野区域指定

+   Soil_Type (40 个二元列) / 定性 / 0（缺失）或 1（存在） / 土壤类型指定

## **类别数：**

+   Cover_Type (7 种类型) / 整数 / 1 到 7 / 森林覆盖类型指定

# 加载数据集

这里有一个简单的函数可以将这些数据加载到你的笔记本中作为数据框。

```py
columns = ['Elevation', 'Aspect', 'Slope', 'Horizontal_Distance_To_Hydrology', 'Vertical_Distance_To_Hydrology', 'Horizontal_Distance_To_Roadways',
 'Hillshade_9am', 'Hillshade_Noon', 'Hillshade_3pm', 'Horizontal_Distance_To_Fire_Points', 'Wilderness_Area_0', 'Wilderness_Area_1', 'Wilderness_Area_2',
 'Wilderness_Area_3', 'Soil_Type_0', 'Soil_Type_1', 'Soil_Type_2', 'Soil_Type_3', 'Soil_Type_4', 'Soil_Type_5', 'Soil_Type_6', 'Soil_Type_7', 'Soil_Type_8',
 'Soil_Type_9', 'Soil_Type_10', 'Soil_Type_11', 'Soil_Type_12', 'Soil_Type_13', 'Soil_Type_14', 'Soil_Type_15', 'Soil_Type_16', 'Soil_Type_17', 'Soil_Type_18',
 'Soil_Type_19', 'Soil_Type_20', 'Soil_Type_21', 'Soil_Type_22', 'Soil_Type_23', 'Soil_Type_24', 'Soil_Type_25', 'Soil_Type_26', 'Soil_Type_27', 'Soil_Type_28',
 'Soil_Type_29', 'Soil_Type_30', 'Soil_Type_31', 'Soil_Type_32', 'Soil_Type_33', 'Soil_Type_34', 'Soil_Type_35', 'Soil_Type_36', 'Soil_Type_37', 'Soil_Type_38',
 'Soil_Type_39']  

from sklearn import datasets
def sklearn_to_df(sklearn_dataset):
    df = pd.DataFrame(sklearn_dataset.data, columns=columns)
    df['target'] = pd.Series(sklearn_dataset.target)
    return df

df = sklearn_to_df(datasets.fetch_covtype())
df_name=df.columns
df.head(3)
```

使用 df.info() 和 df.describe() 来更好地了解我们的数据，我们发现没有缺失数据，并且它由定量变量组成。数据集也相当大（> 580,000 行）。我最初尝试在整个数据集上运行这个，但花费了很长时间，所以我建议使用数据的一个部分。

关于目标变量，即森林覆盖类别，使用 df.target.value_counts()，我们看到以下分布（按降序排列）：

**类别 2 = 283,301

类别 1 = 211,840

类别 3 = 35,754

类别 7 = 20,510

类别 6 = 17,367

类别 5 = 9,493

类别 4 = 2,747

重要的是要注意我们的类别是不平衡的，我们在选择评估模型的指标时需要牢记这一点。

# 准备你的数据

运行机器学习模型时最常见的误解之一是对数据进行处理而不是拆分。为什么这是一个问题？

假设我们计划使用整个数据集来缩放我们的数据。以下方程来自它们各自的链接。

**Ex1** [**StandardScaler()**](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html#sklearn.preprocessing.StandardScaler)

z = (x — u) / s

**Ex2** [**MinMaxScaler()**](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html#sklearn.preprocessing.MinMaxScaler)

X_std = (X - X.min()) / (X.max() - X.min())

X_scaled = X_std * (max - min) + min

我们应该注意到的最重要的一点是，它们包括了均值、标准差、最小值和最大值等信息。如果我们在拆分之前执行这些函数，训练集中的特征将基于测试集中的信息进行计算。这是一个[数据泄露](https://machinelearningmastery.com/data-leakage-machine-learning/)的例子。

> 数据泄漏是指使用来自训练数据集外的信息来创建模型。这些额外的信息可能使模型学习或知道一些它本来不会知道的东西，从而使正在构建的模型的估计性能无效。

因此，在了解数据集后，第一步是将其拆分，并保持测试集 ***未见*** 直到最后。在下面的代码中，我们将数据拆分为 80%（训练集）和 20%（测试集）。你还会注意到，我只保留了 50,000 个样本，以减少训练和评估模型所需的时间。相信我，你会感谢我的！

还值得注意的是，我们在目标变量上进行了分层。这对不平衡的数据集是一种良好的实践，因为它保持了训练集和测试集中类别的分布。如果我们不这样做，可能会有一些不足代表的类别在训练集或测试集中根本不存在。

```py
# here we are first separating our df into features (X) and target (y)
X =  df[df_name[0:54]]
Y = df[df_name[54]]

# now we are separating into training (80%) and test (20%) sets. The test set won't be seen until we want to test our top model!
X_train, X_test, y_train, y_test =train_test_split(X,Y,
                                                   train_size = 40_000,
                                                   test_size=10_000,
                                                   random_state=SEED,
                                                   stratify=df['target']) # we stratify to ensure similar distribution in train/test
```

# 特征工程

当我们的训练集和测试集准备好之后，我们可以开始处理有趣的内容。这个项目的第一步是生成一些可能为训练我们的模型提供有用信息的特征。

这一步可能有些棘手。在现实世界中，这需要对你所处理的特定领域有专业的知识。完全透明地告诉你，尽管我热爱自然和户外活动，但我对某些树木为什么在特定地区生长并不精通。

出于这个原因，我咨询了 [[1](https://www.kaggle.com/code/skillsmuggler/forest-cover-feature-engineering-and-reduction/notebook#Feature-reduction)] [[2](https://www.kaggle.com/code/codename007/forest-cover-type-eda-baseline-model/notebook)] [[3](https://www.kaggle.com/code/kwabenantim/forest-cover-feature-engineering#Adding-new-features)]，他们对这个领域的理解比我更深入。我结合了这些参考资料中的知识，创建了下面的特征。

```py
# engineering new columns from our df
def FeatureEngineering(X):

    X['Aspect'] = X['Aspect'] % 360
    X['Aspect_120'] = (X['Aspect'] + 120) % 360

    X['Hydro_Elevation_sum'] = X['Elevation'] + X['Vertical_Distance_To_Hydrology']

    X['Hydro_Elevation_diff'] = abs(X['Elevation'] - X['Vertical_Distance_To_Hydrology'])

    X['Hydro_Euclidean'] = np.sqrt(X['Horizontal_Distance_To_Hydrology']**2 +
                                   X['Vertical_Distance_To_Hydrology']**2)

    X['Hydro_Manhattan'] = abs(X['Horizontal_Distance_To_Hydrology'] +
                            X['Vertical_Distance_To_Hydrology'])

    X['Hydro_Distance_sum'] = X['Horizontal_Distance_To_Hydrology'] + X['Vertical_Distance_To_Hydrology']

    X['Hydro_Distance_diff'] = abs(X['Horizontal_Distance_To_Hydrology'] - X['Vertical_Distance_To_Hydrology'])

    X['Hydro_Fire_sum'] = X['Horizontal_Distance_To_Hydrology'] + X['Horizontal_Distance_To_Fire_Points']

    X['Hydro_Fire_diff'] = abs(X['Horizontal_Distance_To_Hydrology'] + X['Horizontal_Distance_To_Fire_Points'])

    X['Hydro_Fire_mean'] = (X['Horizontal_Distance_To_Hydrology'] + X['Horizontal_Distance_To_Fire_Points'])/2

    X['Hydro_Road_sum'] = X['Horizontal_Distance_To_Hydrology'] + X['Horizontal_Distance_To_Roadways']

    X['Hydro_Road_diff'] = abs(X['Horizontal_Distance_To_Hydrology'] + X['Horizontal_Distance_To_Roadways'])

    X['Hydro_Road_mean'] = (X['Horizontal_Distance_To_Hydrology'] + X['Horizontal_Distance_To_Roadways'])/2

    X['Road_Fire_sum'] = X['Horizontal_Distance_To_Roadways'] + X['Horizontal_Distance_To_Fire_Points']

    X['Road_Fire_diff'] = abs(X['Horizontal_Distance_To_Roadways'] - X['Horizontal_Distance_To_Fire_Points'])

    X['Road_Fire_mean'] = (X['Horizontal_Distance_To_Roadways'] + X['Horizontal_Distance_To_Fire_Points'])/2

    X['Hydro_Road_Fire_mean'] = (X['Horizontal_Distance_To_Hydrology'] + X['Horizontal_Distance_To_Roadways'] + 
                                  X['Horizontal_Distance_To_Fire_Points'])/3

    return X

X_train = X_train.swifter.apply(FeatureEngineering, axis = 1) 
X_test = X_test.swifter.apply(FeatureEngineering, axis = 1) 
```

另外，当你处理大数据集时，pandas 可能会有些慢。使用 [swifter](https://pypi.org/project/swifter/)，正如上面最后两行所示，你可以显著加快对数据框应用函数的时间。文章 → 这里 比较了几种加速此过程的方法。

# 特征选择

目前我们有超过 70 个特征。如果目标是获得表现最佳的模型，那么你可以尝试使用所有这些特征作为输入。话虽如此，商业中常常需要考虑性能和复杂性之间的权衡。

举个例子，假设我们使用所有这些特征时模型的准确率为 94%。然后，假设只有 4 个特征时准确率为 89%。我们愿意为一个更具可解释性的模型支付什么样的代价。始终权衡性能和复杂性。

记住这一点，我将立即执行特征选择，以尽量减少复杂性。[Sklearn](https://scikit-learn.org/stable/modules/feature_selection.html)提供了许多值得考虑的选项。在这个例子中，我将使用[SelectKBest](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectKBest.html#sklearn.feature_selection.SelectKBest)，它将选择预先指定数量的特征，这些特征提供最佳性能。下面，我请求（并列出了）表现最佳的 15 个特征。这些特征将用于在下一部分中训练模型。

```py
selector = SelectKBest(f_classif, k=15)
selector.fit(X_train, y_train)
mask = selector.get_support()
X_train_reduced_cols = X_train.columns[mask]

X_train_reduced_cols

>>> Index(['Elevation', 'Wilderness_Area_3', 'Soil_Type_2', 'Soil_Type_3',
       'Soil_Type_9', 'Soil_Type_37', 'Soil_Type_38', 'Hydro_Elevation_sum',
       'Hydro_Elevation_diff', 'Hydro_Road_sum', 'Hydro_Road_diff',
       'Hydro_Road_mean', 'Road_Fire_sum', 'Road_Fire_mean',
       'Hydro_Road_Fire_mean'],
        dtype='object')
```

# 基线模型

在这一部分，我将比较三种不同的分类器：

+   [KNeighboursClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html)

+   [RandomForestClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)

+   [ExtraTreesClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.ExtraTreesClassifier.html)

我提供了链接以便那些希望进一步研究每个模型的人。这些链接在超参数调优部分也会很有帮助，你可以在其中找到所有可以调整的参数，以改善你的模型。下面你会发现两个函数，用于定义和评估基线模型。

```py
# baseline models
def GetBaseModels():
    baseModels = []
    baseModels.append(('KNN'  , KNeighborsClassifier()))
    baseModels.append(('RF'   , RandomForestClassifier()))
    baseModels.append(('ET'   , ExtraTreesClassifier()))

    return baseModels
```

```py
def ModelEvaluation(X_train, y_train,models):
    # define number of folds and evaluation metric
    num_folds = 10
    scoring = "f1_weighted" #This is suitable for imbalanced classes

    results = []
    names = []
    for name, model in models:
        kfold = StratifiedKFold(n_splits=num_folds, random_state=SEED, shuffle = True)
        cv_results = cross_val_score(model, X_train, y_train, cv=kfold, scoring=scoring, n_jobs = -1)
        results.append(cv_results)
        names.append(name)
        msg = "%s: %f (%f)" % (name, cv_results.mean(), cv_results.std())
        print(msg)

    return names, results
```

第二个函数中有一些关键元素值得进一步讨论。其中第一个是[StratifiedKFold](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.StratifiedKFold.html)。请回忆一下，我们将原始数据集拆分为 80%的训练集和 20%的测试集。测试集将保留用于最终评估我们表现最好的模型。

使用交叉验证将为我们提供更好的模型评估。具体来说，我设置了一个 10 折交叉验证。对于那些不熟悉的人，该模型在每一步中都在 k — 1 个折上进行训练，并在剩下的折上进行验证。最终，你将能够访问到 k 个模型的平均值和变异性，这比简单的训练-测试评估提供了更好的见解。正如我之前提到的，分层 K 折用于确保每个折中目标类的表示大致相等。

第二点值得讨论的是评分指标。有许多[指标](https://scikit-learn.org/stable/modules/model_evaluation.html)可用于评估模型的性能，通常有几个适合你的项目。重要的是要记住你试图通过结果展示什么。如果你在商业环境中工作，通常选择最容易向没有数据背景的人解释的指标。

另一方面，有些指标不适合你的分析。对于这个项目，我们有不平衡的类别。如果你访问上面提供的链接，你会发现适用于这种情况的选项。我选择使用加权 F1 分数。让我们简要讨论一下为什么我选择了这个指标。

一个非常常见的分类指标是准确率，它是正确分类的百分比。虽然这可能看起来是一个很好的选项，但假设我们有一个目标类不均衡的二分类问题（即组 1 = 90，组 2 = 10）。我们可能会得到 90% 的准确率，这听起来很棒，但如果我们进一步探讨，我们发现正确分类了所有组 1 的样本，却未能分类组 2 的任何样本。在这种情况下，我们的模型信息量并不大。

如果我们使用加权 F1 分数，我们会得到 42.6% 的结果。如果你有兴趣了解更多关于 F1 分数的信息 → 这里 有一篇文章解释了它是如何计算的。

在训练基准模型后，我已绘制了每个模型的结果。所有基准模型的表现都相对良好。请记住，此时我对数据没有做任何处理（即未进行转换或去除异常值）。额外树分类器的加权 F1 分数最高，为 86.9%。

![](img/f5d1439a88b1bb065aadfcaa8202ffdb.png)

10 倍交叉验证的结果如下。KNN 的表现最低，为 78.8%，RF 排在第二位，为 85.9%，而 ET 的加权 F1 分数最高，为 86.9%。*图片由作者提供*

# 数据转换

这个项目的下一步将研究数据转换对模型性能的影响。虽然许多基于决策树的算法对数据的量纲不敏感，但可以合理预期，测量样本之间距离的模型，例如 KNN，当数据缩放时性能会有所不同 [[4](https://ai.stackexchange.com/questions/22307/why-are-decision-trees-and-random-forests-scale-invariant#:~:text=Decision%20Tree%20and%20Random%20Forest,work%20fine%20without%20feature%20scaling.)] [[5](https://stats.stackexchange.com/questions/244507/what-algorithms-need-feature-scaling-beside-from-svm)]。在本节中，我们将使用上述描述的 StandardScaler 和 MinMaxScaler 对数据进行缩放。下面是一个函数，它描述了一个管道，该管道将应用缩放器，然后使用缩放后的数据训练模型。

```py
def GetScaledModel(nameOfScaler):

    if nameOfScaler == 'standard':
        scaler = StandardScaler()
    elif nameOfScaler =='minmax':
        scaler = MinMaxScaler()

    pipelines = []
    pipelines.append((nameOfScaler+'KNN' , Pipeline([('Scaler', scaler),('KNN' , KNeighborsClassifier())])))
    pipelines.append((nameOfScaler+'RF'  , Pipeline([('Scaler', scaler),('RF'  , RandomForestClassifier())])))
    pipelines.append((nameOfScaler+'ET'  , Pipeline([('Scaler', scaler),('ET'   , ExtraTreesClassifier())])))

    return pipelines 
```

使用 StandardScaler 的结果如下。我们看到有关数据缩放的假设似乎成立。随机森林和额外树分类器的表现几乎相同，而 KNN 的性能提高了大约 4%。尽管有所提升，但这两个基于树的分类器仍然优于缩放后的 KNN。

![](img/f929c80f19245ca85081e472bacfb1db.png)

使用 StandardScaler 对数据进行 10 折交叉验证的结果。尽管表现有所提升至 83.8%，KNN 的表现仍然最低。RF 排名第二，为 85.8%，ET 再次以 86.8%的加权 F1 分数名列最高。*图像由作者提供*

使用 MinMaxScaler 时可以看到类似的结果。所有模型的结果几乎与使用 StandardScaler 时所呈现的结果完全相同。

![](img/75dba5daf7061c01f44fa4fd86846478.png)

使用 MinMaxScaler 对数据进行 10 折交叉验证的结果。每个模型的表现几乎与使用 StandardScaler 时相同。KNN 的表现仍然最低，为 83.9%。RF 排名第二，为 86.0%，ET 再次以 87.0%的加权 F1 分数名列最高。*图像由作者提供*

值得注意的是，我还检查了去除异常值的效果。为此，我移除了每个特征值超出±3 个标准差的值。我在这里没有展示结果，因为没有值超出这个范围。如果你有兴趣了解如何执行此操作，请随时查看本文开头提供的笔记本链接。

# 使用 GridSearchCV 进行超参数调整

下一步是通过调整超参数来尝试改进我们的模型。我们将在缩放后的数据上进行，因为它在考虑我们的三种模型时表现最佳。Sklearn 对此进行了更详细的讨论 → [这里](https://scikit-learn.org/stable/modules/grid_search.html)。

我选择使用[GridSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html)（CV 表示交叉验证）。下面你将找到一个对我们一直使用的模型进行 10 折交叉验证的函数。这里唯一的额外细节是我们需要提供希望评估的超参数列表。

到目前为止，我们甚至没有查看我们的测试集。在开始网格搜索之前，我们将使用 StandardScaler 对训练数据和测试数据进行缩放。我们在这里这样做是因为我们要找到每个模型的最佳超参数，并将这些超参数作为输入到 VotingClassifier 中（我们将在下一节讨论）。

为了正确缩放我们的完整数据集，我们必须按照以下程序进行。你将看到缩放器仅在训练数据上进行拟合。训练集和测试集都基于训练集找到的缩放参数进行转换，从而消除任何数据泄漏的可能性。

```py
scaler = StandardScaler()
X_train_scaled = pd.DataFrame(scaler.fit_transform(X_train_reduced), columns=X_train_reduced.columns)
X_test_scaled = pd.DataFrame(scaler.transform(X_test_reduced), columns=X_test_reduced.columns)
```

```py
class GridSearch(object):

    def __init__(self,X_train,y_train,model,hyperparameters):

        self.X_train = X_train
        self.y_train = y_train
        self.model = model
        self.hyperparameters = hyperparameters

    def GridSearch(self):

        cv = 10
        clf = GridSearchCV(self.model,
                                 self.hyperparameters,
                                 cv=cv,
                                 verbose=0,
                                 n_jobs=-1,
                                 )
        # fit grid search
        best_model = clf.fit(self.X_train, self.y_train)
        message = (best_model.best_score_, best_model.best_params_)
        print("Best: %f using %s" % (message))

        return best_model,best_model.best_params_

    def BestModelPredict(self,X_train):

        best_model,_ = self.GridSearch()
        pred = best_model.predict(X_train)
        return pred
```

接下来，我提供了对每个模型测试的网格搜索参数。

```py
# 1) KNN
model_KNN = KNeighborsClassifier()
neighbors = [1,3,5,7,9,11,13,15,17,19] # Number of neighbors to use by default for k_neighbors queries
param_grid = dict(n_neighbors=neighbors)

# 2) RF
model_RF = RandomForestClassifier()
n_estimators_value = [50,100,150,200,250,300] # The number of trees 
criterion = ['gini', 'entropy', 'log_loss'] # The function to measure the quality of a split
param_grid = dict(n_estimators=n_estimators_value, criterion=criterion)

# 3) ET
model_ET = ExtraTreesClassifier()
n_estimators_value = [50,100,150,200,250,300] # The number of trees 
criterion = ['gini', 'entropy', 'log_loss'] # The function to measure the quality of a split
param_grid = dict(n_estimators=n_estimators_value, criterion=criterion) 
```

# 投票集成分类器

我们已经确定了优化我们模型的最佳参数组合。这些参数将作为输入用于一个[投票分类器](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.VotingClassifier.html)，这是一个集成估计器，它训练几个模型，然后汇总结果以获得更稳健的预测。我找到了一篇→ 文章，提供了关于投票分类器及其使用方式的详细概述。

每个模型的最佳参数列在下面。投票分类器的输出显示，我们在训练集上达到了 87.5% 的加权 F1 分数，在测试集上达到了 88.4%。还不错！

```py
param = {'n_neighbors': 1}
model1 = KNeighborsClassifier(**param)

param = {'criterion': 'entropy', 'n_estimators': 300}
model2 = RandomForestClassifier(**param)

param = {'criterion': 'gini', 'n_estimators': 300}
model3 = ExtraTreesClassifier(**param)

# create the models based on above parameters
estimators = [('KNN',model1), ('RF',model2), ('ET',model3)]

# create the ensemble model
kfold = StratifiedKFold(n_splits=10, random_state=SEED, shuffle = True)
ensemble = VotingClassifier(estimators)
results = cross_val_score(ensemble, X_train_scaled, y_train, cv=kfold)
print('F1 weighted score on train: ',results.mean())
ensemble_model = ensemble.fit(X_train_scaled,y_train)
pred = ensemble_model.predict(X_test_scaled)
print('F1 weighted score on test:' , (y_test == pred).mean())

>>> F1 weighted score on train:  0.8747
>>> F1 weighted score on test: 0.8836
```

# 错误分析

我们模型的表现相当不错。尽管如此，调查模型失败的地方可能非常有见地。下面，你将找到生成混淆矩阵的代码。让我们看看是否能学到一些东西。

```py
from sklearn.metrics import plot_confusion_matrix
cfm_raw = plot_confusion_matrix(ensemble_model, X_test_scaled, y_test, values_format = '') # add normalize = 'true' for precision matrix or 'pred' for recall matrix
plt.savefig("cfm_raw.png")
```

![](img/d5369074906634124bff8df76e7c9c89.png)

测试集上的混淆矩阵。图片由作者提供

立刻可以看出，代表性不足的类别学习得不是很好。这非常重要，因为尽管使用了适合评估不平衡类别的指标，你还是无法让模型学习那些不存在的东西。

为了分析我们的错误，我们可以创建可视化；然而，考虑到有 15 个特征和 7 个类别，这可能会开始感觉像那种你盯着看直到图像形成的迷幻立体图像。另一种方法如下。

# 机器学习错误分类

在本节中，我将比较预测值与测试集中的实际值，并创建一个新变量，‘*error*’。下面，我正在设置一个数据集，用于二分类分析，目标是错误与非错误，使用与上述相同的特征。

既然我们已经知道代表性不足的类别没有很好地学习，那么这里的目标是查看哪些特征与错误的关联最为明显，而不考虑类别。

```py
# add predicted values test_df to compare with ground truth
test_df['predicted'] = pred

# create class 0 = no error , 1 = error
test_df['error'] = (test_df['target']!=test_df['predicted']).astype(int)

# create our error classification set
X_error = test_df[['Elevation', 'Wilderness_Area_3', 'Soil_Type_2', 'Soil_Type_3', 'Soil_Type_9', 'Soil_Type_37', 'Soil_Type_38',
                       'Hydro_Elevation_sum', 'Hydro_Elevation_diff', 'Hydro_Road_sum', 'Hydro_Road_diff', 'Hydro_Road_mean', 'Road_Fire_sum',
                       'Road_Fire_mean', 'Hydro_Road_Fire_mean']]

X_error_names = X_error.columns
y_error = test_df['error']
```

使用我们的新数据集，下一步是构建一个分类模型。这一次我们将添加一个步骤，使用[SHAP](https://shap.readthedocs.io/en/latest/example_notebooks/overviews/An%20introduction%20to%20explainable%20AI%20with%20Shapley%20values.html)。这将使我们能够理解每个特征如何影响模型，而在我们的案例中，这个模型是错误。

在下文中，我们使用了随机森林来拟合数据。我们再次使用 K 折交叉验证来更好地估计每个特征的贡献。最后，我生成了一个数据框，其中包含平均值、标准差和最大 SHAP 值。

```py
import shap
kfold = StratifiedKFold(n_splits=10, random_state=SEED, shuffle = True)

list_shap_values = list()
list_test_sets = list()
for train_index, test_index in kfold.split(X_error, y_error):
    X_error_train, X_error_test = X_error.iloc[train_index], X_error.iloc[test_index]
    y_error_train, y_error_test = y_error.iloc[train_index], y_error.iloc[test_index]
    X_error_train = pd.DataFrame(X_error_train,columns=X_error_names)
    X_error_test = pd.DataFrame(X_error_test,columns=X_error_names)

    #training model
    clf = RandomForestClassifier(criterion = 'entropy', n_estimators = 300, random_state=SEED)
    clf.fit(X_error_train, y_error_train)

    #explaining model
    explainer = shap.TreeExplainer(clf)
    shap_values = explainer.shap_values(X_error_test)
    #for each iteration we save the test_set index and the shap_values
    list_shap_values.append(shap_values)

# flatten list of lists, pick the sv for 1 class, stack the result (you only need to look at 1 class for binary classification since values will be opposite to one another)
shap_values_av = np.vstack([sv[1] for sv in list_shap_values])
sv = np.abs(shap_values_av).mean(0) 
sv_std = np.abs(shap_values_av).std(0) 
sv_max = np.abs(shap_values_av).max(0) 
importance_df = pd.DataFrame({
    "column_name": X_error_names,
    "shap_values_av": sv,
    "shap_values_std": sv_std,
    "shap_values_max": sv_max
})
```

为了更好的视觉体验，下面是一个 SHAP 总结图。左侧是特征名称。该图展示了每个特征对模型的影响，针对不同特征值的影响。虽然分散度（向右或向左的距离）描述了特征对模型的整体影响，着色则为我们提供了额外的信息。

![](img/ca2d8b4f786111e9c1a89c5ec21d74f0.png)

错误分类模型的 SHAP 总结图。图像由作者提供

我们首先注意到，对模型影响最大的特征更多地与距离特征（即水源、道路或火源点）相关，而不是森林类型（荒野区域）或土壤类型。

接下来，当我们查看颜色分布时，我们可以看到第一个特征 Hydro_Road_Fire_mean 在高值与低值之间的差异比其他特征更明显。对于 Road_Fire_mean 也是如此，尽管程度较轻。

为了解释这意味着什么，我们可以提出这样的说法：*当到水源、火源点和道路的平均距离较低时，更容易出现错误。*

再次强调，我的林业‘*专业知识*’仅限于几周。我确实做了一些研究来帮助我解读这可能意味着什么，并发现了一些文章 [[6](https://www.researchgate.net/publication/352180034_Application_of_remote_sensing_and_machine_learning_algorithms_for_forest_fire_mapping_in_a_Mediterranean_area/figures?lo=1)] [[7](https://www.researchgate.net/publication/233978116_GIS-grid-based_and_multi-criteria_analysis_for_identifying_and_mapping_peat_swamp_forest_fire_hazard_in_Pahang_Malaysia/figures?lo=1)]，这些文章表明，道路的距离是森林火灾风险的一个重要因素。

这让我推测森林火灾可能是影响我们数据集上的错误的重要因素。对我来说，被火灾影响的区域与未受影响的区域在森林多样性上的表现差异很大是合乎逻辑的。我相信有更多经验的人可以告诉我这是否有道理 :)

# 总结

今天，我们详细讲解了一个逐步的机器学习多分类问题。我们讨论了进行这些分析时的一些重要考虑因素，即在开始处理数据集之前分割数据集的重要性。这是机器学习项目中最常见的陷阱之一，可能导致严重问题，限制了我们推广发现的能力。

我们还讨论了选择适当的评估指标以评估我们模型的重要性。在这里，我们使用了加权 F1 分数，这适用于不平衡的类别。尽管如此，我们仍然看到被低估的类别没有得到很好的学习。

在我的笔记本中，我还包含了一个关于过采样的部分，使用了[ADASYN](https://imbalanced-learn.org/stable/references/generated/imblearn.over_sampling.ADASYN.html)来创建平衡的类别，它是[SMOTE](https://imbalanced-learn.org/stable/references/generated/imblearn.over_sampling.SMOTE.html)的一种变体。为了让你们不再悬念，过采样显著改善了训练集的结果（但没有改善测试集的结果）。

这引出了错误分析，这是任何机器学习项目中的重要部分。进行了二分类错误分析，结果可能表明森林火灾在许多模型错误中起到了作用。这也在一定程度上解释了为什么过采样没有改善我们的最终模型。

最后，感谢大家抽时间阅读这篇文章！我希望你们中的一些人觉得它有帮助 :)
