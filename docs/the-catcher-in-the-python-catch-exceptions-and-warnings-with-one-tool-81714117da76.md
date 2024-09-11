# 《Python 中的接球手… 捕获异常和警告的单一工具》

> 原文：[`towardsdatascience.com/the-catcher-in-the-python-catch-exceptions-and-warnings-with-one-tool-81714117da76?source=collection_archive---------19-----------------------#2023-05-23`](https://towardsdatascience.com/the-catcher-in-the-python-catch-exceptions-and-warnings-with-one-tool-81714117da76?source=collection_archive---------19-----------------------#2023-05-23)

## PYTHON 编程

## 为什么不实现一个工具来捕获 Python 中的异常和警告呢？

[](https://medium.com/@nyggus?source=post_page-----81714117da76--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----81714117da76--------------------------------)[](https://towardsdatascience.com/?source=post_page-----81714117da76--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----81714117da76--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----81714117da76--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-catcher-in-the-python-catch-exceptions-and-warnings-with-one-tool-81714117da76&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----81714117da76---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----81714117da76--------------------------------) · 13 min 阅读 · 2023 年 5 月 23 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F81714117da76&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-catcher-in-the-python-catch-exceptions-and-warnings-with-one-tool-81714117da76&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----81714117da76---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F81714117da76&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-catcher-in-the-python-catch-exceptions-and-warnings-with-one-tool-81714117da76&source=-----81714117da76---------------------bookmark_footer-----------)![](img/6f42c45eba05eb69d8a1a9adcc012803.png)

这篇文章讨论了在 Python 中捕获异常和警告的内容。图片由 [Keith Johnston](https://unsplash.com/de/@acfb5071?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python 对处理异常有一些很好的方法。你可以捕获异常、抛出异常、重新抛出异常、同时抛出两个异常（或者更确切地说，从一个异常中抛出另一个异常）、记录它们，并创建自定义错误及其层级，仅举几个最重要的。如果你想处理警告，也可以做到这一点——但你需要两个独立的工具：一个用于异常，另一个用于警告。

阅读这篇文章，了解如何编写一个既能捕获异常也能捕获警告的单一捕获器，以及如何根据你的需求调整它。

我真的很喜欢 Python 的异常处理。我喜欢它的简单性，例如可以在这里看到：

```py
class MultiplicationError(Exception): ...

def multiply_str(x: str, n: int) -> str:
    try:
        xn = x * n
    except TypeError as e:
        raise MultiplicationError(
            "Can't multiply objects of "
            f"{type(x).__name__} "
            f"and {type(n).__name__} types"
            ) from e
    if not isinstance(xn, str):
        raise MultiplicationError(
            f"{type(x).__name__} multiplied by"…
```
