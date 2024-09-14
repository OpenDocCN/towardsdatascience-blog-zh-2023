# XGBoost：深度学习如何取代梯度提升和决策树 — 第一部分

> 原文：[`towardsdatascience.com/xgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-291dc9365656`](https://towardsdatascience.com/xgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-291dc9365656)

[](https://medium.com/@guillaume.saupin?source=post_page-----291dc9365656--------------------------------)![Saupin Guillaume](https://medium.com/@guillaume.saupin?source=post_page-----291dc9365656--------------------------------)[](https://towardsdatascience.com/?source=post_page-----291dc9365656--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----291dc9365656--------------------------------) [Saupin Guillaume](https://medium.com/@guillaume.saupin?source=post_page-----291dc9365656--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----291dc9365656--------------------------------) ·阅读时长 7 分钟·2023 年 5 月 29 日

--

![](img/d836a60a327155b897054d6830cd01dc.png)

图片由 [Preethi Viswanathan](https://unsplash.com/@sallybrad2016?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在本文中，你将了解如何使用可微编程方法重写决策树，这种方法由 NODE 论文提出，使得该模型可以以类似于神经网络的方式进行重新表述。

推导这种表述是一个很好的练习，因为它提出了在构建和训练自定义神经网络时常见的许多问题：

+   如何避免梯度消失问题？

+   什么是良好的初始权重选择？

+   为什么使用批量归一化？

在回答这些问题之前，让我们看看如何将决策树重新表述为神经网络的数学框架：可微编程。

更新 — 第二篇文章已上线：

[](/xgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-part-2-training-b432620750f8?source=post_page-----291dc9365656--------------------------------) ## XGBoost：深度学习如何取代梯度提升和决策树 — 第二部分：训练

### 一个没有 if 的世界

towardsdatascience.com

# XGBoost 非常高效，但…

如果你读过我之前关于梯度提升和决策树的文章，你会知道梯度提升结合决策树集成在分类或回归任务中表现出色，尤其是在涉及表格数据时。

[](https://amzn.to/3IKyqTF?source=post_page-----291dc9365656--------------------------------) [## 实用的梯度提升：深入探讨 Python 中的梯度提升

### 本书关于梯度提升方法，旨在为希望深入了解的学生、学者、工程师和数据科学家提供参考…

amzn.to](https://amzn.to/3IKyqTF?source=post_page-----291dc9365656--------------------------------)

尽管 XGBoost、LightGBM 或 CatBoost 在增强决策树集成构建方面做出了努力，但训练这样的模型仍然依赖于蛮力方法。它涉及系统地评估拆分值和特征相关的增益。

请参考我之前关于此主题的文章：

[](/diy-xgboost-library-in-less-than-200-lines-of-python-69b6bf25e7d9?source=post_page-----291dc9365656--------------------------------) ## 在不到 200 行 Python 代码中实现 XGBoost 库

### XGBoost 的解释以及梯度提升方法和 HP 调优，通过构建你自己的梯度提升库来实现…

towardsdatascience.com

现代机器学习模型及程序的训练方法涉及使用可微分编程。如果你对这一概念不熟悉，我已经撰写了一篇介绍性文章：

[](/differentiable-programming-from-scratch-abba0ebebc1c?source=post_page-----291dc9365656--------------------------------) ## 从零开始的可微分编程

### 去年我有幸参加了 Yann Lecun 在 Facebook 人工智能部门的讲座…

towardsdatascience.com

本质上，思想是识别模型/程序中需要优化的参数，并使用基于梯度的方法来找到最优参数。然而，为了使其工作，你的模型必须对感兴趣的参数是可微分的。

尽管决策树集成方法采用了基于梯度的方法进行训练，但它们的构建过程却不可微分。

# 构建决策树不是一个可微分的过程

决策树是一种简单而强大的数据结构，具有广泛的应用。当为回归或分类任务训练这样的结构时，我们的目标是识别三个主要参数：

1.  用于每个决策节点的特征，以最小化错误，这涉及特征选择。

1.  用于在每个节点拆分输入数据的阈值。具有所选特征值大于阈值的数据将分配到右子节点，而其余的数据将分配到左子节点。

1.  分配给树叶的值。

前两步是不可微分的。在当前的梯度提升树实现中，第一步需要对特征进行迭代，并且是不光滑的。第二步涉及使用结果数据分区和可能值列表来评估增益。这个过程也是不可微分的。

幸运的是，2019 年，Popov、Morozov 和 Babenko 提出了可微分的决策树公式。

[](https://arxiv.org/abs/1909.06312?source=post_page-----291dc9365656--------------------------------) [## 神经盲决策集成用于表格数据的深度学习

### 如今，深度神经网络（DNNs）已成为广泛范围内机器学习任务的主要工具...

arxiv.org](https://arxiv.org/abs/1909.06312?source=post_page-----291dc9365656--------------------------------)

我强烈推荐阅读他们的论文，因为从源头获取信息总是有益的。不过，为了方便理解，我用 Jax 编写了一些 Python 代码。

论文中讨论的问题包括：

1.  我们如何重新表述特征选择，使其平滑且可微分？

1.  我们如何重新表述比较，使其平滑且可微分？

# 特征选择正则化

如前所述，标准梯度提升算法中的特征选择是通过穷举搜索来进行的：我们尝试每一个特征，并保留提供最高增益的特征。

我们需要将选择重新表述为一个函数，该函数返回确保最大增益的特征值，而不是这种迭代过程。

首先，假设特征被收集在一个向量中。我们需要一种方法来从该数组中提取所选特征的值，理想情况下使用向量运算。从我们已经知道要保留的特征开始，我们可以通过简单的点积来实现：

```py
features = np.array([1.1, 3.2, 4.5])
selection_weights = np.array([0, 0, 1])
selected_feature_value = np.dot(selection_weights, features)
print(selected_feature_value)
# Output: 4.5
```

因此，我们可以通过将所有特征乘以 0（除了需要保留的特征），然后求和所有乘积，来从特征数组中提取特定值。

学习实现最高性能的特征涉及学习向量`selection_weights`，并附加约束条件：其元素之和必须等于 1。理想情况下，这 1 应该集中在向量的单一元素上。

对选择向量的范数施加此约束可以通过函数来实现。一个显然的候选者是 softmax 函数，它将权重向量的范数强制为 1。然而，softmax 生成的是光滑分布，无法生成 1 作为最高权重而其他为 0。

以下代码片段演示了这一点：

```py
import numpy as np
from jax.nn import softmax
from entmax_jax import entmax15, sparsemax

weights = np.array([2, 3, 5, 7, 11, 17])
print(np.linalg.norm(softmax(weights)))
# Output: 0.99747807
print(softmax(weights))
# Output: [3.0512990e-07 8.2942910e-07 6.1286983e-06 4.5285295e-05 2.4724933e-03 9.9747497e-01]
```

不仅范数不完全等于 1.0，而且分布分布在所有维度上。

因此，NODE 论文建议使用 entmax 函数：

```py
import numpy as np
from jax.nn import softmax
from entmax_jax import entmax15, sparsemax

weights = np.array([2, 3, 5, 7, 11, 17])
print(np.linalg.norm(entmax15(weights)))
# Output: 1.0
print(entmax15(weights))
# Output: [0\. 0\. 0\. 0\. 0\. 1.]
```

这似乎是一个不错的选择，因为它生成了预期的结果！

通过将点积与`entmax`函数结合使用，我们可以检索给定特征的值。

可微特征选择可以简单到如下：

```py
def pick_features(weights, features):
    feature = jnp.dot(entmax15(weights), features)
    return feature
```

在训练阶段，权重将被训练以学习在给定决策中保留哪个特征。

# 引入阈值

在决策节点中使用的另一个参数是阈值，这必须被学习。如果所选特征的值大于或等于该阈值，则预测沿着树的右路径进行。相反，如果值严格低于阈值，则采取左路径。

可以使用类似的方法生成一个 1，如果值高于阈值或低于阈值，通过使用点积和*entmax*函数，但使用一个大小为 2 的向量。第一个元素表示选定值与阈值之间的差异，而第二个元素是 0。

借助*entmax*函数，如果值低于阈值，向量的第一个元素是负数。当应用*entmax*函数时，0 会被转化为 1，负数部分变为 0。相反，当值大于阈值时，第一个元素是 1，第二个元素是 0。

这是一个示例实现：

```py
import numpy as np
import jax.numpy as jnp
from jax.nn import softmax
from entmax_jax import entmax15, sparsemax

def choose(feature, threshold, left, right):
    choices = jnp.array([left, right])
    steer = jnp.array([threshold - feature, 0])
    pred = jnp.dot(entmax15(steer), choices)
    return pred
print(choose(12, 10, -1, 1))
# Output: 1
print(choose(8, 10, -1, 1))
# Output: -1
```

在训练过程中，需要学习的参数不仅是阈值，还有附加在这个简单 1 级树的左右叶子上的权重。

![](https://www.buymeacoffee.com/guillaumes0)

[`www.buymeacoffee.com/guillaumes0`](https://www.buymeacoffee.com/guillaumes0)

# 下一步

在这篇文章中，我们已经看到了如何对梯度提升的高度非光滑结构进行正则化，即决策树。这是从梯度提升方法过渡到支撑神经网络的可微编程范式的最低要求。

然而，仍然有三个问题需要解决：

1.  如何扩展该方法以支持多级决策树？

1.  如何学习这种树的参数？

1.  我们如何确保保持在`entmax`函数的线性部分，并确保梯度不会消失？

我将在即将发布的文章中提供这些主题的更多细节。

更新 — 第二篇文章已上线：

[](/xgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-part-2-training-b432620750f8?source=post_page-----291dc9365656--------------------------------) ## XGBoost: 深度学习如何取代梯度提升和决策树 — 第二部分：训练

### 没有 if 的世界

[towardsdatascience.com

与此同时，如果你想提高对梯度提升的掌握，可以查看我关于该主题的书籍：

[](https://amzn.to/3IKyqTF?source=post_page-----291dc9365656--------------------------------) [## 实用的梯度提升：深入了解 Python 中的梯度提升

### 本书关于梯度提升方法，旨在为希望深入了解这一领域的学生、学者、工程师和数据科学家提供指导…

amzn.to](https://amzn.to/3IKyqTF?source=post_page-----291dc9365656--------------------------------)
