# 初学者指南：必知的 LightGBM 超参数

> 原文：[`towardsdatascience.com/beginners-guide-to-the-must-know-lightgbm-hyperparameters-a0005a812702`](https://towardsdatascience.com/beginners-guide-to-the-must-know-lightgbm-hyperparameters-a0005a812702)

## 最重要的 LightGBM 参数，它们的作用以及如何调整它们

[](https://medium.com/@iamleonie?source=post_page-----a0005a812702--------------------------------)![Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----a0005a812702--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a0005a812702--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0005a812702--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----a0005a812702--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0005a812702--------------------------------) ·5 分钟阅读·2023 年 3 月 7 日

--

![](img/8e37b170e50ee11ec08887e47126e55b.png)

调整 LightGBM 超参数的旋钮（图片来源：作者）

[LightGBM](https://lightgbm.readthedocs.io/en/latest/index.html) 是一个流行的梯度提升框架。通常，你将开始指定以下 **核心参数**：

+   `objective` 和 `metric` 用于问题设置

+   `seed` 用于可复现性

+   `verbose` 用于调试

+   `num_iterations`、`learning_rate` 和 `early_stopping_round` 用于训练

但接下来该怎么做呢？[LightGBM](https://lightgbm.readthedocs.io/en/latest/index.html) 具有超过 100 个可以调整的参数 [2]。此外，每个参数还有一个或多个别名，这使得初学者很难清晰地了解重要参数。

因此，本文讨论了最重要和最常用的 [LightGBM](https://lightgbm.readthedocs.io/en/latest/index.html) 超参数，列举如下：

+   树的形状 — `num_leaves` 和 `max_depth`

+   树的生长 — `min_data_in_leaf` 和 `min_gain_to_split`

+   数据采样 — `bagging_fraction`、`bagging_freq` 和 `feature_fraction`

+   正则化 — `lambda_l1` 和 `lambda_l2`

在以下内容中，默认值取自 [文档](https://lightgbm.readthedocs.io/en/latest/Parameters.html) [2]，并且超参数调优的推荐范围参考了 文章 [5] 以及书籍 [1] 和 [4]。

# 树的形状

与 [XGBoost](https://xgboost.readthedocs.io/en/stable/) 相比，[LightGBM](https://lightgbm.readthedocs.io/en/latest/index.html) 以 **叶子为单位** 生长决策树，而不是按层生长。你可以使用 `num_leaves` 和 `max_depth` 来控制单棵树的大小。

![](img/859942cf85723cfbbb3730ab85e3fa34.png)

使用 num_leaves 和 max_depth 指定 LightGBM 树形状（作者提供的图片）

参数 `num_leaves` 控制树中叶子的最大数量 [2]。

+   默认值：31

+   基线的良好起点：16

+   调整范围：（8，256），`num_leaves < 2^(max_depth)` [3]

参数 `max_depth` 控制树模型的最大深度 [2]。

+   默认值：-1（无限制）

+   基线的良好起点：默认

+   调整范围：（3，16）

树越小（`num_leaves` 和 `max_depth` 较小），训练速度越快——但这也可能降低准确性 [3]。

由于 `num_leaves` 对 LGBM 中的树生长影响大于 `max_depth` [5]，Morohashi [4] 不一定推荐调整该参数并偏离默认值。

# 树的生长

除了深度和叶子数量外，你还可以指定叶子分裂的条件。因此，你可以指定树的生长方式。

![](img/6149f3798899054888d50b0c0a662d31.png)

使用 `min_data_in_leaf` 和 `min_gain_to_split` 指定 LightGBM 树的生长（作者提供的图片）

参数 `min_data_in_leaf` 指定一个叶子中最少的数据点数 [2]。如果该参数过小，模型将过拟合训练数据 [2]。

+   默认值：20

+   基线的良好起点：默认

+   调整范围：（5，300），但取决于数据集的大小。对于大型数据集，数百个已经足够 [3]。经验法则：数据集越大，`min_data_in_leaf` 越大。

参数 `min_gain_to_split` 指定叶子进行分裂所需的最小增益 [2]。

+   默认值：0

+   基线的良好起点：默认

+   调整范围：（0，15）

如果通过增加参数 `min_gain_to_split` 来限制树的生长，得到的较小树将导致训练时间更快——但这也可能降低准确性 [3]。

# 数据采样

数据采样是一种强制模型泛化的技术。总体思路是每次迭代时不给模型提供所有数据。相反，模型每次迭代只会看到训练数据的一部分。

## Bagging

每隔 `bagging_freq` 次迭代，LGBM 将随机选择 `bagging_fraction * 100 %` 的数据用于下一个 `bagging_freq` 次迭代 [2]。例如，如果 `bagging_fraction = 0.8` 和 `bagging_freq = 2`，LGBM 将在每个第二次迭代前抽样 80% 的训练数据。

该技术可用于加快训练速度 [2]。

+   默认值：`bagging_fraction = 1.0` 和 `bagging_freq = 0`（禁用）

+   基线的良好起点：`bagging_fraction = 0.9` 和 `bagging_freq = 1`

+   调整范围：`bagging_fraction`（0.5，1）

![](img/b3f8556ef84542bdcce02a40e7f52377.png)

使用 bagging_fraction = 0.8 进行 LightGBM 的 bagging（作者提供的图片）

## 子特征采样

在每次迭代中，LGBM 将随机选择 `feature_fraction * 100 %` 的数据[2]。例如，如果`feature_fraction = 0.8`，LGBM 将在训练每棵树之前抽样 80 % 的特征。

+   默认：1

+   基线的良好起点：0.9

+   调优范围：（0.5, 1）

![](img/47329ee15ee33c25a91e026f9961c814.png)

LightGBM 中的子特征抽样，feature_fraction = 0.8（图源：作者）

虽然子特征抽样也可以像袋装方法[2]一样加速训练，但如果特征中存在多重共线性，它也能有所帮助[1]。

# 正则化

你可以将正则化技术应用于你的机器学习模型，以处理过拟合。正如参数名称所示，参数`lambda_l1`用于 L1 正则化，`lambda_l2`用于 L2 正则化。

+   **L1 正则化**惩罚权重的绝对值，因此对异常值具有鲁棒性

+   **L2 正则化**惩罚权重的平方和，因此对异常值敏感

你可以选择仅使用这两种正则化中的一种，或者如果你愿意，也可以将它们结合使用。

对于这两个参数，参数值的表现类似：

+   默认：0（禁用）

+   基线的良好起点：默认

+   调优范围：（0.01, 100）

# 总结

本文为你快速概述了最重要的[LightGBM](https://lightgbm.readthedocs.io/en/latest/index.html)超参数调优。下面你可以找到它们及其推荐调优范围的概览。

![](img/6de94bd423c395935434537385e9e285.png)

最重要的[LightGBM](https://lightgbm.readthedocs.io/en/latest/index.html)超参数及其调优范围概览（图源：作者）。

当然，[LightGBM](https://lightgbm.readthedocs.io/en/latest/index.html)还有许多其他可以使用的超参数。

例如，参数`min_sum_hessian_in_leaf`指定一个叶子中的最小 Hessian 和，并且还可以帮助缓解过拟合[2]。当你的数据集不平衡时，还有一个可以调优的参数`scale_pos_weight`。或者你可以使用`max_bin`指定特征将被分桶的最大数量。

# 享受这个故事了吗？

[*免费订阅*](https://medium.com/subscribe/@iamleonie) *以便在我发布新故事时收到通知。*

[](https://medium.com/@iamleonie/subscribe?source=post_page-----a0005a812702--------------------------------) [## 每当 Leonie Monigatti 发布时获取电子邮件。

### 每当 Leonie Monigatti 发布时获取电子邮件。通过注册，如果你还没有 Medium 账户，你将创建一个...

medium.com](https://medium.com/@iamleonie/subscribe?source=post_page-----a0005a812702--------------------------------)

*在* [*LinkedIn*](https://www.linkedin.com/in/804250ab/)、[*Twitter*](https://twitter.com/helloiamleonie)*和* [*Kaggle*](https://www.kaggle.com/iamleonie)*上找我！*

# 参考文献

[1] K. Banachewicz, L. Massaron (2022). 《Kaggle 书》。Packt

[2] LightGBM (2023). [参数](https://lightgbm.readthedocs.io/en/latest/Parameters.html)（访问于 2023 年 3 月 3 日）

[3] LightGBM (2023). [参数调优](https://lightgbm.readthedocs.io/en/latest/Parameters-Tuning.html)（访问于 2023 年 3 月 3 日）

[4] M. Morohashi (2022). Kaggle 中磨练机器学习实战能力。

[5] [Bex T.](https://medium.com/u/39db050c2ac2?source=post_page-----a0005a812702--------------------------------) (2021). 2021 年 Kaggler’s Guide to LightGBM 超参数调优与 Optuna（访问于 2023 年 3 月 3 日）
