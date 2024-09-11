# 使 Python 更快的方法：函数缓存技术——记忆化

> 原文：[https://towardsdatascience.com/make-python-faster-by-caching-functions-memoization-4fca250ab5f6?source=collection_archive---------5-----------------------#2023-11-11](https://towardsdatascience.com/make-python-faster-by-caching-functions-memoization-4fca250ab5f6?source=collection_archive---------5-----------------------#2023-11-11)

## PYTHON 编程

## 文章讨论了使用 Python 标准库实现的记忆化技术。`functools.lru_cache` 装饰器使这一过程变得非常简单！

[](https://medium.com/@nyggus?source=post_page-----4fca250ab5f6--------------------------------)[![Marcin Kozak](../Images/d7faf62e48ed81dab5d8ad92819fff54.png)](https://medium.com/@nyggus?source=post_page-----4fca250ab5f6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4fca250ab5f6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4fca250ab5f6--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----4fca250ab5f6--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmake-python-faster-by-caching-functions-memoization-4fca250ab5f6&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----4fca250ab5f6---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4fca250ab5f6--------------------------------) ·11 分钟阅读·2023 年 11 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4fca250ab5f6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmake-python-faster-by-caching-functions-memoization-4fca250ab5f6&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----4fca250ab5f6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4fca250ab5f6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmake-python-faster-by-caching-functions-memoization-4fca250ab5f6&source=-----4fca250ab5f6---------------------bookmark_footer-----------)![](../Images/e3774b4417d263b6337423d06fdb6a85.png)

你可以要求 Python 记住哪些函数已经返回过，并加以利用。[Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) 拍摄的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我们都知道 Python 可能会慢：

[](https://medium.com/pythoniq/the-speed-of-python-it-aint-that-bad-9f703dd2924e?source=post_page-----4fca250ab5f6--------------------------------) [## Python 的速度：其实并不糟糕！

### 我经常听到 Python 太慢了。真的吗？

medium.com](https://medium.com/pythoniq/the-speed-of-python-it-aint-that-bad-9f703dd2924e?source=post_page-----4fca250ab5f6--------------------------------)

通常在Python中最耗时的是调用执行昂贵处理的函数和类方法。试想一下，你需要对相同的参数运行这样的函数两次；即使两个调用产生相同的输出，也会需要两倍的时间。有没有可能记住这个输出，并在需要时再次使用它？

是的，你可以！这称为[memoization](https://en.wikipedia.org/wiki/Memoization)，这是编程中的一个常见术语。你可以实现自己的memoization技术，但实际上你不必这样做。Python提供了一个强大的memoization工具，并且它在标准库中存在：`functools.lru_cache`装饰器。
