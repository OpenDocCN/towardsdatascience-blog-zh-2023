# 什么是 ARIMA？

> 原文：[`towardsdatascience.com/how-to-forecast-with-arima-96b3d4db111a?source=collection_archive---------15-----------------------#2023-01-31`](https://towardsdatascience.com/how-to-forecast-with-arima-96b3d4db111a?source=collection_archive---------15-----------------------#2023-01-31)

## ARIMA 预测模型简介及其在时间序列中的应用

[](https://medium.com/@egorhowell?source=post_page-----96b3d4db111a--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----96b3d4db111a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----96b3d4db111a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----96b3d4db111a--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----96b3d4db111a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-forecast-with-arima-96b3d4db111a&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----96b3d4db111a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----96b3d4db111a--------------------------------) ·7 分钟阅读·2023 年 1 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F96b3d4db111a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-forecast-with-arima-96b3d4db111a&user=Egor+Howell&userId=1cac491223b2&source=-----96b3d4db111a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F96b3d4db111a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-forecast-with-arima-96b3d4db111a&source=-----96b3d4db111a---------------------bookmark_footer-----------)![](img/ed7671dae965b26c6dcdd9c5e5d2973a.png)

图片由[Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

在我之前的帖子中，我们已经介绍了[***自回归（AR）***](https://medium.com/towards-data-science/how-to-forecast-time-series-using-autoregression-1d45db71683)和[***移动平均（MA）***](https://medium.com/towards-data-science/how-to-forecast-with-moving-average-models-6f3c9cbba60d)模型。然而，你知道这两种模型中还有什么更好的选择吗？一个结合了这两者的单一模型！

[***自回归积分移动平均***](https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average)，更为人知的名称是 ARIMA，可能是最常用的时间序列预测模型，它是前述各模型的组合。在这篇文章中，我将深入探讨 ARIMA 模型背后的理论和框架。然后，我们将通过一个简单的 Python 演练，使用 `statsmodels` [包](https://www.statsmodels.org/dev/generated/statsmodels.tsa.arima.model.ARIMA.html)进行 ARIMA 预测！

# 什么是 ARIMA？

## 概述

如上所述，ARIMA 代表**自**回归 **整合** 移动平均，是这三种（实际上是两种）组件的组合：

**自回归（AR）：**
