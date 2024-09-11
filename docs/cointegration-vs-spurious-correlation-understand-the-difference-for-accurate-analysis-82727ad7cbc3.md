# 协整与相关性

> 原文：[`towardsdatascience.com/cointegration-vs-spurious-correlation-understand-the-difference-for-accurate-analysis-82727ad7cbc3?source=collection_archive---------7-----------------------#2023-07-17`](https://towardsdatascience.com/cointegration-vs-spurious-correlation-understand-the-difference-for-accurate-analysis-82727ad7cbc3?source=collection_archive---------7-----------------------#2023-07-17)

## *为什么相关性不等于因果关系（在时间序列中）*

[](https://medium.com/@egorhowell?source=post_page-----82727ad7cbc3--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----82727ad7cbc3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----82727ad7cbc3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----82727ad7cbc3--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----82727ad7cbc3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcointegration-vs-spurious-correlation-understand-the-difference-for-accurate-analysis-82727ad7cbc3&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----82727ad7cbc3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----82727ad7cbc3--------------------------------) ·6 分钟阅读·2023 年 7 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F82727ad7cbc3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcointegration-vs-spurious-correlation-understand-the-difference-for-accurate-analysis-82727ad7cbc3&user=Egor+Howell&userId=1cac491223b2&source=-----82727ad7cbc3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F82727ad7cbc3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcointegration-vs-spurious-correlation-understand-the-difference-for-accurate-analysis-82727ad7cbc3&source=-----82727ad7cbc3---------------------bookmark_footer-----------)![](img/9788afa1302a50836e76fc8fd6db80b2.png)

图片来源：[Wance Paleri](https://unsplash.com/fr/@wance0003000?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

在时间序列分析中，理解一个序列是否影响另一个序列是很有价值的。例如，商品交易商知道*商品 A*的增加是否会导致*商品 B*的增加是很有用的。最初，这种关系是通过线性回归来测量的，但在 1980 年代，[**Clive Granger**](https://en.wikipedia.org/wiki/Clive_Granger)和[**Paul Newbold**](https://en.wikipedia.org/wiki/Paul_Newbold)显示这种方法会得出不正确的结果，特别是对于[**非平稳**](https://medium.com/towards-data-science/time-series-stationarity-simply-explained-125269968154)时间序列。因此，他们提出了[**协整**](https://en.wikipedia.org/wiki/Cointegration)的概念，这也为 Granger 赢得了诺贝尔奖。在这篇文章中，我想讨论协整的需求和应用，以及为什么这是数据科学家应该理解的一个重要概念。

# 假相关

## 概述

在我们讨论协整之前，先讨论一下它的必要性。历史上，统计学家和经济学家使用[**线性回归**](https://en.wikipedia.org/wiki/Linear_regression)来确定不同时间序列之间的关系。然而，Granger 和 Newbold 显示这种方法是不正确的，会导致所谓的[**假相关**](https://statisticsbyjim.com/basics/spurious-correlation/#:~:text=With%20this%20definition%20in%20mind,there%20are%20more%20shark%20attacks.)。

假相关是指两个时间序列看似相关，但实际上它们缺乏因果关系。这是经典的‘*相关不意味着*…
