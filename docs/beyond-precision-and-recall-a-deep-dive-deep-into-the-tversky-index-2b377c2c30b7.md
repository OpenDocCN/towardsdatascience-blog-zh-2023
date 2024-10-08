# 超越精确度和召回率：深入探讨 Tversky 指数

> 原文：[`towardsdatascience.com/beyond-precision-and-recall-a-deep-dive-deep-into-the-tversky-index-2b377c2c30b7`](https://towardsdatascience.com/beyond-precision-and-recall-a-deep-dive-deep-into-the-tversky-index-2b377c2c30b7)

## 探索另一种分类度量标准

[](https://mikhailklassen.medium.com/?source=post_page-----2b377c2c30b7--------------------------------)![米哈伊尔·克拉森](https://mikhailklassen.medium.com/?source=post_page-----2b377c2c30b7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2b377c2c30b7--------------------------------)![数据科学之道](https://towardsdatascience.com/?source=post_page-----2b377c2c30b7--------------------------------) [米哈伊尔·克拉森](https://mikhailklassen.medium.com/?source=post_page-----2b377c2c30b7--------------------------------)

·发表于 [数据科学之道](https://towardsdatascience.com/?source=post_page-----2b377c2c30b7--------------------------------) ·阅读时间 7 分钟·2023 年 9 月 2 日

--

![](img/007d37c92072ebd8b214107ae1e5014e.png)

图片由 [Ricardo Arce](https://unsplash.com/@jrarce?utm_source=medium&utm_medium=referral) 提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在数据科学的世界中，度量标准是指导我们模型成功的指南针。虽然许多人对经典的精确度和召回率度量很熟悉，但实际上还有许多其他值得探索的选项。

在本文中，我们将深入探讨 Tversky 指数。这一度量标准是 Dice 和 Jaccard 系数的推广，在尝试平衡精确度和召回率时非常有用。当作为神经网络的损失函数实施时，它可以成为处理类别不平衡的强大工具。

## 关于精确度和召回率的快速复习

想象一下你是一名侦探，负责捕捉你镇上的罪犯。实际上，有 10 个罪犯在街上游荡。

在你的第一个月里，你带来了 8 个你认为是罪犯的嫌疑人。只有 4 人最终被判定有罪，其余 4 人是无辜的。

如果你是一个机器学习模型，你将根据你的精确度和召回率来进行评估。

**精确度**问道：“在你抓到的所有人中，有多少人是罪犯？”

**召回率**问道：“在镇上所有的罪犯中，你抓到了多少个？”

**精确度**是一个度量标准，它衡量你预测的准确性，而不计算你错过了多少个真正的正例（假阴性）。**召回率**衡量你捕获了多少个真正的正例，而不管你有多少个假正例。

![](img/86e004501ee6e7e2cca31918adf79448.png)

你的侦探技能在这些度量标准中表现如何？

+   precision = 4 / (4 + 4) = 0.5

+   recall = 4 / (4 + 6) = 0.4

## 平衡精确度和召回率：F1 度量

在理想情况下，你的分类器具有高精度和高召回率。作为衡量分类器在这两个方面表现如何的指标，F1 统计量衡量二者之间的调和平均值：

![](img/7780f4c972fd0400459e034122a091b8.png)

这个指标有时也被称为 Dice 相似性系数（DSC）。

## 另一种测量相似性的方法：Jaccard 系数

另一种测量两个集合相似性的方法是 **Jaccard 相似性系数**，它通常用于计算机视觉应用中，目标是识别对象。

![](img/93776d9f2a4381aa34277c333f838c81.png)

一个目标检测算法预测了一个停牌的边界框（红色）。人工标注者标记了期望的边界区域（绿色）。来源：[Adrian Rosebrock](http://www.pyimagesearch.com/2016/11/07/intersection-over-union-iou-for-object-detection/)，[CC BY-SA 4.0](https://commons.wikimedia.org/w/index.php?curid=57718561)

Jaccard 系数测量两个集合的交集与它们并集的比率：

![](img/f36d0ce33b094ef866693b6e7fe8a613.png)

直观展示，交集与并集的比率（IoU）是

![](img/68c50f484216e713a23508d0a3663211.png)

Jaccard 相似性系数也被称为交集与并集的比率（IoU）。来源：[Adrian Rosebrock](http://www.pyimagesearch.com/2016/11/07/intersection-over-union-iou-for-object-detection/)，[CC BY-SA 4.0](https://commons.wikimedia.org/w/index.php?curid=57718561)

# 介绍 Tversky 指数

Tversky 指数是 Dice 系数（F1 指标）和 Jaccard 系数的一个推广，也用于比较两个集合 *X* 和 *Y* 的相似性。

索引可以被公式化为：

![](img/057b8fea01957d2c7a78a331d79a27d1.png)

如果集合论的语言不太熟悉，让我们来解释一下：

+   *| X* ∩ *Y |* 表示集合 *X* 和 *Y* 之间的公共元素数量。

+   *| X* ∖ *Y |* 表示在 *X* 中但不在 *Y* 中的元素数量。它被称为 *Y* 在 *X* 中的相对补集。

+   *| Y* ∖ *X |* 表示在 *Y* 中但不在 *X* 中的元素数量。它被称为 *X* 在 *Y* 中的相对补集。

+   *α* 和 *β* 是参数，用于确定假阳性和假阴性的相对重要性。

参数 *α* 和 *β* 非常重要。如果我们设置 *α* = *β* = 0.5，我们得到的是 Dice 系数。如果我们设置 *α* = *β* = 1.0，我们得到的是 Jaccard 系数。通常，我们保持 *α* + *β* = 1，这允许我们在假阴性和假阳性之间调整权重。

## 真正例、假阳性和假阴性

现在，让我们在分类的背景下讨论它。假设 *X* 是真实情况，而 *Y* 是我们的预测。在这种情况下：

+   *X* ∩ *Y*：真正例（正确预测为正的项）

+   *X* ∖ *Y*：假阴性（被预测为负但实际上是正的项）

+   *Y* ∖ *X*：假阳性（实际为负例但被预测为正例的项）

## 它与精确度和召回率有什么关系？

让我们回到精确度和召回率的定义：

![](img/86e004501ee6e7e2cca31918adf79448.png)

1.  精确度是所有预测为正例中的真正正例的比例。它告诉我们预测为正例的实例中实际有多少是正例。

1.  召回率，也称为敏感性或真正率（TPR），是所有实际正例中真正预测的比例。它告诉我们模型正确预测了多少实际正例。

在考虑这些解释后，让我们将 Tversky 指数与精确度和召回率联系起来。

1.  当*α* = 1 且*β* = 0 时：

![](img/26e20271cd771050d2d74030b938560b.png)

这是**召回率**的定义。

1.  当*α* = 0 且*β* = 1 时：

![](img/54c843a0469ca561246e2a39d7138141.png)

这是**精确度**的定义。

通过调整参数*α*和*β*，Tversky 指数可以反映出精确度或召回率，或两者的加权混合。这使得 Tversky 指数成为一个灵活的度量标准，根据应用需求，可以优先考虑召回率或精确度。

# 应用于神经网络：Tversky 损失

Tversky 指数可以作为机器学习应用中的损失函数。在执行分类任务的神经网络中，如语义图像分割，我们可以使用 Tversky 指数来定义损失函数。这是对通常使用的分类交叉熵的一种替代。

由于优化器总是尝试最小化损失，Tversky 损失（TL）仅定义为

![](img/1e9158bdf335ef2345f444aab5f7769e.png)

其中 TI 是 Tversky 指数。

或者，它可以被定义为

![](img/331b239fc8608147d12c3cb29f5c8244.png)

这里它的值始终在 0 和 1 之间。因此，随着损失接近 0，我们的分类器接近完美准确度。

使用 Tversky 损失的优点是我们可以调整*α*和*β*参数，以偏向召回率或精确度。

## 焦点 Tversky 损失

焦点 Tversky 损失是一个先进的损失函数，旨在解决分割任务中的类别不平衡问题，特别是在医学成像中。它定义为

![](img/eea60aa8fc5ea3310d1e73e726d248dd.png)

通过结合 Tversky 指数（它是对 Dice 系数的推广，按不同方式权衡假阳性和假阴性）和一个焦点调节（γ），以赋予难以分割的实例更多重要性，这个损失函数对小而模糊的结构变得更敏感。

在某些区域或类别被低估，或某些误分类的代价高于其他情况时，标准的损失函数如分类交叉熵可能无法达到最佳效果。在这种情况下，研究人员可能会选择 Focal Tversky 损失，因为它优先学习具有挑战性和稀有的区域，在存在不平衡和细微特征的情况下提供更好的分割性能。

想了解更多关于 Focal Tversky 损失的信息，请查看这篇来自 Towards Data Science 的文章：

[](/dealing-with-class-imbalanced-image-datasets-1cbd17de76b5?source=post_page-----2b377c2c30b7--------------------------------) ## 使用 Focal Tversky 损失处理类别不平衡的图像数据集

### 在类别不平衡问题中的损失比较，以及为什么 Focal Tversky 损失可能是最适合你的选择

towardsdatascience.com

## 结论

在分类指标的领域中，Tversky 指数作为一种多功能且适应性强的工具脱颖而出。它弥合了传统指标如 Dice 和 Jaccard 系数之间的差距，为数据科学家提供了一个更为细致的工具来评估模型。通过α和β参数在平衡假阳性和假阴性方面的灵活性，确保它可以根据具体需求进行调整。作为神经网络中的损失函数，尤其是在带有焦点参数的情况下，它也成为训练深度神经网络的强大工具。
