# 如何在不重采样的情况下应对类别不平衡

> 原文：[`towardsdatascience.com/how-to-tackle-class-imbalance-without-resampling-47bbeb2180aa?source=collection_archive---------5-----------------------#2023-03-01`](https://towardsdatascience.com/how-to-tackle-class-imbalance-without-resampling-47bbeb2180aa?source=collection_archive---------5-----------------------#2023-03-01)

## 超越重采样、阈值调整或成本敏感模型的不平衡分类

[](https://vcerq.medium.com/?source=post_page-----47bbeb2180aa--------------------------------)![Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----47bbeb2180aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----47bbeb2180aa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----47bbeb2180aa--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----47bbeb2180aa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-tackle-class-imbalance-without-resampling-47bbeb2180aa&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----47bbeb2180aa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----47bbeb2180aa--------------------------------) ·7 分钟阅读·2023 年 3 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F47bbeb2180aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-tackle-class-imbalance-without-resampling-47bbeb2180aa&user=Vitor+Cerqueira&userId=efb5f27c836d&source=-----47bbeb2180aa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F47bbeb2180aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-tackle-class-imbalance-without-resampling-47bbeb2180aa&source=-----47bbeb2180aa---------------------bookmark_footer-----------)![](img/f9ebb8f645bb4e67587a6f3ae607c680.png)

[照片由 Denise Johnson](https://unsplash.com/@auntneecey?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

不平衡分类是一个相关的机器学习任务。这个问题通常通过三种方法之一来处理：重采样、成本敏感模型或阈值调整。

在这篇文章中，你将学习一种不同的方法。我们将深入探讨如何利用聚类分析来解决不平衡分类问题。

# 引言

许多现实世界中的问题涉及不平衡的数据集。在这些数据集中，某些类别较为稀少，通常对用户来说更为重要。

以欺诈检测为例。欺诈案件在大量正常活动中是稀有的实例。准确检测这些稀有但欺诈的活动在许多领域中都是至关重要的。其他涉及不平衡数据集的常见例子包括客户流失预测或信用违约预测。

不平衡的分布对机器学习算法构成了挑战。关于少数类的信息相对较少。这阻碍了算法训练良好模型的能力，因为算法往往倾向于偏向多数类。

# 如何处理类别不平衡
