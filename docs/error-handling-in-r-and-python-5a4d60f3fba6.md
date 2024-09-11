# R 和 Python 中的错误处理

> 原文：[https://towardsdatascience.com/error-handling-in-r-and-python-5a4d60f3fba6?source=collection_archive---------6-----------------------#2023-04-27](https://towardsdatascience.com/error-handling-in-r-and-python-5a4d60f3fba6?source=collection_archive---------6-----------------------#2023-04-27)

## 如果错误发生在函数中间，可以处理

[](https://gustavorsantos.medium.com/?source=post_page-----5a4d60f3fba6--------------------------------)[![Gustavo Santos](../Images/a19a9f4525cdeb6e7a76cd05246aa622.png)](https://gustavorsantos.medium.com/?source=post_page-----5a4d60f3fba6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5a4d60f3fba6--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5a4d60f3fba6--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----5a4d60f3fba6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ferror-handling-in-r-and-python-5a4d60f3fba6&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----5a4d60f3fba6---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----5a4d60f3fba6--------------------------------) ·5分钟阅读·2023年4月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5a4d60f3fba6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ferror-handling-in-r-and-python-5a4d60f3fba6&user=Gustavo+Santos&userId=4429d99b1245&source=-----5a4d60f3fba6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5a4d60f3fba6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ferror-handling-in-r-and-python-5a4d60f3fba6&source=-----5a4d60f3fba6---------------------bookmark_footer-----------)![](../Images/aa8bcccb535506d244fe491e4c34f330.png)

图片由 [Joshua Hoehne](https://unsplash.com/@mrthetrain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/WPrTKRw8KRQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 简介

有一件事是肯定的：*如果你曾经写过代码，你就遇到过错误。就这么简单。*

错误并不总是坏事。我同意，它们有时会让我们抓狂，尤其是当我们一遍遍地检查代码却找不到错误时。然而，错误消息必须被理解为代码中出现了预期之外的问题。

面对现实吧，我们可以创建N个测试场景，但最终用户会发现第N+1个错误。这没关系，只要我们能规划最常见的错误就好。

在这篇文章中，我们将介绍一些可以让我们的代码在出现错误时继续运行的已知错误处理函数。我们将看到 R 和 Python 的代码片段。

继续阅读。

# 错误处理

> 错误处理是一种编程资源，使您的代码在发现错误后继续正常运行，而不会中断。

想象一下，你正在编写一个函数，它接受两个数字，计算它们之间的差值，并返回与第一个数字相比的差值的百分比值。这是一个相当…
