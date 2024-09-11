# 在 Python 中结合多进程和异步 IO 以提升性能

> 原文：[https://towardsdatascience.com/combining-multiprocessing-and-asyncio-in-python-for-performance-boosts-15496ffe96b?source=collection_archive---------1-----------------------#2023-05-04](https://towardsdatascience.com/combining-multiprocessing-and-asyncio-in-python-for-performance-boosts-15496ffe96b?source=collection_archive---------1-----------------------#2023-05-04)

## [PYTHON 并发](https://medium.com/@qtalen/list/python-concurrency-2c979347da3b)

## 使用实际案例演示 Map-Reduce 程序

[](https://qtalen.medium.com/?source=post_page-----15496ffe96b--------------------------------)[![Peng Qian](../Images/9ce9aeb381ec6b017c1ee5d4714937e2.png)](https://qtalen.medium.com/?source=post_page-----15496ffe96b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----15496ffe96b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----15496ffe96b--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----15496ffe96b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombining-multiprocessing-and-asyncio-in-python-for-performance-boosts-15496ffe96b&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----15496ffe96b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----15496ffe96b--------------------------------) · 7 分钟阅读 · 2023 年 5 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F15496ffe96b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombining-multiprocessing-and-asyncio-in-python-for-performance-boosts-15496ffe96b&user=Peng+Qian&userId=8e2fe735546d&source=-----15496ffe96b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F15496ffe96b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombining-multiprocessing-and-asyncio-in-python-for-performance-boosts-15496ffe96b&source=-----15496ffe96b---------------------bookmark_footer-----------)![](../Images/77f3f292511afaa667c8f02679022e8b.png)

照片由 [Mitchell Luo](https://unsplash.com/@mitchel3uo?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

由于 GIL 的存在，使用多个线程来执行 CPU 绑定任务一直不是一个选项。随着多核 CPU 的普及，Python 提供了一种多进程解决方案来执行 CPU 绑定任务。但直到现在，直接使用多进程相关的 API 仍然存在一些问题。

在我们开始之前，我们还有一小段代码来辅助演示：

该方法接受一个参数，从0开始累加到这个参数。打印方法执行时间并返回结果。

## 多进程问题
