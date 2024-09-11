# Python **enumerate()** 函数详解

> 原文：[`towardsdatascience.com/python-enumerate-function-explained-7907837e171c?source=collection_archive---------15-----------------------#2023-02-20`](https://towardsdatascience.com/python-enumerate-function-explained-7907837e171c?source=collection_archive---------15-----------------------#2023-02-20)

## 在本文中，我们将深入探讨如何使用 Python **enumerate()** 函数

[](https://pyshark.medium.com/?source=post_page-----7907837e171c--------------------------------)![Misha Sv](https://pyshark.medium.com/?source=post_page-----7907837e171c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7907837e171c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7907837e171c--------------------------------) [Misha Sv](https://pyshark.medium.com/?source=post_page-----7907837e171c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F685c2995a8e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-enumerate-function-explained-7907837e171c&user=Misha+Sv&userId=685c2995a8e&source=post_page-685c2995a8e----7907837e171c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7907837e171c--------------------------------) ·3 分钟阅读·2023 年 2 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7907837e171c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-enumerate-function-explained-7907837e171c&user=Misha+Sv&userId=685c2995a8e&source=-----7907837e171c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7907837e171c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-enumerate-function-explained-7907837e171c&source=-----7907837e171c---------------------bookmark_footer-----------)![](img/227b6430ee6fd418032c78c73fbda97f.png)

图片由 [Nick Fewings](https://unsplash.com/ko/@jannerboy62?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，[Unsplash](https://unsplash.com/photos/6H8a6vNGDKg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**目录**

+   介绍

+   使用 enumerate() 与列表

+   使用 enumerate() 与字符串

+   使用 enumerate() 与字典

+   使用 enumerate() 与 zip()

+   结论

# 介绍

Python **enumerate()** 函数是一个内置函数，允许在遍历 Python 可迭代对象的同时跟踪其索引。

Python **enumerate()** 函数的语法是：

```py
enumerate(iterable, start=0)
```

其中：

+   *可迭代* — 可以是任何可迭代的 Python 对象，如字符串、元组、列表、集合、[字典](https://pyshark.com/python-dictionary-data-structure/)等。

+   *start* — 用于指定起始索引的参数（可选）。默认为 0。

# 使用 enumerate() 与列表
