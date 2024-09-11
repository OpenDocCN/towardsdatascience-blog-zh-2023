# 如何解释逻辑回归系数

> 原文：[`towardsdatascience.com/how-to-interpret-logistic-regression-coefficients-db9381379ab3?source=collection_archive---------8-----------------------#2023-08-24`](https://towardsdatascience.com/how-to-interpret-logistic-regression-coefficients-db9381379ab3?source=collection_archive---------8-----------------------#2023-08-24)

## 计算逻辑回归系数的均值边际效应

[](https://medium.com/@jarom.hulet?source=post_page-----db9381379ab3--------------------------------)![Jarom Hulet](https://medium.com/@jarom.hulet?source=post_page-----db9381379ab3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----db9381379ab3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----db9381379ab3--------------------------------) [Jarom Hulet](https://medium.com/@jarom.hulet?source=post_page-----db9381379ab3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F88982a88b4e5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-interpret-logistic-regression-coefficients-db9381379ab3&user=Jarom+Hulet&userId=88982a88b4e5&source=post_page-88982a88b4e5----db9381379ab3---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----db9381379ab3--------------------------------) · 10 分钟阅读·2023 年 8 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdb9381379ab3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-interpret-logistic-regression-coefficients-db9381379ab3&user=Jarom+Hulet&userId=88982a88b4e5&source=-----db9381379ab3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdb9381379ab3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-interpret-logistic-regression-coefficients-db9381379ab3&source=-----db9381379ab3---------------------bookmark_footer-----------)![](img/6b8de6ded7e16028cbbbdf6fbf4cd355.png)

图片来源于 Pexels.com 上的 Dominika Roseclay

你是否喜欢逻辑回归，但讨厌解释任何形式的对数变换？好吧，我不能说你是个好伙伴，但我可以说你确实有*我*做伴！

在这篇文章中，我将讨论如何解释逻辑回归系数——以下是大纲：

1.  解释*线性*回归系数

1.  为什么逻辑回归系数的解释具有挑战性

1.  如何解释逻辑回归系数

1.  使用*statsmodels*包计算均值边际效应

1.  结论

**解释线性回归系数**

大多数具有基本统计学知识的人都能完全理解线性回归中系数的解释。如果你是其中之一，你可以考虑跳过文章中讨论逻辑回归系数的部分。

解释线性回归系数非常简单易懂。解释的简易性是线性回归仍然是一个非常受欢迎工具的原因之一，尽管…
