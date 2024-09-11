# Python 可调用对象：基础与秘密

> 原文：[`towardsdatascience.com/python-callables-the-basics-and-the-secrets-ba88bf0729aa?source=collection_archive---------5-----------------------#2023-10-27`](https://towardsdatascience.com/python-callables-the-basics-and-the-secrets-ba88bf0729aa?source=collection_archive---------5-----------------------#2023-10-27)

## PYTHON 编程

## 了解 Python 可调用对象的强大功能。

[](https://medium.com/@nyggus?source=post_page-----ba88bf0729aa--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----ba88bf0729aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ba88bf0729aa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ba88bf0729aa--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----ba88bf0729aa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-callables-the-basics-and-the-secrets-ba88bf0729aa&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----ba88bf0729aa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ba88bf0729aa--------------------------------) ·10 分钟阅读·2023 年 10 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fba88bf0729aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-callables-the-basics-and-the-secrets-ba88bf0729aa&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----ba88bf0729aa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fba88bf0729aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-callables-the-basics-and-the-secrets-ba88bf0729aa&source=-----ba88bf0729aa---------------------bookmark_footer-----------)![](img/16289801bda2c8ec8dfa0b3a3bba76f4.png)

在 Python 中，有许多可调用对象可以选择。照片由 [Pavan Trikutam](https://unsplash.com/@ptrikutam?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在编程语言中，可调用对象通常与函数相关联，这也有其原因。函数可能是最好的可调用对象示例，但它们并不是唯一的。在 Python 中，还有许多其他可调用类型，这些类型可以非常有用且强大。你也可以创建自己的可调用对象。本文讨论了这两方面内容。

*一个可调用对象* 是可以通过一对括号调用的对象，例如下面的例子，我们使用内置函数 `sum()`：

```py
>>> sum([1, 1, 1])
3
```

对于一个可调用对象，根据其定义，可以进行调用。

+   无任何参数，如 `no_args_callable()`

+   或一系列位置参数和/或关键字参数，如 `args_callable(arg1, arg2)`、`args_callable(arg1, arg2=value2)` 或 `args_callable(arg1=value1, arg2=value2)`

上面，我将可调用对象描述为名词。然而，*callable* 这个词也用作形容词，意指 *成为一个可调用对象*。因此，可调用对象等同于可调用对象。

Python 有一个内置函数 `callable()`，它检查一个对象是否是可调用的，或者换句话说，如果…
