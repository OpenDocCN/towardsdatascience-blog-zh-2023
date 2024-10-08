# Bagging 是否有助于防止决策树的过拟合？

> 原文：[`towardsdatascience.com/does-bagging-help-to-prevent-overfitting-in-decision-trees-42262943a81e`](https://towardsdatascience.com/does-bagging-help-to-prevent-overfitting-in-decision-trees-42262943a81e)

## 理解为什么决策树高度容易过拟合及其潜在的补救措施

[](https://medium.com/@gurjinderkaur95?source=post_page-----42262943a81e--------------------------------)![Gurjinder Kaur](https://medium.com/@gurjinderkaur95?source=post_page-----42262943a81e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----42262943a81e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----42262943a81e--------------------------------) [Gurjinder Kaur](https://medium.com/@gurjinderkaur95?source=post_page-----42262943a81e--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----42262943a81e--------------------------------) ·12 分钟阅读·2023 年 12 月 13 日

--

![](img/49dc05d1ffe1c84260864fe2ab15c16c.png)

图片由[Jan Huber](https://unsplash.com/@jan_huber?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

决策树是一类机器学习算法，以其解决分类和回归问题的能力而闻名，同时它们提供了易于解释的优点。然而，如果没有得到适当控制，它们容易出现过拟合问题，可能无法很好地泛化。

在本文中，我们将讨论什么是过拟合，决策树在多大程度上会对训练数据过拟合，为什么这是一个问题，以及如何解决它。

然后，我们将了解一种集成技术，即*bagging*，并看看它是否可以用来使决策树更强健。

我们将涵盖以下内容：

+   使用 NumPy 创建我们的回归数据集。

+   使用 scikit-learn 训练一个决策树模型。

+   通过查看同一模型在训练集和测试集上的表现，理解过拟合的含义。

+   讨论为什么在非参数模型（如决策树）中过拟合更为常见（当然也了解一下非参数的含义），以及如何通过正则化来防止过拟合。

+   了解什么是*自助聚合*（简称*bagging*），以及它如何有可能帮助解决过拟合问题。

+   最后，我们将实现决策树的 bagging 版本，看看它是否有帮助🤞

**还在犹豫是否值得阅读？** 🤔 如果你曾经想知道为什么随机森林通常比普通的决策树更受欢迎，这是最好的起点，因为随机森林使用了*bagging 加上一些其他方法*来改进决策树。

让我们开始吧！

我们首先将设置一个 Python 笔记本并导入相关库。

```py
import pandas as pd
import numpy as np
import plotly.graph_objects as go
from sklearn.tree import DecisionTreeRegressor
from sklearn import tree
from sklearn.model_selection import train_test_split
```

## **步骤 1：创建数据集**

我们将使用一个类似于二次函数的数据集，其中 *y* 为目标变量，*X* 为自变量。由于 *y* 是数值型的，我们将对这个数据集拟合一个回归树。让我们按如下方式构建数据集：

```py
np.random.seed(0)

# Constants for the quadratic equation
a, b, c = 1, 2, 3

# Create a DataFrame with a single feature
n = 500  # number of data points
x = np.linspace(-10, 10, n)  # feature values from -10 to 10
noise = np.random.normal(0, 10, n)  # some random noise
y = a * x**2 + b * x + c + noise  # quadratic equation with noise

data = pd.DataFrame({'X': x, 'y': y})
data["X"] = data["X"].round(3)
data["y"] = data["y"].round(3)
```

我们创建了一个包含 500 个样本的数据集，其中 *X* 和 *y* 都是连续的，如下所示。完整的笔记本及其可视化的链接可以在本文末尾找到，所以不用担心本文中缺失的可视化代码。

![](img/0437751797643f16d797f0f7643e3809.png)

来源：作者提供的图片

## **步骤 2：训练-测试拆分**

我们可以使用 scikit-learn 的 *train_test_split* 将数据集拆分为训练集和测试集，如下所示：

```py
X_train, X_test, y_train, y_test = train_test_split(data[["X"]], 
                        data["y"], test_size=0.2, random_state=0)

train_df = pd.concat([X_train, y_train], axis=1)
test_df = pd.concat([X_test, y_test], axis=1)

print(f"Shape of Training DF: {train_df.shape}")
print(f"Shape of Test DF: {test_df.shape}")
```

`单元输出：`

![](img/fd12191c5e8907a2adb39bb4d9e74aba.png)

我们将仅使用训练集来训练我们的模型，并将测试集保留用于测试模型的性能。这将确保我们测试模型的样本是模型从未见过的，从而帮助我们评估模型的泛化能力。聪明吧？ 😎

好的，接下来是我们的训练集和测试集的样子：

![](img/f8bd44f240b3660c4fc379aa69a2b617.png)

来源：作者提供的图片

## **步骤 3：在训练集上拟合回归树**

使用 scikit-learn 拟合决策树回归器只需两行代码。然而，如果你不确定背后发生了什么，并且是一个好奇的学习者，[这篇文章](https://medium.com/p/fbb908cf548b) 将是你了解决策树如何解决回归问题的必备指南。

```py
# Fit the regression tree
regressor = DecisionTreeRegressor()
regressor.fit(X_train, y_train)
```

`单元输出：`

![](img/8f2679ca1bf0088971339172164d23c4.png)

## **步骤 4：在训练集和测试集上评估回归树**

现在，既然我们的模型已经训练好了，让我们用它来进行预测：

+   **训练集** 即模型已经“熟悉”的数据。

+   **测试集** 即模型从未见过的数据（***真正的考验就在这里***）。

我们将使用均方误差来评估我们预测的质量。

```py
# Predict for training data and compute mean squared error
train_yhat = regressor.predict(X_train)
train_mse = mean_squared_error(y_train, train_yhat)

# Predict for test data and compute mean squared error
test_yhat = regressor.predict(X_test)
test_mse = mean_squared_error(y_test, test_yhat)

print(f"MSE on training set: {np.round(train_mse, 3)}")
print(f"MSE on test set: {np.round(test_mse, 3)}")
```

`单元输出：`

![](img/c2ad8618288b2a377c418f63a9ed893b.png)

决策树回归器在训练集上的误差为零。我们几乎要将这个模型冠以“史上最伟大”的称号，直到我们看到测试集上的结果，那时我们才停下 🥶

相同的模型在测试集上的均方误差达到了惊人的 173.336。看来它在真实测试中表现糟糕 😔

这是因为模型过度拟合（过度学习、过度依赖、过度填充、过度记忆）了训练数据点，导致它未能学习数据中的潜在模式；相反，它抓住了仅对训练集特有的噪声，而与数据的整体行为无关。这被称为 ***过拟合。***

> **过拟合**是指模型在训练期间对示例标签的预测非常准确，但在应用于未见过的示例时经常出现错误的特性——安德里·布尔科夫（《百页机器学习书》）

在以下图表中，我们可以看到训练集的预测值与实际值完全重叠，但测试集却不是。

![](img/41b4140e94e371aa57a713bdc3d10df4.png)

来源：作者提供的图片

以下是给定训练数据和测试数据的预测结果。可以清楚地看到，模型试图非常紧密地拟合训练数据。 `Depth=None` 表示除非特别指定，否则树可以达到的最大深度没有限制。这是 *max_depth* 超参数的默认值。

![](img/53e9d9b07733811a938afe0e59d22531.png)

来源：作者提供的图片

***仅仅思考：*** *“过拟合”一词比“紧拟合”更能准确表达模型的实际行为* 🤷🏻‍♀️ 不过，还是不要打破规则，坚持使用过拟合。

## **为什么过拟合在决策树中自然发生？**

决策树是 *非参数* 的，即它们不对训练数据做出任何假设。如果没有限制，树的结构将完全适应训练数据，紧密拟合，很可能会导致过拟合。

决策树被称为 *非参数* 不是因为它没有参数，而是因为参数的数量在训练之前没有确定，因此模型结构可以自由地紧贴训练数据（*与线性回归不同，线性回归有固定数量的系数，即我们希望模型学习的参数，所以其自由度是有限的*）

## **为什么过拟合是一个问题？**

过拟合是不受欢迎的，因为它不允许模型在新数据上进行良好的泛化，如果发生这种情况，模型将无法在其最初设计的分类或预测任务中表现良好。

## **我们本可以做得不同**

如果我们的模型出现过拟合的迹象，我们可以推断模型过于复杂，需要***正则化*。**

> 正则化是通过对模型施加一些约束来限制其自由度的过程，从而减少过拟合的机会。

可以调整几个超参数，例如最大深度、叶子节点中的最小样本数等，以对决策树进行正则化。

我们可以做的最少措施是将 *max_depth* 设置为限制树的过度生长。默认情况下，*max_depth* 的值设置为 *None*，意味着对决策树的生长没有限制。减少 *max_depth* 会对模型进行正则化，从而降低过拟合的风险。

以下是不同*max_depth*值的训练集和测试集的预测与实际结果图。

![](img/307b5d4c500bb509fba6ae041a4709e9.png)

来源：作者插图

## **你注意到权衡了吗？**

+   随着*max_depth*的增加，模型在训练集上的表现越来越好，但在测试集上的表现却越来越差。

+   增加*max_depth*使模型更复杂，从而降低其泛化能力。*这与高* ***方差* 相同。

+   减少*max_depth*使模型更简单，因此可能会发生欠拟合（这发生在模型即使在训练集上也表现不佳，更不用说测试集了）。*这与高* ***偏差* 相同。

在下图中，我们可以看到不同*max_depth*值的模型预测，这有助于我们理解高偏差导致欠拟合，而高方差导致过拟合。

![](img/efb0f439058fd1b7c4aad2d47c017786.png)

理解偏差-方差权衡（来源：作者插图）

尝试减少偏差会增加方差，反之亦然。我们需要找到一个最佳点，使偏差和方差都不太高但也不太低。**这就是偏差-方差权衡。**

好消息是我们不需要手动完成这一任务。我们可以利用自动超参数调整和交叉验证来找到最佳的正则化超参数值，这些值不仅限于*max_depth*。

## **过拟合是唯一的问题吗？**

*短答案：* 不（但这不太有帮助，你仍然需要阅读长答案，对不起😅）

*长答案：* 你可能会想，如果通过正则化可以防止过拟合，那么为何还需要装袋或其他集成技术。问题在于，除了过拟合之外，***决策树还容易出现不稳定性。***

决策树对数据集中的小变化非常敏感。即使是训练数据的微小变化也会导致完全不同的决策树。这种不稳定性可以通过在数据的随机子样本上训练多个树，然后对这些树的预测结果进行平均来限制。

## **集成学习的思想**

*集成* 是一组模型，而聚合这些模型预测的技术被称为***集成学习。***

集成学习有两种方法：

+   对每个预测器使用不同的训练算法，如决策树、SVM 等，并在给定的训练集上进行训练。

+   对每个预测器使用相同的训练算法，并在训练集的不同子集上进行训练。*装袋属于这一类别。*

## **装袋简介**

装袋是***bootstrap aggregation***的简称。

> 装袋是一种集成方法，其中多个模型在训练集的不同随机子样本上进行训练，并且抽样是有放回的。

带替换采样意味着某些实例可能会被多个预测器多次采样，而其他实例可能根本不会被采样。这确保了对训练数据中微小变化的敏感性得到考虑，并且不再影响最终集成的稳定性。

![](img/a8efccce1215892f87eb106f225e6a04.png)

袋装示意图 (作者提供的图片)

***注意:*** *我们可以选择带替换或不带替换地对训练集进行子采样。* ***当采样是带替换的，这被称为袋装。当采样是不带替换的，这被称为过去法。***

一旦所有模型在训练数据的随机子样本上训练完成，它们的预测可以被聚合为：

+   回归预测的平均

+   分类的多数投票

现在我们对集成学习和袋装有了一定了解，让我们在 scikit-learn 中实现它。接下来让我们在笔记本中继续执行以下步骤。

## **步骤 5: 在 scikit-learn 中实现袋装**

我们可以简单地将决策树回归器传递给袋装回归器，并指定我们想要训练的模型数量 (*n_estimators*)，以及每个模型训练时考虑的样本数量 (*max_samples*)。

在这里，`bootstrap=True` 意味着数据将进行带替换的采样，如果我们想使用过去法而不是袋装，可以将 `bootstrap=False` 设置为 False。

```py
from sklearn.ensemble import BaggingRegressor

bag_regressor = BaggingRegressor(
    DecisionTreeRegressor(), n_estimators=200,
    max_samples=100, bootstrap=True, n_jobs=-1
)

bag_regressor.fit(X_train, y_train)
```

`单元输出:`

![](img/750e3796c72cc86d955b7d1b67cac124.png)

这意味着我们分别训练了 200 棵决策树，每棵决策树都使用了大小为 100 的随机子样本作为训练集。最终预测将是所有预测的平均值。

## **步骤 6: 评估决策树回归器的袋装版本**

我们将再次使用均方误差来评估模型在训练集和测试集上的样本预测效果。

```py
# Predict for training data and compute mean squared error
bag_train_yhat = bag_regressor.predict(X_train)
bag_train_mse = mean_squared_error(y_train, bag_train_yhat)

# Predict for test data and compute mean squared error
bag_test_yhat = bag_regressor.predict(X_test)
bag_test_mse = mean_squared_error(y_test, bag_test_yhat)

print(f"MSE on training set: {np.round(bag_train_mse, 3)}")
print(f"MSE on test set: {np.round(bag_test_mse, 3)}")
```

`单元输出:`

![](img/d59594848411f96d374f5a0a10075b58.png)

使用袋装后，训练的均方误差从 0 上升到 69.438，但测试的均方误差从 173.336 下降到 101.521，这确实是一个改进！

我们可以从下面的图中验证，经过袋装集成的决策树的最终预测具有比之前更好的泛化能力。

![](img/d64363495f57cadc97ac5144dde52b7c.png)

来源: 作者提供的图片

以下图显示了袋装回归器对给定训练和测试数据的预测：

![](img/3e4ed0b338726ca7465bc44100249ec6.png)

来源: 作者提供的图片

来自集成的最终预测比单棵决策树的预测更平滑，模型在训练集和测试集上的拟合情况相似。

## 完整笔记本链接

你可以在[这里](https://github.com/gurjinderbassi/Machine-Learning/blob/main/Overfitting%20and%20Bagging%20in%20Decision%20Trees.ipynb)找到笔记本。

# 奖励: 随机森林

在本文开头，我提到随机森林使用了袋装加上*其他内容*的理念。我不想让你继续琢磨这个*其他内容*是什么，既然你已经快到达文章的结尾了，这一额外部分就是你的奖励 😸

> 随机森林是通过袋装方法训练的决策树集成。

揭示*其他内容：* 随机森林算法在生长树木时引入了额外的随机性。在分裂一个节点时，它不是搜索整个特征空间，而是在随机特征子集中寻找最佳特征。这进一步增强了模型的多样性，并减少了方差，从而产生了更好的集成模型。

随机森林也可以使用 scikit-learn 实现，适用于回归和分类任务。它具有控制单棵树生长的所有超参数，如*DecisionTreeRegressor（或 DecisionTreeClassifier）*，以及*BaggingRegressor（或 BaggingClassifier）* 的所有超参数，但有一些例外。另一个超参数集还用于控制每个节点考虑的特征采样。

# 结论

在本文中，我们讨论了决策树中的过拟合和不稳定性问题，以及如何使用诸如袋装（bagging）等集成方法来克服这些问题。

+   决策树是强大的机器学习算法，能够解决回归和分类问题，但它们容易过拟合且不稳定。

+   过拟合发生在模型过于完美地拟合训练数据，以至于无法很好地概括和学习数据的潜在行为。

+   正则化可以通过限制决策树的增长来减少过拟合的可能性。

+   决策树的另一个问题是对数据的小变化非常敏感，使其变得不稳定。这可以通过使用集成技术来克服。

+   集成学习包括在训练数据的随机子集上训练多个预测器，然后聚合它们的预测。袋装是一种使用替换的训练数据采样技术。

+   随机森林通过在每个节点上结合袋装和随机特征选择来改进决策树，从而减少总体方差。

感谢阅读，希望对你有所帮助！

欢迎任何反馈或建议。

# 参考文献：

[1] [`www.ibm.com/topics/overfitting`](https://www.ibm.com/topics/overfitting)

[2] 《动手机器学习：使用 Scikit-Learn、Keras 和 TensorFlow 构建智能系统（第 2 版）》 O'Reilly。奥雷利安·热罗，2019 年。

[3] 《百页机器学习书》，安德烈·布尔科夫，2019 年。
