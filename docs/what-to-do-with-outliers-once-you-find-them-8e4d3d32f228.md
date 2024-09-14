# 找到异常值后该怎么做

> 原文：[`towardsdatascience.com/what-to-do-with-outliers-once-you-find-them-8e4d3d32f228`](https://towardsdatascience.com/what-to-do-with-outliers-once-you-find-them-8e4d3d32f228)

[](https://ibexorigin.medium.com/?source=post_page-----8e4d3d32f228--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----8e4d3d32f228--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8e4d3d32f228--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8e4d3d32f228--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----8e4d3d32f228--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----8e4d3d32f228--------------------------------) ·阅读时间 5 分钟·2023 年 2 月 15 日

--

![](img/29afd747b095ce34bcc4b024ce5a9a8b.png)

图片由[Ralf Kunze](https://pixabay.com/users/realworkhard-23566/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=199054)提供，来源于[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=199054)

异常值检测只是故事的一部分。真正的挑战在于弄清楚如何处理这些异常值。很容易就将异常值一笔勾销，但实际情况中有很多细微差别和需要考虑的因素。

盲目删除异常值可能会产生不良后果。虽然发现某种类型的异常值可能暗示数据存在更严重的问题，但检测到另一种类型的异常值可能根本没有问题。因此，重要的是不要做出草率的决定，因为这可能导致数据质量下降或后续机器学习模型的偏差。

今天，我们将从两个角度分析这个问题。首先，我们将根据异常值存在的原因查看适当的行动方案，然后讨论根据数据集中异常值的数量应采取的措施。让我们开始吧！

查看这系列关于异常值检测的早期部分，我们更注重代码方面：

![Bex T.](img/3f2835593290cd528199731ac48cf262.png)

[Bex T.](https://ibexorigin.medium.com/?source=post_page-----8e4d3d32f228--------------------------------)

## BEXGBoost - 异常值检测

[查看列表](https://ibexorigin.medium.com/list/bexgboost-outlier-detection-16cfc06cd0f3?source=post_page-----8e4d3d32f228--------------------------------)3 个故事![](img/04febaa835c6275808dbb5c9cee9e16b.png)![](img/5ec34d780dee50a815538e11ee75c3f7.png)![](img/662f87df807ee91113997c66cccd58a0.png)

## 1. 存在的原因：错误

异常值最常见的原因之一是人为和设备错误。有人搞错了零的个数，按错了键，忘记测量某样东西（或测量了两次），或者故障的仪器产生了不一致的读数；软件故障记录了错误的数据，等等。

作为数据科学家，这些事情超出了你的控制范围，因为它们通常发生在数据收集过程中。第一步适当的行动是尝试修正这些错误的异常值。尝试修正那些错别字或将数字值更改为常识性的替代值（例如，当有人在调查中说自己 200 岁时，你将其更改为 20 岁）。

当无法修正或修正成本过高时，只能过滤掉这些数据，因为你知道它们是不正确的值。

## 2\. 存在原因：抽样错误

统计学家和机器学习工程师使用小样本来对特定目标群体做出结论。然而，在数据收集过程中，不属于该群体的数据点可能会泄漏到收集的样本中。

例如，假设你正在研究你朋友果园中的苹果生长。这个研究定义的总体是果园中的所有苹果树，而样本是 1000 棵随机选择的苹果树。但是，由于它们之间的围栏不太明显，几十棵邻居果园的树木被混入了你的样本中。

虽然邻居的苹果不一定是不正常的，但它们来自不同的群体，可能会扭曲你的整个研究。

当异常值是由于这种抽样错误造成的时，删除它们是唯一的解决办法。

## 3\. 存在原因：自然变异

世界充满了惊喜和不确定性。一些异常值可能突然出现，但仍然是目标群体中固有的自然变异的一部分。

例如，有些人天生拥有特定基因使他们非常高、非常矮、是天才等，或者一些动物能够跳得异常高或寿命远超同类……例子无穷无尽。

如果你抽取了足够大的样本，你肯定会遇到自然分布中的奇异值。它们不一定是问题，但会引入数据的变异性。

虽然删除这些异常值很有诱惑力（因为它们会降低数据的统计显著性），但不能仅仅为了获得更好的指标而这么做。

## 4\. 异常值的数量

处理异常值也会极大地依赖于它们相对于数据集大小的数量。

如果它们只是少数（低于 1%）且你有充足的数据，可以安全地排除它们以保持透明。当然，你必须与收集数据的人或领域实验讨论，以确保它们不是自然变异的一部分。

如果异常值过多，以至于引起怀疑，那么它们的存在可能有某种未知原因。也许它们只是相对于大多数数据点来说是异常值，但实际上是目标人群的关键特征。在这种情况下，你所处理的样本可能并不完全代表其目标人群。

最终的情况是数据中有太多异常值，以至于它们形成了一个新的簇或子群体。在这里，推荐使用相同的方法——分析最初的机器学习或数据科学问题是如何框定的，目标人群和样本是如何选择的，以及为什么会有这么多异常值进入数据集。

## 如果你不去除异常值，该怎么处理它们？

异常值通常会被删除，以避免它们扭曲特征的均值和标准差，从而最终导致模型性能下降。但在某些情况下，异常值本身也是值得关注的。

异常检测的一些应用包括入侵检测（网络安全）、欺诈检测、故障检测、系统健康监测、传感器网络中的事件检测、生态系统扰动检测、使用机器视觉的图像缺陷检测和医疗诊断。在这些场景中，异常值的性质和存在被广泛研究，以推动业务、隐私和医疗决策。

如果你*确实*决定去除异常值以提高模型性能、改进视觉效果或统计测试，你应该在方法上保持透明。利益相关者（直接从项目中受益或失去的人）应该被告知这一决定。最好是，你应该提供两个结果：一个包含异常值，另一个不包含。

还有一些非侵入性且轻量级的替代方法。一个流行的方法是*百分位修剪*，即对超出第一个和第 99 个百分位的极端值进行限制。你可以在 Pandas 或 NumPy 中轻松执行此操作：

```py
import pandas as pd

low = distribution.quantile(0.01)
high = distribution.quantile(0.99)

outlier_free = distribution.clip(low, high)
```

你也可以用中位数或众数来替代异常值。这可以通过 Pandas 的`replace`函数或 NumPy 的`where`函数来完成。

## 结论

本文的主要目标是强调对异常值采取适当行动的重要性。过去，你可能只是为了更好的指标而盲目删除它们，但现在，你可以根据异常值的类型和数量做出更明智的决定。

查看这系列异常检测的其余文章：

![Bex T.](img/3f2835593290cd528199731ac48cf262.png)

[Bex T.](https://ibexorigin.medium.com/?source=post_page-----8e4d3d32f228--------------------------------)

## BEXGBoost - 异常检测

[查看列表](https://ibexorigin.medium.com/list/bexgboost-outlier-detection-16cfc06cd0f3?source=post_page-----8e4d3d32f228--------------------------------)3 stories![](img/04febaa835c6275808dbb5c9cee9e16b.png)![](img/5ec34d780dee50a815538e11ee75c3f7.png)![](img/662f87df807ee91113997c66cccd58a0.png)

感谢阅读！

[ibexorigin.medium.com](https://ibexorigin.medium.com/membership?source=post_page-----8e4d3d32f228--------------------------------) [## 使用我的推荐链接加入 Medium - Bex T.

### 获取对我所有⚡优质⚡内容和 Medium 上所有内容的独家访问权限，没有限制。通过购买支持我的工作...

[ibexorigin.medium.com](https://ibexorigin.medium.com/membership?source=post_page-----8e4d3d32f228--------------------------------) [## 订阅 Bex T. 的更新

### 每当 Bex T. 发布文章时，都会向您发送电子邮件。通过注册，如果您还没有 Medium 账户，将为您创建一个...

[ibexorigin.medium.com](https://ibexorigin.medium.com/subscribe?source=post_page-----8e4d3d32f228--------------------------------)

更多我的文章...

[5 个迹象表明你已经成为高级 Pythonista，而自己却没有意识到](https://pub.towardsai.net/how-to-create-highly-organized-ml-projects-anyone-can-reproduce-with-dvc-pipelines-fc3ac7867d16?source=post_page-----8e4d3d32f228--------------------------------) ## 5 个迹象表明你已经成为高级 Pythonista，而自己却没有意识到

[towardsdatascience.com [## 5 个优秀的 Julia 特性，Python 开发者只能羡慕

### 朱莉亚与 Python 辩论的继续

[medium.com](https://medium.com/geekculture/5-excellent-julia-features-that-python-developers-can-only-wish-they-had-e1531a596239?source=post_page-----8e4d3d32f228--------------------------------) [## 如何创建高度组织化的 ML 项目，任何人都可以通过 DVC 管道重现

### 什么是机器学习管道？

[pub.towardsai.net](https://pub.towardsai.net/how-to-create-highly-organized-ml-projects-anyone-can-reproduce-with-dvc-pipelines-fc3ac7867d16?source=post_page-----8e4d3d32f228--------------------------------)
