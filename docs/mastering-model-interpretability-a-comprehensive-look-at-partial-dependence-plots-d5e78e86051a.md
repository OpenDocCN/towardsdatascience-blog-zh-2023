# 掌握模型可解释性：对部分依赖图的全面分析

> 原文：[https://towardsdatascience.com/mastering-model-interpretability-a-comprehensive-look-at-partial-dependence-plots-d5e78e86051a?source=collection_archive---------14-----------------------#2023-07-07](https://towardsdatascience.com/mastering-model-interpretability-a-comprehensive-look-at-partial-dependence-plots-d5e78e86051a?source=collection_archive---------14-----------------------#2023-07-07)

## 开始你在可解释 AI 世界中的旅程。

[](https://medium.com/@tiagotoledojr?source=post_page-----d5e78e86051a--------------------------------)[![Tiago Toledo Jr.](../Images/577748ae15ec9eb7ead9355f94287a9d.png)](https://medium.com/@tiagotoledojr?source=post_page-----d5e78e86051a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d5e78e86051a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d5e78e86051a--------------------------------) [Tiago Toledo Jr.](https://medium.com/@tiagotoledojr?source=post_page-----d5e78e86051a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff4eeaf479b0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-model-interpretability-a-comprehensive-look-at-partial-dependence-plots-d5e78e86051a&user=Tiago+Toledo+Jr.&userId=f4eeaf479b0c&source=post_page-f4eeaf479b0c----d5e78e86051a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d5e78e86051a--------------------------------) ·7 分钟阅读·2023年7月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd5e78e86051a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-model-interpretability-a-comprehensive-look-at-partial-dependence-plots-d5e78e86051a&user=Tiago+Toledo+Jr.&userId=f4eeaf479b0c&source=-----d5e78e86051a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd5e78e86051a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-model-interpretability-a-comprehensive-look-at-partial-dependence-plots-d5e78e86051a&source=-----d5e78e86051a---------------------bookmark_footer-----------)![](../Images/f605a8b589d514ca9f8aca161c4d675a.png)

图片由 [David Pupăză](https://unsplash.com/@davfts?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

了解如何解释你的模型对于确定其是否表现异常至关重要。你对模型了解得越多，当它投入生产时，你就越不容易被其行为所惊讶。

此外，你对模型的领域知识越多，你就越能更好地向业务部门推销它。最糟糕的情况是他们意识到你实际上并不确定你在向他们销售什么。

我从未开发过一个不需要解释如何根据输入变量进行预测的模型。至少，向业务说明哪些特征有正面或负面影响是必不可少的。

你可以使用的一个工具来理解你的模型如何工作的就是部分依赖图（PDP），我们将在这篇文章中探讨它。

# PDP是什么

PDP是一种全局可解释性方法，专注于向你展示模型的特征值如何与模型的输出相关。

这不是理解数据的方法，它只为你的模型生成见解，因此无法从中推断出目标与特征之间的因果关系。然而，它可以让你对模型进行因果推断。

这是因为该方法探测你的模型，因此你可以准确看到当特征变量改变时模型的表现。

# 它是如何工作的

首先，PDP允许我们一次只研究一个或两个特征。在这篇文章中，我们将专注于单个特征分析的情况。

在模型训练后，我们生成一个探测数据集。该数据集按照以下算法创建：

+   我们选择我们感兴趣的特征的每个唯一值

+   对于每个唯一值，我们制作一份整个数据集的副本，将特征值设置为该唯一值

+   然后，我们使用模型对这个新数据集进行预测

+   最后，我们对每个唯一值的模型预测进行平均

让我们举个例子。假设我们有以下数据集：

![](../Images/2db5862e15add1cf4952cf3d1f895135.png)

现在，如果我们想将PDP应用于特征0，我们将为特征的每个唯一值重复数据集，例如：

![](../Images/618c3d56e22e67fac47025e30905cc16.png)

然后，在应用我们的模型后，我们将得到类似这样的结果：

![](../Images/49281fb530c5d0d5ae15bbf9eece114f.png)

然后，我们计算每个值的平均输出，最终得到以下数据集：

![](../Images/9b7d1b62a620559bb2d52a7ba95490a7.png)

然后只是将这些数据用折线图绘制出来。

对于回归问题，计算每个特征值的平均输出是简单的。对于分类方法，我们可以使用每个类别的预测概率，然后对这些值进行平均。在这种情况下，我们将为数据集中的每个特征和类别对生成一个PDP。

# 数学解释

PDP的解释是我们在边际化一个或两个特征，以评估它们对模型预测输出的边际效应。这由以下公式给出：

![](../Images/5e121619c3a2fafb87fad2084cb7bdf5.png)

其中 $f$ 是机器学习模型，$x_S$ 是我们感兴趣分析的特征集合，$x_C$ 是我们将要平均的其他特征集合。上述函数可以使用以下近似计算：

![](../Images/47887c4183fb43a36ea4b995ce532135.png)

# PDP的问题

PDP 有一些我们必须注意的限制。首先，由于我们在每个特征值上平均输出，我们最终会得到一个覆盖数据集中每个值的图，即使该值仅出现一次。

因此，你可能会看到数据集中一些非常少见区域的行为，这可能不代表如果这些值更频繁时会发生的情况。因此，在查看PDP时，始终查看特征的分布是有帮助的，以了解哪些值更可能发生。

另一个问题发生在特征值相互抵消的情况下。例如，如果你的特征具有以下分布：

![](../Images/25c53b5861af9646f5490f4743458aaa.png)

计算该特征的PDP时，我们将得到类似于以下的结果：

![](../Images/7176ed7a7773ebc6cc1f2b8897ee94c0.png)

注意特征的影响绝非零，但其平均值为零。这可能会误导你认为特征是无用的，而实际上并非如此。

这种方法的另一个问题是，当我们分析的特征与我们正在平均的特征相关时。如果特征之间存在相关性，如果我们强制数据集中的每个值都具有感兴趣特征的每个值，我们将创建不现实的点。

想象一个包含降雨量和天空中云量的数据集。当我们对降雨量进行平均时，我们会得到一些说天空中有降雨却没有云的点，这是一个不可行的点。

# 解读PDP

让我们看看如何分析部分依赖图。请查看下面的图像：

![](../Images/71598657893794801e31a18c01af3358.png)

在x轴上，我们有特征0的值，在y轴上，我们有模型对每个特征值的平均输出。注意，对于小于-0.10的值，模型输出的目标预测非常低，此后预测值上升，然后在特征值超过0.09之前，预测值围绕150波动，在特征值超过0.09时，预测值开始急剧上升。

因此，我们可以说特征与目标预测之间存在正相关关系，但这种相关性不是线性的。

# ICE图

ICE图尝试解决特征值相互抵消的问题。基本上，在ICE图中，我们绘制模型对每个值的每个单独预测，而不仅仅是其平均值。

# 在Python中实现PDP

让我们在Python中实现PDP。为此，我们首先要导入所需的库：

```py
import numpy as np
import matplotlib.pyplot as plt
from tqdm import tqdm
from sklearn.datasets import load_diabetes
from sklearn.ensemble import RandomForestRegressor
```

我们将使用来自sklearn的糖尿病数据集。tqdm库将用于为我们的循环生成进度条。

现在，我们将加载数据集并拟合一个随机森林回归器：

```py
X, y = load_diabetes(return_X_y=True)
rf = RandomForestRegressor().fit(X, y)
```

现在，对于我们数据集中的每个特征，我们将计算在该特征固定值的情况下模型对数据集的平均预测：

```py
features = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

features_averages = {}
for feature in tqdm(features):
    features_averages[feature] = ([], [])

  # For each unique value in the feature
    for feature_val in np.unique(X[:, feature]):
        features_averages[feature][0].append(feature_val)

    # We remove the feature from the dataset
        aux_X = np.delete(X, feature, axis=1)
    # We add the feature value for every row of the dataset
        aux_X = np.hstack((aux_X, np.array([feature_val for i in range(aux_X.shape[0])])[:, None]))

    # We calculate the average prediction
        features_averages[feature][1].append(np.mean(rf.predict(aux_X)))
```

现在，我们为每个特征绘制PDP图：

```py
for feature in features_averages:
    plt.figure(figsize=(5,5))
    values = features_averages[feature][0]
    predictions = features_averages[feature][1]

    plt.plot(values, predictions)
    plt.xlabel(f'Feature: {feature}')
    plt.ylabel('Target')
```

例如，特征 3 的图示为：

![](../Images/d6ee9c6136c8fe6609437b9dcc2d8dbf.png)

## 结论

现在你有了另一个工具，可以用来提升你的工作效果，并帮助业务部门理解你展示的那个黑箱模型发生了什么。

但不要让理论消失。拿一个你正在开发的模型，将PDP可视化应用于它。了解模型在做什么，并在假设上更为精准。

另外，这不是唯一的解释性方法。实际上，我们还有其他方法在处理相关特征时效果更佳。请关注我的下一篇文章，其中会涵盖这些方法。

# 参考文献

[https://ethen8181.github.io/machine-learning/model_selection/partial_dependence/partial_dependence.html](https://ethen8181.github.io/machine-learning/model_selection/partial_dependence/partial_dependence.html)

[https://scikit-learn.org/stable/modules/partial_dependence.html](https://scikit-learn.org/stable/modules/partial_dependence.html)

[https://christophm.github.io/interpretable-ml-book/pdp.html](https://christophm.github.io/interpretable-ml-book/pdp.html)
