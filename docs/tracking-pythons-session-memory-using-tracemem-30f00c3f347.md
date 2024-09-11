# 使用 Tracemem 跟踪 Python 的会话内存

> 原文：[`towardsdatascience.com/tracking-pythons-session-memory-using-tracemem-30f00c3f347?source=collection_archive---------5-----------------------#2023-12-11`](https://towardsdatascience.com/tracking-pythons-session-memory-using-tracemem-30f00c3f347?source=collection_archive---------5-----------------------#2023-12-11)

## PYTHON 编程

## Tracemem 是一个轻量级库，帮助您追踪 Python 会话的完整内存。

[](https://medium.com/@nyggus?source=post_page-----30f00c3f347--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----30f00c3f347--------------------------------)[](https://towardsdatascience.com/?source=post_page-----30f00c3f347--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----30f00c3f347--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----30f00c3f347--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftracking-pythons-session-memory-using-tracemem-30f00c3f347&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----30f00c3f347---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----30f00c3f347--------------------------------) ·9 min read·Dec 11, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F30f00c3f347&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftracking-pythons-session-memory-using-tracemem-30f00c3f347&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----30f00c3f347---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F30f00c3f347&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftracking-pythons-session-memory-using-tracemem-30f00c3f347&source=-----30f00c3f347---------------------bookmark_footer-----------)![](img/6648173ce353664cef3a6ada910ce9e0.png)

Tracemem 是 Python 中的会话内存跟踪器。照片由[Ronan Furuta](https://unsplash.com/@ronan18?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)拍摄。

`Tracemem`是一个轻量级的 Python 性能分析工具，它允许你在特定时刻测量 Python 会话的完整内存使用情况，并跟踪随后的变化。这可以用来调试代码中的内存问题，或者仅仅记录内存使用情况。`Tracemem`的功能集非常有限，因此它是一个非常轻量级的工具，对会话内存的影响很小。然而，像任何内存分析工具一样，它可能会显著影响程序的执行时间。

在底层，这个包是一个围绕`pympler.asizeof.asizeof()`的包装器，`pympler`是一个测量 Python 会话内存使用的函数。这意味着`tracemem`提供了一个简单的 API 来跟踪和评估会话内存。

这种简单性有其代价。你不能使用这个工具来测量特定函数、对象或代码片段的内存使用。如果你的需求超过了仅仅评估会话内存，你可以使用其他工具，例如：

+   `[pympler](https://pypi.org/project/Pympler/)`

+   `[memory_profiler](https://pypi.org/project/memory-profiler/)`

+   `[perftester](https://pypi.org/project/perftester/)`

+   `[memray](https://pypi.org/project/memray/)`
