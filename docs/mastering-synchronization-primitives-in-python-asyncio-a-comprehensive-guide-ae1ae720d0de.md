# 精通 Python Asyncio 中的同步原语：一份全面指南

> 原文：[https://towardsdatascience.com/mastering-synchronization-primitives-in-python-asyncio-a-comprehensive-guide-ae1ae720d0de?source=collection_archive---------11-----------------------#2023-05-29](https://towardsdatascience.com/mastering-synchronization-primitives-in-python-asyncio-a-comprehensive-guide-ae1ae720d0de?source=collection_archive---------11-----------------------#2023-05-29)

## [PYTHON CONCURRENCY](https://medium.com/@qtalen/list/python-concurrency-2c979347da3b)

## asyncio.Lock、asyncio.Semaphore、asyncio.Event 和 asyncio.Condition 的最佳实践

[](https://qtalen.medium.com/?source=post_page-----ae1ae720d0de--------------------------------)[![Peng Qian](../Images/9ce9aeb381ec6b017c1ee5d4714937e2.png)](https://qtalen.medium.com/?source=post_page-----ae1ae720d0de--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ae1ae720d0de--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ae1ae720d0de--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----ae1ae720d0de--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-synchronization-primitives-in-python-asyncio-a-comprehensive-guide-ae1ae720d0de&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----ae1ae720d0de---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ae1ae720d0de--------------------------------) ·8分钟阅读·2023年5月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fae1ae720d0de&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-synchronization-primitives-in-python-asyncio-a-comprehensive-guide-ae1ae720d0de&user=Peng+Qian&userId=8e2fe735546d&source=-----ae1ae720d0de---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fae1ae720d0de&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-synchronization-primitives-in-python-asyncio-a-comprehensive-guide-ae1ae720d0de&source=-----ae1ae720d0de---------------------bookmark_footer-----------)![](../Images/251106288dbd9d21a2c3fc7e21ad3d1d.png)

图片来源：由作者创建，[Canva](https://www.canva.com/)

这是我在 Python 并发系列中的一篇文章，如果你觉得有用，可以从 [这里](https://medium.com/@qtalen/list/python-concurrency-2c979347da3b) 阅读其他文章。

# 介绍

在本文中，我将介绍为什么你需要Python的asyncio中的同步原语以及几种同步原语的最佳实践。在文章的最后部分，我将为你演示同步原语的一个例子。

## 为什么在asyncio中需要同步原语

任何使用过Python多线程的人都知道多个线程共享相同的内存块。因此，当多个线程同时对相同区域执行非原子操作时，就会出现线程安全问题。

由于asyncio在单个线程上运行，它是否不会有类似的线程安全问题呢？答案是否定的。

asyncio中的并发任务是异步执行的，这意味着在同一时间可能会有多个任务交替执行。并发bug 是…
