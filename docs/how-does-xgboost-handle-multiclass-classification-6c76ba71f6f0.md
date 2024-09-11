# XGBoost 如何处理多类别分类？

> 原文：[https://towardsdatascience.com/how-does-xgboost-handle-multiclass-classification-6c76ba71f6f0?source=collection_archive---------0-----------------------#2023-01-07](https://towardsdatascience.com/how-does-xgboost-handle-multiclass-classification-6c76ba71f6f0?source=collection_archive---------0-----------------------#2023-01-07)

## 理解这种模型在分类中的基本工作原理至关重要，因为它会影响性能。

[](https://medium.com/@guillaume.saupin?source=post_page-----6c76ba71f6f0--------------------------------)[![Saupin Guillaume](../Images/d9112d3cdfe6f335b6ff2c875fba6bb5.png)](https://medium.com/@guillaume.saupin?source=post_page-----6c76ba71f6f0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6c76ba71f6f0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6c76ba71f6f0--------------------------------) [Saupin Guillaume](https://medium.com/@guillaume.saupin?source=post_page-----6c76ba71f6f0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F891e27328f3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-does-xgboost-handle-multiclass-classification-6c76ba71f6f0&user=Saupin+Guillaume&userId=891e27328f3a&source=post_page-891e27328f3a----6c76ba71f6f0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6c76ba71f6f0--------------------------------) · 7 分钟阅读 · 2023年1月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6c76ba71f6f0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-does-xgboost-handle-multiclass-classification-6c76ba71f6f0&user=Saupin+Guillaume&userId=891e27328f3a&source=-----6c76ba71f6f0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6c76ba71f6f0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-does-xgboost-handle-multiclass-classification-6c76ba71f6f0&source=-----6c76ba71f6f0---------------------bookmark_footer-----------)![](../Images/23c7c84600c2af0aa36124702225ee8e.png)

图片由 [Andrew Coop](https://unsplash.com/@andrewcoop?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，我们将探讨使用像 XGBoost、LightGBM 和 CatBoost 这样的梯度提升库训练的决策树集成如何进行多类别分类。

确实，决策树集成将一个实际值与一组特征相关联，因此问题是：决策树集成如何将标量值转化为多类别标签？

理解使用这种模型进行分类的基本原理至关重要，因为它会影响性能。

我们将根据下面的计划逐步深入主题：

+   二分类的提醒和Python中的示例

+   使用XGBoost作为回归器的第一次二分类

+   使用XGBoost作为分类器的第二次二分类

+   使用XGBoost进行多分类

# 基于决策树的集成模型的多样性

XGBoost、LightGBM或CatBoost是共享（默认情况下）相同的库……
