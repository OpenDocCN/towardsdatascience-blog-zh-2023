# Python 中并发和并行的综合指南

> 原文：[`towardsdatascience.com/comprehensive-guide-to-concurrency-and-parallelism-in-python-415ee5fbec1a?source=collection_archive---------6-----------------------#2023-04-14`](https://towardsdatascience.com/comprehensive-guide-to-concurrency-and-parallelism-in-python-415ee5fbec1a?source=collection_archive---------6-----------------------#2023-04-14)

## 使用 multiprocessing、threading 和 asyncio

[](https://medium.com/@hrmnmichaels?source=post_page-----415ee5fbec1a--------------------------------)![Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----415ee5fbec1a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----415ee5fbec1a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----415ee5fbec1a--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----415ee5fbec1a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff2daf6260cca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomprehensive-guide-to-concurrency-and-parallelism-in-python-415ee5fbec1a&user=Oliver+S&userId=f2daf6260cca&source=post_page-f2daf6260cca----415ee5fbec1a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----415ee5fbec1a--------------------------------) · 8 分钟阅读 · 2023 年 4 月 14 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F415ee5fbec1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomprehensive-guide-to-concurrency-and-parallelism-in-python-415ee5fbec1a&user=Oliver+S&userId=f2daf6260cca&source=-----415ee5fbec1a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F415ee5fbec1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomprehensive-guide-to-concurrency-and-parallelism-in-python-415ee5fbec1a&source=-----415ee5fbec1a---------------------bookmark_footer-----------)

在这篇文章中，我们将详细介绍 Python 中的并发和并行。我们将介绍这些术语，然后展示如何在 Python 中使用 `multiprocessing`、`threading` 和 `asyncio` 应用它们。我们将学习何时使用多进程，何时使用多线程，并为每种情况提供实际示例。

![](img/d46b0e529b898fbbd9fbb143c1f18a48.png)

照片由 [Tomas Sobek](https://unsplash.com/@tomas_nz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/plwud_FPvwU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 概述

首先从计算机科学的角度介绍标题中的两个术语：**并发**指的是“同时”访问相同的资源，例如 CPU 核心或磁盘，而**并行**则描述了多个任务访问不同的资源，例如不同的 CPU 核心。

在 Python 中，**并行**是通过 `multiprocessing` 包实现的——它允许创建多个独立的进程。**并发**可以通过 `threading` 包实现，允许创建不同的线程——或者使用 `asyncio`，它遵循略有不同的哲学。

这两者有什么区别和相似之处？*进程*是由底层操作系统管理的独立过程。一个进程可以启动多个*线程*——线程可以被视为进程的一个子程序。默认情况下，进程是……
