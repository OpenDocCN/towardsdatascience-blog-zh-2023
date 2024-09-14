# XGBoost：权威指南（第二部分）

> 原文：[`towardsdatascience.com/xgboost-the-definitive-guide-part-2-c38ef02f74d0`](https://towardsdatascience.com/xgboost-the-definitive-guide-part-2-c38ef02f74d0)

## 从头开始用 Python 实现 XGBoost 算法

[](https://medium.com/@roiyeho?source=post_page-----c38ef02f74d0--------------------------------)![Roi Yehoshua 博士](https://medium.com/@roiyeho?source=post_page-----c38ef02f74d0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c38ef02f74d0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c38ef02f74d0--------------------------------) [Roi Yehoshua 博士](https://medium.com/@roiyeho?source=post_page-----c38ef02f74d0--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c38ef02f74d0--------------------------------) ·阅读时间 14 分钟 ·2023 年 8 月 15 日

--

![](img/798f47ae83cd1fe10d15404d5b3ca636.png)

图片由 [StockSnap](https://pixabay.com/users/stocksnap-894430/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2557468) 提供，来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2557468)

在 [上一篇文章](https://medium.com/towards-data-science/xgboost-the-definitive-guide-part-1-cc24d2dcd87a) 中，我们讨论了 XGBoost 算法并展示了其伪代码实现。在这篇文章中，我们将从头开始用 Python 实现该算法。

提供的代码是 XGBoost 算法的简洁轻量实现（仅约 300 行代码），旨在演示其核心功能。因此，它没有针对速度或内存使用进行优化，也没有包括 XGBoost 库提供的所有选项（有关库功能的更多细节，请参见 [`xgboost.readthedocs.io/`](https://xgboost.readthedocs.io/)）。更具体地说：

1.  代码是用纯 Python 编写的，而 XGBoost 库的核心是用 C++ 编写的（其 Python 类只是对 C++ 实现的薄包装）。

1.  它不包括允许 XGBoost 处理大量数据的各种优化，例如加权分位数草图、外部树学习以及数据的并行和分布式处理。这些优化将在系列文章的下一篇中详细讨论。

1.  目前的实现仅支持回归和二分类任务，而 XGBoost 库还支持多分类和排序问题。

1.  实现仅提供了 XGBoost 库中存在的超参数的一个小子集。具体而言，它支持以下超参数：

+   *n_estimators*（默认值 = 100）：集合中的回归树数量（也就是提升迭代的数量）。

+   *max_depth*（默认值 = 6）：每棵树的最大深度（级别数）。

+   *learning_rate*（默认值 = 0.3）：应用于树的步长收缩。

+   *reg_lambda*（默认值 = 1）：应用于叶子权重的 L2 正则化项。

+   *gamma*（默认值 = 0）：分割给定节点所需的最小损失减少。

为了保持一致性，我保持了这些超参数在 XGBoost 库中定义的名称和默认值。

尽管存在上述限制，但提供的实现所获得的结果与 XGBoost 库在多个基准数据集上获得的结果可比（稍后将在文章中演示）。

# 代码结构

源代码由五个类组成：

1.  XGBNode 表示回归树中的一个单独节点。

1.  XGBTree 表示集合中的回归树。

1.  XGBBaseModel 是 XGBoost 模型的基础估计器。

1.  XGBRegressor 是 XGBBaseModel 的一个子类，提供回归任务的估计器。

1.  XGBClassifier 是 XGBBaseModel 的一个子类，提供二分类任务的估计器。

为了更好的可读性和易于维护，每个类都写在自己的模块（.py）文件中。

完整的源代码可以在我的 github 上找到：[`github.com/roiyeho/medium/tree/main/xgboost/xgboost_from_scratch`](https://github.com/roiyeho/medium/tree/main/xgboost/xgboost_from_scratch)

我们现在将逐步讲解代码的每一部分。

# XGBNode 类

这个类表示回归树中的一个节点。它实现了算法中在给定节点找到最佳分割的核心功能。

在类的构造函数中，我们将节点初始化为非叶子节点，并将其左、右子节点的指针设置为 None：

```py
import numpy as np

class XGBNode:
    """A node object that recursively builds itself to construct a regression tree
    """
    def __init__(self):
        self.is_leaf: bool = False
        self.left_child: XGBNode = None
        self.right_child: XGBNode = None
```

*build*() 方法递归地构建节点，直到达到以下停止标准之一：

1.  节点上只剩下一个样本。

1.  当前树的层级已达到最大深度。

1.  从分割节点中我们可以获得的最佳增益（损失减少）小于 *gamma*。

```py
def build(self, X, grads, hessians, curr_depth, max_depth, reg_lambda, gamma):
    """Recursively build the node until a stopping criterion is reached
    """
    if len(X) == 1 or curr_depth >= max_depth:
        # Stopping criterion (1): there is only one sample left at this node
        # Stopping criterion (2): max depth of the tree has been reached 
        self.is_leaf = True
        self.weight = self.calc_leaf_weight(grads, hessians, reg_lambda)
        return

    best_gain, best_split = self.find_best_split(X, grads, hessians, reg_lambda)

    if best_gain < gamma:
        # Stopping criterion (3): the best gain is less than the minimum split gain 
        self.is_leaf = True
        self.weight = self.calc_leaf_weight(grads, hessians, reg_lambda)
        return        
    else:
        # Split the node according to the best split found
        feature_idx, threshold, left_samples_idx, right_samples_idx = best_split

        self.split_feature_idx = feature_idx
        self.split_threshold = threshold

        self.left_child = XGBNode()
        self.left_child.build(X[left_samples_idx],
                            grads[left_samples_idx],
                            hessians[left_samples_idx],
                            curr_depth + 1,
                            max_depth, reg_lambda, gamma)

        self.right_child = XGBNode()
        self.right_child.build(X[right_samples_idx],
                            grads[right_samples_idx],
                            hessians[right_samples_idx],
                            curr_depth + 1,
                            max_depth, reg_lambda, gamma)
```

如果已达到某个停止标准，我们将当前节点转换为叶子节点，然后通过调用辅助方法 *calc_leaf_weight*() 计算其权重（分数）：

```py
def calc_leaf_weight(self, grads, hessians, reg_lambda):
    """Calculate the optimal weight of this leaf node (eq.(5) in [1])
    """
    return -np.sum(grads) / (np.sum(hessians) + reg_lambda)
```

另一方面，如果节点可以被分割，我们调用辅助方法 *find_best_split*()，该方法遍历所有可能的分割并返回增益最高的分割：

```py
def find_best_split(self, X, grads, hessians, reg_lambda):
    """Scan through every feature and find the best split point (Algorithm 1 in [1])
    """
    G = np.sum(grads)
    H = np.sum(hessians)

    best_gain = float('-inf')   
    best_split = None

    # Iterate over all the possible features
    for j in range(X.shape[1]):
        G_left, H_left = 0, 0

        # Sort the samples according to their value in the current feature
        sorted_samples_idx = np.argsort(X[:, j])

        # Calculate the gain of every possible split point
        for i in range(X.shape[0] - 1):   
            G_left += grads[sorted_samples_idx[i]]
            H_left += hessians[sorted_samples_idx[i]]

            G_right = G - G_left
            H_right = H - H_left
            curr_gain = self.calc_split_gain(G, H, G_left, H_left, G_right, H_right, reg_lambda)

            if curr_gain > best_gain:
                # Update the properties of the best split
                best_gain = curr_gain     
                feature_idx = j 
                threshold = X[sorted_samples_idx[i]][j]
                left_samples_idx = sorted_samples_idx[:i + 1]
                right_samples_idx = sorted_samples_idx[i + 1:]
                best_split = (feature_idx, threshold, left_samples_idx, right_samples_idx)

    return best_gain, best_split
```

*find_best_split*() 函数使用以下辅助函数来计算给定分割获得的增益：

```py
def calc_split_gain(self, G, H, G_left, H_left, G_right, H_right, reg_lambda):
    """Compute the loss reduction (eq. (7) in [1])
    """
    def calc_term(g, h):
        return g**2 / (h + reg_lambda)

    gain = calc_term(G_left, H_left) + calc_term(G_right, H_right) - calc_term(G, H)
    return 0.5 * gain
```

最后，*predict*() 方法根据样本被映射到的叶子节点的权重返回给定样本的分数。它使用树节点中保存的最佳分割阈值来确定沿着树的哪个路径进行。

```py
def predict(self, x):
    """Return the score of a given sample x
    """
    if self.is_leaf:
        return self.weight
    else:
        if x[self.split_feature_idx] <= self.split_threshold:
            return self.left_child.predict(x)
        else:
            return self.right_child.predict(x)
```

# XGBTree 类

这个类表示一个单独的回归树。在类的构造函数中，我们将树的根节点初始化为 None：

```py
from xgb_node import XGBNode

class XGBTree:
    """A single tree object that will be used for gradient boosting
    """
    def __init__(self):
        self.root: XGBNode = None
```

在 *build*() 方法中，我们简单地调用根节点的 *build*() 方法，并将训练算法的超参数传递给它：

```py
def build(self, X, grads, hessians, max_depth, reg_lambda, gamma):
    """Recursively build the root node of the tree 
    """
    self.root = XGBNode()
    curr_depth = 0
    self.root.build(X, grads, hessians, curr_depth, max_depth, reg_lambda, gamma)
```

最后，*predict*() 方法通过委托调用根节点的 *predict*() 方法来返回给定样本的评分：

```py
def predict(self, x):
    """Return the score of a given sample x
    """
    return self.root.predict(x)
```

# XGBBaseModel 类

现在我们构建 XGBoost 估计器的基类。为了使这个类兼容 Scikit-Learn 估计器 API，我们将其作为 BaseEstimator（来自 sklearn.base）的子类，并实现 *fit*() 和 *predict*() 方法。这将允许我们将这个估计器与其他 Scikit-Learn 机制（如管道和网格搜索）集成。

我们首先导入所需的库：

```py
import numpy as np

from sklearn.base import BaseEstimator
from xgb_tree import XGBTree
from typing import List
from abc import ABC, abstractmethod
```

在类的构造函数中，我们初始化模型的超参数：

```py
class XGBBaseModel(ABC, BaseEstimator):
    """Base class for the XGBoost estimators
    """
    def __init__(
        self,
        n_estimators=100,     # The number of trees (boosting rounds)
        max_depth=6,          # Maximum depth of a tree        
        learning_rate=0.3,    # Step size shrinkage applied to the leaf weights
        reg_lambda=1,         # L2 regularization term on the leaf weights
        gamma=0,              # Minimum loss reduction required to split a node
        verbose=0             # Verbosity of the log messages (change to 1 for debug mode)
    ):
        self.n_estimators = n_estimators        
        self.max_depth = max_depth       
        self.learning_rate = learning_rate
        self.reg_lambda = reg_lambda
        self.gamma = gamma
        self.verbose = verbose
```

*fit*() 方法为给定的数据集构建一个 XGBoost 树的集成：

```py
def fit(self, X, y):
    """Build an ensemble of trees for the given training set
    """
    # Get the initial prediction of the ensemble
    self.base_pred = self.get_base_prediction(y)

    self.estimators: List[XGBTree] = []
    for i in range(self.n_estimators):
        # Compute the first and second order gradients of the loss function with respect
        # to the predictions of the current ensemble
        out = self.get_output_values(X)
        grads = self.calc_gradients(y, out)
        hessians = self.calc_hessians(y, out)

        # Add a new tree to the ensemble
        tree = XGBTree()
        tree.build(X, grads, hessians, self.max_depth, self.reg_lambda, self.gamma)
        self.estimators.append(tree)

        if self.verbose and i % 10 == 0:
            print(f'Boosting iteration {i}')
    return self
```

*fit*() 方法使用一个叫做 *get_output_values*() 的辅助方法来获取当前集成对给定数据集的预测值。请注意，输出值不一定与模型的最终预测标签相同。例如，在分类任务中，集成的输出值是预测的对数几率，而不是类标签。

```py
def get_output_values(self, X):
    """Return the predicted values of the ensemble for the given data set
    """
    # Initialize the output values with the base prediction
    output = np.full(X.shape[0], self.base_pred)

    # Add the predictions of the base trees scaled by the learning rate
    if len(self.estimators) > 0:
        for i in range(len(X)):            
            output[i] += np.sum(self.learning_rate * estimator.predict(X[i]) 
                                    for estimator in self.estimators)
    return output
```

最后，我们定义了几个需要具体估计器实现的抽象方法：

```py
@abstractmethod
def get_base_prediction(self, y):
    """Return the initial prediction of the model"""
    pass

@abstractmethod
def calc_gradients(self, y, out):
    """Calculate the first order gradients""" 
    pass

@abstractmethod
def calc_hessians(self, y, out):
    """Calculate the second order gradients"""
    pass

@abstractmethod
def predict(self, X):
    """Return the final predicted labels for the given samples"""
    pass
```

# XGBRegressor 类

这是 XGBBaseModel 的一个子类，用于解决回归任务。要实现这个类，我们首先导入所需的库和函数：

```py
import numpy as np

from xgb_base_model import XGBBaseModel
from sklearn.metrics import r2_score
```

类的构造函数只是将超参数传递给基估计器的构造函数：

```py
class XGBRegressor(XGBBaseModel):
    """An XGBoost estimator for regression tasks
    """
    def __init__(
        self, 
        n_estimators=100, 
        max_depth=6,         
        learning_rate=0.3, 
        reg_lambda=1, 
        gamma=0,
        verbose=0
    ):
        super().__init__(n_estimators, max_depth, learning_rate, reg_lambda, gamma, verbose)
```

回归器的基预测只是目标值的均值：

```py
def get_base_prediction(self, y):
    # The initial prediction is the mean of the targets
    return np.mean(y)
```

平方损失的梯度和赫西矩阵是使用 [前一篇文章](https://medium.com/towards-data-science/xgboost-the-definitive-guide-part-1-cc24d2dcd87a)（参见“XGBoost for Regression”部分）中给出的方程计算的：

```py
def calc_gradients(self, y, out):
    # The first order gradients are twice the residuals
    grads = 2 * (out - y)
    return grads

def calc_hessians(self, y, out):
    # The second order gradients are equal to the constant 2
    hessians = np.full(len(y), 2)
    return hessians
```

*predict*() 方法返回来自集成的输出值（在回归任务中，预测标签与集成的输出值相同）：

```py
def predict(self, X):
    # The predicted labels are the same as the output values
    y_pred = self.get_output_values(X)
    return y_pred
```

最后，我们添加一个 *score*() 方法来返回模型在给定数据集上的 *R*² 分数：

```py
def score(self, X, y):
    y_pred = self.predict(X)
    return r2_score(y, y_pred)
```

# 回归示例

经过这些艰苦的工作，我们现在准备测试我们的 XGBoostRegressor！

首先，我们将使用 [make_regression](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.make_regression.html)() 函数从 Scikit-Learn 生成一个合成数据集。该函数的默认设置生成具有 100 个样本和 100 个特征的随机线性回归数据，其中只有 10 个特征是有信息的（即，与目标相关）。

```py
from sklearn.datasets import make_regression
X, y = make_regression(random_state=0)
```

请注意，像 XGBoost 这样的基于树的模型不需要对特征进行缩放或归一化。

接下来，我们将数据集拆分为 80% 的训练集和 20% 的测试集：

```py
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)
```

为了在这个数据集上评估不同的模型，让我们编写一个通用的工具函数来训练给定模型，测量其训练时间，并评估其在训练集和测试集上的表现：

```py
def evaluate_model(name, model, X_train, y_train, X_test, y_test):
    print(name)
    print('-' * 30)    
    train_start_time = time.time()
    model.fit(X_train, y_train)
    elapsed = time.time() - train_start_time
    print(f'Training time: {elapsed:.3f} sec')

    train_score = model.score(X_train ,y_train)
    print(f'R2 score (train): {train_score:.4f}')
    test_score = model.score(X_test, y_test)
    print(f'R2 score (test): {test_score:.4f}')
```

让我们开始在数据集上评估我们对 XGBRegressor 的实现。我们将使用类中定义的默认超参数设置。

```py
from xgb_regressor import XGBRegressor

my_xgb_reg = XGBRegressor()
evaluate_model('XGBoost (custom)', my_xgb_reg, X_train, y_train, X_test, y_test)
```

我们得到的结果是：

```py
XGBoost (custom)
------------------------------
Training time: 14.522 sec
R2 score (train): 1.0000
R2 score (test): 0.3218
```

我们的模型在训练集上获得了完美的*R*²分数，但在测试集上得分相对较低。

让我们将其性能与 xgboost 库中的 XGBClassifier 进行比较。首先，通过运行以下命令确保你已安装了该库：

```py
pip install xgboost
```

我们现在可以从此包中定义的 XGBRegressor 类创建一个实例。由于它的名称与我们的类相同，我们将使用其完全限定名称（包括模块名）：

```py
import xgboost

original_xgb_reg = xgboost.XGBRegressor()
evaluate_model('XGBoost (original)', original_xgb_reg, X_train, y_train, X_test, y_test)
```

我们得到的结果是：

```py
XGBoost (original)
------------------------------
Training time: 1.074 sec
R2 score (train): 1.0000
R2 score (test): 0.3077
```

令人惊讶的是，我们的模型在*R*²测试分数上略优于原始的 XGBoost！然而，它的训练时间慢得多（大约慢 14 倍）。这是因为我们没有实现任何运行时优化（而且我们的代码是用 Python 编写的，而不是 C++）。

为了完整性，让我们将这些模型与 Scikit-Learn 提供的经典梯度提升和基于直方图的梯度提升进行比较：

```py
from sklearn.ensemble import GradientBoostingRegressor
gboost_reg = GradientBoostingRegressor(random_state=0)
evaluate_model('Gradient boosting', gboost_reg, X_train, y_train, X_test, y_test)

from sklearn.ensemble import HistGradientBoostingRegressor
hist_gboost_reg = HistGradientBoostingRegressor(random_state=0)
evaluate_model('Histogram-based gradient boosting', hist_gboost_reg, X_train, y_train, X_test, y_test)
```

```py
Gradient boosting
-----------------------------------
Training time: 0.129 sec
R2 score (train): 1.0000
R2 score (test): 0.4376

Histogram-based gradient boosting
-----------------------------------
Training time: 0.188 sec
R2 score (train): 0.9756
R2 score (test): 0.3984
```

在这种情况下，最佳测试分数是由经典梯度提升算法获得的。请注意，所有模型都使用了其默认超参数，因此这些结果可能会在超参数调整后发生变化。

让我们也在一个真实世界的数据集上评估我们的实现，即[加州住房数据集](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.fetch_california_housing.html)，它来自 Scikit-Learn。这次我们将通过将所有模型定义在一个列表中，然后在循环中调用评估函数，来使评估代码更加简洁：

```py
from sklearn.datasets import fetch_california_housing

data = fetch_california_housing()
X, y = data.data, data.target

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)
names = ['XGBoost (custom)', 'XGBoost (original)', 'Gradient boosting', 'Histogram-based gradient boosting', 
         'Linear regression']
models = [XGBRegressor(), 
          xgboost.XGBRegressor(), 
          GradientBoostingRegressor(random_state=0), 
          HistGradientBoostingRegressor(random_state=0),
          LinearRegression()]

for name, model in zip(names, models):
    evaluate_model(name, model, X_train, y_train, X_test, y_test)
```

我们得到以下结果：

```py
XGBoost (custom)
-----------------------------------
Training time: 434.099 sec
R2 score (train): 0.9026
R2 score (test): 0.8253

XGBoost (original)
-----------------------------------
Training time: 1.003 sec
R2 score (train): 0.9396
R2 score (test): 0.8360

Gradient boosting
-----------------------------------
Training time: 2.918 sec
R2 score (train): 0.8027
R2 score (test): 0.7774

Histogram-based gradient boosting
-----------------------------------
Training time: 0.975 sec
R2 score (train): 0.8801
R2 score (test): 0.8381

Linear regression
-----------------------------------
Training time: 0.007 sec
R2 score (train): 0.6089
R2 score (test): 0.5943
```

我们模型的*R*²测试分数略低于 XGBoost 库的分数（0.825 对比 0.836），但显著高于经典梯度提升算法的分数（0.777）。正如预期的那样，我们模型的训练时间相比其他模型要长得多。

# XGBClassifier 类

这也是 XGBBaseModel 的一个子类，用于解决分类任务（目前只支持二分类）。要实现此类，我们首先导入所需的库和函数：

```py
import numpy as np

from xgb_base_model import XGBBaseModel
from sklearn.metrics import accuracy_score
```

类的构造函数简单地将超参数传递给基础估计器的构造函数：

```py
class XGBClassifier(XGBBaseModel):
    """An XGBoost estimator for binary classification tasks
    """
    def __init__(
        self, 
        n_estimators=100, 
        max_depth=6,         
        learning_rate=0.3, 
        reg_lambda=1, 
        gamma=0,
        verbose=0
    ):
        super().__init__(n_estimators, max_depth, learning_rate, reg_lambda, gamma, verbose)
```

分类器的基础预测是正类的对数几率：

```py
def get_base_prediction(self, y):
    # The initial prediction is the log odds of the positive class
    prob = np.sum(y == 1) / len(y)
    return np.log(prob / (1 - prob))
```

为了计算对数损失的梯度，我们需要 sigmoid 函数，因此首先实现它：

```py
def sigmoid(self, x):
    return 1 / (1 + np.exp(-x))
```

对数损失的梯度和 Hessian 是使用[前一篇文章](https://medium.com/towards-data-science/xgboost-the-definitive-guide-part-1-cc24d2dcd87a)（见“XGBoost 分类”部分）中给出的方程计算的：

```py
def calc_gradients(self, y, out):
    # The first order gradients are the residuals between the predicted probabilities 
    # (sigmoid of the log odds) and the true labels
    prob = self.sigmoid(out)    
    grads = prob - y
    return grads

def calc_hessians(self, y, out):
    # The second order gradients are p(1 - p) where p is the predicted probability
    prob = self.sigmoid(out)
    hessians = prob * (1 - prob)
    return hessians
```

类似于 Scikit-Learn 中的其他概率分类器（如 LogisticRegression），我们的分类器将提供*predict_proba*() 和 *predict*() 方法。

方法 *predict_proba*() 返回给定样本的正类预测概率。这些概率是通过对集成返回的对数赔率进行 sigmoid 计算得到的：

```py
def predict_proba(self, X):
    # The predicted probability is the sigmoid of the log odds
    log_odds = self.get_output_values(X)
    prob = self.sigmoid(log_odds)
    return prob
```

方法 *predict*() 返回给定数据集中样本的类别标签，使用 0.5 作为将样本分配给正类的阈值：

```py
def predict(self, X):
    # Assign a label of 1 if the probability of the positive class is > 0.5,
    # otherwise assign a label of 0
    prob = self.predict_proba(X)
    y_pred = np.where(prob > 0.5, 1, 0)
    return y_pred
```

最后，我们添加了一个*score*() 方法，返回模型在给定数据集上的准确率：

```py
def score(self, X, y):
    y_pred = self.predict(X)
    return accuracy_score(y, y_pred)
```

# 分类示例

为了评估我们的 XGBoostClassifier，我们将使用 [make_classification](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.make_classification.html)() 函数来自 Scikit-Learn。该函数在* n *维超立方体的顶点周围创建正态分布点的簇（默认 100 个点），边长由名为*class_sep*的超参数定义（默认 1.0）。

首先，我们将使用该函数生成 1,000 个样本点，类分离度为 0.1：

```py
from sklearn.datasets import make_classification
X, y = make_classification(n_samples=1000, class_sep=0.1, random_state=0)
```

类似于回归示例，我们将编写一个通用实用函数来评估给定模型并测量其训练时间：

```py
def evaluate_model(name, model, X_train, y_train, X_test, y_test):
    print(name)
    print('-' * 35)
    train_start_time = time.time()
    model.fit(X_train, y_train)
    elapsed = time.time() - train_start_time
    print(f'Training time: {elapsed:.3f} sec')

    train_score = model.score(X_train ,y_train)
    print(f'Accuracy (train): {train_score:.4f}')
    test_score = model.score(X_test, y_test)
    print(f'Accuracy (test): {test_score:.4f}')
    print()
```

我们将比较以下算法：

```py
names = ['XGBoost (custom)', 'XGBoost (original)', 'Gradient boosting', 'Histogram-based gradient boosting', 
         'Logistic regression']
models = [XGBClassifier(), 
          xgboost.XGBClassifier(), 
          GradientBoostingClassifier(random_state=0), 
          HistGradientBoostingClassifier(random_state=0),
          LogisticRegression()]

for name, model in zip(names, models):
    evaluate_model(name, model, X_train, y_train, X_test, y_test)
```

我们得到的结果是：

```py
XGBoost (custom)
-----------------------------------
Training time: 39.782 sec
Accuracy (train): 1.0000
Accuracy (test): 0.6900

XGBoost (original)
-----------------------------------
Training time: 1.111 sec
Accuracy (train): 1.0000
Accuracy (test): 0.6850

Gradient boosting
-----------------------------------
Training time: 0.409 sec
Accuracy (train): 0.9213
Accuracy (test): 0.6550

Histogram-based gradient boosting
-----------------------------------
Training time: 0.810 sec
Accuracy (train): 1.0000
Accuracy (test): 0.6600

Logistic regression
-----------------------------------
Training time: 0.007 sec
Accuracy (train): 0.5962
Accuracy (test): 0.5300
```

我们的模型在测试集上达到了最高的准确率！但它的速度比其他模型显著慢（比 XGBoost 库慢约 36 倍）。

# 练习

提供的实现可以在许多方面进行改进和扩展。例如，您可以尝试向代码库中添加以下功能：

1.  添加对多类别分类的支持。

1.  添加对分类特征的支持。

1.  实现早期停止，即当验证集上的分数在指定的轮数内没有改善时停止提升。

1.  添加对子采样的支持，即仅在训练数据的随机子集上训练每棵树。

1.  添加额外的超参数，例如*min_class_weight*，定义在子节点中所需的最小实例权重（Hessian）的总和。

在下一篇文章中，我们将讨论 XGBoost 库实现的各种硬件和软件优化，使其运行如此迅速。

感谢阅读！

## 参考文献

[1] 陈天石，& 顾斯特林，C.（2016）。XGBoost: 一个可扩展的树提升系统。*第 22 届 ACM SIGKDD 国际知识发现与数据挖掘大会论文集*，785–794。

加州住房数据集信息：

**引用**：Pace, R. Kelley 和 Ronald Barry（1997），稀疏空间自回归，统计与概率通讯，33，291–297。

**许可证**：创作共用 CC0：公有领域。
