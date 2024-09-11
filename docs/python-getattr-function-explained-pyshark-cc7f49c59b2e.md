# Python getattr() 函数解释

> 原文：[https://towardsdatascience.com/python-getattr-function-explained-pyshark-cc7f49c59b2e?source=collection_archive---------22-----------------------#2023-03-20](https://towardsdatascience.com/python-getattr-function-explained-pyshark-cc7f49c59b2e?source=collection_archive---------22-----------------------#2023-03-20)

## 在本文中，我们将探讨如何使用 Python 的 `getattr()` 函数。

[](https://pyshark.medium.com/?source=post_page-----cc7f49c59b2e--------------------------------)[![Misha Sv](../Images/d3f9605e2c7020246ff793869728e218.png)](https://pyshark.medium.com/?source=post_page-----cc7f49c59b2e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cc7f49c59b2e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cc7f49c59b2e--------------------------------) [Misha Sv](https://pyshark.medium.com/?source=post_page-----cc7f49c59b2e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F685c2995a8e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-getattr-function-explained-pyshark-cc7f49c59b2e&user=Misha+Sv&userId=685c2995a8e&source=post_page-685c2995a8e----cc7f49c59b2e---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cc7f49c59b2e--------------------------------) ·4分钟阅读·2023年3月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcc7f49c59b2e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-getattr-function-explained-pyshark-cc7f49c59b2e&user=Misha+Sv&userId=685c2995a8e&source=-----cc7f49c59b2e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcc7f49c59b2e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-getattr-function-explained-pyshark-cc7f49c59b2e&source=-----cc7f49c59b2e---------------------bookmark_footer-----------)![](../Images/a5e64fb6704e20c5a2c1070c9a117bc2.png)

图片由 [Shane Aldendorff](https://unsplash.com/@pluyar?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/3AzL-IR3v7Y?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## **目录**

+   介绍

+   使用 getattr() 动态访问对象的属性

+   使用 getattr() 构建动态 API

+   使用 getattr() 动态加载模块

+   结论

# 介绍

Python **getattr()** 函数是一个内置函数，允许动态访问对象的属性。具体来说，它用于检索 Python 对象的属性名称。

Python **getattr()** 函数的语法是：

```py
getattr(object, name[, default])
```

其中：

+   *object* — 我们希望从中检索属性的 Python 对象

+   *name* — Python 对象的命名属性的名称

+   *default* — 可选参数，用于指定如果未找到命名属性时的返回值。如果未指定，代码将返回**AttributeError**

**getattr()** 函数在调用时，会搜索给定 Python 对象的指定命名属性，并返回其值。
