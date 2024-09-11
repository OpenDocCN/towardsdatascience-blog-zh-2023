# 树集成：袋装法、提升法和梯度提升法

> 原文：[https://towardsdatascience.com/tree-ensembles-theory-and-practice-1cf9eb27781?source=collection_archive---------6-----------------------#2023-01-26](https://towardsdatascience.com/tree-ensembles-theory-and-practice-1cf9eb27781?source=collection_archive---------6-----------------------#2023-01-26)

## 理论与实践详解

[](https://jorgemartinlasaosa.medium.com/?source=post_page-----1cf9eb27781--------------------------------)[![Jorge Martín Lasaosa](../Images/21b4e500b7d14204ea76f579c3e2433f.png)](https://jorgemartinlasaosa.medium.com/?source=post_page-----1cf9eb27781--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1cf9eb27781--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1cf9eb27781--------------------------------) [Jorge Martín Lasaosa](https://jorgemartinlasaosa.medium.com/?source=post_page-----1cf9eb27781--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdd62b41dbbf5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftree-ensembles-theory-and-practice-1cf9eb27781&user=Jorge+Mart%C3%ADn+Lasaosa&userId=dd62b41dbbf5&source=post_page-dd62b41dbbf5----1cf9eb27781---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1cf9eb27781--------------------------------) · 10分钟阅读 · 2023年1月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1cf9eb27781&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftree-ensembles-theory-and-practice-1cf9eb27781&user=Jorge+Mart%C3%ADn+Lasaosa&userId=dd62b41dbbf5&source=-----1cf9eb27781---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1cf9eb27781&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftree-ensembles-theory-and-practice-1cf9eb27781&source=-----1cf9eb27781---------------------bookmark_footer-----------)![](../Images/ab39fad56deee0716f620ca35605c141.png)

图片由[Arnaud Mesureur](https://unsplash.com/@tbzr?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**树集成** 是一种监督学习的机器学习技术，由一组独立训练的决策树组成，这些决策树被定义为*弱*学习者或*基础*学习者，单独表现可能不佳。将这些*弱*学习者聚合起来产生一个新的*强*模型，这通常比之前的模型更为准确。集成学习方法主要有三种类型：**自助法（bagging）**、**提升法（boosting）**和**梯度提升法（gradient boosting）**。每种方法都可以与其他*弱*学习者结合使用，但在本文中，只考虑树模型。

文章的其余部分分为两个部分：

+   **直觉与历史。** 这部分解释了每种集成学习方法的起源，并简要描述了它们。

+   **实际演示。** 逐步展开每种集成学习方法。为此，还提供了一个小型合成数据集，以帮助解释。

> 除非另有说明，所有图片均由作者提供。

# **直觉与历史**

## 自助法（Bagging）

该术语由 Breiman（1996）首次定义[1]，是 **B**ootstrap **Agg**regation 的缩写。对于这种集成，每棵决策树使用的数据…
