# 使用 cProfile 对 Python 代码进行性能分析

> 原文：[`towardsdatascience.com/profiling-python-code-with-cprofile-328ae152fdfc?source=collection_archive---------19-----------------------#2023-02-10`](https://towardsdatascience.com/profiling-python-code-with-cprofile-328ae152fdfc?source=collection_archive---------19-----------------------#2023-02-10)

## 在本文中，我们将探讨如何使用 **cProfile** 模块对 Python 代码进行性能分析

[](https://pyshark.medium.com/?source=post_page-----328ae152fdfc--------------------------------)![Misha Sv](https://pyshark.medium.com/?source=post_page-----328ae152fdfc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----328ae152fdfc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----328ae152fdfc--------------------------------) [Misha Sv](https://pyshark.medium.com/?source=post_page-----328ae152fdfc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F685c2995a8e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprofiling-python-code-with-cprofile-328ae152fdfc&user=Misha+Sv&userId=685c2995a8e&source=post_page-685c2995a8e----328ae152fdfc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----328ae152fdfc--------------------------------) ·6 分钟阅读·2023 年 2 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F328ae152fdfc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprofiling-python-code-with-cprofile-328ae152fdfc&user=Misha+Sv&userId=685c2995a8e&source=-----328ae152fdfc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F328ae152fdfc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprofiling-python-code-with-cprofile-328ae152fdfc&source=-----328ae152fdfc---------------------bookmark_footer-----------)![](img/89cacd18fab67e3c61470469f4cd9bca.png)

图片由 [Arnold Francisca](https://unsplash.com/@clark_fransa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/f77Bh3inUpE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**目录**

+   介绍

+   什么是代码分析？

+   cProfile 的基本使用

+   从终端使用 cProfile 对 Python 代码进行性能分析

+   在 Python 中使用 cProfile 对代码进行性能分析

+   在 Python 中使用 cProfile 进行代码分析

+   导出 cProfile 数据

+   结论

# 介绍

现在程序员可以在几天内编写数千行代码。新程序和应用的复杂性不断演变，代码库包括多个功能，其中一些可能会降低整个程序的性能。

专注于代码分析，特别是分析你的 Python 代码以识别性能瓶颈，可以显著提高软件的性能，并创造更好的…
