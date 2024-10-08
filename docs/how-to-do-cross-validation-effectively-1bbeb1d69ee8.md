# 如何有效地进行交叉验证

> 原文：[`towardsdatascience.com/how-to-do-cross-validation-effectively-1bbeb1d69ee8`](https://towardsdatascience.com/how-to-do-cross-validation-effectively-1bbeb1d69ee8)

## 交叉验证最佳实践指南：重新训练和嵌套

[](https://vcerq.medium.com/?source=post_page-----1bbeb1d69ee8--------------------------------)![Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----1bbeb1d69ee8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1bbeb1d69ee8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1bbeb1d69ee8--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----1bbeb1d69ee8--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1bbeb1d69ee8--------------------------------) ·6 分钟阅读·2023 年 2 月 6 日

--

![](img/8ae7c75ea0734849abe1195cffecca97.png)

[5 折蒙特卡罗交叉验证](https://medium.com/towards-data-science/monte-carlo-cross-validation-for-time-series-ed01c41e2995)。图像来源于作者。

交叉验证是构建稳健机器学习模型的关键因素。然而，它往往未能发挥其全部潜力。

在这篇文章中，我们将探讨两种重要的实践，以充分利用交叉验证：重新训练和嵌套。

让我们开始吧！

# 什么是交叉验证？

交叉验证是一种评估模型性能的技术。

这个过程通常涉及测试几种技术，或对特定方法进行超参数优化。在这种情况下，你的目标是检查哪种替代方案最适合输入数据。

关键是选择能最大化性能的方法。这就是将被部署到生产中的模型。此外，你还希望对该模型的性能有一个可靠的估计。

# 交叉验证后的重新训练

![](img/0842a1b43d9c8f5bb44fee8cb06c0ff4.png)

交叉验证后，你应该使用所有可用数据重新训练最佳模型。图像来源于作者。

假设你进行交叉验证以选择一个模型。你使用 5 折交叉验证测试许多备选方案。然后，线性回归模型表现最佳。

那么接下来应该做什么呢？

是否应该使用所有可用数据重新训练线性回归模型？还是应该使用交叉验证过程中训练的模型？

这一部分在数据科学家中会引发一些困惑——不仅在初学者中，还有更有经验的专业人士中也会有。

交叉验证后，你应该使用所有可用数据重新训练最佳方法。以下是来自传奇书籍 *《统计学习的元素 [1]*（括号内为我所加）*：*

> 我们在交叉验证后的最终选择模型是 f(x)，然后将其拟合到所有数据上。

但这一想法并不被普遍接受。

一些从业者在交叉验证期间保留了最佳模型。按照上述示例，你会保留 5 个线性回归模型。然后，在部署阶段，你会对每个预测进行预测平均。

这不是交叉验证的工作方式。

这种方法有两个问题：

+   它使用较少的数据进行训练；

+   它导致了成本增加，因为需要维护多个模型。

## 数据较少

如果不重新训练，你将不会利用所有可用的实例来创建模型。

如果没有大量数据，这可能会导致次优模型。使用所有可用实例进行训练更可能实现更好的泛化。

重新训练在时间序列中特别重要，因为最新的观察结果用于测试。如果不在这些时间点重新训练，模型可能会遗漏新出现的模式。

## 成本增加

有人认为，将交叉验证期间训练的 5 个模型结合起来可以带来更好的性能。

然而，理解其影响是很重要的。你不再使用简单、可解释的线性回归。

你的模型是一个集成体，其中的个别模型通过随机子采样进行训练。随机子采样是一种[在集成模型中引入多样性](https://medium.com/towards-data-science/how-to-measure-and-improve-the-diversity-of-forecasting-ensembles-2ec899014d6)的方法。集成模型通常比单一模型表现更好。但它们也会导致额外的成本和较低的透明度。

如果你只保留一个模型，而不是将所有模型结合起来，会怎么样？

这将解决成本增加的问题。然而，仍不清楚你应该选择哪个版本的模型。

重新训练可以跳过的两个原因。如果数据集很大或者重新训练成本太高。这两个问题通常是相关的。

## 重新训练——实际示例

下面是如何在交叉验证后重新训练最佳模型的一个示例：

```py
from sklearn.datasets import make_regression
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import GridSearchCV, KFold

# creating a dummy dataset
X, y = make_regression(n_samples=100)

# 5-fold cross-validation
cv = KFold(n_splits=5)

# optimizing the number of trees of a RF
model = RandomForestRegressor()
param_search = {'n_estimators': [10, 50, 100]}

# applying cross-validation with a grid-search
# and re-training the best model afterwards
gs = GridSearchCV(estimator=model, cv=cv, refit=True, param_grid=param_search)
gs.fit(X, y)
```

目标是优化随机森林中的树木数量。这是通过*scikit-learn*中的*GridSearchCV*类完成的。你可以设置参数*refit=True*，这样在交叉验证后，最佳模型将自动重新训练。

你可以通过从*GridSearchCV*获得最佳参数来显式地初始化新模型：

```py
best_model = RandomForestRegressor(**gs.best_params_)
best_model.fit(X, y)
```

# 获得可靠的性能估计

在开发模型时，你希望达到三个目标：

1.  从众多备选方案中选择一个模型；

1.  训练所选模型并部署它；

1.  获得所选模型的可靠性能估计。

交叉验证和重新训练涵盖了前两个要点，但不包括第三个。

为什么会这样？

交叉验证通常会重复进行多次，以便选择最终模型。你会测试不同的转换和超参数。因此，你最终会调整方法，直到对结果感到满意为止。

这可能导致过拟合，因为验证集的细节可能泄漏到模型中。因此，你从交叉验证中得到的性能估计可能过于乐观。你可以在参考文献[2]中阅读更多相关内容。

这也是为什么 Kaggle 比赛有两个排行榜，一个是公共的，另一个是私人的。这可以防止竞争者对测试集进行过拟合。

那么，你该如何解决这个问题呢？

你应该增加一个额外的评估步骤。在交叉验证后，你需要在一个保留的测试集上评估选择的模型。完整的工作流程如下：

1.  将可用数据划分为训练集和测试集；

1.  使用训练集应用交叉验证来选择模型；

1.  使用训练数据重新训练选择的模型，并在测试集上进行评估。这为你提供了一个无偏的性能估计；

1.  使用所有可用数据重新训练选择的模型并部署它。

这是这个过程的可视化描述：

![](img/19bde3c87a9d8918ee34d28dd41448a4.png)

应用交叉验证和训练数据。在交叉验证之后，再训练选择的模型并在测试集上进行评估。最后，再次训练选择的模型并部署它。图片来源：作者。

## 实际例子

这是使用*scikit-learn*的完整过程的实际例子。你可以查看评论以获取更多背景信息。

```py
import numpy as np
import pandas as pd

from sklearn.datasets import make_regression
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import r2_score
from sklearn.model_selection import (GridSearchCV,
                                     KFold,
                                     train_test_split)
# creating a dummy data set
X, y = make_regression(n_samples=100)

# train test split
X_train, X_test, y_train, y_test = \
   train_test_split(X, y, shuffle=False, test_size=0.2)

# cv procedure
cv = KFold(n_splits=5)

# defining the search space
# a simple optimization of the number of trees of a RF
model = RandomForestRegressor()
param_search = {'n_estimators': [10, 50, 100]}

# applying CV with a gridsearch on the training data
gs = GridSearchCV(estimator=model, 
                  cv=cv, 
                  param_grid=param_search)
## the proper way of doing model selection
gs.fit(X_train, y_train)

# re-training the best approach for testing
chosen_model = RandomForestRegressor(**gs.best_params_)
chosen_model.fit(X_train, y_train)

# inference on test set and evaluation
preds = chosen_model.predict(X_test)
## unbiased performance estimate on test set
estimated_performance = r2_score(y_test, preds)

# final model for deployment
final_model = RandomForestRegressor(**gs.best_params_)
final_model.fit(X, y)
```

## 嵌套交叉验证

上述内容是所谓嵌套交叉验证的简化版本。

在嵌套交叉验证中，你在外部交叉验证过程的每个折叠中执行完整的内部交叉验证过程。内部过程的目标是选择最佳模型。然后，外部过程为该模型提供无偏的性能估计。

嵌套交叉验证很快变得效率低下。它仅在小型数据集上是实际可行的。

大多数从业者都采用上述过程。如果你有一个大型数据集，你也可以用单次划分来替代交叉验证程序。这样，你会得到三个部分：训练、验证和测试。

# 关键要点

嵌套和再训练是交叉验证的两个重要方面。

交叉验证选择的模型的性能估计可能过于乐观。因此，你应该进行三重划分以获得可靠的估计。三重划分是一种嵌套交叉验证形式。

在选择模型或估计其性能后，你应使用所有可用数据重新训练模型。这样，模型在新观察数据中的表现更可能更好。

感谢阅读，下次故事见！

## 相关文章

+   应用时间序列交叉验证时需要做的 4 件事

+   时间序列的蒙特卡罗交叉验证

## 参考文献

[1] [Hastie, Trevor, 等. *统计学习的要素：数据挖掘、推断与预测*. 第 2 卷. 纽约: springer, 2009](https://hastie.su.domains/Papers/ESLII.pdf).

[2] Cawley, Gavin C., 和 Nicola LC Talbot. “模型选择中的过拟合及其对性能评估的选择偏差。” *机器学习研究期刊* 11 (2010): 2079–2107.
