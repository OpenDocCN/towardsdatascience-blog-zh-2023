# 初学者指南：必知的 LightGBM 超参数

> 原文：[`towardsdatascience.com/beginners-guide-to-the-must-know-lightgbm-hyperparameters-a0005a812702?source=collection_archive---------6-----------------------#2023-03-07`](https://towardsdatascience.com/beginners-guide-to-the-must-know-lightgbm-hyperparameters-a0005a812702?source=collection_archive---------6-----------------------#2023-03-07)

## 最重要的 LightGBM 参数、它们的作用以及如何调整它们

[](https://medium.com/@iamleonie?source=post_page-----a0005a812702--------------------------------)![Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----a0005a812702--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a0005a812702--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0005a812702--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----a0005a812702--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeginners-guide-to-the-must-know-lightgbm-hyperparameters-a0005a812702&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----a0005a812702---------------------post_header-----------) 发布于 [数据科学入门](https://towardsdatascience.com/?source=post_page-----a0005a812702--------------------------------) ·5 min 阅读·2023 年 3 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa0005a812702&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeginners-guide-to-the-must-know-lightgbm-hyperparameters-a0005a812702&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----a0005a812702---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa0005a812702&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeginners-guide-to-the-must-know-lightgbm-hyperparameters-a0005a812702&source=-----a0005a812702---------------------bookmark_footer-----------)![](img/8e37b170e50ee11ec08887e47126e55b.png)

调整 LightGBM 超参数的旋钮（图由作者提供）

[LightGBM](https://lightgbm.readthedocs.io/en/latest/index.html) 是一个流行的梯度提升框架。通常，你将开始指定以下 **核心参数**：

+   `objective` 和 `metric` 用于你的问题设置

+   `seed` 用于重现性

+   `verbose` 用于调试

+   `num_iterations`、`learning_rate` 和 `early_stopping_round` 用于训练

但接下来你该如何操作？[LightGBM](https://lightgbm.readthedocs.io/en/latest/index.html) 具有超过 100 个可以调整的参数 [2]。此外，每个参数都有一个或多个别名，这使得初学者很难清晰地了解关键参数。

因此，本文讨论了最重要和最常用的 [LightGBM](https://lightgbm.readthedocs.io/en/latest/index.html) 超参数，这些超参数列举如下：

+   树的形状 — `num_leaves` 和 `max_depth`

+   树的生长 — `min_data_in_leaf` 和 `min_gain_to_split`

+   数据采样 — `bagging_fraction`、`bagging_freq` 和 `feature_fraction`

+   正则化 — `lambda_l1` 和 `lambda_l2`

以下内容中的默认值取自 [文档](https://lightgbm.readthedocs.io/en/latest/Parameters.html) [2]，而超参数调优的推荐范围是…
