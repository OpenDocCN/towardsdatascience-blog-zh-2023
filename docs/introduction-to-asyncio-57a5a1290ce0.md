# asyncio 介绍

> 原文：[`towardsdatascience.com/introduction-to-asyncio-57a5a1290ce0?source=collection_archive---------8-----------------------#2023-03-24`](https://towardsdatascience.com/introduction-to-asyncio-57a5a1290ce0?source=collection_archive---------8-----------------------#2023-03-24)

## 管理 I/O 密集型并发的 Python

[](https://medium.com/@hrmnmichaels?source=post_page-----57a5a1290ce0--------------------------------)![Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----57a5a1290ce0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----57a5a1290ce0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----57a5a1290ce0--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----57a5a1290ce0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff2daf6260cca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-asyncio-57a5a1290ce0&user=Oliver+S&userId=f2daf6260cca&source=post_page-f2daf6260cca----57a5a1290ce0---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----57a5a1290ce0--------------------------------) ·6 分钟阅读·2023 年 3 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F57a5a1290ce0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-asyncio-57a5a1290ce0&user=Oliver+S&userId=f2daf6260cca&source=-----57a5a1290ce0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F57a5a1290ce0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-asyncio-57a5a1290ce0&source=-----57a5a1290ce0---------------------bookmark_footer-----------)

并发和并行性表示程序或计算机同时运行多个操作的能力。通常区分多进程（并行性）和多线程（并发性）——前者指的是运行多个进程，而后者则指在同一进程内生成多个线程。在 Python 中，由于 [全局解释器锁](https://realpython.com/python-gil/)（GIL），一次只能执行一个线程，这使得任何多线程应用实际上都是单核的。然而，例如对于 I/O 密集型程序（程序在等待输入时花费了更多时间——即这是瓶颈而非计算），即使在 Python 中，多线程也是有意义的。在这篇文章中，我们将介绍 [asyncio](https://docs.python.org/3/library/asyncio.html)，它是一种优雅且简单的实现方式。

![](img/56706dacea79f62fb82537889418d6a7.png)

照片由 [Gabriel Gusmao](https://unsplash.com/@gcsgpp?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，来源于 [Unsplash](https://unsplash.com/photos/pMmw3ynuXHw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 为什么使用 asyncio？

我们将为多进程与多线程之间的差异以及 asyncio 在其中的作用指定一个未来的帖子。尽管如此，我们仍然希望在这里激励其使用。如上所示，多线程特别适用于 I/O 密集型，即非 CPU 密集型 应用程序。而 asyncio 是实现这一目标的一种方式，许多开发者在某些用例中偏好这种方式。

asyncio 对于 I/O 密集型程序表现出色，我们希望编写简单、结构清晰的代码，以及……
