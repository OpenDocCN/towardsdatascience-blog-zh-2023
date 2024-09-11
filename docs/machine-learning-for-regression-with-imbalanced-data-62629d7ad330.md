# 《处理不平衡数据的回归机器学习》

> 原文：[`towardsdatascience.com/machine-learning-for-regression-with-imbalanced-data-62629d7ad330?source=collection_archive---------5-----------------------#2023-08-10`](https://towardsdatascience.com/machine-learning-for-regression-with-imbalanced-data-62629d7ad330?source=collection_archive---------5-----------------------#2023-08-10)

## 为什么很难预测数据集中的异常值，以及你可以做些什么来应对这些问题。

[](https://medium.com/@caroline.arnold_63207?source=post_page-----62629d7ad330--------------------------------)![Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----62629d7ad330--------------------------------)[](https://towardsdatascience.com/?source=post_page-----62629d7ad330--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----62629d7ad330--------------------------------) [Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----62629d7ad330--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9367198e7a3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-for-regression-with-imbalanced-data-62629d7ad330&user=Caroline+Arnold&userId=9367198e7a3c&source=post_page-9367198e7a3c----62629d7ad330---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----62629d7ad330--------------------------------) ·6 min read·2023 年 8 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F62629d7ad330&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-for-regression-with-imbalanced-data-62629d7ad330&user=Caroline+Arnold&userId=9367198e7a3c&source=-----62629d7ad330---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F62629d7ad330&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-for-regression-with-imbalanced-data-62629d7ad330&source=-----62629d7ad330---------------------bookmark_footer-----------)

## 什么是不平衡数据？

许多现实世界的数据集都存在不平衡问题，即某些类型的样本在数据集中过度代表，而其他类型的样本出现频率较低。以下是一些例子：

+   在对信用卡交易进行欺诈或合法分类时，大多数交易将属于合法类别。

+   严重降雨发生的频率低于中等降雨，但可能对人类和基础设施造成更大损害。

+   在尝试识别土地用途时，表示森林和农业的像素比城市定居点的像素更多。

在这篇文章中，我们旨在对为何机器学习算法在处理不平衡数据时会遇到困难提供一个**直观的解释**，展示如何通过**分位数评估**来量化算法的性能，并介绍**三种不同的策略**来提升算法的性能。

![](img/9d2254efcdffa0b4fe4ec4c5e2a0b963.png)

图片由[Elena Mozhvilo](https://unsplash.com/@miracleday?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上

## 回归示例数据集：加州住房

数据集不平衡通常在分类问题中被说明，其中一个主要类别掩盖了…
