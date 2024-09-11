# 使用这些方法提升你 Python 并发任务的性能

> 原文：[https://towardsdatascience.com/use-these-methods-to-make-your-python-concurrent-tasks-perform-better-b693b7a633e1?source=collection_archive---------7-----------------------#2023-04-18](https://towardsdatascience.com/use-these-methods-to-make-your-python-concurrent-tasks-perform-better-b693b7a633e1?source=collection_archive---------7-----------------------#2023-04-18)

## [PYTHON CONCURRENCY](https://medium.com/@qtalen/list/python-concurrency-2c979347da3b)

## asyncio.gather、asyncio.as_completed 和 asyncio.wait 的最佳实践

[](https://qtalen.medium.com/?source=post_page-----b693b7a633e1--------------------------------)[![Peng Qian](../Images/9ce9aeb381ec6b017c1ee5d4714937e2.png)](https://qtalen.medium.com/?source=post_page-----b693b7a633e1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b693b7a633e1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b693b7a633e1--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----b693b7a633e1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-these-methods-to-make-your-python-concurrent-tasks-perform-better-b693b7a633e1&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----b693b7a633e1---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b693b7a633e1--------------------------------) ·6 min 阅读·2023年4月18日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb693b7a633e1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-these-methods-to-make-your-python-concurrent-tasks-perform-better-b693b7a633e1&source=-----b693b7a633e1---------------------bookmark_footer-----------)![](../Images/c24747b6b245e92b0b50f2dd4c219345.png)

图片由 [Aleksandr Popov](https://unsplash.com/@5tep5?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 问题所在

Python 的多线程性能一直未能达到预期，因为 GIL 的存在。

自 Python 3.4 版本以来，Python 引入了 asyncio 包以通过并发执行 IO 绑定任务。经过几次迭代，asyncio 的 API 表现非常好，并且与多线程版本相比，并发任务的性能有了显著提高。

然而，程序员在使用 asyncio 时仍然会犯很多错误：

一种错误，如下图所示，是直接使用 await 协程方法，这种方式将对并发任务的调用从异步转换为同步，最终丧失了并发特性。
