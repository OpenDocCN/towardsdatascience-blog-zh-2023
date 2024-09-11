# Python: **__init__ 不是构造函数**：深入探讨 Python 对象创建

> 原文：[`towardsdatascience.com/python-init-is-not-a-constructor-a-deep-dive-in-python-object-creation-9134d971e334?source=collection_archive---------1-----------------------#2023-11-27`](https://towardsdatascience.com/python-init-is-not-a-constructor-a-deep-dive-in-python-object-creation-9134d971e334?source=collection_archive---------1-----------------------#2023-11-27)

## 操作 Python 的构造函数以创建快速、内存高效的类

[](https://mikehuls.medium.com/?source=post_page-----9134d971e334--------------------------------)![Mike Huls](https://mikehuls.medium.com/?source=post_page-----9134d971e334--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9134d971e334--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9134d971e334--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----9134d971e334--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ffb62c607ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-init-is-not-a-constructor-a-deep-dive-in-python-object-creation-9134d971e334&user=Mike+Huls&userId=7ffb62c607ee&source=post_page-7ffb62c607ee----9134d971e334---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9134d971e334--------------------------------) · 9 min 阅读 · 2023 年 11 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9134d971e334&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-init-is-not-a-constructor-a-deep-dive-in-python-object-creation-9134d971e334&user=Mike+Huls&userId=7ffb62c607ee&source=-----9134d971e334---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9134d971e334&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-init-is-not-a-constructor-a-deep-dive-in-python-object-creation-9134d971e334&source=-----9134d971e334---------------------bookmark_footer-----------)![](img/50dee26173534a7a06708762a2ea94ac.png)

Python 如何构建对象（图片由 ChatGPT 提供）

你知道 `__init__` 方法**不是构造函数**吗？但如果 `__init__` 不*创建*对象，那究竟是什么在创建对象？在 Python 中对象是如何被创建的？Python 真的有构造函数吗？

本文的目标是 **更好地理解 Python 如何创建对象** 并 **操控这一过程以便开发更好的应用程序**。

首先，我们将深入探讨 Python 如何创建对象。接下来，我们将应用这些知识，并讨论一些有趣的使用案例和一些实际示例。开始编程吧！

# 1\. 理论：在 Python 中创建对象

在这一部分，我们将弄清楚当你创建一个对象时，Python 在幕后做了什么。在下一部分，我们将利用这一新知识并应用于第二部分。

## 如何在 Python 中创建对象？

这应该非常简单；你只需创建一个类的实例。或者你可以创建一个新的内置类型，如`str`或`int`。在下面的代码中，创建了一个基本类的实例。它仅包含一个`__init__`函数和一个`say_hello`方法：
