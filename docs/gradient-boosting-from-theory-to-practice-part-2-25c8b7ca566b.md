# 理论与实践中的梯度提升（第二部分）

> 原文：[`towardsdatascience.com/gradient-boosting-from-theory-to-practice-part-2-25c8b7ca566b`](https://towardsdatascience.com/gradient-boosting-from-theory-to-practice-part-2-25c8b7ca566b)

## 使用 Scikit-Learn 中的梯度提升类解决不同的分类和回归问题

[](https://medium.com/@roiyeho?source=post_page-----25c8b7ca566b--------------------------------)![Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----25c8b7ca566b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----25c8b7ca566b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----25c8b7ca566b--------------------------------) [Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----25c8b7ca566b--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----25c8b7ca566b--------------------------------) ·阅读时间 12 分钟·2023 年 7 月 19 日

--

![](img/d549d077e8ed878675b861b84fcdffb5.png)

照片由[Luca Bravo](https://unsplash.com/@lucabravo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，来源于[Unsplash](https://unsplash.com/wallpapers/nature/forest?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在[第一部分](https://medium.com/towards-data-science/gradient-boosting-from-theory-to-practice-part-1-940b2c9d8050)中，我们介绍了梯度提升算法，并展示了其伪代码实现。

在这部分文章中，我们将探讨 Scikit-Learn 中实现这一算法的类，讨论它们的各种参数，并演示如何使用它们解决几个分类和回归问题。

尽管 XGBoost 库提供了一个更优化和高度可扩展的梯度提升实现，但对于小到中等规模的数据集，使用 Scikit-Learn 中的梯度提升类通常更为简便，因为它们有更简单的接口和显著更少的超参数需要调整。

# Scikit-Learn 中的梯度提升

Scikit-Learn 提供了以下实现梯度提升决策树（GBDT）模型的类：

1.  [GradientBoostingClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingClassifier.html)用于分类问题。

1.  [GradientBoostingRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingRegressor.html)用于回归问题。

除了[决策树](https://medium.com/@roiyeho/decision-trees-part-1-da4e613d2369)的标准参数，如*criterion, max_depth*（默认设置为 3）和*min_samples_split*，这些类还提供以下参数：

1.  *损失* — 需要优化的损失函数。在`GradientBoostingClassifier`中，这个函数可以是`'log_loss'`（默认值）或`'exponential'`（这将使梯度提升表现得像[AdaBoost](https://medium.com/@roiyeho/adaboost-illustrated-3084183a2086)）。在 GradientBoostingRegressor 中，这个函数可以是`'squared_error'`（默认值）、`'absolute_error'`、`'huber'`或‘quantile’（有关这些损失函数之间的差异，请参见[这篇文章](https://medium.com/towards-data-science/loss-functions-in-machine-learning-9977e810ac02)）。

1.  *n_estimators* — 提升迭代的次数（默认值为 100）。

1.  *学习率* — 缩小每棵树贡献的因子（默认值为 0.1）。

1.  *子样本* — 用于训练每棵树的样本比例（默认值为 1.0）。

1.  *max_features* — 搜索每个节点最佳分割时要考虑的特征数量。选项包括指定特征数量的整数、指定特征比例的浮点数、使用特征的平方根的‘sqrt’、使用特征的对数的‘log2’，以及使用所有特征的 None（默认值）。

1.  *验证比例* — 用作早期停止的验证集的训练集比例（默认值为 0.1）。

1.  *n_iter_no_change* — 当验证评分在前* n_iter_no_change *次迭代中未改善至少*tol*时终止训练（默认值为 0.0001）。默认情况下，*n_iter_no_change* 设置为 None，这意味着早期停止被禁用。

梯度提升估计器还有以下属性，它们是从数据中学习到的：

+   *n_estimators_* — 通过早期停止确定的拟合树数量（如果指定，否则设置为*n_estimators*）。

+   *估计器* — 已拟合的树集合。

+   *特征重要性* — 模型估计的特征重要性（将在本文后面讨论）。

+   *oob_scores* — 每次迭代时的袋外样本损失值（仅在*subsample* < 1.0 时可用）。

+   *train_score* — 每次迭代时训练集上的损失值。

# GradientBoostingClassifier

例如，我们可以对[Iris 数据集](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_iris.html)进行梯度提升分类器的拟合，使用每朵花的前两个特征（萼片宽度和萼片长度）。作为提醒，我们在这个数据集上使用[随机森林](https://medium.com/@roiyeho/random-forests-98892261dc49)能够获得 81.58%的测试准确率（经过超参数调整）。让我们看看梯度提升是否能做得更好。

我们首先加载数据集：

```py
from sklearn.datasets import load_iris

iris = load_iris()
X = iris.data[:, :2] # we only take the first two features
y = iris.target
```

然后我们将数据集分为训练集和测试集（使用与之前实验相同的随机种子）：

```py
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=42)
```

现在我们创建一个 GradientBoostingClassifier 模型，使用其默认设置（即，一个由 100 棵树组成的集成，max_depth=3），并将其拟合到训练集：

```py
from sklearn.ensemble import GradientBoostingClassifier

clf = GradientBoostingClassifier(random_state=42)
clf.fit(X_train, y_train)
```

我们固定梯度提升分类器的随机状态，以便结果可以重复。模型的性能是：

```py
print(f'Train accuracy: {clf.score(X_train, y_train):.4f}')
print(f'Test accuracy: {clf.score(X_test, y_test):.4f}')
```

```py
Train accuracy: 0.9554
Test accuracy: 0.7895
```

这些结果与之前通过随机森林获得的结果相同，但在超参数调整之前。

## 调整超参数

让我们在一些梯度提升超参数上运行随机搜索，以找到更好的模型。为了公平比较，我们使用与随机森林相同的搜索迭代次数（50 次）。

```py
from sklearn.model_selection import RandomizedSearchCV

params = {
    'n_estimators': [10, 50, 100, 200, 500],
    'max_depth': np.arange(3, 11),
    'subsample': np.arange(0.5, 1.0, 0.1),
    'max_features': ['sqrt', 'log2', None]    
}

search = RandomizedSearchCV(GradientBoostingClassifier(random_state=42), params, n_iter=50, cv=3, n_jobs=-1)
search.fit(X_train, y_train)

print(search.best_params_)
```

随机搜索找到的最佳模型是：

```py
{'subsample': 0.6, 'n_estimators': 10, 'max_features': 'sqrt', 'max_depth': 3}
```

即，最佳模型由 10 棵决策树组成，max_depth = 3，每棵树在训练集的 60%随机子样本上进行训练。该模型在训练集和测试集上的准确性是：

```py
best_clf = search.best_estimator_
print(f'Train accuracy: {best_clf.score(X_train, y_train):.4f}')
print(f'Test accuracy: {best_clf.score(X_test, y_test):.4f}')
```

```py
Train accuracy: 0.8125
Test accuracy: 0.8684
```

测试集上的准确性显著高于调整后随机森林的结果（86.84%对 81.58%）。

让我们来检查这个分类器找到的决策边界：

![](img/69ae522fb1c8be0476b3a6dcdf18e0a5.png)

梯度提升分类器在鸢尾数据集上找到的决策边界

与随机森林分类器找到的决策边界相比，我们可以看到梯度提升分类器能够捕捉到更多的紫花地丁区域，而不会对异常值过拟合。

# GradientBoostingRegressor

为了演示梯度提升回归模型，我们将使用[加州住房数据集](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.fetch_california_housing.html#sklearn.datasets.fetch_california_housing)。该数据集的目标是基于该区的 8 个不同特征（如中位收入或每户平均房间数），预测加州某个区（房块）的中位房价。

我们首先获取数据集：

```py
from sklearn.datasets import fetch_california_housing

data = fetch_california_housing()
X, y = data.data, data.target
feature_names = data.feature_names
```

然后，我们将数据集分为 80%的训练集和 20%的测试集：

```py
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)
```

接下来，我们使用默认设置（即，一个由 100 棵树组成的集成，max_depth=3）拟合一个 GradientBoostingRegressor 到训练集：

```py
from sklearn.ensemble import GradientBoostingRegressor

reg = GradientBoostingRegressor(random_state=0)
reg.fit(X_train, y_train)
```

该模型的*R*²得分是：

```py
train_score = reg.score(X_train, y_train)
print(f'R2 score (train): {train_score:.4f}')

test_score = reg.score(X_test, y_test)
print(f'R2 score (test): {test_score:.4f}')
```

```py
R2 score (train): 0.8027
R2 score (test): 0.7774
```

这是一个略逊于我们在调整前使用随机森林得到的结果（测试集上的*R*²得分=0.798）。然而，请注意，默认情况下 RandomForestRegressor 中的树不会被修剪（它们的最大深度没有限制），而 GradientBoostingRegressor 中的树的默认最大深度仅为 3，同时两种估计器的默认树数相同（100）。

## 调整超参数

让我们通过运行以下随机搜索来调整梯度提升回归模型的超参数：

```py
from sklearn.model_selection import RandomizedSearchCV

params = {
    'n_estimators': [10, 50, 100, 200, 500],
    'max_depth': np.arange(3, 11),
    'subsample': np.arange(0.5, 1.0, 0.1),
    'max_features': ['sqrt', 'log2', None]    
}

search = RandomizedSearchCV(GradientBoostingRegressor(random_state=0), params, n_iter=50, cv=3, n_jobs=-1)
search.fit(X_train, y_train)

print(search.best_params_)
```

随机搜索找到的最佳模型是：

```py
{'subsample': 0.7999999999999999, 'n_estimators': 500, 'max_features': 'log2', 'max_depth': 7}
```

也就是说，最佳模型使用 500 棵树，最大深度为 7，每棵树在训练集的 80%的随机子样本上进行训练，每个节点分裂使用特征的对数数量。

该模型在训练集和测试集上的*R*²分数是：

```py
best_reg = search.best_estimator_
print(f'R2 score (train): {best_reg.score(X_train, y_train):.4f}')
print(f'R2 score (test): {best_reg.score(X_test, y_test):.4f}')
```

```py
R2 score (train): 0.9849
R2 score (test): 0.8519
```

在测试集上的*R*²分数明显高于随机森林回归器调优后的结果（0.8166）。

## 学习曲线

我们还可以绘制每次提升迭代中的训练误差和测试误差。训练误差存储在估计器的*train_score_*属性中。测试误差可以通过调用*staged_predict*()方法获得，该方法返回一个生成器，按每次迭代返回给定数据集上的模型预测。

```py
from sklearn.metrics import mean_squared_error as MSE

test_score = np.zeros(best_reg.n_estimators_)
for i, y_test_pred in enumerate(best_reg.staged_predict(X_test)):
    test_score[i] = MSE(y_test, y_test_pred)

plt.plot(np.arange(best_reg.n_estimators), best_reg.train_score_, label='Training loss')
plt.plot(np.arange(best_reg.n_estimators), test_score, 'r', label='Test loss')

plt.xlabel('Boosting iterations')
plt.ylabel('MSE')
plt.legend()
```

![](img/c92ac457269c4b8f01540b546004cddb.png)

加州住房数据集上梯度提升回归器的学习曲线

我们可以看到，最小测试误差在大约 100 次迭代后达到，也就是说，该数据集的最佳树木数量约为 100。此外，随着我们向集成中添加更多树，测试误差保持稳定，这表明模型不容易过拟合。

另一种找到最佳树木数量的方法是使用**早停法**。我们来运行相同的随机搜索，但这次不改变估计器的数量，而是将其设置为固定的 500，并通过将*n_iter_no_change*设置为 5 来启用早停法。这会自动将训练集的 10%留作验证集，并在验证分数在 5 次迭代内没有提高时终止训练。

```py
from sklearn.model_selection import RandomizedSearchCV

params = {    
    'max_depth': np.arange(3, 11),
    'subsample': np.arange(0.5, 1.0, 0.1),
    'max_features': ['sqrt', 'log2', None]    
}

search = RandomizedSearchCV(GradientBoostingRegressor(random_state=0, n_estimators=500, n_iter_no_change=5), 
                            params, n_iter=50, cv=3, n_jobs=-1)
search.fit(X_train, y_train)

print(search.best_params_)

reg = search.best_estimator_
print(f'R2 score (train): {reg.score(X_train, y_train):.4f}')
print(f'R2 score (test): {reg.score(X_test, y_test):.4f}')
```

```py
{'subsample': 0.8999999999999999, 'max_features': 'log2', 'max_depth': 7}
R2 score (train): 0.9227
R2 score (test): 0.8402
```

与之前的模型相比，这个估计器在测试集上的准确度稍差，这可以通过搜索的随机化以及它只使用了 90%的训练集来构建集成来解释。

我们可以检查在启用早停法之前实际构建了多少棵树，通过检查*n_estimators_*属性来实现：

```py
reg.n_estimators_
```

```py
118
```

## 特征重要性

与其他树基集成方法类似，梯度提升树可以提供数据集中特征的重要性估计，即每个特征对模型预测的贡献程度。这对于模型解释以及进行特征选择都很有用。

单棵决策树中特征的重要性由特征在树中的位置（位于树顶部的特征对树的预测贡献更大）和使用该特征分裂节点所实现的节点纯度减少来决定。在树基集成方法中，如随机森林和梯度提升树，我们对集成中的所有树的特征重要性进行平均。

例如，我们可以绘制我们梯度提升回归器在加州住房数据集中找到的特征重要性：

```py
# Sort the features by their importance
feature_importance = best_reg.feature_importances_
sorted_idx = np.argsort(feature_importance)

# Plot the feature importances
pos = np.arange(len(feature_importance))
plt.barh(pos, feature_importance[sorted_idx])
plt.yticks(pos, np.array(feature_names)[sorted_idx])
plt.xlabel('Feature importance')
```

![](img/9be44d4793192a1d53141f62437256d6.png)

加州住房数据集中特征的重要性

在这个数据集中，最重要的特征是 MedInc（中位收入）、房屋位置（经度和纬度）以及 AveOccup（家庭成员的平均数量）。

# 基于直方图的梯度提升

Scikit-Learn 0.21 引入了两种基于直方图的梯度提升实现：[HistGradientBoostingClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.HistGradientBoostingClassifier.html#sklearn.ensemble.HistGradientBoostingClassifier) 和 [HistGradientBoostingRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.HistGradientBoostingRegressor.html)，它们类似于 LightGBM [1] 中使用的基于直方图的算法。

这些估计器首先将数据集中的连续特征离散化为整数值的区间（默认为 255 个区间）。在训练过程中，这些区间用于根据达到树中特定节点的样本值构建特征直方图。然后，基于这些直方图找到该节点的最佳分割点。

这种离散化具有以下优点：

1.  显著减少了在树的每个节点上考虑的分割点数量。

1.  这避免了在每个节点上对连续特征值进行排序的需要（请参阅[这篇文章](https://medium.com/@roiyeho/decision-trees-part-1-da4e613d2369)中的“处理连续特征”部分，了解为什么需要从一开始就进行排序）。

此外，直方图估计器的许多部分都进行了并行化。例如，梯度计算在样本之间进行并行化，而在节点上寻找最佳分割点则在特征之间进行并行化。

离散化和并行化共同使得基于直方图的估计器在样本数量较大（*n* > 10,000）时运行速度远快于标准梯度提升估计器。

此外，基于直方图的估计器内置支持缺失值和分类特征，这避免了在数据预处理时使用填补器或独热编码器的需要。

大多数基于直方图的估计器的参数与 GradientBoostingClassifier 和 GradientBoostingRegressor 相同，唯一的变化如下：

+   估计器的参数被称为 *max_iter*，而不是 *n_estimators*。

+   树的默认大小已被修改：*max_depth* 默认为 None（而不是 3），*max_leaf_nodes* 设置为 31，而 *min_samples_leaf* 设置为 20。

+   当样本数量超过 10,000 时，早期停止功能会自动启用。

此外，添加了以下参数：

+   *max_bins* 表示要使用的最大区间数。不得大于 255。一个额外的区间保留用于缺失值。

+   *categorical_features* 是一个整数列表，表示数据集中分类特征的位置。

+   *interaction_cst* 指定交互约束，即在子节点分裂中可以相互交互的特征集（默认为 None）。

## HistGradientBoostingClassifier 示例

例如，让我们比较 HistGradientBoostingClassifier 和 GradientBoostingClassifier 在一个人工生成的数据集上的表现。

我们将使用 Scikit-Learn 中的函数 [make_hastie_10_2](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.make_hastie_10_2.html#sklearn.datasets.make_hastie_10_2)，它生成一个二分类的 10 维数据集，与 Hastie 等人[2]中的示例 10.2 相同。

数据集包含 10 个遵循标准高斯分布的特征，目标是由以下定义的二进制标签：

![](img/5048cf864774f3284bc35ce66e7bed9c.png)

也就是说，负类位于一个半径为 9.34 的 10 维球体内。

让我们首先生成一个包含 50,000 个样本的数据集：

```py
from sklearn.datasets import make_hastie_10_2

X, y = make_hastie_10_2(n_samples=50000, random_state=0)
```

然后我们将其分为 80% 的训练集和 20% 的测试集：

```py
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)
```

现在我们来训练一个 GradientBoostingClassifier 并测量它的训练时间：

```py
from sklearn.ensemble import GradientBoostingClassifier

clf = GradientBoostingClassifier(random_state=0)
%timeit clf.fit(X_train, y_train)
```

```py
12.6 s ± 358 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

训练这个模型平均需要 12.6 秒。它在训练集和测试集上的表现是：

```py
print(f'Train accuracy: {clf.score(X_train, y_train):.4f}')
print(f'Test accuracy: {clf.score(X_test, y_test):.4f}')
```

```py
Train accuracy: 0.9392
Test accuracy: 0.9231
```

现在我们来训练一个 HistGradientBoostingClassifier 在相同的数据集上：

```py
from sklearn.ensemble import HistGradientBoostingClassifier

clf = HistGradientBoostingClassifier(random_state=0)
%timeit clf.fit(X_train, y_train)
```

```py
1.53 s ± 120 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

训练这个模型平均只需 1.53 秒（比 GradientBoostingClassifier 快 8 倍以上）。它在训练集和测试集上的表现是：

```py
print(f'Train accuracy: {clf.score(X_train, y_train):.4f}')
print(f'Test accuracy: {clf.score(X_test, y_test):.4f}')
```

```py
Train accuracy: 0.9725
Test accuracy: 0.9467
```

测试集上的准确率显著提高（94.67% 代替 92.31%）。

# 总结

让我们总结一下梯度提升与其他监督学习模型相比的优缺点。

**优点**：

+   提供高度准确的模型，通常在结构化数据集上表现最佳。

+   通过组合多个弱模型，能够捕捉数据集中的复杂交互和模式。

+   能够通过自动选择相关特征有效处理高维数据集。

+   相较于其他模型，对异常值的敏感性较低，因为每个基础模型从前一个模型的残差中学习。

+   可以处理异质数据类型，包括数值和类别特征。

+   可以处理缺失值而无需插补。

+   提供特征重要性的度量。

+   可以应用于回归和分类任务，并支持多种损失函数。

**缺点**：

+   训练可能会消耗大量计算资源，特别是在处理大型数据集或当集成模型有很多树时。此外，基础模型的训练无法并行化（例如，与随机森林不同）。

+   比决策树或线性回归等简单模型更难解释，因为解释模型的决策需要跟踪多个树的路径。

+   需要调整多个超参数，包括树的数量、每棵树的大小和学习率。

+   如果没有适当的正则化，或者提升迭代次数过多，可能会对训练集过拟合。

+   与其他模型相比，预测可能会更慢，因为它需要遍历多个树并聚合它们的预测。

# 最终备注

所有图片均为作者提供，除非另有说明。

你可以在我的 GitHub 上找到本文的代码示例：[`github.com/roiyeho/medium/tree/main/gradient_boosting`](https://github.com/roiyeho/medium/tree/main/gradient_boosting)

鸢尾花数据集信息：

**引用**: Fisher, R. A. (1988). Iris. UCI 机器学习库。 [`doi.org/10.24432/C56C76.`](https://doi.org/10.24432/C56C76.)

**许可证**: Creative Commons CC BY 4.0。

加利福尼亚住房数据集信息：

**引用**: Pace, R. Kelley 和 Ronald Barry (1997), Sparse Spatial Autoregressions, Statistics and Probability Letters, 33, 291-297。

**许可证**: Creative Commons CC0: 公共领域。

## 参考资料

[1] Ke 等人 (2017), [“LightGBM: A Highly Efficient Gradient Boosting Decision Tree”](https://papers.nips.cc/paper/6907-lightgbm-a-highly-efficient-gradient-boosting-decision-tree)。

[2] T. Hastie, R. Tibshirani 和 J. Friedman (2009), “Elements of Statistical Learning Ed. 2”, Springer。
