# 如何在 Python 中进行异常值检测：第一部分

> 原文：[`towardsdatascience.com/how-to-perform-outlier-detection-in-python-in-easy-steps-for-machine-learning-1-8f9a3e6c88b5`](https://towardsdatascience.com/how-to-perform-outlier-detection-in-python-in-easy-steps-for-machine-learning-1-8f9a3e6c88b5)

## 地球是一个异常值——这一理论

[](https://ibexorigin.medium.com/?source=post_page-----8f9a3e6c88b5--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----8f9a3e6c88b5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8f9a3e6c88b5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8f9a3e6c88b5--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----8f9a3e6c88b5--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8f9a3e6c88b5--------------------------------) ·阅读时间 6 分钟·2023 年 1 月 28 日

--

![](img/da07cff3f5f613c032464ed39dcf1906.png)

图片由 [0fjd125gk87](https://pixabay.com/users/0fjd125gk87-51581/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1784245) 提供，来源于 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1784245)

## 什么是异常值？

我们生活在一个异常值上。地球是银河系中唯一拥有生命的岩石块。我们银河系中的其他行星是所谓的恒星和行星数据库中的正常数据点或内点。

异常值有许多定义。简单来说，我们将异常值定义为数据集中显著不同于大多数数据点的数据点。异常值是那些不符合或不对齐于数据集中内点的稀有、极端样本。

从统计学角度来看，异常值来自于与特征中其他样本不同的分布。它们表现出统计学上显著的异常。

这些定义取决于我们如何定义“正常”。例如，CEO 赚取数百万美元是完全正常的，但如果我们将他们的薪水信息添加到家庭收入的数据集中，他们就会变得异常。

异常值检测是统计学和机器学习领域的一个分支，使用各种技术和算法来检测这些极端样本。

你可以在这里查看系列的第二部分：

[](/how-to-perform-univariate-outlier-detection-in-python-for-machine-learning-b9fb05e72661?source=post_page-----8f9a3e6c88b5--------------------------------) ## 如何在 Python 中进行单变量异常值检测

### 单变量异常值检测，解释得非常清楚

[towardsdatascience.com

## 为什么要费心检测异常值？

那么，为什么呢？我们为什么需要找到它们？它们有什么害处？考虑一下这组 12 个范围从 50 到 100 的数字分布。其中一个数据点是 2534，这显然是一个异常值。

```py
import numpy as np

array = [97, 87, 95, 62, 53, 66, 2534, 60, 68, 90, 52, 63, 65]
array
```

```py
[97, 87, 95, 62, 53, 66, 2534, 60, 68, 90, 52, 63, 65]
```

均值和标准差是分布中最常用和最关键的属性之一，因此我们必须在拟合机器学习模型时提供这些指标的真实值。

让我们为我们的样本分布计算它们。

均值：

```py
np.mean(array)
```

```py
260.9230769230769
```

标准差：

```py
np.std(array)
```

```py
656.349984212042
```

现在，让我们做同样的操作，移除异常值：

```py
# Array without the outlier
array_wo = [97, 87, 95, 62, 53, 66, 60, 68, 90, 52, 63, 65]

np.mean(array_wo)
```

```py
71.5
```

```py
np.std(array_wo)
```

```py
15.510748961069977
```

如你所见，没有异常值的分布具有 3.6 倍更小的均值和接近 45 倍更小的标准差。

除了扭曲均值和标准差的实际值外，异常值还会在训练数据中产生噪声。它们在分布中创建了趋势和属性，使机器学习模型从数据中的实际模式中分心，导致性能损失。

因此，发现异常值、探究其存在原因并在适当时将其移除是至关重要的。

## 本教程中你将学到的内容

一旦你理解了该过程背后的重要理论，使用像 PyOD 或 Sklearn 这样的库在代码中执行异常值检测就变得简单了。例如，这里是如何使用流行的隔离森林算法进行异常值检测。

```py
from pyod.models.iforest import IForest

iforest = IForest().fit(training_features)

# 0 for inliers, 1 for outliers
labels = iforest.labels_

outliers = training_features[labels == 1]
len(outliers)
```

```py
136
```

只需要几行代码。

因此，本教程将更多关注理论。具体而言，我们将探讨无监督学习中的异常值检测，数据集中污染的概念，异常、离群和新颖的区别，以及单变量/多变量异常值。

让我们开始吧。

## 异常值检测是一个无监督问题

与许多其他机器学习任务不同，异常值检测是一个无监督学习问题。我们这是什么意思？

例如，在分类中，我们有一组特征映射到特定的输出。我们有标签告诉我们哪个样本是狗，哪个是猫。

在异常值检测中情况并非如此。当我们面对一个新数据集时，我们没有异常值的先验知识。这带来了几个挑战（但没有我们无法处理的）。

首先，我们没有简单的方法来衡量异常值检测方法的有效性。在分类中，我们使用诸如准确率或精确度等指标来衡量算法如何适应我们的训练数据集。在异常值检测中，我们不能使用这些指标，因为我们没有标签可以用来将预测与实际情况进行比较。

由于我们无法使用传统指标来衡量性能，我们不能高效地进行超参数调整。这使得很难找到适合当前任务的最佳异常值分类器（一个返回每个数据集行的内点/异常值标签的算法）。

但是，不必灰心。我们将在下一教程中看到两个优秀的解决方案。

## 异常与异常值与新奇性

你会发现“异常”与“新奇性”这两个术语在许多来源中经常与异常值一起出现。虽然它们的含义接近，但存在重要的区别。

异常是一个泛指的术语，包括任何不寻常和异常的东西。异常可以指训练集或测试集中的不规则性。

至于异常值，它们仅存在于训练数据中。异常值检测是指从训练集中找出异常数据点。异常值分类器仅对训练数据进行`fit`操作，并返回正常/异常标签。

另一方面，新奇性仅存在于测试集中。在新奇性检测中，你拥有一个干净的、不含异常值的数据集，并试图查看新的、未见过的观察是否具有不同于训练样本的属性。因此，测试集中的不规则实例变成了新奇性。

简而言之，异常检测是异常值检测和新奇性检测的母领域。异常值仅指训练数据中的异常样本，而新奇性存在于测试集中。

这一区分在我们开始使用异常值分类器的下一教程中至关重要。

## 单变量与多变量异常值

单变量和多变量异常值指的是不同类型数据中的异常值。

顾名思义，单变量异常值仅存在于单一分布中。例如，在一个身高测量数据集中，一个非常高的人就是一个异常值。

多变量异常值有些棘手。它们指的是具有两个或更多属性的异常值，这些属性单独看时并不显得异常，但当所有属性一起考虑时，才会显得异常。

一个多变量异常值的例子可以是一个里程非常低的旧车。这个车的属性单独看可能是正常的，但当所有属性组合在一起时，你会发现旧车通常具有与其年龄成正比的高里程。（虽然有许多旧车和许多里程低的车，但同时具备这两个特征的车却很少）。

在选择检测算法时，异常值类型之间的区别变得重要。

由于单变量异常值存在于仅有一列的数据显示集中，你可以使用简单且轻量的方法，如[z-scores](https://en.wikipedia.org/wiki/Standard_score)或[修改的 z-scores](https://en.wikipedia.org/wiki/Median_absolute_deviation)。

多变量异常值带来了更大的挑战，因为它们可能只在多个数据集列中显现。因此，你必须使用大招如孤立森林、KNN、本地异常因子等。

在接下来的教程中，我们将看到如何使用上述一些方法。

## 结论

就这样！你现在了解了异常值检测的所有基本术语和理论，剩下的就是在实践中使用异常值分类器来应用这些知识。

在文章的下一部分，我们将深入探讨使用 PyOD 库的一些最受欢迎且强大的异常值分类器。敬请关注！

[](https://ibexorigin.medium.com/membership?source=post_page-----8f9a3e6c88b5--------------------------------) [## 使用我的推荐链接加入 Medium - Bex T.

### 获得对我所有⚡优质⚡内容的独家访问权限，无限制地访问 Medium 上的所有内容。通过请我喝一杯支持我的工作…

ibexorigin.medium.com](https://ibexorigin.medium.com/membership?source=post_page-----8f9a3e6c88b5--------------------------------)

更多文章来自…

[](https://medium.com/geekculture/5-excellent-julia-features-that-python-developers-can-only-wish-they-had-e1531a596239?source=post_page-----8f9a3e6c88b5--------------------------------) [## 5 个优秀的 Julia 特性，Python 开发者只能羡慕的特性

### Julia 与 Python 辩论的延续

medium.com](https://medium.com/geekculture/5-excellent-julia-features-that-python-developers-can-only-wish-they-had-e1531a596239?source=post_page-----8f9a3e6c88b5--------------------------------) [](/5-best-python-synthetic-data-generators-and-how-to-use-them-when-you-lack-data-f62bcf62d43c?source=post_page-----8f9a3e6c88b5--------------------------------) ## 5 个最佳 Python 合成数据生成器及其在数据不足时的使用方法

### 获取更多数据

towardsdatascience.com [](https://pub.towardsai.net/bentoml-vs-fastapi-the-best-ml-model-deployment-framework-and-why-its-bentoml-f0ed26cae88d?source=post_page-----8f9a3e6c88b5--------------------------------) [## Bentoml 与 FastAPI：最佳 ML 模型部署框架及其为何是 Bentoml

### BentoML 与 FastAPI 在模型部署方面的详细比较。

pub.towardsai.net](https://pub.towardsai.net/bentoml-vs-fastapi-the-best-ml-model-deployment-framework-and-why-its-bentoml-f0ed26cae88d?source=post_page-----8f9a3e6c88b5--------------------------------) [](https://pub.towardsai.net/how-to-create-highly-organized-ml-projects-anyone-can-reproduce-with-dvc-pipelines-fc3ac7867d16?source=post_page-----8f9a3e6c88b5--------------------------------) [## 如何创建高度组织化的 ML 项目，以便任何人都可以用 DVC 管道复现

### 什么是机器学习管道？

pub.towardsai.net](https://pub.towardsai.net/how-to-create-highly-organized-ml-projects-anyone-can-reproduce-with-dvc-pipelines-fc3ac7867d16?source=post_page-----8f9a3e6c88b5--------------------------------)
