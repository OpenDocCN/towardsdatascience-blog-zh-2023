# Python help() 函数解释

> 原文：[`towardsdatascience.com/python-help-function-explained-fba9c15f42b1?source=collection_archive---------17-----------------------#2023-01-13`](https://towardsdatascience.com/python-help-function-explained-fba9c15f42b1?source=collection_archive---------17-----------------------#2023-01-13)

## 在本文中，我们将深入探讨如何使用 Python **help()** 函数

[](https://pyshark.medium.com/?source=post_page-----fba9c15f42b1--------------------------------)![Misha Sv](https://pyshark.medium.com/?source=post_page-----fba9c15f42b1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fba9c15f42b1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fba9c15f42b1--------------------------------) [Misha Sv](https://pyshark.medium.com/?source=post_page-----fba9c15f42b1--------------------------------)

·

[阅读](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F685c2995a8e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-help-function-explained-fba9c15f42b1&user=Misha+Sv&userId=685c2995a8e&source=post_page-685c2995a8e----fba9c15f42b1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fba9c15f42b1--------------------------------) ·4 分钟阅读·2023 年 1 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffba9c15f42b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-help-function-explained-fba9c15f42b1&user=Misha+Sv&userId=685c2995a8e&source=-----fba9c15f42b1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffba9c15f42b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-help-function-explained-fba9c15f42b1&source=-----fba9c15f42b1---------------------bookmark_footer-----------)![](img/478d5da12e5951b60853775b6e7feca4.png)

图片来源：[Toa Heftiba](https://unsplash.com/@heftiba?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 摄影，[Unsplash](https://unsplash.com/photos/_UIVmIBB3JU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**目录**

+   介绍

+   使用交互式帮助工具访问文档

+   使用 `help()` 访问对象文档

+   使用 `help()` 访问用户定义的函数文档

+   结论

# 介绍

在 Python 中，我们经常使用一些我们以前没用过的新模块、函数、类或对象，而这些文档我们还没阅读过。

我们可以利用 Python **help()** 函数更快地获取信息，而不是浏览文档网站寻找特定的函数或类。

Python **help()** 函数用于显示指定模块、函数、类或对象的文档。

**help()** 函数的处理过程定义如下：

```py
help([object]) -> display documentation
```
