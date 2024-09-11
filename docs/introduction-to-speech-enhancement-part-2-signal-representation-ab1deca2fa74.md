# 语音增强简介：第二部分 — 信号表示

> 原文：[`towardsdatascience.com/introduction-to-speech-enhancement-part-2-signal-representation-ab1deca2fa74?source=collection_archive---------22-----------------------#2023-01-31`](https://towardsdatascience.com/introduction-to-speech-enhancement-part-2-signal-representation-ab1deca2fa74?source=collection_archive---------22-----------------------#2023-01-31)

## 让我们深入探讨信号表示、傅里叶变换、频谱和谐波。

[](https://medium.com/@mattiadigangi?source=post_page-----ab1deca2fa74--------------------------------)![Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----ab1deca2fa74--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ab1deca2fa74--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab1deca2fa74--------------------------------) [Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----ab1deca2fa74--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a5b9f193a3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-speech-enhancement-part-2-signal-representation-ab1deca2fa74&user=Mattia+Di+Gangi&userId=8a5b9f193a3c&source=post_page-8a5b9f193a3c----ab1deca2fa74---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab1deca2fa74--------------------------------) ·10 分钟阅读·2023 年 1 月 31 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fab1deca2fa74&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-speech-enhancement-part-2-signal-representation-ab1deca2fa74&source=-----ab1deca2fa74---------------------bookmark_footer-----------)![](img/089f61fe7f5fa79d6f139a1233f7e860.png)

图片由 [Richard Horvath](https://unsplash.com/@orwhat?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文是一个系列的一部分：

+   [语音增强简介：第一部分 — 概念和任务定义](https://medium.com/towards-data-science/introduction-to-speech-enhancement-part-1-df6098b47b91)

+   **语音增强简介：第二部分 — 信号表示**

## 引言

在深入探讨语音增强之前，我们需要明确一些关于数字信号处理的概念。在本系列的上一章中，我们介绍了一些概念，并附上了进一步阅读的链接。

在第二部分中，我们将探讨数字信号如何表示、如何改变表示，以及傅里叶变换为何如此重要。代码和图表将帮助你更好地理解并与示例进行更多的互动。

## 信号基础

本文的主要目标是理解什么是信号，它如何被表示，以及傅里叶变换的重要性，傅里叶变换是一种经过时间验证的方式，用于根据我们的需求改变表示方法。

音频信号通常被表示和记录为某种变化的…
