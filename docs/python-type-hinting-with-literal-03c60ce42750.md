# Python 字面量类型提示

> 原文：[`towardsdatascience.com/python-type-hinting-with-literal-03c60ce42750?source=collection_archive---------8-----------------------#2023-11-28`](https://towardsdatascience.com/python-type-hinting-with-literal-03c60ce42750?source=collection_archive---------8-----------------------#2023-11-28)

## PYTHON 编程

## 比看起来更强大：使用 `typing.Literal` 创建字面量类型

[](https://medium.com/@nyggus?source=post_page-----03c60ce42750--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----03c60ce42750--------------------------------)[](https://towardsdatascience.com/?source=post_page-----03c60ce42750--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----03c60ce42750--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----03c60ce42750--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-type-hinting-with-literal-03c60ce42750&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----03c60ce42750---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----03c60ce42750--------------------------------) ·15 分钟阅读·2023 年 11 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F03c60ce42750&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-type-hinting-with-literal-03c60ce42750&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----03c60ce42750---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F03c60ce42750&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-type-hinting-with-literal-03c60ce42750&source=-----03c60ce42750---------------------bookmark_footer-----------)![](img/0bdd431c39d954c0d9226fc02679e589.png)

`typing.Literal` 创建了一个具有选择项的类型。照片由 [Caleb Jones](https://unsplash.com/@gcalebjones?utm_source=medium&utm_medium=referral) 供稿，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

我承认：我以前并不是 `typing.Literal` 的粉丝，这是一种在 Python 中创建字面量类型的方法。实际上，我不仅低估了字面量类型，而且完全忽视了它们，拒绝使用它们。不知道为什么，直到今天我也不太明白，我无法找到字面量类型的实际价值。

我错得太离谱了。我未能认识到这个简单工具的强大，因此我的代码受到了影响。如果你像我一样忽视了字面量类型，我鼓励你阅读这篇文章。我希望能说服你，尽管`typing.Literal`很简单，但它可以成为你 Python 编程工具箱中的一个非常有用的工具。

即使你已经认识到字面量类型的价值，也不要停止阅读。虽然我们不会深入探讨`typing.Literal`的所有复杂细节，但这篇文章会提供比官方 Python 文档更全面的介绍，而不会像[PEP 586](https://peps.python.org/pep-0586/)那样陷入过多细节。

字面量类型非常简单，以至于它们可以使代码比没有这些类型的代码更清晰、更易读。这种简单性既是`typing.Literal`的优势也是劣势，因为它没有提供任何额外的…
