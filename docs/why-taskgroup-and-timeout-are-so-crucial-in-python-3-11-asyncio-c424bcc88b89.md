# 为什么 Taskgroup 和 Timeout 在 Python 3.11 Asyncio 中如此关键

> 原文：[`towardsdatascience.com/why-taskgroup-and-timeout-are-so-crucial-in-python-3-11-asyncio-c424bcc88b89?source=collection_archive---------5-----------------------#2023-04-12`](https://towardsdatascience.com/why-taskgroup-and-timeout-are-so-crucial-in-python-3-11-asyncio-c424bcc88b89?source=collection_archive---------5-----------------------#2023-04-12)

## [PYTHON CONCURRENCY](https://medium.com/@qtalen/list/python-concurrency-2c979347da3b)

## 拥抱 Python 3.11 的结构化并发

[](https://qtalen.medium.com/?source=post_page-----c424bcc88b89--------------------------------)![Peng Qian](https://qtalen.medium.com/?source=post_page-----c424bcc88b89--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c424bcc88b89--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c424bcc88b89--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----c424bcc88b89--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-taskgroup-and-timeout-are-so-crucial-in-python-3-11-asyncio-c424bcc88b89&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----c424bcc88b89---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c424bcc88b89--------------------------------) · 6 分钟阅读 · 2023 年 4 月 12 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc424bcc88b89&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-taskgroup-and-timeout-are-so-crucial-in-python-3-11-asyncio-c424bcc88b89&user=Peng+Qian&userId=8e2fe735546d&source=-----c424bcc88b89---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc424bcc88b89&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-taskgroup-and-timeout-are-so-crucial-in-python-3-11-asyncio-c424bcc88b89&source=-----c424bcc88b89---------------------bookmark_footer-----------)![](img/d2c98fe735728660298181954df9a9cc.png)

图片由[Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# **Python 3.11 asyncio 包的新特性**

## **1\. 介绍**

在去年的 Python 3.11 版本中，asyncio 包添加了 `TaskGroup` 和 `timeout` APIs。这两个 APIs 引入了正式的结构化并发特性，帮助我们更好地管理并发任务的生命周期。今天，我将介绍如何使用这两个 APIs 以及结构化并发给 Python 并发编程带来的重大改进。

## **2\. TaskGroup**

`TaskGroup` 是使用异步上下文管理器创建的，可以通过 `create_task` 方法向组中添加并发任务，以下是代码示例：

当上下文管理器退出时，它会等待组中的所有任务完成。在等待期间，我们仍然可以添加新任务到…
