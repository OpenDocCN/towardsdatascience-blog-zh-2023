# 决策树回归器——Scikit Learn 的视觉指南

> 原文：[https://towardsdatascience.com/decision-tree-regressor-a-visual-guide-with-scikit-learn-2aa9e01f5d7f?source=collection_archive---------3-----------------------#2023-03-27](https://towardsdatascience.com/decision-tree-regressor-a-visual-guide-with-scikit-learn-2aa9e01f5d7f?source=collection_archive---------3-----------------------#2023-03-27)

![](../Images/1c7a634936cbcb80db53a4f5a26d81d1.png)

图片由 [niko photos](https://unsplash.com/@niko_photos?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 无数学知识的决策树理解

[](https://medium.com/@angela.shi?source=post_page-----2aa9e01f5d7f--------------------------------)[![Angela and Kezhan Shi](../Images/a89d678f2f3887c0c2ff3928f9d767b4.png)](https://medium.com/@angela.shi?source=post_page-----2aa9e01f5d7f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2aa9e01f5d7f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2aa9e01f5d7f--------------------------------) [Angela and Kezhan Shi](https://medium.com/@angela.shi?source=post_page-----2aa9e01f5d7f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2bf03e38122e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecision-tree-regressor-a-visual-guide-with-scikit-learn-2aa9e01f5d7f&user=Angela+and+Kezhan+Shi&userId=2bf03e38122e&source=post_page-2bf03e38122e----2aa9e01f5d7f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2aa9e01f5d7f--------------------------------) ·5分钟阅读·2023年3月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2aa9e01f5d7f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecision-tree-regressor-a-visual-guide-with-scikit-learn-2aa9e01f5d7f&user=Angela+and+Kezhan+Shi&userId=2bf03e38122e&source=-----2aa9e01f5d7f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2aa9e01f5d7f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecision-tree-regressor-a-visual-guide-with-scikit-learn-2aa9e01f5d7f&source=-----2aa9e01f5d7f---------------------bookmark_footer-----------)

在这篇文章中，我们将使用 python 中的 scikit-learn 实现 DecisionTreeRegressor，以可视化展示该模型的工作原理。我们不会使用任何数学术语，而是通过可视化来演示决策树回归器的工作方式以及一些超参数的影响。

在这种背景下，决策树回归器尝试通过将特征变量切割成小区域来预测一个连续的目标变量，每个区域会有一个预测。我们将从一个连续变量开始，然后是两个连续变量。我们不会使用分类变量，因为对于决策树来说，连续变量在进行分割时最终会被当作分类数据处理。

我们将主要研究两个超参数的影响，这两个超参数非常直观易懂：`max depth` 和 `min_samples_leaf`。其他超参数类似，主要思想是限制规则的大小。

# 一个非线性变量

让我们使用一些简单的数据，仅有特征变量 x。

```py
# Import the required libraries
from sklearn.tree import DecisionTreeRegressor
import numpy as np
# Define the dataset
X = np.array([[1], [3], [4], [7], [9]…
```
