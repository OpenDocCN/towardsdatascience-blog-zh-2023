# 图片由 [regularguy.eth](https://unsplash.com/@moneyphotos?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 能够分析你的时间序列模型对于诊断其性能至关重要。一种方法是通过拟合模型的残差来实现。在这篇文章中，我们将介绍什么是残差以及如何利用它们来改进你的模型，并提供一个 Python 示例。

## 了解如何使用预测模型的残差来提升其性能

原文：[https://towardsdatascience.com/how-to-analyse-your-time-series-model-using-residuals-f980f597332e?source=collection_archive---------6-----------------------#2023-01-03](https://towardsdatascience.com/how-to-analyse-your-time-series-model-using-residuals-f980f597332e?source=collection_archive---------6-----------------------#2023-01-03)

[](https://medium.com/@egorhowell?source=post_page-----f980f597332e--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----f980f597332e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f980f597332e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f980f597332e--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----f980f597332e--------------------------------)

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-analyse-your-time-series-model-using-residuals-f980f597332e&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----f980f597332e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f980f597332e--------------------------------) · 5 分钟阅读 · 2023年1月3日

如何使用残差分析你的时间序列模型

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff980f597332e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-analyse-your-time-series-model-using-residuals-f980f597332e&user=Egor+Howell&userId=1cac491223b2&source=-----f980f597332e---------------------clap_footer-----------)

什么是残差？

# --

背景

# [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff980f597332e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-analyse-your-time-series-model-using-residuals-f980f597332e&source=-----f980f597332e---------------------bookmark_footer-----------)![](../Images/1b43c40b3df3d4128f518ab3b0f3669f.png)

在时间序列分析中，残差，***r***，是*拟合值*，***ŷ***，和*实际值*，***y***，之间的差异。

明确[残差](https://en.wikipedia.org/wiki/Residual_(numerical_analysis))和[误差](https://en.wikipedia.org/wiki/Forecast_error)之间的区别很重要。误差是*实际值*和*预测值*之间的差异。然而，残差，如上所示，是*实际值*与*拟合值*之间的差异。这些*拟合值*是模型在拟合训练数据时所做的预测。由于模型知道所有观察值的数值，它在技术上不再是…
