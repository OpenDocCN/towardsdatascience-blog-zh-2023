# XGBoost 现在支持将 MAE 作为目标

> 原文：[https://towardsdatascience.com/xgboost-now-support-mae-as-objective-fdbbe18e173?source=collection_archive---------10-----------------------#2023-01-18](https://towardsdatascience.com/xgboost-now-support-mae-as-objective-fdbbe18e173?source=collection_archive---------10-----------------------#2023-01-18)

## 既然 MAE 是非光滑的，这怎么可能呢？

[](https://medium.com/@guillaume.saupin?source=post_page-----fdbbe18e173--------------------------------)[![Saupin Guillaume](../Images/d9112d3cdfe6f335b6ff2c875fba6bb5.png)](https://medium.com/@guillaume.saupin?source=post_page-----fdbbe18e173--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fdbbe18e173--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fdbbe18e173--------------------------------) [Saupin Guillaume](https://medium.com/@guillaume.saupin?source=post_page-----fdbbe18e173--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F891e27328f3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-now-support-mae-as-objective-fdbbe18e173&user=Saupin+Guillaume&userId=891e27328f3a&source=post_page-891e27328f3a----fdbbe18e173---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fdbbe18e173--------------------------------) ·5 分钟阅读·2023年1月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffdbbe18e173&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-now-support-mae-as-objective-fdbbe18e173&user=Saupin+Guillaume&userId=891e27328f3a&source=-----fdbbe18e173---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffdbbe18e173&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-now-support-mae-as-objective-fdbbe18e173&source=-----fdbbe18e173---------------------bookmark_footer-----------)![](../Images/929fde4fb46becfb224284a1e3507976.png)

照片由 [Ajay Karpur](https://unsplash.com/@ajaykarpur?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在基于梯度提升的模型中，选择一个关键参数是目标。实际上，决策树的整个构建过程源自目标及其一阶和二阶导数。

XGBoost 最近引入了对一种新目标的支持：无二阶导数的非光滑目标。在这些目标中，著名的**MAE**（平均绝对误差）现在可以在 XGBoost 中原生激活。

在这篇文章中，我们将详细说明 XGBoost 如何被修改以处理这种类型的目标。

# 梯度提升需要平滑的目标

XGBoost、LightGBM 和 CatBoost 都有一个共同的限制：它们需要平滑的（从数学角度讲）目标来计算决策树叶节点的最优权重。

对于 XGBoost 来说，这种情况不再成立，因为它在 1.7.0 版本开始引入了对 MAE 的支持，并采用了线性搜索方法。

如果你愿意详细掌握梯度提升，看看我的书：
