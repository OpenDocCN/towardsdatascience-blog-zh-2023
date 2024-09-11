# 在创建准确的时间序列预测中损失函数选择的关键作用

> 原文：[https://towardsdatascience.com/the-critical-role-of-loss-function-selection-in-creating-accurate-time-series-forecasts-77cf69dc9d2f?source=collection_archive---------12-----------------------#2023-03-14](https://towardsdatascience.com/the-critical-role-of-loss-function-selection-in-creating-accurate-time-series-forecasts-77cf69dc9d2f?source=collection_archive---------12-----------------------#2023-03-14)

## 使用机器学习掌握时间序列预测

## 你的损失函数选择如何决定你的时间序列预测的成败

[](https://johnadeojo.medium.com/?source=post_page-----77cf69dc9d2f--------------------------------)[![John Adeojo](../Images/f6460fae462b055d36dce16fefcd142c.png)](https://johnadeojo.medium.com/?source=post_page-----77cf69dc9d2f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----77cf69dc9d2f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----77cf69dc9d2f--------------------------------) [John Adeojo](https://johnadeojo.medium.com/?source=post_page-----77cf69dc9d2f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff933e1637e40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-critical-role-of-loss-function-selection-in-creating-accurate-time-series-forecasts-77cf69dc9d2f&user=John+Adeojo&userId=f933e1637e40&source=post_page-f933e1637e40----77cf69dc9d2f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----77cf69dc9d2f--------------------------------) ·8分钟阅读·2023年3月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F77cf69dc9d2f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-critical-role-of-loss-function-selection-in-creating-accurate-time-series-forecasts-77cf69dc9d2f&user=John+Adeojo&userId=f933e1637e40&source=-----77cf69dc9d2f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F77cf69dc9d2f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-critical-role-of-loss-function-selection-in-creating-accurate-time-series-forecasts-77cf69dc9d2f&source=-----77cf69dc9d2f---------------------bookmark_footer-----------)![](../Images/50c340d8f06526401323807ca45397a4.png)

图片由[Dan Asaki](https://unsplash.com/@danasaki?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

在这篇文章中，我将展示一个在机器学习中我认为经常被忽视的东西的重要性，那就是损失函数的选择。我会通过展示我在[Driven Data](https://www.drivendata.org/competitions/44/dengai-predicting-disease-spread/page/80/)举办的登革热比赛中的方法来做到这一点。

我已经建立了一个Ridge回归器作为我的基线模型，并使用不同损失函数的多个“口味”的XGBoost回归器。

*参赛者需要预测伊基托斯和圣胡安的每周登革热病例总数。每位参赛者根据其模型对测试数据集的平均绝对误差（MAE）进行排名。要了解更多关于挑战、登革热或自己参赛的信息，你可以访问挑战的* [*主页*](https://www.drivendata.org/competitions/44/dengai-predicting-disease-spread/page/80/)*。*

## 笔记本和代码库

我已通过以下链接将我的Jupyter笔记本和GitHub代码库分享给你。笔记本可能需要几分钟…
