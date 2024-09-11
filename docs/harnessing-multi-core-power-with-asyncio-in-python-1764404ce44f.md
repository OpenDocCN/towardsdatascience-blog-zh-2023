# 利用 Python 中的多核性能

> 原文：[`towardsdatascience.com/harnessing-multi-core-power-with-asyncio-in-python-1764404ce44f?source=collection_archive---------6-----------------------#2023-05-09`](https://towardsdatascience.com/harnessing-multi-core-power-with-asyncio-in-python-1764404ce44f?source=collection_archive---------6-----------------------#2023-05-09)

## [PYTHON 并发](https://medium.com/@qtalen/list/python-concurrency-2c979347da3b)

## 通过有效利用多个 CPU 核心，提升 Python 应用程序的性能

[](https://qtalen.medium.com/?source=post_page-----1764404ce44f--------------------------------)![Peng Qian](https://qtalen.medium.com/?source=post_page-----1764404ce44f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1764404ce44f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1764404ce44f--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----1764404ce44f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fharnessing-multi-core-power-with-asyncio-in-python-1764404ce44f&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----1764404ce44f---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1764404ce44f--------------------------------) ·7 分钟阅读·2023 年 5 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1764404ce44f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fharnessing-multi-core-power-with-asyncio-in-python-1764404ce44f&user=Peng+Qian&userId=8e2fe735546d&source=-----1764404ce44f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1764404ce44f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fharnessing-multi-core-power-with-asyncio-in-python-1764404ce44f&source=-----1764404ce44f---------------------bookmark_footer-----------)![](img/229d20ad602218a5869ea3daecce8616.png)

图片来源：作者创建，[Canva](https://www.canva.com/)

# 介绍

在这篇文章中，我将展示如何在多核 CPU 上执行 Python asyncio 代码，以解锁并发任务的全部性能。

## 我们的问题是什么？

asyncio 仅使用一个核心。

在之前的文章中，我详细介绍了如何使用 Python asyncio。通过这些知识，你可以了解到 asyncio 通过手动切换任务执行，绕过 GIL 争用过程，从而允许 IO 绑定任务以高速度执行。

理论上，IO 绑定任务的执行时间取决于从发起到 IO 操作响应的时间，而不依赖于你的 CPU 性能。因此，我们可以同时发起数万的 IO 任务并迅速完成。

但最近，我在编写一个需要同时抓取数万网页的程序时发现，尽管我的 asyncio 程序比使用迭代抓取网页的程序高效得多……
