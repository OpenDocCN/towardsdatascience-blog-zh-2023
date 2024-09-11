# 什么是泊松分布

> 原文：[`towardsdatascience.com/predicting-the-unpredictable-an-introduction-to-the-poisson-distribution-5afd4d70b1d7?source=collection_archive---------10-----------------------#2023-06-06`](https://towardsdatascience.com/predicting-the-unpredictable-an-introduction-to-the-poisson-distribution-5afd4d70b1d7?source=collection_archive---------10-----------------------#2023-06-06)

## 对最著名的概率分布之一的概述

[](https://medium.com/@egorhowell?source=post_page-----5afd4d70b1d7--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----5afd4d70b1d7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5afd4d70b1d7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5afd4d70b1d7--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----5afd4d70b1d7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpredicting-the-unpredictable-an-introduction-to-the-poisson-distribution-5afd4d70b1d7&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----5afd4d70b1d7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5afd4d70b1d7--------------------------------) ·4 分钟阅读·2023 年 6 月 6 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5afd4d70b1d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpredicting-the-unpredictable-an-introduction-to-the-poisson-distribution-5afd4d70b1d7&source=-----5afd4d70b1d7---------------------bookmark_footer-----------)![](img/c681c78181e7038f18a1dd892ae89d7f.png)

照片由 [安妮·尼戈德](https://unsplash.com/de/@polarmermaid?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上

# 背景

[**泊松分布**](https://en.wikipedia.org/wiki/Poisson_distribution)是一种普遍存在的[**离散概率分布**](https://en.wikipedia.org/wiki/Probability_distribution#Discrete_probability_distribution)。它由[**西蒙·德尼·泊松**](https://en.wikipedia.org/wiki/Sim%C3%A9on_Denis_Poisson)在 19 世纪初发布，后来在保险、流行病学和电子商务等多个行业中得到了应用。因此，这是数据科学家必须了解的一个基本概念。在这篇文章中，我们将深入探讨该分布的复杂性，并提供实际的例子。

## 直觉

泊松分布的核心概念是量化事件在给定时间间隔内发生特定次数的概率。

例如，假设一家零售店平均每小时接待 20 位顾客。利用泊松分布，我们可以计算店铺在一个小时内接待特定数量顾客的概率，如 10 位、15 位或 30 位。

# 理论
