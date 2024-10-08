# 如何在 Python 的 PyOD 中进行多变量异常值检测

> 原文：[`towardsdatascience.com/how-to-perform-multivariate-outlier-detection-in-python-pyod-for-machine-learning-b0a9c557a21c`](https://towardsdatascience.com/how-to-perform-multivariate-outlier-detection-in-python-pyod-for-machine-learning-b0a9c557a21c)

## 异常值检测系列，第三部分

[](https://ibexorigin.medium.com/?source=post_page-----b0a9c557a21c--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----b0a9c557a21c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b0a9c557a21c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b0a9c557a21c--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----b0a9c557a21c--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b0a9c557a21c--------------------------------) ·9 分钟阅读·2023 年 2 月 7 日

--

![](img/f1e3a4d96c11ab2134a5310a7737a0d8.png)

照片由 [Takashi Miyazaki](https://unsplash.com/@miyatankun?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，出处 [Unsplash](https://unsplash.com/backgrounds/art/abstract?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 动机

以下是一个*非常*可疑的箱型图：

![](img/567acd70936349dc6af5c43bb04d9a66.png)

作者提供的图片。

它描绘了钻石品质与价格之间的关系。六个品质类别按降序排列，因此最好的钻石在*理想*类别中，而最低质量的钻石在*公平*类别中。

现在，这里是一些奇怪的部分。首先，所有类别都有许多异常值，这些异常值用胡须上方的暗点标记。

其次，尽管理想钻石应该是最好的，但它们的中位数价格却低于任何其他类别（中位数以箱子内部的线表示）。

这种钻石品质与价格之间的奇怪关系让我们提出了一个问题：那些异常值*真的*是异常值吗？

今天，我们将回答这个确切的问题，换句话说，我们将学习如何区分多变量异常值以及如何检测它们。

这是我们异常值检测系列的第三部分。请查看下面的前两部分：

[](/how-to-perform-outlier-detection-in-python-in-easy-steps-for-machine-learning-1-8f9a3e6c88b5?source=post_page-----b0a9c557a21c--------------------------------) ## 如何在 Python 中进行异常值检测：第一部分

### 地球是一个异常值——这个理论

towardsdatascience.com

## 更多关于多变量异常值的内容

在我们的 [第一部分](https://medium.com/towards-data-science/how-to-perform-outlier-detection-in-python-in-easy-steps-for-machine-learning-1-8f9a3e6c88b5) 中，我们提出了以下观点。

单变量异常值，顾名思义，仅存在于单个分布或数据集的单独列中。它们通过 z 分数或中位数绝对偏差等方法更容易检测。例如，在记录仅有人身高（单列）的数据集中，一个非常高的人。

多变量异常值更难发现，因为它们通常在同时查看多个数据集列时才会出现。

例如，假设我们有一个医院记录的数据集。它的列包括病人的身体特征和他们所患的疾病类型。现在，我们查看亨利的记录：

年龄：18 身高：178 厘米 体重：65 公斤 非吸烟者：是 疾病：肺癌

如果我们单独考虑亨利的特征，它们在我们的数据集上下文中看起来很普通。在我们的数据集中，有许多 18 岁的人，也有许多人身高 178 厘米或体重 65 公斤。

另外，许多不吸烟的 18 岁青少年或肺癌患者都在医院。但是，我们没有任何一个不吸烟的 18 岁青少年，同时体重 65 公斤且身高 178 厘米且患有肺癌。

只有当我们同时考虑亨利的所有身体特征时，亨利作为多变量异常值的事实才会浮出水面。

亨利的类似案例可能会出现在另一个数据集中的几十个甚至几百个属性中。因此，我们必须学习更复杂的方法来检测多变量异常值。

这就是机器学习方法，特别是异常值分类器，发挥作用的地方。

## 加载数据集

让我们首先加载我们将要使用的数据集。这是上一个教程中相同的 Diamonds 数据集：

```py
import seaborn as sns 

diamonds = sns.load_dataset("diamonds")
diamonds.head()
```

![](img/143af438d5bead17872f4ddaec9cb5c2.png)

图片由作者提供

我们进行一个小的预处理步骤，将类别特征编码为 Sklearn 的 OrdinalEncoder 类：

```py
import numpy as np
from sklearn.preprocessing import OrdinalEncoder

# Extract feature and target arrays
X, y = diamonds.drop("carat", axis=1), diamonds["carat"]
# Select categorical column names
cats = X.select_dtypes(exclude=np.number).columns.tolist()

# Encode categoricals
X.loc[:, cats] = OrdinalEncoder().fit_transform(X[cats])
```

现在，让我们开始吧！（搓手）

## 使用 PyOD 进行多变量异常检测

尽管理论可能有些困难，但得益于 Python Outlier Detection (PyOD) 库，执行多变量异常检测在代码中非常简单。让我们通过 Local Outlier Factor 算法来看一个例子：

```py
# Import LOF
from pyod.models.lof import LOF

# Initialize
lof = LOF(n_neighbors=30).fit(X)

# Extract inlier/outlier labels
labels = lof.labels_
outliers_X_lof = X[labels == 1]
```

在从 `pyod` 中导入 `LOF` 估算器后，我们用 30 个邻居初始化它，并拟合到 `X`（不用担心 `LOF` 的细节，我们稍后会讨论）。

然后，我们访问它的 `labels_` 属性，该属性返回 `X` 每一行的 0（正常值）或 1（异常值）。让我们来计算一下异常值：

```py
num_outliers = len(outliers_X_lof)
print(f"The number of outliers: {num_outliers}")
print(f"Percentage of outliers: {num_outliers / len(X):.3f}")
```

```py
The number of outliers: 5394
Percentage of outliers: 0.100
```

嗯，我们发现了 5394 个异常值，正好占数据集的 10%。这是不是巧合？让我们尝试使用另一个异常值分类器 Isolation Forest 进行相同的操作：

```py
from pyod.models.iforest import IForest

iforest = IForest(n_estimators=500).fit(X)

labels = iforest.labels_
outliers_X_iforest = X[labels == 1]

num_outliers = len(outliers_X_iforest)
print(f"The number of outliers: {num_outliers}")
print(f"Percentage of outliers: {num_outliers / len(X):.3f}")
```

```py
The number of outliers: 5394
Percentage of outliers: 0.100
```

我们得到了相同的可疑 10%。这不可能是巧合（实际上，没有什么是巧合的 — 乌龟大师）。

## 异常检测中的污染是什么？

像隔离森林或局部异常因子这样的异常值分类算法实际上并不是分类器。当它们拟合到数据集时，其内部算法会计算数据集中每一行的异常分数。以下是 IForest 的一个示例：

```py
iforest = IForest().fit(X)

iforest.decision_scores_[:10]
```

```py
array([-0.03364944,  0.0287027 ,  0.07729889, -0.06363647, -0.03095093,
        0.05240712,  0.0230652 , -0.02713253,  0.06674287,  0.03475134])
```

使用`decision_scores_`属性，你可以访问任何算法的异常分数，一旦它们被拟合。

这些分数告诉我们数据集中每个样本的异常程度。默认情况下，PyOD 中的所有算法将具有最高 10%异常分数的样本标记为异常点。这是作为*contamination*参数提供的：

![](img/186ae0b8082ab23c040aa06a6d072b75.png)

图片由作者提供

基本上，异常值分类器对我们说：“这是我认为每个样本的异常程度。现在，我将选择 10%最异常的样本作为异常点，但你可以随时通过我的污染参数进行更改。”

基本上，`contamination`是所有分类器的超参数，用于控制产生的异常点数量。

现在，这可能看起来有些矛盾——我们想要使用分类器检测异常点，但分类器却告诉我们如何选择这些异常点。这正是异常值检测的最大挑战。

你，作为机器学习工程师，掌握着所有决策——你需要使用你的经验、领域知识、直觉和试错来决定设置污染的百分比。

## 概率置信度

如果找到正确的污染值听起来很复杂，有一个解决方法可以让我们摆脱污染。这个解决方法就是异常值概率分数。

一旦 PyOD 估计器拟合到数据集，它将拥有`predict_proba`方法。它为每个样本返回两列；第一列是样本为内点的概率，第二列是样本为异常点的概率。我们简称这些为内点概率和异常点概率。

```py
from pyod.models.iforest import IForest

iforest = IForest(n_estimators=1000).fit(X)

probs = iforest.predict_proba(X)
probs[:5]
```

```py
array([[0.6309838 , 0.3690162 ],
       [0.48315014, 0.51684986],
       [0.3044389 , 0.6955611 ],
       [0.73513599, 0.26486401],
       [0.59298752, 0.40701248]])
```

这些概率是通过使用 MinMax 缩放（也称为规范化）对`decision_scores_`数组进行规范化生成的。

规范化数组将强制其在 0 到 1 的范围内，这意味着我们可以将异常分数解释为样本为异常点的概率。所以，`predict_proba`在后台的实现如下：

```py
from sklearn.preprocessing import minmax_scale

# Create an empty array with two columns
probs = np.empty((len(X), 2))

# The second column is outlier probabilities
probs[:, 1] = minmax_scale(iforest.decision_scores_)

# The first column is inlier probabilities
probs[:, 0] = 1 - probs[:, 1]

# Check if the probs match
probs[:5] == iforest.predict_proba(X)[:5]
```

```py
array([[ True,  True],
       [ True,  True],
       [ True,  True],
       [ True,  True],
       [ True,  True]])
```

现在，我们可以基于概率置信度来选择异常点，而不是基于污染。我们可以选择至少有 80%、90%或 n%异常点概率的样本，而不是选择 10%最异常的样本。下面是示例：

```py
# Set a confidence threshold
threshold = 0.9

# Create a mask that returns True if probs over threshold
is_outlier = probs[:, 1] > threshold
outliers_X_probs = X[is_outlier]

# Count up the outliers
num_outliers = len(outliers_X_probs)
print(f"The number of outliers: {num_outliers}")
print(f"Percentage of outliers: {num_outliers / len(X):.4f}")
```

```py
The number of outliers: 12
Percentage of outliers: 0.0002
```

使用概率置信度方法，我们只找到了 12 个异常点，而不是超过 5000 个。最棒的是，我们有 90%的把握这 12 个异常点确实是异常点！

## 如何选择合适的概率阈值

选择概率阈值取决于多个因素，如数据集大小、你试图解决的任务和业务领域。

如果你有一个小型数据集，你需要确保你所隔离的异常值确实是异常值。通过删除它们，你不希望丢失在小型数据集中重要的信息和模式，这些都不可避免地会影响机器学习模型。对于这种情况，你应该设置一个较高的阈值，比如 80%–90%。

如果你处理的是像医疗记录这样的敏感数据，在这种情况下，避免假阳性是至关重要的，你应该将阈值设定为超过 90%。95% 和 99% 也是比较常见的。

一切都取决于你在解决手头业务问题时对异常值的信心或必须有的信心。

## 如何选择合适的异常值分类器

在监督学习任务中，你的工作会简单一些。你可以使用像 RMSE 或准确率这样的度量标准来评估一系列算法，并选择最有前途的那个。

然而，由于异常检测是一个无监督学习问题，你实际上无法将一个算法与另一个算法进行比较，因为你无法用度量标准来衡量它们的表现。

然而，有一些算法如果使用得当，表现始终良好。

+   [孤立森林](https://pyod.readthedocs.io/en/latest/pyod.models.html#module-pyod.models.iforest)：适用于几乎任何类型的数据集，但特别设计用于高维和复杂的数据集。尽管它是不同类型的决策树的集合，称为 ITrees，但它速度快且性能高。它还可以处理具有不规则、非正态分布的数据。

+   [局部异常因子](https://pyod.readthedocs.io/en/latest/pyod.models.html#module-pyod.models.lof)：特别适用于数据点被聚集在一起的数据集，因为它通过局部密度来计算异常分数。它对高维数据表现良好，并且比 IForest 更快。

如果你想了解更多关于如何选择合适的分类器，可以查看[我在 DataCamp 上的异常检测课程](https://app.datacamp.com/learn/courses/anomaly-detection-in-python)，在课程中我讲解了近 10 种异常值分类器，如何以及何时使用它们，以及如何调整它们的超参数。

此外，[这个页面](https://pyod.readthedocs.io/en/latest/pyod.models.html#) 的 PyOD 文档列出了超过 40 种算法。你可以通过查看这个[表格](https://pyod.readthedocs.io/en/latest/benchmark.html)来了解它们的表现，表格中列出了 10 种最佳异常值分类器在 15 个数据集上的表现。

## 结论

本文讨论了机器学习中的多变量异常检测主题，并展示了如何使用 Python 中的 PyOD 实现。通过将异常分数转化为概率置信度，选择最佳的异常值分类器，并确定合适的概率阈值，你可以通过从数据中去除异常值来提升你的机器学习模型。有了这些知识，你现在可以在自己的项目中进行高效的异常检测。

喜欢这篇文章吗？让我们面对现实吧，它的写作风格确实很奇特。想象一下拥有更多类似的文章，全部由一位聪明、迷人、机智的作者（顺便说一下，就是我 :）。

仅需 4.99$的会员资格，你将不仅能访问我的故事，还能获得来自 Medium 上最优秀头脑的知识宝库。如果你使用[我的推荐链接](https://ibexorigin.medium.com/membership)，你将获得我的超级感激以及一个虚拟的击掌，感谢你对我工作的支持。

[](https://ibexorigin.medium.com/membership?source=post_page-----b0a9c557a21c--------------------------------) [## 使用我的推荐链接加入 Medium — Bex T.

### 获得对我所有⚡高级⚡内容以及 Medium 上所有内容的无限制访问。通过给我买一杯咖啡来支持我的工作……

ibexorigin.medium.com](https://ibexorigin.medium.com/membership?source=post_page-----b0a9c557a21c--------------------------------) ![](img/95a45f54ecebc1b690dfd77f973617d2.png)
