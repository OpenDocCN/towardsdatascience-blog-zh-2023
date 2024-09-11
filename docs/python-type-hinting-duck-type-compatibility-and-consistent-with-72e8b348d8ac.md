# Python 类型提示：鸭子类型兼容性与一致性

> 原文：[`towardsdatascience.com/python-type-hinting-duck-type-compatibility-and-consistent-with-72e8b348d8ac?source=collection_archive---------8-----------------------#2023-06-06`](https://towardsdatascience.com/python-type-hinting-duck-type-compatibility-and-consistent-with-72e8b348d8ac?source=collection_archive---------8-----------------------#2023-06-06)

## PYTHON PROGRAMMING

## 你在提示浮点数时不需要提示`int`，在提示元组时也不需要提示`namedtuple`。为什么？

[](https://medium.com/@nyggus?source=post_page-----72e8b348d8ac--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----72e8b348d8ac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----72e8b348d8ac--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----72e8b348d8ac--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----72e8b348d8ac--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-type-hinting-duck-type-compatibility-and-consistent-with-72e8b348d8ac&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----72e8b348d8ac---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----72e8b348d8ac--------------------------------) ·8 分钟阅读·2023 年 6 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F72e8b348d8ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-type-hinting-duck-type-compatibility-and-consistent-with-72e8b348d8ac&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----72e8b348d8ac---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F72e8b348d8ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-type-hinting-duck-type-compatibility-and-consistent-with-72e8b348d8ac&source=-----72e8b348d8ac---------------------bookmark_footer-----------)![](img/7661c8753bdda73a66f4cc31ac70bc99.png)

图片由 [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

有时，Python 类型提示可以让事情变得更简单。确实，并非总是如此——但至少在我看来，它很经常如此——前提是做得明智一些。有人不同意，但我不打算与他们争论：在我看来，这个话题相当主观。

我在以下文章中写了我对 Python 类型提示的看法，如何使用它来提高代码可读性，以及如何*不要*使用它达到相反的效果：

[## Python 的类型提示：朋友、敌人还是只是个头疼的难题？

### 类型提示在 Python 社区中的受欢迎程度正在上升。这将把我们引向何处？我们可以做些什么来使用它…

[更好的编程](https://betterprogramming.pub/pythons-type-hinting-friend-foe-or-just-a-headache-73c7849039c7?source=post_page-----72e8b348d8ac--------------------------------)

今天，我们将讨论在 Python 类型中*一致性*和*鸭子类型兼容性*的含义。

想象一下你在提示使用`float`，如下函数所示：

```py
from collections.abc import Sequence

def sum_of_squares(x: Sequence[float]) -> float:
    n, s = len(x), sum(x)
    return sum((x_i - s/n)**2 for x_i in x)
```
