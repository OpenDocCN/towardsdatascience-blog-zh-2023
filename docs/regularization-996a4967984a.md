# 正则化为什么实际上有效？

> 原文：[https://towardsdatascience.com/regularization-996a4967984a?source=collection_archive---------10-----------------------#2023-01-02](https://towardsdatascience.com/regularization-996a4967984a?source=collection_archive---------10-----------------------#2023-01-02)

## 解构机器学习中正则化的数学概念

[](https://guenterroehrich.medium.com/?source=post_page-----996a4967984a--------------------------------)[![Günter Röhrich](../Images/31a1d0dc835c7ad31197f8c387023d10.png)](https://guenterroehrich.medium.com/?source=post_page-----996a4967984a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----996a4967984a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----996a4967984a--------------------------------) [Günter Röhrich](https://guenterroehrich.medium.com/?source=post_page-----996a4967984a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3718a9423801&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregularization-996a4967984a&user=G%C3%BCnter+R%C3%B6hrich&userId=3718a9423801&source=post_page-3718a9423801----996a4967984a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----996a4967984a--------------------------------) ·9分钟阅读·2023年1月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F996a4967984a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregularization-996a4967984a&user=G%C3%BCnter+R%C3%B6hrich&userId=3718a9423801&source=-----996a4967984a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F996a4967984a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregularization-996a4967984a&source=-----996a4967984a---------------------bookmark_footer-----------)

虽然有很多很好的资源展示了正则化在机器学习中的实现，但本指南是对正则化为何对我们的模型有益的简要介绍。本文将讨论在线性回归背景下正则化的概念，并涵盖以下主题：

1.  [OLS 的基本思想](#a40e)

1.  [Tikhonov 正则化（L2 正则化）](#cea1)

1.  [L1 正则化](#ac08)

1.  [指定 Lambda](#3a78)

1.  [深度学习中的正则化](#32fe)

1.  [结论](#877d)

## OLS 的基本思想

首先，让我们快速回顾一下基本的普通最小二乘（OLS）方程，这在线性回归中起着至关重要的作用。在线性回归中，我们的目标是找到一个预测值（y-hat），该值非常接近真实（观察到的）值。为了实现这一点，我们通常尝试找到一组系数，使我们能够通过数据点绘制一条直线。当这条直线**最小化与数据点的距离**时，它被认为是最优的。以下是使用R语言中的著名汽车数据集的一个示例：

![](../Images/71919088f3dd9ad31d586bdad3cb08cd.png)

一个简单的线性回归演示 — 作者使用“cars”数据集制作的图像
