# Python 类型提示：从类型别名到类型变量及新类型

> 原文：[`towardsdatascience.com/python-type-hinting-from-type-aliases-to-type-variables-and-new-types-a4a9e0400b6b?source=collection_archive---------11-----------------------#2023-04-26`](https://towardsdatascience.com/python-type-hinting-from-type-aliases-to-type-variables-and-new-types-a4a9e0400b6b?source=collection_archive---------11-----------------------#2023-04-26)

## PYTHON 编程

## 查看类型别名、类型变量和新类型的实际应用

![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----a4a9e0400b6b--------------------------------) ![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4a9e0400b6b--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----a4a9e0400b6b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-type-hinting-from-type-aliases-to-type-variables-and-new-types-a4a9e0400b6b&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----a4a9e0400b6b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4a9e0400b6b--------------------------------) ·15 分钟阅读·2023 年 4 月 26 日

--

![](img/7194d200a270aae3a3eee222159f61df.png)

Python 提供了类型提示。选择权仍在于你。图片由 [William Felker](https://unsplash.com/@gndclouds?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

正如我在下面的文章中所写，如果你想在 Python 中使用类型提示，应该正确地使用：

[](https://betterprogramming.pub/pythons-type-hinting-friend-foe-or-just-a-headache-73c7849039c7?source=post_page-----a4a9e0400b6b--------------------------------) [## Python 的类型提示：朋友、敌人还是仅仅是头疼的问题？

### 类型提示在 Python 社区中的受欢迎程度不断上升。这将引导我们到哪里？我们可以做些什么来使用它……

[betterprogramming.pub](https://betterprogramming.pub/pythons-type-hinting-friend-foe-or-just-a-headache-73c7849039c7?source=post_page-----a4a9e0400b6b--------------------------------)

什么是*正确的方式*？简单地说，就是从静态类型检查器的角度来看，使代码*可读*和*正确*的方式。因此，两个方面：*可读*和*正确*。

在上文提到的内容中，创建 [类型别名](https://mypy.readthedocs.io/en/stable/kinds_of_types.html#type-aliases) 是提高可读性的一个好方法。我们将从这里开始讨论，重点关注它们确实能够提供帮助的情况。接着，我们将讨论使用类型变量（`typing.TypeVar`）和新类型（`typing.NewType`），它们将帮助我们实现常规类型别名无法实现的目标。

我将使用 Python 3.11 和 `mypy` 版本 1.2.0。

简单来说，使用类型别名的目的有两个方面：

1.  让用户以相对简单的方式了解……
