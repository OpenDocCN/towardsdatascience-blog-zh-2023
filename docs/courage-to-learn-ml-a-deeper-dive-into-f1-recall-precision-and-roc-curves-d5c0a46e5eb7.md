# 勇敢学习机器学习：深入探讨 F1 分数、召回率、精确度和 ROC 曲线

> 原文：[`towardsdatascience.com/courage-to-learn-ml-a-deeper-dive-into-f1-recall-precision-and-roc-curves-d5c0a46e5eb7?source=collection_archive---------2-----------------------#2023-12-17`](https://towardsdatascience.com/courage-to-learn-ml-a-deeper-dive-into-f1-recall-precision-and-roc-curves-d5c0a46e5eb7?source=collection_archive---------2-----------------------#2023-12-17)

## F1 分数：处理不平衡数据的关键指标——但你真的知道为什么吗？

[](https://amyma101.medium.com/?source=post_page-----d5c0a46e5eb7--------------------------------)![Amy Ma](https://amyma101.medium.com/?source=post_page-----d5c0a46e5eb7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d5c0a46e5eb7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d5c0a46e5eb7--------------------------------) [Amy Ma](https://amyma101.medium.com/?source=post_page-----d5c0a46e5eb7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd6d8df787b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcourage-to-learn-ml-a-deeper-dive-into-f1-recall-precision-and-roc-curves-d5c0a46e5eb7&user=Amy+Ma&userId=d6d8df787b&source=post_page-d6d8df787b----d5c0a46e5eb7---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d5c0a46e5eb7--------------------------------) ·12 分钟阅读·2023 年 12 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd5c0a46e5eb7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcourage-to-learn-ml-a-deeper-dive-into-f1-recall-precision-and-roc-curves-d5c0a46e5eb7&user=Amy+Ma&userId=d6d8df787b&source=-----d5c0a46e5eb7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd5c0a46e5eb7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcourage-to-learn-ml-a-deeper-dive-into-f1-recall-precision-and-roc-curves-d5c0a46e5eb7&source=-----d5c0a46e5eb7---------------------bookmark_footer-----------)![](img/f5a423ae041e3906b5aed54f7892aeb6.png)

我们将使用整理洗衣服的类比来说明召回率和精确度的核心概念；照片由[Ace Maxwell](https://unsplash.com/@acedibwai?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

欢迎回到我们的‘[勇敢学习机器学习](http://towardsdatascience.com/tagged/courage-to-learn-ml)’系列。在本节中，我们将探索指标的细微世界。许多资源介绍这些指标或深入其数学方面，但这些‘简单’数学背后的逻辑有时仍然不够明确。对于那些对这个话题不熟悉的人，我推荐查看 Shervin 详细的 文章，以及 [neptune.ai](https://neptune.ai/blog/performance-metrics-in-machine-learning-complete-guide) 的全面指南。

在典型的数据科学面试准备中，处理不平衡数据时常用的指标是 F1 分数，它被称为召回率和精确率的调和均值。然而，为什么 F1 分数特别适用于这种情况的理由通常没有解释清楚。这篇文章致力于揭示这些原因，帮助你理解在不同场景下选择特定指标的依据。

和往常一样，这篇文章将概述我们要解决的所有问题。如果你也在思考这些问题，那么你来对地方了：

+   精确率和召回率到底是什么？我们如何直观地理解它们？

+   为什么精确率和召回率……
