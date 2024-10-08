# 类别特征：标签编码的问题所在

> 原文：[`towardsdatascience.com/categorical-features-whats-wrong-with-label-encoding-81184c3dfb69`](https://towardsdatascience.com/categorical-features-whats-wrong-with-label-encoding-81184c3dfb69)

## 为什么我们不能随意编码类别特征

[](https://harrisonfhoffman.medium.com/?source=post_page-----81184c3dfb69--------------------------------)![Harrison Hoffman](https://harrisonfhoffman.medium.com/?source=post_page-----81184c3dfb69--------------------------------)[](https://towardsdatascience.com/?source=post_page-----81184c3dfb69--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----81184c3dfb69--------------------------------) [Harrison Hoffman](https://harrisonfhoffman.medium.com/?source=post_page-----81184c3dfb69--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----81184c3dfb69--------------------------------) ·10 分钟阅读·2023 年 11 月 20 日

--

![](img/49e87931489a1c96a6402b71184b64fa.png)

云朵。作者提供的图像。

众所周知，**许多**机器学习模型无法原生处理类别特征。虽然也有一些例外，但通常需要实践者决定每个类别特征的数值表示。有[多种方法](https://contrib.scikit-learn.org/category_encoders/)可以实现这一点，但一种很少推荐的策略是[标签编码](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html)。

标签编码用一个任意的数字替换每个类别值。例如，如果我们有一个包含字母的特征，标签编码可能会将字母“A”分配为 0，将字母“B”分配为 1，然后继续这个模式直到“Z”，即 25。经过这个过程，从技术上讲，任何算法都应该能够处理这个编码后的特征。

那么这有什么问题呢？难道复杂的机器学习模型不能处理这种编码方式吗？为什么像[Catboost](https://catboost.ai/)和[其他编码策略](https://contrib.scikit-learn.org/category_encoders/)这样的库存在，用来处理高基数类别特征？

这篇文章将探讨两个示例，演示**为什么**标签编码对机器学习模型可能存在问题。这些示例将帮助我们理解为什么有如此多的[替代方案](https://contrib.scikit-learn.org/category_encoders/)存在，以及加深我们对数据复杂性和模型性能之间关系的理解。

# 直观示例

获得机器学习概念直观理解的最佳方法之一是了解其在低维空间中的工作原理，并尝试将结果外推到更高维度。这种思维外推并不总是与现实一致，但对于我们的目的来说，我们只需一个特征即可了解为什么需要更好的分类编码策略。

## 一个具有 25 个类别的特征

让我们从一个具有单个特征和连续目标的基本玩具数据集开始。以下是我们需要的依赖项：

```py
import numpy as np
import polars as pl
import matplotlib.pyplot as plt
from sklearn.preprocessing import LabelEncoder
from sklearn.tree import DecisionTreeRegressor
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_absolute_error
from category_encoders.target_encoder import TargetEncoder
from category_encoders.ordinal import OrdinalEncoder
```

让我们读取数据集并探索一些属性：

```py
>>> data = pl.read_csv("basic_categorical_dataset_1.csv")
>>> data.shape
(852, 2)

>>> data.sample(5)
shape: (5, 2)
┌─────────────┬───────────┐
│ cat_feature ┆ target    │
│ ---         ┆ ---       │
│ str         ┆ f64       │
╞═════════════╪═══════════╡
│ v           ┆ 16.287324 │
│ z           ┆ 16.285893 │
│ j           ┆ 5.718953  │
│ p           ┆ 14.290943 │
│ y           ┆ 2.972485  │
└─────────────┴───────────┘

>>> data.select("cat_feature").n_unique()
25
```

该数据集包含一个分类特征`cat_feature`，具有 25 个唯一类别和一个连续的`target`。目标是学习一个函数，将每个类别映射到其对应的最佳拟合目标值。我们不需要机器学习来做到这一点，但它将帮助我们理解为什么在处理更复杂的实际问题时需要良好的分类编码策略。

接下来，我们将创建训练集和测试集：

```py
>>> x = data.select("cat_feature").to_numpy()
>>> y = data.select("target").to_numpy()
>>> x_train, x_test, y_train, y_test = train_test_split(
...     x, y, test_size=0.20, random_state=3
... )
>>> x_train.shape
(681, 1)
```

我们已将 80% 的数据用于训练，20% 用于测试，现在准备为`cat_feature`选择一种分类编码策略。与常见的机器学习智慧相悖，我们决定对`cat_feature`进行标签编码。**标签编码将每个分类值替换为介于 0 和类别数减一之间的任意数字**。在这个例子中，这些类别将被替换为介于 0 和 24 之间的数字：

```py
>>> label_encoder = LabelEncoder()
>>> x_train_label_encoded = label_encoder.fit_transform(x_train.ravel())
>>> x_test_label_encoded = label_encoder.transform(x_test.ravel())
```

使用 Scikit-learn 的`LabelEncoder`类，我们使用训练集来决定编码并转换训练集和测试集。我们现在拥有一个单一的数值特征和一个目标，并且可以通过散点图可视化它们之间的关系：

```py
>>> fig, ax = plt.subplots(figsize=(10, 6))
>>> ax.scatter(x_train_label_encoded, y_train)
>>> ax.set_xlabel("Label-Encoded Categorical Feature")
>>> ax.set_ylabel("Target")
>>> plt.show()
```

散点图如下所示：

![](img/9ac6668a69491fd2011c863c1907ba12.png)

标签编码训练数据的散点图。图片由作者提供。

正如预期的那样，每个类别都被分配了一个介于 0 和 24 之间的唯一整数，并且在每个类别内似乎有目标值的分布。我们还可以看到，编码后的`cat_feature`与目标之间的关系高度非线性，**这排除了线性回归作为可行模型。**

尽管这些训练数据看起来很复杂，但机器学习模型足够强大，可以拟合这种关系。让我们看看当我们将决策树拟合到训练数据时会发生什么：

```py
>>> model = DecisionTreeRegressor(max_depth=4)
>>> model.fit(x_train_label_encoded.reshape(-1, 1), y_train)
>>> preds = model.predict(x_test_label_encoded.reshape(-1, 1))
```

我们用`max_depth`为 4 的 Sklearn 的`DecisionTreeRegressor`对训练数据进行拟合，并对测试数据进行预测。由于我们只有一个特征，我们可以暂时忽略任何回归指标，简单地绘制叠加在测试集上的预测结果：

```py
>>> fig, ax = plt.subplots(figsize=(10, 6))
>>> ax.scatter(x_test_label_encoded, y_test, label="Actual Test Set" )
>>> ax.scatter(x_test_label_encoded, preds, label="Predictions")
>>> ax.set_xlabel("Label-Encoded Categorical Feature")
>>> ax.set_ylabel("Target")
>>> ax.set_title("Actual vs Predicted Test Set: Max Depth = 4")
>>> ax.legend()
>>> plt.show()
```

结果如下所示：

![](img/6d6e96082b89877785d596ae5f5f27d0.png)

使用标签编码的分类特征和深度为 4 的决策树的实际测试集与模型预测。图片由作者提供。

哎呀！我们不需要评估回归指标就知道这个模型不是很好。虽然模型在一些区域表现尚可，但这些结果并不是我们希望展示给老板的。

希望还没有完全丧失。由于我们的决策树深度为 4，我们怀疑增加深度，从而增加模型的复杂性，将会得到更好的结果。让我们将深度增加到 14 看看会得到什么：

![](img/57f7d17239ceb955020f4badd29df356.png)

实际测试集与使用标签编码分类特征和深度为 14 的决策树的模型预测。图片来源：作者。

这看起来好多了！深度为 14 的决策树似乎很好地拟合了关系。

我们从中可以得出什么结论？也许我们最初的想法是，这只是一个复杂的关系，需要一个更大、更复杂的模型来拟合。这可以解释为什么深度为 4 的决策树效果不好，但深度为 14 的树效果很好。也许标签编码是一个有效的选择？

这可能是真的，但为了成为优秀的数据科学家，我们需要评估另一种分类编码方案。虽然它可能容易导致过拟合，但一种流行的策略是**目标均值编码**。在最简单的形式中，目标均值编码将每个分类值替换为该类别所有观察值的均值目标。我们可以使用[category_encoders](https://pypi.org/project/category-encoders/)来实现这一点：

```py
>>> target_mean_encoder = TargetEncoder(smoothing=0, min_samples_leaf=1)
>>> x_train_target_encoded = target_mean_encoder.fit_transform(x_train, y_train).values
>>> x_test_target_encoded = target_mean_encoder.transform(x_test).values
```

`smoothing` 和 `min_samples_leaf` 参数用于对每个类别的目标均值计算进行正则化。在这个例子中，我们不希望进行任何正则化，因此我们不应用平滑，并且只要求类别中存在一个样本。如之前所述，我们可以可视化结果：

```py
>>> fig, ax = plt.subplots(figsize=(10, 6))
>>> ax.scatter(x_train_target_encoded, y_train)
>>> ax.set_xlabel("Target-Encoded Categorical Feature")
>>> ax.set_ylabel("Target")
>>> plt.show()
```

下面是目标均值编码特征与目标的散点图：

![](img/20fa41f2f9d08af7db587d0cb3b39c4d.png)

目标均值编码训练数据的散点图。图片来源：作者。

这里发生了什么？目标均值编码揭示了分类特征与目标之间的线性关系。这与我们在应用标签编码时观察到的情况大相径庭。我们更愿意使用这种编码，因为它允许我们使用更简单的甚至是线性的模型。

线性回归对这个数据集效果很好，但我们将再次使用决策树来查看这种编码为什么更强大：

```py
>>> model = DecisionTreeRegressor(max_depth=4)
>>> model.fit(x_train_target_encoded.reshape(-1, 1), y_train)
>>> preds = model.predict(x_test_target_encoded.reshape(-1, 1))

>>> fig, ax = plt.subplots(figsize=(10, 6))
>>> ax.scatter(x_test_target_encoded, y_test, label="Actual Test Set" )
>>> ax.scatter(x_test_target_encoded, preds, label="Predictions")
>>> ax.set_xlabel("Target-Encoded Categorical Feature")
>>> ax.set_ylabel("Target")
>>> ax.set_title("Actual vs Predicted Test Set: Max Depth = 4")
>>> ax.legend()
>>> plt.show()
```

这是将预测结果覆盖在测试集上的效果：

![](img/6d45462b754530cd9b8c746db5b99c76.png)

实际测试集与使用目标均值编码分类特征和深度为 4 的决策树的模型预测。图片来源：作者。

使用目标均值编码的分类特征，我们可以用深度为 4 的树很好地拟合关系。与此相比，标签编码特征需要更高的树深度。一种**更好的分类编码方案揭示了一个模型更容易学习的关系。**

我们可以通过查看每种编码策略所需的模型复杂度（即树深度）来进一步理解这个想法，以确定它们在测试集上的收敛情况：

![](img/ae2473d8186f009e688b75cee682ea83.png)

每种分类编码策略的测试集 MAE 与树深度的关系。图片来源于作者。

我们在这里看到的正是为什么一个好的分类编码策略至关重要的核心。这个图表告诉我们，目标均值编码模型在较低树深度时的测试集误差显著低于标签编码模型。例如，在深度为 2 时，目标均值模型的测试误差不到标签编码模型的一半。

目标均值模型的收敛速度也比标签模型更快。目标均值模型在深度为 5 时达到了最小测试误差，而标签模型则需要到深度为 9 时才能达到最小值。

在这种情况下，正确编码分类特征揭示了一个关系，使我们能够使用更简单的模型。然而，即使标签编码需要更复杂的模型来拟合关系，我们仍然使用它找到了一个与目标编码模型一样好的模型。

也就是说，即使`cat_feature`被标签编码，我们仍然找到了一个效果与其他模型一样好的模型。此外，我们知道还有许多更复杂的编码策略可以选择。选择一个好的编码策略真的值得吗？

## 一个具有数百个类别的特征

为了真正说服自己标签编码的不好，我们来看一个数据集：

```py
>>> data = pl.read_csv("basic_categorical_dataset_2.csv")
>>> data.shape
(3777, 2)

>>> data.sample(5)
shape: (5, 2)
┌───────────────────────────────────┬────────────┐
│ cat_feature                       ┆ target     │
│ ---                               ┆ ---        │
│ str                               ┆ f64        │
╞═══════════════════════════════════╪════════════╡
│ 79a5808e-4e18-401a-94e7-7a478260… ┆ 369.232239 │
│ c29ae2f2-c4c0-4a48-b15f-ba72a81e… ┆ 492.785283 │
│ df809121-1f19-4f89-bb42-e7c593d9… ┆ 602.706521 │
│ 5840ee5f-69e9-4f5b-ac20-17e01033… ┆ 731.986467 │
│ b5c74247-6aff-4762-844e-3ebb3cbf… ┆ 404.087366 │
└───────────────────────────────────┴────────────┘

>>> data.select("cat_feature").n_unique()
917
```

这个数据集包含一个名为`cat_feature`的单一分类特征，具有 917 个唯一类别和一个连续的`target`。具有如此多类别的特征对我们如何编码`cat_feature`有什么影响？标签编码仍然能收敛吗？

和之前一样，我们将创建使用标签和目标均值编码的训练集和测试集：

```py
>>> x = data.select("cat_feature").to_numpy()
>>> y = data.select("target").to_numpy()
>>> x_train, x_test, y_train, y_test = train_test_split(
...     x, y, test_size=0.20, random_state=3
... )
>>> x_train.shape
(3021, 1)

>>> label_encoder = OrdinalEncoder(handle_unknown="value")
>>> x_train_label_encoded = label_encoder.fit_transform(x_train, y_train).values
>>> x_test_label_encoded = label_encoder.transform(x_test).values

>>> target_mean_encoder = TargetEncoder(
...     smoothing=0, min_samples_leaf=1, handle_unknown="value"
... )
>>> x_train_target_encoded = target_mean_encoder.fit_transform(x_train, y_train).values
>>> x_test_target_encoded = target_mean_encoder.transform(x_test).values
```

在这个例子中，我们使用`OrdinalEncoder`而不是`LabelEncoder`，因为它可以处理在测试数据中可能出现的先前未见过的类别。我们可以再次可视化标签编码的分类特征与目标之间的关系：

![](img/d66d69faaa61a1b96d8017ab08992558.png)

这是一个标签编码训练数据的散点图。图片来源于作者。

这看起来不太好。标签编码在`cat_feature`和目标之间产生了大量噪声，很难想象一个机器学习模型在没有更多特征的情况下能够拟合这种关系。让我们将其与目标均值编码数据进行比较：

![](img/30516971dcd54a457a5ce6807db2498b.png)

目标均值编码训练数据的散点图。图片来源于作者。

然而，当我们使用目标均值编码时，一个更简单，甚至是线性的关系再次出现。我们已经知道这允许我们使用更简单的模型，但我们说的“更简单”有多简单呢？

![](img/b3e88c1754eaae769a901a3ef7a14ba2.png)

每种分类编码策略的测试集 MAE 与树深度的关系。图片来源于作者。

对于这个数据集，目标均值编码模型在深度为 6 时收敛，而标签编码模型直到深度为 29 才收敛。从不同的角度来看，最佳标签编码模型最终有 866 个叶子节点，而目标编码模型只有 128 个。**也就是说，标签编码模型必须将单一特征划分 866 次才能收敛——这几乎是每个类别一个划分。**

尽管这两个例子都使用了单一特征来预测目标，但我们可以想象一个更现实的场景，其中可能使用数百个特征。**如果类别特征被标签编码，常用的基于决策树的算法可能会忽略这些特征，因为它们与目标的关系可能比其他特征复杂得多。**

再次强调，我们不应得出目标均值编码是最佳策略的结论，因为它往往会过拟合训练数据。然而，我们确实看到了为什么需要一个好的编码策略，以及为什么标签编码会阻碍模型性能。

# **主要收获**

这篇文章探讨了标签编码，这是一种将每个类别替换为任意数字的类别编码方法。我们发现**标签编码可能会在类别特征与目标之间创建不必要的复杂性**，需要更大的模型来进行拟合。找到合适的编码方法就是揭示有意义的关系，从而允许我们使用更简单的模型。

*成为会员:* [*https://harrisonfhoffman.medium.com/membership*](https://harrisonfhoffman.medium.com/membership)
