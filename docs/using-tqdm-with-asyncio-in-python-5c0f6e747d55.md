# 在Python中使用Tqdm和Asyncio

> 原文：[https://towardsdatascience.com/using-tqdm-with-asyncio-in-python-5c0f6e747d55?source=collection_archive---------12-----------------------#2023-05-02](https://towardsdatascience.com/using-tqdm-with-asyncio-in-python-5c0f6e747d55?source=collection_archive---------12-----------------------#2023-05-02)

## [PYTHON CONCURRENCY](https://medium.com/@qtalen/list/python-concurrency-2c979347da3b)

## 监控并发任务进度的高效方法

[](https://qtalen.medium.com/?source=post_page-----5c0f6e747d55--------------------------------)[![Peng Qian](../Images/9ce9aeb381ec6b017c1ee5d4714937e2.png)](https://qtalen.medium.com/?source=post_page-----5c0f6e747d55--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5c0f6e747d55--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5c0f6e747d55--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----5c0f6e747d55--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-tqdm-with-asyncio-in-python-5c0f6e747d55&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----5c0f6e747d55---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5c0f6e747d55--------------------------------) ·6分钟阅读·2023年5月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5c0f6e747d55&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-tqdm-with-asyncio-in-python-5c0f6e747d55&user=Peng+Qian&userId=8e2fe735546d&source=-----5c0f6e747d55---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5c0f6e747d55&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-tqdm-with-asyncio-in-python-5c0f6e747d55&source=-----5c0f6e747d55---------------------bookmark_footer-----------)![](../Images/93400ad3e3d4ab25829de2c79ad64482.png)

图片由 [Jungwoo Hong](https://unsplash.com/@hjwinunsplsh?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 引言

## 令我困扰的是

在Python中使用并发编程以提高效率对于数据科学家来说并不罕见。观察各种子进程或并发线程在后台运行，以保持计算或IO绑定任务的有序总是令人满意的。

但仍然困扰我的一件事是，当我同时处理数百或数千个文件，或在后台执行数百个进程时，我总是担心某些任务会悄悄挂起，导致整个代码永远无法完成。我也很难知道代码当前的执行状态。

最糟糕的部分是，当我看着空白的屏幕时，很难判断我的代码还需要多长时间才能执行完毕，或者预计完成时间是什么。这对我安排工作日程非常不利。

因此，我想要一种方法来让我知道代码执行到了哪里。

## 过去是如何做到的

更传统的方法是共享任务之间的内存区域，在这个内存区域放置一个计数器，让这个计数器+1…
