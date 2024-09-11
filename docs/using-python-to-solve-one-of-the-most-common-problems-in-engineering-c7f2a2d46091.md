# 使用 Python 解决工程领域中最常见的问题之一

> 原文：[https://towardsdatascience.com/using-python-to-solve-one-of-the-most-common-problems-in-engineering-c7f2a2d46091?source=collection_archive---------3-----------------------#2023-01-02](https://towardsdatascience.com/using-python-to-solve-one-of-the-most-common-problems-in-engineering-c7f2a2d46091?source=collection_archive---------3-----------------------#2023-01-02)

## 创建一个通用的操作点分析框架

[](https://medium.com/@nhemenway2013?source=post_page-----c7f2a2d46091--------------------------------)[![Nick Hemenway](../Images/a38cade9fe4f81c7e43e5d3155a21d78.png)](https://medium.com/@nhemenway2013?source=post_page-----c7f2a2d46091--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c7f2a2d46091--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c7f2a2d46091--------------------------------) [Nick Hemenway](https://medium.com/@nhemenway2013?source=post_page-----c7f2a2d46091--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7db1695b5485&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-python-to-solve-one-of-the-most-common-problems-in-engineering-c7f2a2d46091&user=Nick+Hemenway&userId=7db1695b5485&source=post_page-7db1695b5485----c7f2a2d46091---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c7f2a2d46091--------------------------------) ·17分钟阅读·2023年1月2日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc7f2a2d46091&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-python-to-solve-one-of-the-most-common-problems-in-engineering-c7f2a2d46091&user=Nick+Hemenway&userId=7db1695b5485&source=-----c7f2a2d46091---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc7f2a2d46091&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-python-to-solve-one-of-the-most-common-problems-in-engineering-c7f2a2d46091&source=-----c7f2a2d46091---------------------bookmark_footer-----------)![](../Images/d75a1ef4baf1f261a7f26902bc2698be.png)

由 [SpaceX](https://unsplash.com/@spacex?utm_source=medium&utm_medium=referral) 提供的照片，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

某些类型的问题在工程中经常出现。本文的重点是一个特定类型的问题，这种问题在我的日常工作中经常出现，因此我想分享一下我如何使用 Python 解决这个问题。我们在讨论什么类型的问题？就是解决系统工作点的问题！在深入到一些更复杂的代码示例之前，让我们通过一个简单的例子来说明我的意思。

我们希望求解下面简单电路的工作点。这可以通过重新排列欧姆定律（V=IR）来实现，从而将电流以已知的输入电压和电阻的形式隔离出来。

![](../Images/9af806798a24dee231c112156b56d79a.png)

图片来源：作者

简单，对吧？不幸的是，大多数现实世界的问题从来没有这么简单。例如，如果我告诉你，当电阻器加热时，它的电阻值会发生变化，从而使电阻成为电流的函数。我们会得到如下形式的方程：

在不知道电阻的实际函数形式的情况下，我们不能仅仅求解…
