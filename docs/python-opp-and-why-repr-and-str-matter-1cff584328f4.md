# Python OPP，以及为什么 repr() 和 str() 很重要

> 原文：[https://towardsdatascience.com/python-opp-and-why-repr-and-str-matter-1cff584328f4?source=collection_archive---------6-----------------------#2023-11-03](https://towardsdatascience.com/python-opp-and-why-repr-and-str-matter-1cff584328f4?source=collection_archive---------6-----------------------#2023-11-03)

## PYTHON PROGRAMMING

## 本文探讨了使用 repr() 和 str() 处理 Python 类时的各种情况

[](https://medium.com/@nyggus?source=post_page-----1cff584328f4--------------------------------)[![Marcin Kozak](../Images/d7faf62e48ed81dab5d8ad92819fff54.png)](https://medium.com/@nyggus?source=post_page-----1cff584328f4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1cff584328f4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1cff584328f4--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----1cff584328f4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-opp-and-why-repr-and-str-matter-1cff584328f4&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----1cff584328f4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1cff584328f4--------------------------------) · 14 分钟阅读 · 2023年11月3日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1cff584328f4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-opp-and-why-repr-and-str-matter-1cff584328f4&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----1cff584328f4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1cff584328f4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-opp-and-why-repr-and-str-matter-1cff584328f4&source=-----1cff584328f4---------------------bookmark_footer-----------)![](../Images/f8fb381ea5c85aff97d6f167a83d2e54.png)

Python 类需要字符串表示，以便为用户和开发者提供比一堆字母更多的信息。图片来自 [Surendran MP](https://unsplash.com/@sure_mp?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python 类有很多种表现形式。例如，你可以创建一个空类：

```py
class MyClass:
    pass
```

它仍然可以派上用场，例如，作为 [哨兵值](https://en.wikipedia.org/wiki/Sentinel_value)。你可以添加一个 `__init__()` 方法：

```py
class MyClass:
    def __init__(self, value):
        self.value = value
```

这仍然是一个非常简单的类，但这次它将保持一个特定的值。

Python 类的一个极佳特性是它们可以被用作类型，如下所示：

```py
def foo(x: MyClass, n: int) -> list[MyClass]:
    return [x] * n
```

记住，不实现`__init__()`方法并不意味着它不存在。事实上，我们在上面不仅实现了`__init__()`方法，还进行了重载。这是Python类的另一个重要方面，你需要知道：你可以重载许多其他方法，例如`__new__()`、`__eq__()`和`__setattr__()`。如果你不重载这些方法，有些将有默认实现（如`__init__()`、`__new__()`、`__setattr__()`和`__eq__()`），而其他方法则没有（如`__lt__()`和所有…
