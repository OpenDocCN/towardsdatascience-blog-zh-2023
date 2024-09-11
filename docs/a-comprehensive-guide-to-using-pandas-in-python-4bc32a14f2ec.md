# 使用 Pandas 的全面指南

> 原文：[https://towardsdatascience.com/a-comprehensive-guide-to-using-pandas-in-python-4bc32a14f2ec?source=collection_archive---------6-----------------------#2023-07-18](https://towardsdatascience.com/a-comprehensive-guide-to-using-pandas-in-python-4bc32a14f2ec?source=collection_archive---------6-----------------------#2023-07-18)

## 了解如何使用 Python 中最著名的数据处理库之一

[](https://ivopbernardo.medium.com/?source=post_page-----4bc32a14f2ec--------------------------------)[![Ivo Bernardo](../Images/39887b6f3e63a67c0545e87962ad5df0.png)](https://ivopbernardo.medium.com/?source=post_page-----4bc32a14f2ec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4bc32a14f2ec--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4bc32a14f2ec--------------------------------) [Ivo Bernardo](https://ivopbernardo.medium.com/?source=post_page-----4bc32a14f2ec--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F74eec53531c0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-guide-to-using-pandas-in-python-4bc32a14f2ec&user=Ivo+Bernardo&userId=74eec53531c0&source=post_page-74eec53531c0----4bc32a14f2ec---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4bc32a14f2ec--------------------------------) · 21 min 阅读 · 2023年7月18日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4bc32a14f2ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-guide-to-using-pandas-in-python-4bc32a14f2ec&user=Ivo+Bernardo&userId=74eec53531c0&source=-----4bc32a14f2ec---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4bc32a14f2ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-guide-to-using-pandas-in-python-4bc32a14f2ec&source=-----4bc32a14f2ec---------------------bookmark_footer-----------)![](../Images/47725403daa0167f7868b12b8ce2150f.png)

图片由 [stonewyq](https://unsplash.com/pt-br/@stonewyq) 提供 @ Unsplash.com

当你开始在数据分析、工程或科学的背景下使用 Python 时，`pandas`（很可能）是你需要学习的第一个库之一。这个令人惊叹的库使你能够操作 Python 语言中的两个非常重要的对象——一维的 `Series` 和二维的 `DataFrame`。这些对象是许多数据处理流程的一部分，掌握它们对于启动你的 Python 职业生涯至关重要。

数据框在数据科学和分析中被广泛使用，因为它们能够创建多维和多类型的对象。本文的目标是提供一个非常全面的指南，介绍如何使用一些著名的`pandas`函数以及如何使用该库的最重要功能。希望在阅读完这本指南后，你会准备好使用最重要的`pandas`功能。你可能也很常见地是从SQL背景迁移过来的，所以我会在一些指令中尝试提供SQL代码的对比，以便于在两个框架之间进行比较。但是，请记住，了解SQL绝对不是学习`pandas`的必备条件！
