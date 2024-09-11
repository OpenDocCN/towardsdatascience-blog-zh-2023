# Python 中的异常处理：从基础到高级，再到技巧

> 原文：[`towardsdatascience.com/exception-handling-in-python-from-basic-to-advanced-then-tricks-9b495619730a?source=collection_archive---------6-----------------------#2023-04-03`](https://towardsdatascience.com/exception-handling-in-python-from-basic-to-advanced-then-tricks-9b495619730a?source=collection_archive---------6-----------------------#2023-04-03)

![](img/24d435292333eebb87f13abd8334dfc5.png)

图片来源：[Thomas Malyska](https://pixabay.com/users/fotomek-7986133/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=6124033) 来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=6124033)

## 探索 Python 异常处理的隐藏秘密

[](https://christophertao.medium.com/?source=post_page-----9b495619730a--------------------------------)![Christopher Tao](https://christophertao.medium.com/?source=post_page-----9b495619730a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9b495619730a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9b495619730a--------------------------------) [Christopher Tao](https://christophertao.medium.com/?source=post_page-----9b495619730a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb8176fabf308&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexception-handling-in-python-from-basic-to-advanced-then-tricks-9b495619730a&user=Christopher+Tao&userId=b8176fabf308&source=post_page-b8176fabf308----9b495619730a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9b495619730a--------------------------------) ·10 分钟阅读·2023 年 4 月 3 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9b495619730a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexception-handling-in-python-from-basic-to-advanced-then-tricks-9b495619730a&source=-----9b495619730a---------------------bookmark_footer-----------)

Python 编程中的一个重要方面是异常处理，它指的是在程序执行过程中处理错误和意外事件的方式。异常处理对编写健壮且可靠的代码至关重要，因为它使程序员能够以结构化和可控的方式处理错误和异常。

在这篇文章中，我将提供一个关于 Python 异常处理的全面指南，涵盖从基本的 try-except 块到更高级的技术。无论你是**Python 新手**还是**经验丰富的开发者**（你可以从第三部分开始），这篇文章将为你提供 Python 异常处理的完整概述，以及一些你可能未曾遇到的有用技巧和窍门。因此，无论你是刚刚开始学习 Python 还是希望提升你的异常处理技能，这篇文章都是帮助你入门的完美资源。

# 1\. 基础知识

![](img/a62c6ba5bd93e722422f6cc6f70b2280.png)

图片来自 [Saul](https://pixabay.com/users/saulhm-31267/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=106359) [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=106359)

## 1.1 最简单的异常处理
