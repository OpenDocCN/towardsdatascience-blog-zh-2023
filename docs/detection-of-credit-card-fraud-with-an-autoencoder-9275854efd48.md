# 使用自编码器检测信用卡欺诈

> 原文：[`towardsdatascience.com/detection-of-credit-card-fraud-with-an-autoencoder-9275854efd48?source=collection_archive---------7-----------------------#2023-06-01`](https://towardsdatascience.com/detection-of-credit-card-fraud-with-an-autoencoder-9275854efd48?source=collection_archive---------7-----------------------#2023-06-01)

## 异常检测器实施指南

[](https://tinztwinspro.medium.com/?source=post_page-----9275854efd48--------------------------------)![Janik and Patrick Tinz](https://tinztwinspro.medium.com/?source=post_page-----9275854efd48--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9275854efd48--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9275854efd48--------------------------------) [Janik and Patrick Tinz](https://tinztwinspro.medium.com/?source=post_page-----9275854efd48--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4eb5d9652d9e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdetection-of-credit-card-fraud-with-an-autoencoder-9275854efd48&user=Janik+and+Patrick+Tinz&userId=4eb5d9652d9e&source=post_page-4eb5d9652d9e----9275854efd48---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9275854efd48--------------------------------) ·10 分钟阅读·2023 年 6 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9275854efd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdetection-of-credit-card-fraud-with-an-autoencoder-9275854efd48&user=Janik+and+Patrick+Tinz&userId=4eb5d9652d9e&source=-----9275854efd48---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9275854efd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdetection-of-credit-card-fraud-with-an-autoencoder-9275854efd48&source=-----9275854efd48---------------------bookmark_footer-----------)![](img/ead0f960668e0c078768fd7ab039672b.png)

[Christiann Koepke](https://unsplash.com/@christiannkoepke?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上的照片

你想知道如何使用 Python 和 TensorFlow 创建一个**异常检测器**吗？那么这篇文章就是为你准备的。信用卡公司使用异常检测器来检测欺诈交易。识别欺诈交易非常重要，以便客户不必为他们没有购买的东西付款。

每天都有很多信用卡交易，但极少数交易是欺诈性的。欺诈交易就是异常。本文展示了一个用于检测这些欺诈交易的自编码器模型的实现。首先，我们定义了异常并介绍不同类型的异常。然后，我们描述了用于信用卡欺诈检测的异常检测器的实现。让我们开始吧！

# 异常检测概述

异常检测算法识别获取数据集中新颖和意外的结构。文献中对异常有许多定义。我们为我们的使用案例推导了一个定义。

## 异常定义

Chandola 等人[1]将异常描述为数据中的模式，这些模式不符合明确定义的正常行为的概念。另一个广泛使用的定义…
