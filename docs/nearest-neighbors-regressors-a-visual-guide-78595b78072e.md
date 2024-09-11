# 最近邻回归器 — 可视化指南

> 原文：[https://towardsdatascience.com/nearest-neighbors-regressors-a-visual-guide-78595b78072e?source=collection_archive---------10-----------------------#2023-03-31](https://towardsdatascience.com/nearest-neighbors-regressors-a-visual-guide-78595b78072e?source=collection_archive---------10-----------------------#2023-03-31)

![](../Images/bc24d842e277ced70e9a10b211985b90.png)

## 模型及其超参数影响的可视化理解

[](https://medium.com/@angela.shi?source=post_page-----78595b78072e--------------------------------)[![Angela 和 Kezhan Shi](../Images/a89d678f2f3887c0c2ff3928f9d767b4.png)](https://medium.com/@angela.shi?source=post_page-----78595b78072e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----78595b78072e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----78595b78072e--------------------------------) [Angela 和 Kezhan Shi](https://medium.com/@angela.shi?source=post_page-----78595b78072e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2bf03e38122e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnearest-neighbors-regressors-a-visual-guide-78595b78072e&user=Angela+and+Kezhan+Shi&userId=2bf03e38122e&source=post_page-2bf03e38122e----78595b78072e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----78595b78072e--------------------------------) ·8 min 阅读·2023年3月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F78595b78072e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnearest-neighbors-regressors-a-visual-guide-78595b78072e&user=Angela+and+Kezhan+Shi&userId=2bf03e38122e&source=-----78595b78072e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F78595b78072e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnearest-neighbors-regressors-a-visual-guide-78595b78072e&source=-----78595b78072e---------------------bookmark_footer-----------)

K 最近邻（KNN）是机器学习中最简单的模型之一。实际上，在某种程度上，它没有模型，因为在预测新观察时，它会使用整个训练数据集来根据距离（通常是欧几里得距离）找到“最近邻居”。然后在回归任务的情况下，预测值是通过对这些邻居的目标变量值进行平均来计算的。

由于我们使用距离的概念，因此只能使用数值特征。你可以通过一热编码（one-hot encoding）或标签编码（label encoding）将分类特征转换为数值特征，距离计算的算法仍然有效，但距离的意义就会变得有些无关紧要。

还值得注意的是，目标变量的值不会用于查找邻居。

在本文中，我们将使用一些简单的数据集来可视化KNN回归器（KNN Regressor）是如何工作的，以及超参数k如何影响预测。我们还将讨论特征缩放的影响。同时，我们还将探索一个鲜为人知的最近邻版本，即半径最近邻（radius nearest neighbors）。最后，我们将讨论更为定制的距离版本。
