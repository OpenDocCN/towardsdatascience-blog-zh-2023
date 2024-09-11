# 梯度下降与梯度提升：逐一比较

> 原文：[`towardsdatascience.com/gradient-descent-vs-gradient-boosting-a-side-by-side-comparison-7067bb3c5712?source=collection_archive---------7-----------------------#2023-02-28`](https://towardsdatascience.com/gradient-descent-vs-gradient-boosting-a-side-by-side-comparison-7067bb3c5712?source=collection_archive---------7-----------------------#2023-02-28)

## 从初始化到收敛的简单英文解释

[](https://medium.com/@angela.shi?source=post_page-----7067bb3c5712--------------------------------)![Angela 和 Kezhan Shi](https://medium.com/@angela.shi?source=post_page-----7067bb3c5712--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7067bb3c5712--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7067bb3c5712--------------------------------) [Angela 和 Kezhan Shi](https://medium.com/@angela.shi?source=post_page-----7067bb3c5712--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2bf03e38122e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-descent-vs-gradient-boosting-a-side-by-side-comparison-7067bb3c5712&user=Angela+and+Kezhan+Shi&userId=2bf03e38122e&source=post_page-2bf03e38122e----7067bb3c5712---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7067bb3c5712--------------------------------) ·5 分钟阅读·2023 年 2 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7067bb3c5712&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-descent-vs-gradient-boosting-a-side-by-side-comparison-7067bb3c5712&user=Angela+and+Kezhan+Shi&userId=2bf03e38122e&source=-----7067bb3c5712---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7067bb3c5712&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-descent-vs-gradient-boosting-a-side-by-side-comparison-7067bb3c5712&source=-----7067bb3c5712---------------------bookmark_footer-----------)

# 介绍

梯度下降和梯度提升是两种流行的机器学习算法。尽管它们在方法和应用上有所不同，但这两种算法都基于梯度计算，并且具有若干相同的步骤。本文的主要目的是提供这两种算法的详细比较，帮助读者更好地理解它们的相似性和差异。

![](img/8c776904768c4cbd3516306acf0a29c3.png)

图片由[格雷戈瓦·让诺](https://unsplash.com/es/@gregjeanneau?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 梯度下降

梯度下降是一种常见的优化算法，用于机器学习中以最小化成本函数。目标是找到一组最佳参数，以最小化预测值与实际值之间的误差。该过程开始时随机初始化模型的权重或系数。然后，通过计算成本函数相对于每个参数的梯度，迭代地更新权重，沿着成本函数的最大下降方向前进。

## 梯度提升

梯度提升是一种集成方法，通过将多个弱模型结合起来创建一个更强的预测模型。它通过迭代的方式工作…… 
