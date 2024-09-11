# 有效使用 %timeit、%lprun 和 %mprun 来编写高效的 Python 代码

> 原文：[https://towardsdatascience.com/effectively-use-timeit-lprun-and-mprun-to-write-efficient-python-code-f06fb8457049?source=collection_archive---------5-----------------------#2023-03-08](https://towardsdatascience.com/effectively-use-timeit-lprun-and-mprun-to-write-efficient-python-code-f06fb8457049?source=collection_archive---------5-----------------------#2023-03-08)

## 一份关于如何使用魔法命令来测试最有效 Python 代码的实用指南

[](https://suemnjeri.medium.com/?source=post_page-----f06fb8457049--------------------------------)[![苏珊·迈纳](../Images/a3ac5f206f175c711a9e7779283fc18d.png)](https://suemnjeri.medium.com/?source=post_page-----f06fb8457049--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f06fb8457049--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f06fb8457049--------------------------------) [苏珊·迈纳](https://suemnjeri.medium.com/?source=post_page-----f06fb8457049--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7df9dec030e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffectively-use-timeit-lprun-and-mprun-to-write-efficient-python-code-f06fb8457049&user=Susan+Maina&userId=7df9dec030e&source=post_page-7df9dec030e----f06fb8457049---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f06fb8457049--------------------------------) ·11 min read·2023年3月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff06fb8457049&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffectively-use-timeit-lprun-and-mprun-to-write-efficient-python-code-f06fb8457049&user=Susan+Maina&userId=7df9dec030e&source=-----f06fb8457049---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff06fb8457049&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffectively-use-timeit-lprun-and-mprun-to-write-efficient-python-code-f06fb8457049&source=-----f06fb8457049---------------------bookmark_footer-----------)![](../Images/3e9e1405f4a9150be078fd1dfff4beda.png)

[https://pixabay.com/images/id-534120/](https://pixabay.com/images/id-534120/)

我最近参加了一次编码面试，其中一个通过的考量标准是代码的优化程度。不幸的是，我没能通过测试，不是因为代码不能运行（实际上它可以运行），也不是因为逻辑错误（实际上没有错误），而是因为我的代码没有经过优化。

作为一名数据科学家，高效的代码：

+   在处理和分析大型或复杂数据时节省时间。

+   促进可扩展性，确保你的数据集能够处理更大的数据集和复杂的模型。

+   创建可重用和模块化的代码，这样可以节省时间并减少错误。

+   易于维护和更新，因为它简单易懂。

+   可以与更广泛的受众分享，因为它可以在性能较弱的硬件上运行。

***优化代码 == 高效代码***

在 Python 中，高效代码是：

+   Pythonic — 它使用了 Python 的[独特风格和习惯用法](https://peps.python.org/pep-0020/)，这是创始人和社区所期望的。

+   易读性 — 它易于阅读和理解代码的功能。对于……
