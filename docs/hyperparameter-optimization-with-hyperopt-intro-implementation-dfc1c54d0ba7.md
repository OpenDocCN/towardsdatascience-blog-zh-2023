# **超参数优化与 Hyperopt — 介绍与实现**

> 原文：[`towardsdatascience.com/hyperparameter-optimization-with-hyperopt-intro-implementation-dfc1c54d0ba7`](https://towardsdatascience.com/hyperparameter-optimization-with-hyperopt-intro-implementation-dfc1c54d0ba7)

## 通过超参数优化提升机器学习模型的性能。

[](https://medium.com/@fmnobar?source=post_page-----dfc1c54d0ba7--------------------------------)![Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----dfc1c54d0ba7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dfc1c54d0ba7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dfc1c54d0ba7--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----dfc1c54d0ba7--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dfc1c54d0ba7--------------------------------) ·阅读时间 11 分钟·2023 年 6 月 5 日

--

![](img/ba34fe1074371d14f74e66344336a40b.png)

照片由 [Te NGuyen](https://unsplash.com/@tenguyen?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/Wt7XT1R6sjU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[Hyperopt](https://github.com/hyperopt/hyperopt) 是一个开源的超参数优化工具，我个人使用它来提升我的机器学习项目，并发现它实现起来相当简单。超参数优化是识别最佳超参数组合的过程，以使机器学习模型满足目标函数（通常定义为“最小化”目标函数以保持一致）。换句话说，每个机器学习模型都有各种旋钮和杠杆，我们可以调节这些参数，直到获得我们所期望的结果。找到能得到我们所期望结果的正确超参数组合的过程称为超参数优化。一些这样的参数示例包括：学习率、神经网络的架构（如隐藏层数量）、优化器的选择等。

如果你对探索其他超参数优化策略感兴趣，如网格搜索、随机搜索和贝叶斯优化，请查看下面的帖子：

[](/hyperparameter-optimization-intro-and-implementation-of-grid-search-random-search-and-bayesian-b2f16c00578a?source=post_page-----dfc1c54d0ba7--------------------------------) [## 超参数优化 — 网格搜索、随机搜索和贝叶斯优化的介绍与实现]

### 提升机器学习成果的最常见超参数优化方法。

towardsdatascience.com](/hyperparameter-optimization-intro-and-implementation-of-grid-search-random-search-and-bayesian-b2f16c00578a?source=post_page-----dfc1c54d0ba7--------------------------------)

让我们开始吧！

[](https://medium.com/@fmnobar/membership?source=post_page-----dfc1c54d0ba7--------------------------------) [## 使用我的推荐链接加入 Medium

### 阅读 Farzad（以及 Medium 上其他作者）的每一个故事。您的会员费用直接支持 Farzad 和其他人……

medium.com](https://medium.com/@fmnobar/membership?source=post_page-----dfc1c54d0ba7--------------------------------)

# 1\. 基础

## 1.1\. 概念与安装

首先定义一些使用 [Hyperopt](https://github.com/hyperopt/hyperopt) 的相关概念。

+   **目标函数：** 这是超参数优化尝试最小化的函数。更具体地说，目标函数接受一组超参数作为输入，并返回模型的错误水平（即损失），给定这些接受的超参数。超参数优化的目标是找到使该错误/损失最小化的超参数组合。

+   **搜索空间：** 目标函数接受作为参数的输入值范围（即参数）。

+   **优化算法：** 顾名思义，这是一种用于最小化目标函数的算法。Hyperopt 利用不同的搜索算法，例如随机搜索和 Parzen 估计树（TPE）（[文档](http://hyperopt.github.io/hyperopt/#algorithms)）。

现在我们对这些概念已经很熟悉了，让我们通过运行以下命令来安装 [Hyperopt](https://github.com/hyperopt/hyperopt)：

```py
pip install hyperopt
```

现在我们已经安装了库，我们将首先通过一个非常简单的示例来了解 Hyperopt 是如何工作的。之后，我们将继续处理更有趣和复杂的示例。

## 1.2\. 简单示例

让我们从一个非常简单的例子开始，以帮助我们理解使用 Hyperopt 进行超参数优化的整体过程。我们将从一个二次函数 `f(x) = (x — 1)²` 开始。这个函数的优化点在 `x = 1`，因此我们知道期望是什么。由于我们已经有一段时间没上过微积分课了，让我们查看该函数的图，这有助于我们更好地理解这个点如何最小化该函数。以下代码块将返回该图：

```py
# Import libraries
import numpy as np
import matplotlib.pyplot as plt

# Define the function
def f(x):
    return (x - 1) ** 2

# Generate x values from -5 to 5
x = np.linspace(-4, 6, 100)

# Calculate corresponding y values
y = f(x)

# Find the minimum point
min_point = np.min(y)

# Create the plot
plt.plot(x, y, label='f(x) = (x-1)²')
plt.xlabel('x')
plt.ylabel('f(x)')
plt.title('f(x) = (x-1)²')

# Set the x-axis limits
plt.xlim(-4, 6)

# Add a horizontal dashed line at the minimum point
plt.axhline(y=min_point, color='red', linestyle='dashed', label='Minimum Point')

# Add a legend
plt.legend()

# Display the plot
plt.show()
```

结果：

![](img/bb99a1173530a776353c596c75cda85c.png)

f(x) = (x-1)² 的图

正如我们所见，最小点发生在 `x=1`。让我们使用 Hyperopt 实现这一点，看看它是如何工作的。

为了实现这一点，我们将采取以下步骤：

1.  导入必要的库和包

1.  定义目标函数和搜索空间

1.  运行优化过程

1.  打印结果（即我们期望的优化点是 `x = 1`）

以下代码块，按照上述步骤：

```py
# 1\. Import necessary libraries and packages
from hyperopt import hp, fmin, tpe, Trials

# 2\. Define the objective function and the search space
def objective_function(x):
    return (x - 1)**2

search_space = hp.uniform('x', -2, 2)

# 3\. Run the optimization process

# Trials object to store the results
trials = Trials()

# Run the optimization
best = fmin(fn=objective_function, space=search_space, algo=tpe.suggest, trials=trials, max_evals=100)

# 4\. Print the results
print(best)
```

结果：

![](img/fad78aa76c9fe042b4cfd6230556f462.png)

“best” 返回模型能够找到的最佳超参数组合，在这种情况下，它几乎等于 `x = 1`，正如我们所期望的那样！实现 Hyperopt 的过程通常是相同的，现在我们已经完成了一个简单的示例，让我们继续一个更高级的示例。

# 2\. Hyperopt 实现

我们将实现以下两个独立的示例：

1.  支持向量机的分类

1.  使用随机森林回归器的回归

我们将详细讲解这两个示例中的每一个。

## 2.1\. 支持向量机和鸢尾花数据集

在之前的 文章 中，我使用了网格搜索、随机搜索和贝叶斯优化来进行超参数优化，数据集使用了 [scikit-learn 提供的鸢尾花数据集](https://scikit-learn.org/stable/auto_examples/datasets/plot_iris_dataset.html)。鸢尾花数据集包括三种不同的鸢尾花的花瓣和萼片长度，是分类练习中常用的数据集。在本篇文章中，我们将使用相同的数据集，但我们将使用支持向量机（SVM）作为模型，并对以下两个参数进行优化：

+   `C`：正则化参数，用于权衡训练样本的错误分类与决策面简洁性的 trade-off。

+   `gamma`：核系数，定义了单个训练样本的影响程度。gamma 值越大，其他样本必须越接近才能被影响。

由于本练习的目标是进行超参数优化，我不会深入探讨 SVM 的具体操作，但如果你感兴趣，我发现 [这篇](https://scikit-learn.org/stable/modules/svm.html) scikit-learn 的文章很有帮助。

我们将大致遵循之前简单示例中使用的相同步骤，但在最后也会可视化该过程：

1\. 导入必要的库和包

2\. 定义目标函数和搜索空间

3\. 运行优化过程

4\. 可视化优化过程

## 2.1.1\. 步骤 1 — 导入库和包

让我们先导入库和包，然后加载数据集。

```py
# Import libraries and packages
from sklearn import datasets
from sklearn.svm import SVC
from sklearn.model_selection import cross_val_score

# Load Iris dataset
iris = datasets.load_iris()
X = iris.data
y = iris.target
```

## 2.1.2\. 步骤 2 — 定义目标函数和搜索空间

让我们首先从定义目标函数开始，目标函数将训练一个 SVM，并返回交叉验证得分的负值——这是我们想要最小化的。请注意，我们最小化交叉验证得分的负值，以便与“最小化”目标函数的一般目标保持一致（而不是“最大化”交叉验证得分）。

```py
def objective_function(parameters):
    clf = SVC(**parameters)
    score = cross_val_score(clf, X, y, cv=5).mean()
    return -score
```

接下来我们将定义搜索空间，这些空间包括我们可以为 `C` 和 `gamma` 参数选择的值。请注意，我们将使用 Hyperopt 的 `hp.uniform(label, low, high)`，它返回一个在“low”和“high”之间均匀分布的值（[source](http://hyperopt.github.io/hyperopt/getting-started/search_spaces/)）。

```py
# Search Space
search_space = {
    'C': hp.uniform('C', 0.1, 10),
    'gamma': hp.uniform('gamma', 0.01, 1)
}
```

## 2.1.3\. 运行优化

与之前的简单示例相同，我们将使用 TPE 算法，并将结果存储在`Trials`对象中。

```py
# Trials object to store the results
trials = Trials()

# Run optimization
best = fmin(fn=objective_function, space=search_space, algo=tpe.suggest, trials=trials, max_evals=100)
```

结果：

![](img/bd5ea3f29056654e842c44cb30b39bb8.png)

## 2.1.4\. 可视化优化

正如我们从简单示例中记得的那样，“最佳”包括 Hyperopt 根据实施的优化策略找到的超参数集合。让我们看看结果！

```py
print(best)
```

![](img/8e1681e9afd70df805341e84a54f927d.png)

如预期的那样，现在我们有了一组超参数，这些超参数能够最小化优化函数，使用了 Hyperopt。

让我们直观地观察目标函数值如何随着超参数的变化而变化。我们将从定义一个名为`plot_obj_vs_hp()`的函数开始，该函数实现这个可视化。然后使用这个函数来可视化结果。一定要注意红点——它表示根据我们的超参数优化找到的最佳超参数组合！

```py
# Import libraries
import matplotlib.pyplot as plt

def plot_obj_vs_hp(trials, search_space, best):
    # Extract the results
    results = trials.trials

    # Create a list of hyperparameters
    hyperparameters = list(search_space.keys())

    # Create a new figure with 2 subplots side by side
    fig, axes = plt.subplots(1, 2, figsize=(12, 6))

    # Loop through hyperparameters and generate plots
    for idx, hp in enumerate(hyperparameters):
        # Extract the values of a given hyperparameter
        hp_values = [res['misc']['vals'][f'{hp}'] for res in results]

        # Flatten the list of values
        hp_values = [item for sublist in hp_values for item in sublist]

        # Extract the corresponding objective function values
        objective_values = [res['result']['loss'] for res in results]

        # Create the scatter plot
        axes[idx].scatter(hp_values, objective_values, label='Trial Hyperparameter Combinations')

        # Highlight the best hyperparameters
        axes[idx].scatter(best[hp], min(objective_values), color='red', label='Best Hyperparameter Combinations')
        axes[idx].set_xlabel(f'{hp}')
        axes[idx].set_ylabel('Loss')
        axes[idx].set_title(f'Loss vs. {hp}')
        axes[idx].legend(loc='upper right')

    plt.tight_layout()
    plt.show()
```

```py
# Plot optimization vs. hyperparameters in 2D
plot_obj_vs_hp(trials, search_space, best)
```

结果：

![](img/1d88ed76eee51130d11432d4d197d12e.png)

损失与 C 和 gamma 超参数的子图

请注意，由于`C`和`gamma`实际上彼此没有关系，我们将它们分别展示与目标函数变化的关系。由于我们希望目标函数最小化，所以我们寻找的是上述图表中最低的点，并且根据超参数优化的结果，我们知道我们要找的是`{'C': 5.164418859504847, 'gamma': 0.07084064498886927}`，这会导致目标函数损失约为-0.986，并由红点标示。

我也很好奇以三维方式查看这些图表，所以我创建了下面的函数来实现这一点。让我们看看图表。

```py
# Import libraries
from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt

# Define 3D plot function
def plot_obj_vs_hp_3d(trials, search_space, best):
    # Extract the results
    results = trials.trials

    # Create a list of hyperparameters
    hyperparameters = list(search_space.keys())

    # Extract the values of hyperparameters
    hp_values_0 = [res['misc']['vals'][f'{hyperparameters[0]}'] for res in results]
    hp_values_1 = [res['misc']['vals'][f'{hyperparameters[1]}'] for res in results]

    # Flatten the lists of values
    hp_values_0 = [item for sublist in hp_values_0 for item in sublist]
    hp_values_1 = [item for sublist in hp_values_1 for item in sublist]

    # Extract the corresponding objective function values
    objective_values = [res['result']['loss'] for res in results]

    # Create a new figure
    fig = plt.figure(figsize=(10, 7))

    # Add a 3D subplot
    ax = fig.add_subplot(111, projection='3d')

    # Create the scatter plot
    scatter = ax.scatter(hp_values_0, hp_values_1, objective_values, c=objective_values, cmap='viridis', label='Trial hyperparameters')

    # Highlight the best hyperparameters
    ax.scatter(best[hyperparameters[0]], best[hyperparameters[1]], min(objective_values), color='red', label='Best hyperparameters')

    # Add labels using hyperparameters from search_space
    ax.set_xlabel(hyperparameters[0])
    ax.set_ylabel(hyperparameters[1])
    ax.set_zlabel('Loss')
    ax.set_title('Loss Across Hyperparameters')
    fig.colorbar(scatter)
    ax.legend(loc='upper right')

    plt.show()
```

```py
# Plot optimization vs. hyperparameters in 3D
plot_obj_vs_hp_3d(trials, search_space, best)
```

结果：

![](img/8fab4f3c2de7119c4952281873bbacaa.png)

损失函数与 C 和 gamma 超参数的三维表示

诚然，这不太容易阅读，但我们还是试试看。我们在寻找最低的损失，即图表上最暗的点（红点几乎被一个黑点隐藏）。从视觉上看，它与我们之前生成的二维图表一致。

接下来，让我们专注于一个回归示例。

## 2.2\. 随机森林与糖尿病数据集

这个示例关注于一个回归模型，该模型试图测量基准期后一年内疾病的发展情况。这个数据集同样来自于[scikit-learn](https://scikit-learn.org/stable/datasets/toy_dataset.html#diabetes-dataset)，但不同之处在于这个数据集主要用于回归（而不是我们在鸢尾花示例中看到的分类）。

如果你有兴趣了解回归和分类之间的区别，请查看下面的文章。

[## 机器学习中的分类 vs. 回归 — 我该使用哪个？](https://medium.com/@fmnobar/classification-vs-regression-in-machine-learning-which-one-should-i-use-f6b24d251c46?source=post_page-----dfc1c54d0ba7--------------------------------)

### 概述

[medium.com](https://medium.com/@fmnobar/classification-vs-regression-in-machine-learning-which-one-should-i-use-f6b24d251c46?source=post_page-----dfc1c54d0ba7--------------------------------)

我们将使用一个 [随机森林回归器](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html) 模型进行本示例，并将优化以下两个超参数的目标函数：

+   `n_estimators`：随机森林中的树的数量

+   `max_depth`：随机森林中树的最大深度

优化的整体过程与我们迄今为止所做的相同。所以，让我们将其分解为四个常规步骤！

## 2.2.1\. 第 1 步 — 导入库和包

我们将从导入库和包开始，然后加载数据集。

```py
# Import libraries and packages
from sklearn import datasets
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import cross_val_score
from hyperopt import fmin, tpe, hp, Trials
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

# Load Diabetes dataset
diabetes = datasets.load_diabetes()
X = diabetes.data
y = diabetes.target
```

## 2.2.2\. 第 2 步 — 定义目标函数和搜索空间

与上次类似，我们首先定义目标函数，该函数将训练我们的随机森林回归器并返回交叉验证分数的负值。

接下来，我们将定义搜索空间，包括 `n_estimators` 和 `max_depth` 参数可以取的值。注意，我们将使用 Hyperopt 的 `hp.choice(label, options)`，它为超参数（即 `label`）提供一个值以及该超参数的可能值（即 `options`） ([source](http://hyperopt.github.io/hyperopt/getting-started/search_spaces/))。

```py
# Define objective function
def objective_function(parameters):
    # Initiate RandomForestRegressor
    regressor = RandomForestRegressor(**parameters)

    # Calculate the mean cross-validation score using 5 folds
    score = cross_val_score(regressor, X, y, cv=5).mean()

    return -score

# Define search Space
search_space = {
    'n_estimators': hp.choice('n_estimators', range(10, 300)),
    'max_depth': hp.choice('max_depth', range(1, 30)),
}
```

## 2.2.3\. 运行优化

与之前的示例相同，我们将使用 TPE 算法，并将结果存储在 `Trials` 对象中。

```py
# Trials object to store the results
trials = Trials()

# Run optimization
best = fmin(fn=objective_function, space=search_space, algo=tpe.suggest, trials=trials, max_evals=100)
```

结果：

![](img/2c540458ef2e3d1459e353463455f8a8.png)

## 2.2.4\. 可视化优化

正如我们从简单示例中记得的，“最佳”包括 Hyperopt 基于实施的优化策略找到的超参数集合。让我们来看看结果！

```py
print(best)
```

结果：

![](img/c88625fea1d6b8d0f5e1bd9c76ac864f.png)

正如预期的那样，现在我们有了一组最小化优化函数的超参数组合，使用 Hyperopt。让我们直观地观察目标函数值如何随着超参数的变化而变化，利用我们之前定义的函数创建二维和三维图。

```py
# Plot optimization vs. hyperparameters in 2D
plot_obj_vs_hp(trials, search_space, best)
```

结果：

![](img/5119e66d0173c56a30c3a4a776666070.png)

损失 vs. `n_estimators` 和 `max_depth` 超参数的子图

```py
# Plot optimization vs. hyperparameters in 3D
plot_obj_vs_hp_3d(trials, search_space, best)
```

结果：

![](img/84eeaca7c41ae4b0d984d03e79352c4b.png)

3D 损失函数与 `n_estimators` 和 `max_depth` 超参数的表示

# 结论

在这篇文章中，我们介绍了 [Hyperopt](https://github.com/hyperopt/hyperopt)——一个强大且开源的超参数优化工具，然后通过支持向量机进行分类和通过随机森林回归器进行回归的示例来讲解了实现过程。接着，我们查看了通过这些过程找到的最佳超参数组合，并在二维和三维中可视化了结果。

# 感谢阅读！

如果你觉得这篇文章对你有帮助，请 [关注我的 Medium](https://medium.com/@fmnobar) 并订阅以接收我最新的文章！

*(所有图片，除非另有说明，均由作者提供。)*
