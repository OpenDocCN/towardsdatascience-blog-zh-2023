# 关于正则化你需要了解的一切

> 原文：[https://towardsdatascience.com/everything-you-need-to-know-about-regularization-64734f240622?source=collection_archive---------4-----------------------#2023-01-25](https://towardsdatascience.com/everything-you-need-to-know-about-regularization-64734f240622?source=collection_archive---------4-----------------------#2023-01-25)

![](../Images/707b2d036e756284d5078b0cf9dd7876.png)

图片来源：Dall-E 2。

## 防止机器学习中过拟合的不同方法

[](https://hennie-de-harder.medium.com/?source=post_page-----64734f240622--------------------------------)[![Hennie de Harder](../Images/3e4f2cccd6cb976ca3f8bf15597daea8.png)](https://hennie-de-harder.medium.com/?source=post_page-----64734f240622--------------------------------)[](https://towardsdatascience.com/?source=post_page-----64734f240622--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----64734f240622--------------------------------) [Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----64734f240622--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffb96be98b7b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feverything-you-need-to-know-about-regularization-64734f240622&user=Hennie+de+Harder&userId=fb96be98b7b9&source=post_page-fb96be98b7b9----64734f240622---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----64734f240622--------------------------------) · 6分钟阅读 · 2023年1月25日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F64734f240622&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feverything-you-need-to-know-about-regularization-64734f240622&user=Hennie+de+Harder&userId=fb96be98b7b9&source=-----64734f240622---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F64734f240622&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feverything-you-need-to-know-about-regularization-64734f240622&source=-----64734f240622---------------------bookmark_footer-----------)

**如果你正在使用机器学习模型，你可能听说过正则化。你知道它是什么以及它是如何工作的么？正则化是一种用于防止过拟合和提高模型性能的技术。在这篇文章中，我们将详细讲解不同类型的正则化以及如何利用它们来改进你的模型。此外，你还会学到何时应用不同类型的正则化。**

机器学习中的正则化意味着“简化结果”。如果模型出现过拟合且过于复杂，你可以使用正则化使模型更好地泛化。如果训练集和测试集之间的性能差距很大，你应该使用正则化。这意味着模型捕捉了训练集的过多细节。过拟合与高方差有关，这意味着模型对训练集的特定样本很敏感。

![](../Images/831ab37188815563db4d8358459e8987.png)

分类问题。黑线是模型的决策边界。图像作者提供。

让我们开始讨论不同的正则化技术。每种技术都属于以下两类之一：显式正则化或隐式正则化。可以在同一个问题中结合多种正则化技术。

# 显式正则化

所有属于显式正则化的技术都会显式地向问题中添加一个项。我们将深入探讨三种最常见的显式正则化类型。这些类型包括 L1、L2 和弹性网正则化。

作为示例，我们以一个具有独立变量*x₁*和*x₂*，以及因变量*y*的线性回归模型为例。该模型可以用以下方程表示：

![](../Images/fe979e98dcb2ad315ce68eb302e4d31f.png)

我们希望确定权重：*w₁*、*w₂* 和 *b*。代价函数等于均方误差，我们希望将其最小化。下方是代价函数 *J*：

![](../Images/97e30736f21c790028e8a78b02a87e60.png)

在以下示例中，你可以看到不同类型显式正则化的代价函数如何变化。

## L1 正则化

这种类型的正则化也被称为 Lasso 正则化。它向代价函数中添加了一个与权重系数的绝对值成正比的项：

![](../Images/51ae34b728ff166f414cccf614f9f69a.png)

它倾向于将一些权重系数缩小到零。该项的总和乘以 lambda，它控制正则化的程度。如果 lambda 太高，模型会变得简单，从而产生欠拟合的风险。

## L2 正则化

L2 正则化，或称为岭回归，向代价函数中添加了一个与权重系数的平方成正比的项：

![](../Images/afcef2f5ae3ed74eb31e152332370195.png)

这种项倾向于缩小所有权重系数，但与 L1 正则化不同，它不会将任何权重系数设置为零。

## 弹性网正则化

这是一种 L1 和 L2 正则化的组合。正如你所期望的，使用弹性网正则化时，L1 和 L2 的项都会被添加到代价函数中。而且还添加了一个新的超参数 *α*，用以控制 L1 和 L2 之间的平衡。

![](../Images/25ebac0e7e1c9f4754649980160f38a8.png)

## 何时使用 L1、L2 或弹性网？

在许多scikit-learn模型中，L2是默认的（参见LogisticRegression和SupportVectorMachines）。这是有原因的：L1往往会将一些权重系数缩减为零，这意味着特征从模型中移除。因此，L1正则化更适合特征选择。为了真正防止过拟合，L2可能是更好的选择，因为它不会将任何权重系数设为零。

当你有相关特征并希望在特征选择和防止过拟合之间取得平衡时，弹性网正则化是一个不错的选择。当你不确定L1或L2正则化对于你的数据和模型哪个更合适时，它也很有用。

一般来说，当你有大量特征并且希望在模型中保留大部分特征时，推荐使用L2正则化；当你有高维数据集且有许多相关特征，并希望选择其中一个子集时，推荐使用L1正则化。

# 隐式正则化

那些不显式的技术通常会自动归入隐式类别。这里有许多不同的隐式技术，我将讨论一些常用的。

## 模型参数

使模型更好地泛化的一个简单方法是指定一些模型超参数。以下是一些示例：

1.  对于树模型，如果`model_depth`参数没有设置，容易出现过拟合。此时，模型会继续增长，直到所有叶子都是纯净的，或直到叶子包含少于`min_samples_split`个样本。你可以通过超参数调优尝试不同的值，以发现最佳深度。

1.  在XGBoost中，许多参数与过拟合相关。以下是其中一些：`colsample_bytree`包含树中使用的特征比例，当你使用较少的特征时，可以减少过拟合效应。另一个密切相关的参数是`subsample`，它指定使用的样本比例。将学习率`eta`设置为较低的值也有帮助。`gamma`用于控制分裂所需的最小损失减少量。

1.  在逻辑回归和支持向量机中，`C`参数控制正则化的强度。正则化的强度与`C`成反比。也就是说，你设置得越高，正则化的程度就越低。（这实际上不是完全隐式的，因为它与正则化项有关。）

1.  在神经网络中，层数和每层的神经元数量可能导致过拟合。你可以删除层和神经元来降低模型的复杂性。

不同模型有许多其他参数，你可以在线搜索或阅读文档来发现它们并改进你的模型。

## Dropout正则化

Dropout正则化在深度神经网络的训练过程中应用。它在每次训练迭代中随机丢弃一定数量的神经元，迫使网络依赖多个神经元子集，从而减少过拟合。

![](../Images/92b97102b3c367cb932c4763ea10bb7f.png)

Dropout 层：从网络中移除神经元。图像由作者提供。

## 早期停止

这是一种正则化方法，通过监控模型在验证集上的表现来防止过拟合，当表现开始下降时停止训练。一些模型内置了早期停止功能。[XGBoost](https://xgboost.readthedocs.io/en/stable/python/python_intro.html#early-stopping) 提供了一个参数`early_stopping_rounds`，以及[如何在 Keras 中实现](https://keras.io/api/callbacks/early_stopping/)。

![](../Images/0a01d7970e3585d79f8123a02fc80e68.png)

与其继续训练模型，不如在测试集上的损失开始增加时停止。图像由作者提供。

## 数据增强

这是一种通过对现有数据应用随机但现实的变换来人为扩大训练数据规模的方法，以防止过拟合。数据增强通常应用于图像数据：下方可以看到一些不同的增强技术（镜像、裁剪和添加效果）。还有更多技术，如调整光照、模糊、移动对象等。

![](../Images/ea948320e60083442cac5155421aa8aa.png)

图像数据上的数据增强。图像由作者提供。

# 结论

本文为你提供了正则化技术的解释和概述。防止过拟合是使用机器学习模型时的重要部分，你必须确保以正确的方式处理它。除了本文介绍的技术外，使用交叉验证或处理异常值也有助于减少泛化误差。

## 相关

[## 你是否使用特征分布来检测异常值？

### 这里有三种更好的方法。

[## 面向模型的方法用于解释任何机器学习模型

### 解释方法概述：置换特征重要性、部分依赖图、LIME、SHAP 等。

[## 面向模型的方法用于解释任何机器学习模型
