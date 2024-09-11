# Python `ord()` 和 `chr()` 函数解析

> 原文：[`towardsdatascience.com/python-ord-and-chr-functions-explained-dcb39944c480?source=collection_archive---------22-----------------------#2023-01-12`](https://towardsdatascience.com/python-ord-and-chr-functions-explained-dcb39944c480?source=collection_archive---------22-----------------------#2023-01-12)

## 在这篇文章中，我们将深入探讨如何使用 Python 的 **ord()** 和 **chr()** 函数

[](https://pyshark.medium.com/?source=post_page-----dcb39944c480--------------------------------)![Misha Sv](https://pyshark.medium.com/?source=post_page-----dcb39944c480--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dcb39944c480--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dcb39944c480--------------------------------) [Misha Sv](https://pyshark.medium.com/?source=post_page-----dcb39944c480--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F685c2995a8e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-ord-and-chr-functions-explained-dcb39944c480&user=Misha+Sv&userId=685c2995a8e&source=post_page-685c2995a8e----dcb39944c480---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dcb39944c480--------------------------------) ·3 分钟阅读·2023 年 1 月 12 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdcb39944c480&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-ord-and-chr-functions-explained-dcb39944c480&source=-----dcb39944c480---------------------bookmark_footer-----------)![](img/a8dd2b58383572c073c66cfb43419c4c.png)

照片由 [Brett Jordan](https://unsplash.com/fr/@brett_jordan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/7PYqjNzvrc4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**目录**

+   介绍

+   使用 `ord()` 将字符转换为 Unicode 码点

+   使用 `ord()` 将字符串转换为 Unicode 码点

+   使用 `chr()` 将整数转换为 Unicode 字符

+   结论

# 介绍

Python **ord()** 函数是一个内置函数，返回指定字符的 Unicode 码点。

Unicode 码点是一个整数，用于表示 [Unicode 标准](https://unicode.org/standard/standard.html)中的字符。

**ord()**函数的处理定义如下：

```py
ord(character) -> Unicode code
```

其中*字符*是一个[Unicode 字符](https://en.wikipedia.org/wiki/List_of_Unicode_characters)。

Python 的**chr()**函数是一个内置函数，返回指定字符的 Unicode 码点。
