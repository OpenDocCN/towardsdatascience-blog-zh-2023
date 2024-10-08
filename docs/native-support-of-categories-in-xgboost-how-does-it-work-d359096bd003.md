# XGBoost 如何原生支持类别特征？

> 原文：[`towardsdatascience.com/native-support-of-categories-in-xgboost-how-does-it-work-d359096bd003`](https://towardsdatascience.com/native-support-of-categories-in-xgboost-how-does-it-work-d359096bd003)

[](https://medium.com/@guillaume.saupin?source=post_page-----d359096bd003--------------------------------)![Saupin Guillaume](https://medium.com/@guillaume.saupin?source=post_page-----d359096bd003--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d359096bd003--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d359096bd003--------------------------------) [Saupin Guillaume](https://medium.com/@guillaume.saupin?source=post_page-----d359096bd003--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d359096bd003--------------------------------) ·6 分钟阅读·2023 年 3 月 22 日

--

![](img/58e881d68aca5ae4452f0262d11dbeb4.png)

图片由[Jon Tyson](https://unsplash.com/fr/@jontyson?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

XGBoost 和其他基于决策树的方法通过梯度提升进行训练，使用比较来做出决策。为类别定义一个比较操作符在数学上并不简单。

在这篇文章中，我们将解释可用的选项，详细说明它们的优缺点，并重点关注最近在 XGBoost（以及 LightGBM）中引入的对分类特征的原生支持。

如果你对梯度提升及其在决策树训练中的应用感兴趣，请考虑我的书：

[](https://amzn.to/3LDmbKM?source=post_page-----d359096bd003--------------------------------) [## 实用梯度提升：深入了解 Python 中的梯度提升

### 这本关于梯度提升方法的书面向希望深入了解的学生、学者、工程师和数据科学家……

amzn.to](https://amzn.to/3LDmbKM?source=post_page-----d359096bd003--------------------------------)

# 决策树

如下图所示，决策树基于比较来做出决策：

![](img/84da55d5e84bba3b2cd7d8818c6eb606.png)

一个简单的决策树。图由作者提供。

例如，在这个简单的例子中，如果输入是一行数据，包含两列`A=21`和`B=111`，则输出值将是权重`4`。

这种决策树的局限性在于它只能处理数字作为特征。

# 处理分类特征的标准方法

然而，在数据科学中，经常会遇到分类特征。让我们看看现有的选项。

## 一热编码

处理类别值的一种常见方法是使用 ***独热编码***。这里的想法是通过为每个可能的类别创建一列，将非数值类别值转换为数值值。

当类别适用于数据集的当前行时，相应的列被设置为 1，否则为 0。

以下代码片段展示了使用 pandas 进行独热编码的标准方法：

使用 Pandas 进行独热编码。由作者编写的代码。

独热编码的主要限制之一是，你将为数据集中每个不同的类别值添加相应的列。

同样重要的是，在训练和预测时必须使用相同的离散值，否则在预测过程中会缺少某些列。

## GLMM 编码

另一种选择是使用编码，更具体地说是 GLMM 编码。这里的想法是通过模型将非数值类别转换为数字。

转换是通过 GLMM 完成的，即 ***广义线性混合模型***。

这里 *广义* 表示 GLMM 只是线性模型的一种推广，其中目标变量可以通过对数等函数进行变换。因此，利用相同的数学框架，可以对标准线性回归和逻辑回归进行建模。

而 *混合* 表示这些模型可以整合固定效应和随机效应，即预测变量的均值在一组观测中要么是固定的，要么是随机的。

应用于类别编码的情况时，GLMM 将捕捉每个类别的随机效应，并适配这种模型：

![](img/6b592943e1597960daf8abbac1d04c64.png)

混合模型预测类别 I 的 Y。由作者提供的公式。

其中 `Y_i` 是 Mixel 模型为目标 `Y` 和类别 `i` 预测的值。μ 是 `Y` 的全局均值，表示固定效应，而 `RE_ci` 是由于类别 `i` 引起的随机效应。

因此 `RE_ci` 捕捉了类别的效应，这就是类别的编码值。

使用 `pandas` 和 `categorical_encoders` 库，所有这些理论可以归纳为以下几行代码：

GLMM 编码与 Pandas。由作者编写的代码。

如果你感兴趣，`categorical_encoders` 库中的重要代码行是：

由作者编写的代码。

这正好训练了一个由上面的公式描述的模型。

# XGBoost 原生支持

如上所述，可以通过将类别值转换为实际值（glmm 编码）或通过更改数据集结构并为每个可能的类别引入一列（独热编码）来改变类别值的属性。

但这似乎不是在标准的梯度提升结构中整合类别支持的最佳方式：决策树。

## 包括而非比较

确实，似乎自然地将比较替换为包含作为决策操作。即，与检查一个值是否大于或小于给定阈值不同，我们可以检查该值是否在给定的类别集合中。

这意味着我们可以用一个阈值来替代比较。

![](img/4b5fa8ac89baf4463893a7410c69fef3.png)

作者公式

作者

![](img/4a8f7681f96bd4e7fd18bc2b02b521ad.png)

作者公式

基于我上一篇文章中的代码：

[](/diy-xgboost-library-in-less-than-200-lines-of-python-69b6bf25e7d9?source=post_page-----d359096bd003--------------------------------) ## DIY XGBoost 库，少于 200 行 Python

### XGBoost 解释以及梯度提升方法和超参数调整，通过构建你自己的梯度提升库来实现...

towardsdatascience.com

这可以通过更新创建节点条件函数的方法来实现，以支持包含而不是比较：

使用类别时创建一个包含条件。作者代码。

## 穷尽生成可能的类别列表

在标准方法中，使用值作为分割条件，通过对考虑的列中的值进行排序并保持唯一值来生成候选分割。

根据这个分割值，评估左节点和右节点的增益。

当使用类别时，排序不再相关，候选分割条件不再是单一值，而是类别列表。

由于我们对优化增益的最佳类别分组方式没有先验知识，一种选择是生成所有可能的类别组合。

因此，根据我上一篇文章中的代码，我们提取了创建基于阈值的数值分割的`_numerical_split`代码，并引入了一个新方法`_categorical_split`来生成可能的组合：

为数值和类别类型生成候选分割。作者代码。

请注意，pandas 掩码是一种非常方便的方式来类似地处理这两种情况。

## 将所有内容汇总

完整代码，从头实现了决策树的梯度提升，并支持类别数据和数值数据，见下文：

作者的完整代码。

除了上述两个主要更改外，代码基本与我上一篇文章中的相同，但稍微更通用，以便同时支持数值数据和类别数据。

在提供的基本示例中，使用类别分割或数值分割得到相同的结果。当使用类别支持时，代码只会稍微慢一些，因为探索的组合数量很重要：

探索的组合。作者代码。

请注意，我们使用内部 pandas 代码 `category` 来区分数值数据和分类数据。

# 性能问题

上述实现按预期工作，并支持分类值。

然而，由于我们正在全面探索所有可能的类别组合，使用 python `itertools.combinations` 时，当唯一类别超过几个时，代码会变得非常慢。

实际上，对于一组 `n` 个分类值，`k` 的可能组合数由以下公式给出：

![](img/f382f0e0eb55001c7a1b85c1164d69ef.png)

可能的组合数。公式由作者提供。

这就是为什么 XGBoost 和 LightGBM 使用启发式方法来大幅减少这个数量。更多细节可以在这里找到：

[](https://xgboost.readthedocs.io/en/stable/tutorials/categorical.html?source=post_page-----d359096bd003--------------------------------#optimal-partitioning) [## 分类数据 - xgboost 1.7.4 文档

### 注意：截至 XGBoost 1.6，此功能仍为实验性，功能有限。从 1.5 版本开始，XGBoost 具备了...

xgboost.readthedocs.io](https://xgboost.readthedocs.io/en/stable/tutorials/categorical.html?source=post_page-----d359096bd003--------------------------------#optimal-partitioning) ![](https://www.buymeacoffee.com/guillaumes0)

[`www.buymeacoffee.com/guillaumes0`](https://www.buymeacoffee.com/guillaumes0)

# 结论

我强烈建议在使用 XGBoost 或 LightGBM 时使用对分类数据的内部支持。这不仅是一种更高效的内存和计算方法，而且提供了良好的精度。

它还简化了数据准备流程，并在训练和预测集不重叠时原生支持缺失值。

如果你对梯度提升及其在决策树中的应用感兴趣，请考虑阅读我的书：

[](https://amzn.to/3LDmbKM?source=post_page-----d359096bd003--------------------------------) [## 实用梯度提升：深入探讨 Python 中的梯度提升

### 这本关于梯度提升方法的书旨在为希望...

amzn.to](https://amzn.to/3LDmbKM?source=post_page-----d359096bd003--------------------------------) [](https://www.buymeacoffee.com/guillaumes0?source=post_page-----d359096bd003--------------------------------) [## Guillaume Saupin

### Guillaume Saupin，博士学位持有者及应用数学专家，在数学界因其...

www.buymeacoffee.com](https://www.buymeacoffee.com/guillaumes0?source=post_page-----d359096bd003--------------------------------)
