# Python sorted() 函数解析

> 原文：[`towardsdatascience.com/python-sorted-function-explained-8e46bc002147?source=collection_archive---------21-----------------------#2023-01-16`](https://towardsdatascience.com/python-sorted-function-explained-8e46bc002147?source=collection_archive---------21-----------------------#2023-01-16)

## 在这篇文章中，我们将探讨如何使用 Python **sorted()** 函数。

[](https://pyshark.medium.com/?source=post_page-----8e46bc002147--------------------------------)![Misha Sv](https://pyshark.medium.com/?source=post_page-----8e46bc002147--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8e46bc002147--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8e46bc002147--------------------------------) [Misha Sv](https://pyshark.medium.com/?source=post_page-----8e46bc002147--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F685c2995a8e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-sorted-function-explained-8e46bc002147&user=Misha+Sv&userId=685c2995a8e&source=post_page-685c2995a8e----8e46bc002147---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8e46bc002147--------------------------------) ·4 分钟阅读·2023 年 1 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8e46bc002147&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-sorted-function-explained-8e46bc002147&user=Misha+Sv&userId=685c2995a8e&source=-----8e46bc002147---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8e46bc002147&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-sorted-function-explained-8e46bc002147&source=-----8e46bc002147---------------------bookmark_footer-----------)![](img/5fda325ced75dc2866b7414161b8a2b8.png)

图片由 [Andre Taissin](https://unsplash.com/fr/@andretaissin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/hOwcob_3dpc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**目录**

+   介绍

+   使用 sorted() 进行基本排序

+   使用 key 函数与 sorted()

+   使用 sorted() 对自定义对象进行排序

+   结论

# 介绍

Python **sorted()** 函数是一个内置的排序可迭代对象的函数。

它使用 timsort 作为排序算法，该算法源自归并排序和插入排序。

Python **sorted()** 函数的语法如下：

```py
sorted(iterable, key=None, reverse=False)
```

其中：

+   *可迭代对象* — 可以是任何可迭代的 Python 对象，如字符串、元组、列表、集合、[字典](https://pyshark.com/python-dictionary-data-structure/)等。

+   *键* — 可选参数，允许添加一个函数（例如[lambda 函数](https://pyshark.com/python-lambda-functions/)）作为排序的键。默认为 *None*。
