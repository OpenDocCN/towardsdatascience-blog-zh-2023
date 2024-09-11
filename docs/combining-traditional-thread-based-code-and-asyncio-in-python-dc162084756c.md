# 结合传统线程模型代码与 Python 中的 Asyncio

> 原文：[https://towardsdatascience.com/combining-traditional-thread-based-code-and-asyncio-in-python-dc162084756c?source=collection_archive---------12-----------------------#2023-05-15](https://towardsdatascience.com/combining-traditional-thread-based-code-and-asyncio-in-python-dc162084756c?source=collection_archive---------12-----------------------#2023-05-15)

## [PYTHON 并发](https://medium.com/@qtalen/list/python-concurrency-2c979347da3b)

## 一份关于在 Python 中集成同步和异步编程的全面指南

[](https://qtalen.medium.com/?source=post_page-----dc162084756c--------------------------------)[![Peng Qian](../Images/9ce9aeb381ec6b017c1ee5d4714937e2.png)](https://qtalen.medium.com/?source=post_page-----dc162084756c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dc162084756c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dc162084756c--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----dc162084756c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombining-traditional-thread-based-code-and-asyncio-in-python-dc162084756c&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----dc162084756c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dc162084756c--------------------------------) ·6 分钟阅读·2023年5月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdc162084756c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombining-traditional-thread-based-code-and-asyncio-in-python-dc162084756c&user=Peng+Qian&userId=8e2fe735546d&source=-----dc162084756c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdc162084756c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombining-traditional-thread-based-code-and-asyncio-in-python-dc162084756c&source=-----dc162084756c---------------------bookmark_footer-----------)![](../Images/2d6b4ed69c2a46871c9ec4c44fa56d21.png)

图片来源：作者创作，[Canva](https://www.canva.com/)

# 引言

在本文中，我将解释如何在不实现 asyncio 的 asyncio 程序中调用现有的 IO 阻塞代码，以及如何在基于线程模型的现有程序中调用 asyncio 代码。

在之前的文章中，我向你介绍了 asyncio，这是一个 Python 特性。asyncio 的性能非常高，在现代高并发代码中使用 asyncio 将大幅提高 IO 性能。

但在现实世界中，我们并没有看到 asyncio 代码的使用如预期那样广泛。这是为什么呢？

## 挑战 1：如何在 asyncio 代码中调用旧的 IO 阻塞代码

一种情况是，在我们使用 asyncio 实现新代码时，系统中仍然存在大量传统实现的 IO 阻塞程序。例如，微服务、文件操作等。即使你使用 asyncio 直接调用这些阻塞 API，你仍然无法实现高并发效果。

## 挑战 2：如何调用…
