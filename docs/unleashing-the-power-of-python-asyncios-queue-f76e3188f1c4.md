# 释放 Python Asyncio 队列的力量

> 原文：[`towardsdatascience.com/unleashing-the-power-of-python-asyncios-queue-f76e3188f1c4?source=collection_archive---------9-----------------------#2023-06-06`](https://towardsdatascience.com/unleashing-the-power-of-python-asyncios-queue-f76e3188f1c4?source=collection_archive---------9-----------------------#2023-06-06)

## [PYTHON 并发](https://medium.com/@qtalen/list/python-concurrency-2c979347da3b)

## 通过实际生活中的例子掌握 asyncio 的生产者-消费者模式

[](https://qtalen.medium.com/?source=post_page-----f76e3188f1c4--------------------------------)![Peng Qian](https://qtalen.medium.com/?source=post_page-----f76e3188f1c4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f76e3188f1c4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f76e3188f1c4--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----f76e3188f1c4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funleashing-the-power-of-python-asyncios-queue-f76e3188f1c4&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----f76e3188f1c4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f76e3188f1c4--------------------------------) ·8 分钟阅读·2023 年 6 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff76e3188f1c4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funleashing-the-power-of-python-asyncios-queue-f76e3188f1c4&user=Peng+Qian&userId=8e2fe735546d&source=-----f76e3188f1c4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff76e3188f1c4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funleashing-the-power-of-python-asyncios-queue-f76e3188f1c4&source=-----f76e3188f1c4---------------------bookmark_footer-----------)![](img/24745d20075c5ff8f7fcdfc41ffc42b9.png)

图片来源：作者创建，[Canva](https://www.canva.com/)

在本文中，我将轻松地解释 Python asyncio 中各种队列的 API 使用及应用场景。

在文章的最后，我将展示在经典购物场景中 `asyncio.Queue` 的实际用法。

# 引言

## 为什么我们需要 asyncio.Queue

正如读者们在我之前的文章中了解到的，我喜欢 asyncio，因为它几乎是并发编程的完美解决方案。

然而，在一个大规模、高并发的项目中，大量不可控的并发任务等待会占用系统资源，导致性能下降。

因此，有必要控制并发任务的数量。

## 为什么我们不能使用 asyncio.Semaphore？

在我上一篇关于同步原语的文章中，我介绍了使用`Semaphore`锁来控制同时运行的并发任务数量。
