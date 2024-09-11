# 在 PowerBI 中利用深度学习进行高级时间序列异常检测

> 原文：[`towardsdatascience.com/advanced-time-series-anomaly-detection-with-deep-learning-in-powerbi-4a69c0273357?source=collection_archive---------3-----------------------#2023-04-06`](https://towardsdatascience.com/advanced-time-series-anomaly-detection-with-deep-learning-in-powerbi-4a69c0273357?source=collection_archive---------3-----------------------#2023-04-06)

## 复杂且前沿的方法，如何从计算机视觉中创造性地借鉴并仅需几次点击即可实现。

[](https://thomasdorfer.medium.com/?source=post_page-----4a69c0273357--------------------------------)![托马斯·A·多费尔](https://thomasdorfer.medium.com/?source=post_page-----4a69c0273357--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4a69c0273357--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4a69c0273357--------------------------------) [托马斯·A·多费尔](https://thomasdorfer.medium.com/?source=post_page-----4a69c0273357--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7c54f9b62b90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-time-series-anomaly-detection-with-deep-learning-in-powerbi-4a69c0273357&user=Thomas+A+Dorfer&userId=7c54f9b62b90&source=post_page-7c54f9b62b90----4a69c0273357---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4a69c0273357--------------------------------) ·6 分钟阅读·2023 年 4 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4a69c0273357&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-time-series-anomaly-detection-with-deep-learning-in-powerbi-4a69c0273357&user=Thomas+A+Dorfer&userId=7c54f9b62b90&source=-----4a69c0273357---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4a69c0273357&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-time-series-anomaly-detection-with-deep-learning-in-powerbi-4a69c0273357&source=-----4a69c0273357---------------------bookmark_footer-----------)![](img/d0d781202fa190fba231248030d12944.png)

作者提供的图片。

## 介绍

随着全球范围内应用和服务数量的不断增加，时间序列异常检测已成为捕捉指标回归的无处不在且不可或缺的工具。

然而，设置异常检测系统确实不是简单的事情，通常需要相当多的领域专业知识。在 PowerBI 中，设置只需几次点击，即可在短时间内实现最先进的异常检测系统。

本文将描述 PowerBI 异常检测功能背后的创新算法，并提供逐步的方法来实现和配置它。

## 算法：SR-CNN

在 PowerBI 的异常检测功能背后，隐藏着一个机制，该机制将谱残差（SR）算法与卷积神经网络（CNN）相结合——因此得名 SR-CNN。我知道这里有很多专业术语。那么让我们深入了解一下。
