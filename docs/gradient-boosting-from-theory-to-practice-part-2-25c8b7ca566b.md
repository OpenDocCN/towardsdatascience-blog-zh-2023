# 从理论到实践的梯度提升（第二部分）

> 原文：[https://towardsdatascience.com/gradient-boosting-from-theory-to-practice-part-2-25c8b7ca566b?source=collection_archive---------5-----------------------#2023-07-19](https://towardsdatascience.com/gradient-boosting-from-theory-to-practice-part-2-25c8b7ca566b?source=collection_archive---------5-----------------------#2023-07-19)

## 使用 Scikit-Learn 中的梯度提升类解决不同的分类和回归问题

[](https://medium.com/@roiyeho?source=post_page-----25c8b7ca566b--------------------------------)[![Dr. Roi Yehoshua](../Images/905a512ffc8879069403a87dbcbeb4db.png)](https://medium.com/@roiyeho?source=post_page-----25c8b7ca566b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----25c8b7ca566b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----25c8b7ca566b--------------------------------) [Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----25c8b7ca566b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3886620c5cf9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-boosting-from-theory-to-practice-part-2-25c8b7ca566b&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=post_page-3886620c5cf9----25c8b7ca566b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----25c8b7ca566b--------------------------------) · 12 分钟阅读 · 2023年7月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F25c8b7ca566b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-boosting-from-theory-to-practice-part-2-25c8b7ca566b&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=-----25c8b7ca566b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F25c8b7ca566b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-boosting-from-theory-to-practice-part-2-25c8b7ca566b&source=-----25c8b7ca566b---------------------bookmark_footer-----------)![](../Images/d549d077e8ed878675b861b84fcdffb5.png)

由 [Luca Bravo](https://unsplash.com/@lucabravo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，来源于 [Unsplash](https://unsplash.com/wallpapers/nature/forest?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在 [这篇文章的第一部分](https://medium.com/towards-data-science/gradient-boosting-from-theory-to-practice-part-1-940b2c9d8050) 中，我们介绍了梯度提升算法，并展示了其伪代码实现。

在本文的这一部分，我们将深入探讨 Scikit-Learn 中实现这一算法的类，讨论它们的各种参数，并演示如何使用它们解决若干分类和回归问题。

尽管 [XGBoost](/xgboost-the-definitive-guide-part-1-cc24d2dcd87a) 库提供了更优化且高度可扩展的梯度提升实现，但对于小到中等规模的数据集，使用 Scikit-Learn 中的梯度提升类通常更为便捷，它们的接口更简单且超参数更少。

# Scikit-Learn 中的梯度提升

Scikit-Learn 提供了以下实现梯度提升决策树（GBDT）模型的类：

1.  [GradientBoostingClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingClassifier.html) 用于分类问题。

1.  [GradientBoostingRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingRegressor.html) 用于回归问题。
