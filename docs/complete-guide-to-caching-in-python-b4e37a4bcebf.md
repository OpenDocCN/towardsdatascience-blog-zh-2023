# Python缓存完整指南

> 原文：[https://towardsdatascience.com/complete-guide-to-caching-in-python-b4e37a4bcebf?source=collection_archive---------10-----------------------#2023-12-01](https://towardsdatascience.com/complete-guide-to-caching-in-python-b4e37a4bcebf?source=collection_archive---------10-----------------------#2023-12-01)

## 缓存是如何工作的，以及缓存函数的方法

[](https://kayjanwong.medium.com/?source=post_page-----b4e37a4bcebf--------------------------------)[![Kay Jan Wong](../Images/28e803eca6327d97b6aa97ee4095d7bd.png)](https://kayjanwong.medium.com/?source=post_page-----b4e37a4bcebf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b4e37a4bcebf--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b4e37a4bcebf--------------------------------) [凯·简·黄](https://kayjanwong.medium.com/?source=post_page-----b4e37a4bcebf--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffee8693930fb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomplete-guide-to-caching-in-python-b4e37a4bcebf&user=Kay+Jan+Wong&userId=fee8693930fb&source=post_page-fee8693930fb----b4e37a4bcebf---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b4e37a4bcebf--------------------------------) ·7分钟阅读·2023年12月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb4e37a4bcebf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomplete-guide-to-caching-in-python-b4e37a4bcebf&user=Kay+Jan+Wong&userId=fee8693930fb&source=-----b4e37a4bcebf---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb4e37a4bcebf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomplete-guide-to-caching-in-python-b4e37a4bcebf&source=-----b4e37a4bcebf---------------------bookmark_footer-----------)![](../Images/b0f443c08318e8fb89a059bc3e15b59c.png)

图片来源：[娜娜·斯米尔诺娃](https://unsplash.com/@nananadolgo?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

当使用相同的参数重复调用函数时，会导致计算重复。**记忆化** 在这种情况下非常有用，因为它可以将函数调用的结果“保存”以备将来使用。这不仅节省了时间，还优化了代码，因为代码变得不那么计算密集。**缓存** 是一个更通用的术语，用于指代任何数据的存储。

这篇文章将讨论不同的缓存策略、缓存考虑因素，以及如何启用和实现不同类型的缓存（使用 Python 包和你的实现）！

# 目录

+   [缓存类型](https://medium.com/p/b4e37a4bcebf/#64d2)

+   [缓存考虑因素](https://medium.com/p/b4e37a4bcebf/#4de0)

+   [LRU 缓存](https://medium.com/p/b4e37a4bcebf/#b4c2)

+   [LFU 缓存](https://medium.com/p/b4e37a4bcebf/#b490)

+   [FIFO / LIFO 缓存](https://medium.com/p/b4e37a4bcebf/#420a)

# 缓存类型

根据你的需求，有几种缓存策略，例如：

+   **最近最少使用（LRU）**：移除最近最少使用的数据，这是一种最常见的缓存类型。

+   **最少使用（LFU）**：移除最少使用的数据

+   **先进先出（FIFO）**：移除最旧的数据……
