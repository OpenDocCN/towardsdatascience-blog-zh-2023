# 混叠：你的时间序列在欺骗你

> 原文：[https://towardsdatascience.com/aliasing-your-time-series-is-lying-to-you-c073d1aa7fdd?source=collection_archive---------10-----------------------#2023-07-12](https://towardsdatascience.com/aliasing-your-time-series-is-lying-to-you-c073d1aa7fdd?source=collection_archive---------10-----------------------#2023-07-12)

## 用Python直观介绍信号混叠

[](https://harrisonfhoffman.medium.com/?source=post_page-----c073d1aa7fdd--------------------------------)[![Harrison Hoffman](../Images/5eaa3e2bd0507297eb6c4a7efcf06324.png)](https://harrisonfhoffman.medium.com/?source=post_page-----c073d1aa7fdd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c073d1aa7fdd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c073d1aa7fdd--------------------------------) [Harrison Hoffman](https://harrisonfhoffman.medium.com/?source=post_page-----c073d1aa7fdd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F38889d0801d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faliasing-your-time-series-is-lying-to-you-c073d1aa7fdd&user=Harrison+Hoffman&userId=38889d0801d0&source=post_page-38889d0801d0----c073d1aa7fdd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c073d1aa7fdd--------------------------------) ·12分钟阅读·2023年7月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc073d1aa7fdd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faliasing-your-time-series-is-lying-to-you-c073d1aa7fdd&user=Harrison+Hoffman&userId=38889d0801d0&source=-----c073d1aa7fdd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc073d1aa7fdd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faliasing-your-time-series-is-lying-to-you-c073d1aa7fdd&source=-----c073d1aa7fdd---------------------bookmark_footer-----------)![](../Images/63aeedd4da3a06e394880eb32ae045ff.png)

Aliased Fan. 作者提供的图片。

时间序列数据无处不在，充满了丰富的信息。金融市场、工业过程、传感器读数、健康监测、网络流量以及经济指标等，都是时间序列分析和信号处理必不可少的应用实例。

随着深度学习和其他时间序列预测技术的进步，关注点已经转移离了一些时间序列的基本属性。在开始任何时间序列项目之前，我们必须问自己，“我们能相信这些数据吗？”

本文将探讨离散时间序列的一种病态特性，称为混叠。任何关注时间序列的频率或季节性分析的人必须对混叠及其对结果的影响有敏锐的认识。我们将“时间序列”和“信号”这两个术语互换使用。请享受阅读！

# 一个激励性例子

为了理解什么是混叠以及它可能多么具有欺骗性，我们先从一个经典的例子开始。我们将尝试回答一个关于基本振荡信号的问题。如果你对混叠不熟悉，答案可能会让你大吃一惊。

## 问题
