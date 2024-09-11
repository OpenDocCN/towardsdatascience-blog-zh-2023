# 虚假预言者：将回归模型与Meta的Prophet进行比较

> 原文：[https://towardsdatascience.com/false-prophet-comparing-a-regression-model-to-metas-prophet-bfac00823425?source=collection_archive---------4-----------------------#2023-11-25](https://towardsdatascience.com/false-prophet-comparing-a-regression-model-to-metas-prophet-bfac00823425?source=collection_archive---------4-----------------------#2023-11-25)

## 我那款受Prophet启发的时间序列回归模型——一个怪物般的自制版本——能与真正的预言者竞争吗？

[](https://bradley-stephen-shaw.medium.com/?source=post_page-----bfac00823425--------------------------------)[![Bradley Stephen Shaw](../Images/b3ef5e6e292083ff0f8523ec5ffe89f0.png)](https://bradley-stephen-shaw.medium.com/?source=post_page-----bfac00823425--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bfac00823425--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bfac00823425--------------------------------) [Bradley Stephen Shaw](https://bradley-stephen-shaw.medium.com/?source=post_page-----bfac00823425--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc5cd0a58b5ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffalse-prophet-comparing-a-regression-model-to-metas-prophet-bfac00823425&user=Bradley+Stephen+Shaw&userId=c5cd0a58b5ae&source=post_page-c5cd0a58b5ae----bfac00823425---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bfac00823425--------------------------------) ·7 min read·2023年11月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbfac00823425&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffalse-prophet-comparing-a-regression-model-to-metas-prophet-bfac00823425&user=Bradley+Stephen+Shaw&userId=c5cd0a58b5ae&source=-----bfac00823425---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbfac00823425&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffalse-prophet-comparing-a-regression-model-to-metas-prophet-bfac00823425&source=-----bfac00823425---------------------bookmark_footer-----------)![](../Images/0cee6d014812f7f0aec8ea5101f8be71.png)

图片由 [Piret Ilver](https://unsplash.com/@saltsup?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我构建Meta优秀预测包Prophet的最后一篇文章中，我将会查看我自制的版本与原版相比的表现如何。

这将是一个快速的过程：我们首先查看数据，然后可视化两种方法在时间外数据上的预测。接着，我们将使用一些指标更正式地确定哪个预测器更好，并讨论是否这是一个公平的比较。

让我们开始吧。

*顺便提一下：我提到了其他几篇文章——确切来说是两篇。第一篇涉及基于Prophet方法的时间序列特征工程，可以在这里找到：*

[](/false-prophet-feature-engineering-for-a-homemade-time-series-regression-part-1-of-2-52d9df3d930d?source=post_page-----bfac00823425--------------------------------) [## 虚假预言者：自制时间序列回归特征工程

### 基于Meta的Prophet包的思想，为时间序列机器学习模型创建强大的特征

towardsdatascience.com](/false-prophet-feature-engineering-for-a-homemade-time-series-regression-part-1-of-2-52d9df3d930d?source=post_page-----bfac00823425--------------------------------)

*在续集中，我使用我们全新特征构建模型。可以在这里找到：*

[](/false-prophet-a-homemade-time-series-regression-model-54e296b99438?source=post_page-----bfac00823425--------------------------------) [## 虚假预言者：自制时间序列回归模型

### 借鉴Meta的Prophet构建强大的时间序列回归模型

towardsdatascience.com](/false-prophet-a-homemade-time-series-regression-model-54e296b99438?source=post_page-----bfac00823425--------------------------------)

*今天讨论的许多话题在链接的文章中有更详细的介绍——如果你喜欢细节，值得一读。*

# 数据

我们使用的是英国道路交通事故数据¹，汇总为每月统计。

![](../Images/2fd5691e8f47f45e0c7f03119d4f778d.png)

作者提供的图片

我们在时间序列中看到了一些特征：

+   整个序列中有一个强烈的下降趋势

+   在2012年和2014年之间的某个时间点，减少率发生了变化

+   序列早期的季节性较强

+   潜在的季节性变动，尤其是在序列的末尾。

# 游戏的目标

我们有两个模型——我们将自制的Frankenstein模型称为LASSO模型，而Meta的Prophet就叫做… Prophet。

对于每个模型，我们将生成时间外预测。这本质上意味着拟合我们每月统计数据的一个子集，然后预测未来12个月。

每个预测将与实际观察数据进行比较；哪个模型平均最接近——哪个就赢。

*顺便提一下：这本质上是一个交叉验证测试。如果你熟悉标准的交叉验证方法，但在时间序列分析中没有使用过，你可能会发现下面的（2）非常有用。*

# 图片

我们可以可视化每个模型的时间外预测——LASSO为红色，Prophet为蓝色——并将其与实际数据进行比较。

我们应该记住，每个预测都是使用预测期前的所有数据构建的。例如，2010年的预测是使用截至2009年的数据建立的。

![](../Images/7fa0f27ffe2ed18cc2ab355971c56bc9.png)

作者提供的图片

这是一个相当清晰的画面：除了2013年之外，Prophet看起来都有点偏离预期。

有趣的是，需要注意的是两种方法创建的预测模式的相似性：

+   两种模型都产生*较低*的预测——即它们反映了总体趋势的下降趋势。

+   两种模型都有年内增长和年中高峰——即预测产生类似的季节性模式。

两个模型与现实有多大距离？我们需要查看一些性能指标来找出答案。

# 在数字上

我们将使用常见的性能指标来衡量性能——平均绝对误差（MAE）、平均绝对百分比误差（MAPE）和均方根误差（RMSE）——以及一个新手（至少对我来说是新手）：MASE。

## 平均绝对缩放误差

平均绝对缩放误差（MASE）是“普遍适用的预测准确度测量，避免了其他测量中出现的问题”³，并且“可用于比较单个系列的预测方法以及在系列之间比较预测准确度”³。

从数学上讲，MASE是超出时间预测误差与由天真预测方法产生的样本内预测误差之比。由于我们使用的是月度数据，我将天真预测预测值视为前一年同一时间点的值——例如，2012年5月的预测只是2011年5月的值。非常天真。

在比较预测方法时，具有最低MASE的方法是首选方法³。

需要注意的是，MASE > 1意味着预测方法相对于天真预测表现不佳。

*旁注：我使用了链接文章中描述的实现——即“误差”是平均绝对误差。我相信我们可以在比例误差计算中一致使用其他性能度量，例如MAPE——只要误差度量是一致的。*

## 结果

让我们总结一下使用我们描述的指标来总结折叠外和整体平均模型性能：

![](../Images/f0d72a67d03c1f9b88d00bc3b9723e00.png)

作者提供的图片

对于LASSO模型来说，这是一个相当全面的胜利，Prophet只在局部表现优越。

# 刀光剑影？

如我们所见，如果你是Prophet的粉丝，这并不是一个令人愉快的阅读：Meta的工具设法夺走了一些分数（取决于度量标准），以避免完全被淘汰。公正的评论可能会建议返回俱乐部重新评估策略。

虽然Prophet的结果不太理想，但有几个原因可以解释这种性能。

## 特征

LASSO模型使用了为这一特定时间序列专门设计的特征。它可用的输入特征集本质上是Prophet可用特征的超集，并且有一些额外的特性。

此外，LASSO模型中的某些特征微妙地不同。例如，描述潜在变化点的特征在LASSO模型中不像在Prophet模型中那样受到限制。

把它看作是试图超越别人，但你对他们了解的少一些——或者有些不同。并不容易。

## 建模

超出折叠的数据并不像我描述的那样“未见过”。

在之前的文章中，我们介绍了LASSO模型的参数化：我们如何使用超出折叠的数据来选择优化模型预测能力的正则化强度。从这个意义上讲，LASSO模型已被调整以在每一组数据上进行良好的预测，而Prophet模型则是直接投入实际使用。

在“正常”的超参数优化中，我们通常可以期望性能提升约1%—2%；在时间序列的背景下，性能提升可能更大，因为“超出折叠”确实是“超出时间”。

## 那么是时候结束Prophet的讨论了吗？

不要急于下结论……这系列文章确实突出了几点——让我们逐一讨论一下。

初始状态下，Prophet工作*非常出色*。虽然它确实可以被超越，但做到这一点需要比仅用10行代码启动和预测Prophet更多的工作。

LASSO模型的可解释性远超Prophet模型。是的，Prophet确实为预测提供了不确定性的估计，但我们无法了解究竟是什么驱动了这些预测。我甚至不确定我们能否将Prophet应用于SHAP。

我还发现Prophet的调整并不那么简单。也许是因为我不是该包的高级用户，或者是因为调整参数的方式曲折。LASSO模型显然不是这样。

尽管LASSO方法在性能和可解释性上确实有所提升，也许我们真正需要的是结合这两种方法：一种作为另一种的试金石。例如，如果“天真的”Prophet模型产生了合理的预测，那么复制LASSO方法（“虚假的先知”）以最大化性能可能是合理的。

我说完了。希望你们阅读这系列文章的体验与我写这些文章的乐趣一样。

一如既往，请告诉我你的想法——我非常希望了解你在使用Prophet或以不同方式建模时间序列的经验。

下次见。

# 参考资料和有用资源

1.  [https://roadtraffic.dft.gov.uk/downloads](https://roadtraffic.dft.gov.uk/downloads) 使用自 [开放政府许可证 (nationalarchives.gov.uk)](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/)

1.  [我们来做：时间序列交叉验证 | Python 简明英语](https://python.plainenglish.io/on-times-series-cross-validation-6d685eaf335b)

1.  [均值绝对尺度误差 — 维基百科](https://en.wikipedia.org/wiki/Mean_absolute_scaled_error)
