# 如何使用自回归法进行时间序列预测

> 原文：[https://towardsdatascience.com/how-to-forecast-time-series-using-autoregression-1d45db71683?source=collection_archive---------3-----------------------#2023-01-17](https://towardsdatascience.com/how-to-forecast-time-series-using-autoregression-1d45db71683?source=collection_archive---------3-----------------------#2023-01-17)

## 如何使用 Python 中的自回归模型进行预测的教程

[](https://medium.com/@egorhowell?source=post_page-----1d45db71683--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----1d45db71683--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1d45db71683--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1d45db71683--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----1d45db71683--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-forecast-time-series-using-autoregression-1d45db71683&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----1d45db71683---------------------post_header-----------) 本文发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1d45db71683--------------------------------) · 7 分钟阅读 · 2023年1月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1d45db71683&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-forecast-time-series-using-autoregression-1d45db71683&user=Egor+Howell&userId=1cac491223b2&source=-----1d45db71683---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1d45db71683&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-forecast-time-series-using-autoregression-1d45db71683&source=-----1d45db71683---------------------bookmark_footer-----------)![](../Images/081074115ceaa168aa8c4a20d048276e.png)

图片来源：[Aron Visuals](https://unsplash.com/@aronvisuals?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

预测是一个广泛的领域，有许多模型可以用来模拟你的时间序列。在我之前的文章中，我们介绍了一些[基本预测模型](https://medium.com/towards-data-science/basic-forecasting-techniques-ef4295248e46)并探讨了流行的[指数平滑模型](https://medium.com/towards-data-science/forecasting-with-simple-exponential-smoothing-dd8f8470a14c)。

在这篇文章中，我们将开始探索另一类预测模型，从[自回归](https://en.wikipedia.org/wiki/Autoregressive_model)开始。我们将讨论预测此模型所需的理论和背景，然后深入进行Python教程。

# 什么是自回归？

## 概述

自回归是指通过使用时间序列的前几个值（滞后）的某种线性加权组合来预测该时间序列。由于我们是在对目标值进行自我回归，因此称之为*自*回归。

从数学上讲，我们可以将自回归写成：
