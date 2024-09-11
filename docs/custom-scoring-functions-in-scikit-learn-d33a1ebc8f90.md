# 在 Scikit-Learn 中使用自定义评分函数

> 原文：[`towardsdatascience.com/custom-scoring-functions-in-scikit-learn-d33a1ebc8f90?source=collection_archive---------8-----------------------#2023-11-16`](https://towardsdatascience.com/custom-scoring-functions-in-scikit-learn-d33a1ebc8f90?source=collection_archive---------8-----------------------#2023-11-16)

## 深入探讨 RandomizedSearchCV、GridSearchCV 和 cross_val_score 中的评分函数使用

[](https://medium.com/@raicik.zach?source=post_page-----d33a1ebc8f90--------------------------------)![扎卡里·莱西克](https://medium.com/@raicik.zach?source=post_page-----d33a1ebc8f90--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d33a1ebc8f90--------------------------------)![走向数据科学](https://towardsdatascience.com/?source=post_page-----d33a1ebc8f90--------------------------------) [扎卡里·莱西克](https://medium.com/@raicik.zach?source=post_page-----d33a1ebc8f90--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28b350f36c59&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcustom-scoring-functions-in-scikit-learn-d33a1ebc8f90&user=Zachary+Raicik&userId=28b350f36c59&source=post_page-28b350f36c59----d33a1ebc8f90---------------------post_header-----------) 发表在 [走向数据科学](https://towardsdatascience.com/?source=post_page-----d33a1ebc8f90--------------------------------) ·7 分钟阅读·2023 年 11 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd33a1ebc8f90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcustom-scoring-functions-in-scikit-learn-d33a1ebc8f90&user=Zachary+Raicik&userId=28b350f36c59&source=-----d33a1ebc8f90---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd33a1ebc8f90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcustom-scoring-functions-in-scikit-learn-d33a1ebc8f90&source=-----d33a1ebc8f90---------------------bookmark_footer-----------)![](img/8365cc9231da4ae74d2b17f8e6e04ef9.png)

照片由[engin akyurt](https://unsplash.com/@enginakyurt?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

`RandomizedSearchCV`、`GridSearchCV`和`cross_val_score`是用于优化和评估 scikit-learn 中机器学习模型的工具。这些工具每个都提供了一种系统的超参数调整和模型性能评估方法。

长期以来，我在使用这些工具时都是直接使用默认的评分函数而没有考虑过它的情况。然而，我最终了解到，在使用这些工具时，scikit-learn 默认使用模型固有的评分函数来评估性能。默认的评分指标并不总是合适的，这可能导致关于模型的误判决策。

**本文的其余部分将深入探讨如何以及何时在 scikit-learn 中利用自定义评分函数。**

# 例如：在保险索赔上进行 Tweedie 回归

在这个例子中，我们开发了一个用于预测未来保险索赔成本的回归器，这是一个由于保险数据固有的不确定性而复杂的任务。保险数据的不确定性源于几个方面。

+   一旦有人购买了保险政策，就不能保证他们会提出索赔……
