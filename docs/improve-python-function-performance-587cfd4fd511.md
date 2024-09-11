# 如何提升 Python 函数的性能

> 原文：[https://towardsdatascience.com/improve-python-function-performance-587cfd4fd511?source=collection_archive---------5-----------------------#2023-03-10](https://towardsdatascience.com/improve-python-function-performance-587cfd4fd511?source=collection_archive---------5-----------------------#2023-03-10)

## 提升 Python 中常被调用函数的性能

[](https://gmyrianthous.medium.com/?source=post_page-----587cfd4fd511--------------------------------)[![Giorgos Myrianthous](../Images/ff4b116e4fb9a095ce45eb064fde5af3.png)](https://gmyrianthous.medium.com/?source=post_page-----587cfd4fd511--------------------------------)[](https://towardsdatascience.com/?source=post_page-----587cfd4fd511--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----587cfd4fd511--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----587cfd4fd511--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimprove-python-function-performance-587cfd4fd511&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----587cfd4fd511---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----587cfd4fd511--------------------------------) ·9分钟阅读·2023年3月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F587cfd4fd511&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimprove-python-function-performance-587cfd4fd511&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----587cfd4fd511---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F587cfd4fd511&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimprove-python-function-performance-587cfd4fd511&source=-----587cfd4fd511---------------------bookmark_footer-----------)![](../Images/d4ef78a78679f6b7ede3800157e16a73.png)

图片来源：[Esteban Lopez](https://unsplash.com/@exxteban?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 于 [Unsplash](https://unsplash.com/photos/6yjAC0-OwkA?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在当今数据处理量以空前速度增长的世界中，拥有高效和优化的代码比以往任何时候都更为重要。Python 作为一种流行的编程语言，提供了几种内置工具来优化代码性能。其中之一是 `lru_cache` 装饰器，它可以用于缓存函数的结果，从而提高其性能。

在本文中，我们将深入探讨在 Python 中使用`lru_cache`装饰器的好处，以及它如何帮助你为经常调用的函数编写更快、更高效的代码。

## 缓存简述

缓存是一种广泛采用的方法，用于提升计算机程序的性能。它涉及在指定的时间范围内临时存储计算成本高或经常访问的信息。通过缓存，开发人员可以高效地存储先前计算的结果，从而减少重新计算所需的时间和计算资源。这一过程可以显著提高系统的响应时间和整体性能。

缓存可以通过各种数据结构实现，例如数组、哈希表等…
