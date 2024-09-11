# 如何使用移动平均模型进行预测

> 原文：[https://towardsdatascience.com/how-to-forecast-with-moving-average-models-6f3c9cbba60d?source=collection_archive---------6-----------------------#2023-01-23](https://towardsdatascience.com/how-to-forecast-with-moving-average-models-6f3c9cbba60d?source=collection_archive---------6-----------------------#2023-01-23)

## 关于如何使用移动平均模型进行时间序列分析预测的教程和理论

[](https://medium.com/@egorhowell?source=post_page-----6f3c9cbba60d--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----6f3c9cbba60d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6f3c9cbba60d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6f3c9cbba60d--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----6f3c9cbba60d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-forecast-with-moving-average-models-6f3c9cbba60d&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----6f3c9cbba60d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6f3c9cbba60d--------------------------------) ·6 分钟阅读·2023年1月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6f3c9cbba60d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-forecast-with-moving-average-models-6f3c9cbba60d&user=Egor+Howell&userId=1cac491223b2&source=-----6f3c9cbba60d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6f3c9cbba60d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-forecast-with-moving-average-models-6f3c9cbba60d&source=-----6f3c9cbba60d---------------------bookmark_footer-----------)![](../Images/6fb0947aa4194f06a35db89cc3cb7f8e.png)

图片由 [Sigmund](https://unsplash.com/de/@sigmund?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

在我之前的帖子中，我们讨论了[***自回归***](https://medium.com/towards-data-science/how-to-forecast-time-series-using-autoregression-1d45db71683)。这是通过某些线性加权组合来预测未来值，而这些加权组合是基于时间序列的先前观察值。与使用先前观察值不同，我们可以使用过去的***预测误差***来进行预测。这被称为[***移动平均***](https://en.wikipedia.org/wiki/Moving-average_model) ***(MA)*** 模型。

> 这与[**滚动均值模型**](https://en.wikipedia.org/wiki/Moving_average)不同，后者也被称为移动平均模型。

在这篇文章中，我想探讨移动平均预测模型的理论和框架，然后深入了解如何在Python中实现它！

# 移动平均模型理论

## 概述

如上所述，移动平均模型通过拟合系数***θ***来实现类似*回归*的效果。
