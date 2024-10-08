# XGBoost 如何处理多类分类？

> 原文：[`towardsdatascience.com/how-does-xgboost-handle-multiclass-classification-6c76ba71f6f0`](https://towardsdatascience.com/how-does-xgboost-handle-multiclass-classification-6c76ba71f6f0)

## 理解使用这种模型的分类机制至关重要，因为它影响到性能。

[](https://medium.com/@guillaume.saupin?source=post_page-----6c76ba71f6f0--------------------------------)![Saupin Guillaume](https://medium.com/@guillaume.saupin?source=post_page-----6c76ba71f6f0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6c76ba71f6f0--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----6c76ba71f6f0--------------------------------) [Saupin Guillaume](https://medium.com/@guillaume.saupin?source=post_page-----6c76ba71f6f0--------------------------------)

·发表于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----6c76ba71f6f0--------------------------------) ·阅读时间 7 分钟·2023 年 1 月 7 日

--

![](img/23c7c84600c2af0aa36124702225ee8e.png)

图片来源于 [Andrew Coop](https://unsplash.com/@andrewcoop?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在本文中，我们将了解使用梯度提升库如 XGBoost、LightGBM 和 CatBoost 训练的决策树集成如何进行多类分类。

确实，决策树的集成将一个实数值关联到一组特征，因此问题是：决策树集成如何将一个标量值转换为多类标签？

理解使用这种模型的分类机制至关重要，因为它影响到性能。

我们将按照以下计划逐步进入主题：

+   Python 中二分类的提醒和示例

+   首先，使用 XGBoost 作为回归器进行二分类

+   第二次，使用 XGBoost 作为分类器进行二分类

+   使用 XGBoost 进行多类分类

# 基于决策树的集成模型的多样性

XGBoost、LightGBM 或 CatBoost 是共享（默认情况下）相同类型底层模型的库：决策树。

这些决策树通过梯度提升迭代组合。*即* 向当前树中添加新节点，以便优化非线性目标，通常是平方误差。为了处理非线性，目标使用其梯度和海森矩阵进行线性化。

因此叫做梯度提升。更多细节请参见我之前的论文：

[](/diy-xgboost-library-in-less-than-200-lines-of-python-69b6bf25e7d9?source=post_page-----6c76ba71f6f0--------------------------------) ## 在不到 200 行 Python 代码中实现 DIY XGBoost 库

### XGBoost 的解释以及梯度提升方法和超参数调优，通过构建自己的梯度提升库来实现……

towardsdatascience.com

# 使用决策树集成进行预测

提醒一下，预测过程相对简单：给定一行数据，每棵决策树都会被遍历。

根据特征的值，每棵树会将一个唯一值关联到最终的叶节点上。

然后，每棵树的唯一预测值被简单地加总起来，得到总体预测结果。

下面的图例在一个简单的示例中说明了这一点，其中一个决策树的集成模型了一个在 1 到 4 之间的整数的身份函数：

![](img/ba83d213afa3f8ccc3cfeff175032d65.png)

一个简单的决策树集成。图示由作者提供。

例如，当输入为 1 时，第一个树生成 8，第二个树生成-6，最后一个树生成-1。将这三个值加总得到 1，这是期望的输出。

> 这个例子摘自我的书籍，[实用梯度提升](https://amzn.to/3UVVVgw)，关于梯度提升的内容：

[](https://amzn.to/3QmRqtO?source=post_page-----6c76ba71f6f0--------------------------------) [## 实用梯度提升：深入研究 Python 中的梯度提升

### 这本关于梯度提升方法的书籍是为那些希望……](https://amzn.to/3QmRqtO?source=post_page-----6c76ba71f6f0--------------------------------)

使用单一标量值时，我们所能做的最好是进行二分类，将负预测标记为一个类别，将正预测标记为另一个类别。

## 没有 XGBoost 的二分类

在发现第一个选项，即将 XGBoost 作为回归器进行二分类之前，让我们详细展示二分类是如何完成的。

我们在这里尝试解决的问题很简单：我们想要根据学生花费在学习科目上的小时数来确定其成功的概率。

下面的图展示了收集的数据，即工作小时数和结果：***通过*** 或 ***失败***。

![](img/5f8fa9a1aa130304439e42a47c837ba8.png)

成功的概率取决于学习时间。图示由作者提供。

用于分类的标准模型是 Logistic 函数。该函数类似于线性回归，不同之处在于它生成的值仅在[0, 1]范围内，而不是ℝ范围内。其公式值得了解：

![](img/fd395a8e9b470b506a04011e19e41523.png)

Logistic 函数。公式由作者提供。

和机器学习中的情况一样，为模型（这里是逻辑函数）找到最佳参数是通过最小化误差来完成的。面对一个二分类问题，其中正输出可以用 1 建模，负输出可以用 0 建模，可以将两种误差结合成一个单一的表达式：

![](img/bca3e2741a00b9c3363e4c60fad3c14a.png)

简单的误差函数。由作者提供的公式。

其中`y_k`是观察到的样本，而`f(x_k)`是模型`f`做出的预测。

这个基本误差的问题在于，根据逻辑函数的二元性质，它主要取两个值：零和一，因此相对于模型参数`m`的误差也主要取两个值。因此，在最佳参数的邻域之外，误差将是平坦的。

![](img/fc65388bde9bfdc5c3f4fac1e6d0c772.png)

直接使用误差时的饱和现象。由作者提供的图。

我们可以使用这个公式，这样可以工作，只要我们提供一个相当好的最佳参数估计。如果不是这样，我们可能会陷入一个误差几乎恒定的平坦区域。在这个区域，梯度将几乎为零，最陡下降法的收敛速度将非常缓慢。

我们需要一种方法来处理误差输出，将其从给定样本的范围[0, 1]扩展到ℝ+，以避免饱和现象。

额外的约束是零误差在变换后必须仍然是零误差。

诀窍是认识到 log(1)为零，而 log(0)为-**∞**。

因此使用对数损失：

![](img/cceada6be3af4ee683eaffcc264a3044.png)

对数损失。由作者提供的公式。

其中`y_k`是观察到的样本，而`f(x_k)`是模型`f`做出的预测。注意加法运算符前的负号和`1-f(x_k)`的取反与`f(x_k)`的交换。这是因为`log(1)=0`。

使用对数损失，误差不再饱和：

![](img/7c4a33ea704ffa9529c15e89b3640304.png)

使用对数损失的误差图。由作者提供的图。

最简单的最小化这个误差的方法是使用最陡下降法，这只需要计算误差的梯度。可以有多种方法来实现这一点。这里我们将使用`sympy`进行符号微分：

逻辑回归。由作者提供的代码。

算法找到了预期值 14.77，这个值非常接近理论值。

现在让我们回到主题，使用决策树和梯度提升进行二分类。

## 使用 XGBoost 进行二分类

让我们从一个简单的例子开始，使用[Cleveland Heart Disease Dataset](https://archive.ics.uci.edu/ml/datasets/heart+disease)（CC BY 4.0），其中分类是通过回归完成的。由于我们进行的是二分类，可以使用简单回归，因为我们可以将正标签附加一个正值 1.0，将负标签附加一个负值-1：

使用回归器进行分类。作者提供的代码。

XGBoost 使用的默认错误是平方误差。预测被四舍五入为整数，正如混淆矩阵所示，模型的预测没有误差。

直接使用 XGBoost 分类器也能实现类似的结果：

使用分类器进行分类。作者提供的代码。

在这种情况下，无需将预测值四舍五入以获取相应的类别。所有的工作都是由 XGBClassifier 原生完成的。让我们看看 XGBoost 是如何处理的。

## XGBClassifier 训练多个模型

实际上，当你使用 XGBoost 进行分类时，使用 XGBClassifier（或用适当参数进行分类的 xgb.train），XGBoost 实际上训练了多个模型，每个类别一个。

下面的代码片段展示了如何深入了解 XGBoost 的内部实现。

获取每个类别的单独概率。作者提供的代码。

更具体地说，`predict_proba` 方法允许访问内部模型生成的原始数据。这清楚地揭示了，在进行分类时，XGBoost 为每个类别做出概率预测。

预测的类别是具有最高概率的那个。

查看将 XGBoost 集成到 `sklearn` 的代码，我们确认 XGBoost 进行多个预测：

提取自 XGBoost 的开源代码。参见 [`github.com/dmlc/xgboost/blob/master/python-package/xgboost/sklearn.py#L1541`](https://github.com/dmlc/xgboost/blob/master/python-package/xgboost/sklearn.py#L1541)

如可见，第 25 行中使用了 `argmax` 来检索使用 `softprob` 时具有最高概率的类别索引。在使用 `softmax` 作为目标的情况下，预测值直接转换为整数。

## XGBoost 如何执行多类分类？

通常，关于 XGBoost 如何处理多类分类的解释说明它训练了多个树，每个类别一个。

实际情况并非如此。事实上，所有的树是同时构建的，使用的是向量目标函数而不是标量目标函数。*即* 每个类别都有一个目标。

XGBoost 的 [文档](https://xgboost.readthedocs.io/en/stable/python/examples/custom_softmax.html) 提供了这种目标的示例：

提取自 XGBoost 文档。参见 [`xgboost.readthedocs.io/en/stable/python/examples/custom_softmax.html`](https://xgboost.readthedocs.io/en/stable/python/examples/custom_softmax.html)

这个代码片段中有两件非常有趣的事情：

1.  当使用 XGBoost 中的集成目标时，目标名称是 `multi:softprob`。这很令人困惑，因为实际目标并非 `softprob`，而是 `softmax` 的对数损失。这在代码中表现得很清楚，因为梯度直接是 `softmax`。但 `softmax` 不是 `softmax` 的梯度，而是其对数损失的梯度：

![](img/0eb5f103ba26452f16ba23cfde091843.png)

softmax 的对数损失的梯度。由作者提供的公式。

2\. 另一个点是代码使用了一个变量`hess`，代表海森矩阵。然而，从数学角度来看，这实际上不是海森矩阵，而是二阶导数。因此，这个变量的正确名称应该是拉普拉斯算子。

![](https://www.buymeacoffee.com/guillaumes0)

[`www.buymeacoffee.com/guillaumes0`](https://www.buymeacoffee.com/guillaumes0)

# 结论

我们在这篇论文中展示了 XGBoost 如何处理分类问题。

理解分类`n`个类别会生成`n`倍复杂的树是至关重要的。

同样重要的是注意，XGBoost API 中暴露的目标函数名称并不总是非常明确。例如，在进行分类时，优化的目标不是`softmax`或`softprob`，而是它们的对数损失。

如果你想了解更多关于梯度提升方法的详细信息，请查看我的书：

[](https://amzn.to/3Zeq8tU?source=post_page-----6c76ba71f6f0--------------------------------) [## 实用梯度提升：深入探讨 Python 中的梯度提升

### 这本关于梯度提升方法的书籍是为希望深入了解该领域的学生、学者、工程师和数据科学家们准备的 [梯度提升方法](https://amzn.to/3Zeq8tU?source=post_page-----6c76ba71f6f0--------------------------------)。
