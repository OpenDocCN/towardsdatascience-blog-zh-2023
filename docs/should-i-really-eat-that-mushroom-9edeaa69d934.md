# 我真的应该吃这个蘑菇吗？

> 原文：[`towardsdatascience.com/should-i-really-eat-that-mushroom-9edeaa69d934`](https://towardsdatascience.com/should-i-really-eat-that-mushroom-9edeaa69d934)

## 使用 CatBoost 梯度提升决策树对可食用和有毒蘑菇进行分类

[](https://medium.com/@caroline.arnold_63207?source=post_page-----9edeaa69d934--------------------------------)![Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----9edeaa69d934--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9edeaa69d934--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9edeaa69d934--------------------------------) [Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----9edeaa69d934--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----9edeaa69d934--------------------------------) ·阅读时间 6 分钟·2023 年 8 月 17 日

--

大多数教育和现实世界的数据集包含*分类特征*。今天我们将讨论来自[CatBoost](https://catboost.ai)库的梯度提升决策树，该库原生支持分类数据。我们将使用一个蘑菇数据集，这些蘑菇要么是可食用的，要么是有毒的。蘑菇通过分类特征如颜色、气味和形状进行描述，我们想要回答的问题是：

> 基于其分类特征，这种蘑菇是否安全食用？

如你所见，风险很高。我们希望确保机器学习模型的准确性，以免我们的蘑菇煎蛋卷以灾难收场。**作为额外奖励，最后我们将提供一个特征重要性排名，告诉你*哪个分类特征*是蘑菇安全性的最强预测因素。**

![](img/9cdc72d1005c4c8a46835b140da5f21e.png)

图片由[Andrew Ridley](https://unsplash.com/@aridley88?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 介绍蘑菇数据集

蘑菇数据集可以在这里找到：[`archive.ics.uci.edu/dataset/73/mushroom`](https://archive.ics.uci.edu/dataset/73/mushroom) [1]。为了清晰展示，我们将从原始的简短变量中创建一个[pandas DataFrame](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html)，并用适当的列名和长格式变量进行注释。我们使用 pandas 的`replace`函数，长格式变量来自数据集描述。目标变量只能取*True*和*False*值——数据集创建者采取了保守的方式，将可疑的蘑菇归类为不可食用。

在检查数据集缺失值后，我们发现只有一列——`stalk_root`——受到影响。我们删除了这一列。

数据集的探索揭示数据相当平衡：在 8124 个蘑菇中，4208 个是可食用的，3916 个是有毒的。我们将数据框分为目标变量`is_edible`和其余的蘑菇特征。然后，我们通过对目标变量进行分层，将数据集分为训练数据和测试数据。这确保了两个拆分中的类别分布是可比较的。

## CatBoost 库

[CatBoost](https://catboost.ai/en/docs/) 是一个开源的机器学习包，用于梯度提升决策树。可以通过[按照安装说明](https://catboost.ai/en/docs/concepts/python-installation)来获取 CatBoost Python 包。对我们来说最重要的组件是`catboost.Pool`，它组织数据集并指定分类特征和数值特征，以及我们的模型`catboost.CatBoostClassifier`。分类特征在机器学习算法中可能难以处理，它们必须被编码成数值才能用于训练。每个分类值都与一个数字相关联，例如蘑菇颜色的`brown->0, black->1, yellow->2, ...`。**CatBoost 可以自动处理分类输入变量，这样我们就不用再添加** [**独热编码**](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) **到流程中。这不仅方便，而且 CatBoost 算法也经过优化，以便更快地训练分类变量。**

![](img/d9ccfa30ef98feeaa0672670601b8ba4.png)

由 [Haithem Ferdi](https://unsplash.com/@haithemfrd_off?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 拍摄

## 梯度提升决策树

决策树是成熟的机器学习算法，根据特征值将样本分类为不同的类别。单棵决策树容易过拟合。 因此，通常使用决策树的集合来实现更好的性能。在梯度提升决策树中，树的集合通过迭代更新树来构建。每次迭代的树通过在应用前一个树后留下的*残差*上进行训练，提供了比前一次迭代更小的改进。该过程在损失收敛时停止，即当添加更多树没有增值时，或达到固定的总树数时。有关梯度提升决策树的更详细介绍，请参见页面底部推荐的博客文章。

## 蘑菇分类

在蘑菇数据集中，所有特征都是分类的，并在`Pool`中相应指定。我们为训练和测试分别构建一个`Pool`。目标变量被转换为数值，因为这与`CatBoostClassifier`的损失函数更好地集成。分类器本身的格式类似于 scikit-learn。可以调整许多属性，包括学习率、树的总数和树的正则化。损失函数是`log-loss`，因为我们处理的是二分类问题。

对于二分类目标类的预测，使用的是对数损失或交叉熵函数。实际值 y 与模型提供的概率 p 进行比较。

我们在下面的代码框中定义数据集和模型。为了比较，我们训练了一个单一的决策树和一个完整的梯度提升决策树。

## 评估

现在我们准备评估分类器在测试数据上的表现。食用毒蘑菇可能导致严重的健康问题，因此我们关注减少假阳性。我们计算*精确度指标*，即*实际可食用*的蘑菇数量与*预测为可食用*的蘑菇数量的比例。

单一决策树的精确度为 97%，对于一个分类算法来说相当不错。但是通过梯度提升树，我们可以将精确度提高到 100%，测试数据集中没有毒蘑菇被误标为可食用。混淆矩阵显示，梯度提升决策树在测试集上提供了最佳表现。

![](img/60d9f5dcb042bc0944ace08142073c87.png)

单一决策树（左）和梯度提升决策树（右）的混淆矩阵。

## 特征重要性

这很好，但我们可能没有整天的时间来确定每种我们想吃的蘑菇的 22 个特征。那么，确定蘑菇是否可食用的最重要特征是什么呢？

为了回答这个问题，我们使用内置的模型属性`feature_importances_`来推导梯度提升树分类器的特征重要性排名。结果显示，**气味**在特征重要性排名中占据主导地位，其次是**孢子** **印刷颜色**和**数量**。

![](img/9b3141240daddb3f8c64b2e366ef45b3.png)

从训练好的 CatBoostClassifier 中获得的蘑菇数据集的特征重要性排名。

进一步观察可能的气味值可以发现，这个特征本身已经是一个很好的预测因素，能够判断一只蘑菇是会成为你餐点的美味补充，还是会让你一天结束在医院。数据集中所有散发*茴香*或*杏仁*气味的蘑菇都是可食用的。没有气味的蘑菇大多也是可食用的。你应该远离*腥臭、辛辣、刺鼻、腐臭、木焦油*和*霉味*的蘑菇——老实说，这些蘑菇听起来一开始就不怎么美味。

![](img/342280e37604d4a5a860543fce141109.png)

对蘑菇数据集中的气味特征进行详细分析。

## 摘要

我们介绍了蘑菇数据集，其中包含仅由分类变量描述的可食用和有毒蘑菇样本。我们引入了 catboost 包，它对分类数据效果很好，并提供了梯度提升决策树。训练了一个模型来对蘑菇进行分类，并取得了令人满意的表现。气味是预测蘑菇安全性的最强指标。希望你喜欢这篇博客文章，并对模型在真实蘑菇上的应用不负责任 :-)。

![](img/b4c3717d6a8fcbe8bfb9ba972d530879.png)

[照片由 Zhen H](https://unsplash.com/@zhenh2424?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

## 深入阅读

+   `towardsdatascience.com/decision-trees-and-random-forests-df0c3123f991`

+   `towardsdatascience.com/machine-learning-part-18-boosting-algorithms-gradient-boosting-in-python-ef5ae6965be4`

+   [`medium.com/swlh/essential-embeds-for-mediums-technical-writers-c09a4dda7ae4`](https://medium.com/swlh/essential-embeds-for-mediums-technical-writers-c09a4dda7ae4)

+   [`catboost.ai/en/docs/`](https://catboost.ai/en/docs/)

## 数据集参考

[1] 蘑菇。UCI 机器学习库（1987 年）。[`doi.org/10.24432/C5959T.`](https://doi.org/10.24432/C5959T.) 本数据集采用[创意共享署名 4.0 国际](https://creativecommons.org/licenses/by/4.0/legalcode)（CC BY 4.0）许可。
