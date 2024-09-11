# 别忘了 Python 是动态的！

> 原文：[`towardsdatascience.com/dont-forget-that-python-is-dynamic-e298e2a30118?source=collection_archive---------14-----------------------#2023-06-13`](https://towardsdatascience.com/dont-forget-that-python-is-dynamic-e298e2a30118?source=collection_archive---------14-----------------------#2023-06-13)

## PYTHON 编程

## 越来越多的静态和动态检查……这是我们希望 Python 发展的方向吗？

[](https://medium.com/@nyggus?source=post_page-----e298e2a30118--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----e298e2a30118--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e298e2a30118--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e298e2a30118--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----e298e2a30118--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdont-forget-that-python-is-dynamic-e298e2a30118&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----e298e2a30118---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e298e2a30118--------------------------------) ·8 分钟阅读·2023 年 6 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe298e2a30118&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdont-forget-that-python-is-dynamic-e298e2a30118&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----e298e2a30118---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe298e2a30118&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdont-forget-that-python-is-dynamic-e298e2a30118&source=-----e298e2a30118---------------------bookmark_footer-----------)![](img/6da0e0a508c5e05c453414dadb108c73.png)

Python 未来会走向何处？照片由 [Jamie Templeton](https://unsplash.com/@jamietempleton?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python 是一种动态语言。然而，近年来，越来越多的关注已转向静态类型检查。这反过来导致了对运行时类型检查的兴趣增加。我们会走多远呢？在这篇文章中，我们将回顾为什么 Python 曾经被认为是一个强大的动态类型编程语言。

现在仍然如此吗？

Python 的优势一直在于其简单性，这至少部分是由于动态类型的结果，不仅因为我们可以在 REPL 中编写 Python 代码，还有以下原因：

+   你可以在整个程序中轻松更改变量的类型。

+   你不需要定义变量的类型。

+   代码通常（或可以）容易阅读和理解。

+   有时候，你可以用几行代码来实现甚至相当复杂的算法。静态类型语言通常需要更多的——或者至少更多的——代码行数。

当然，动态类型并非没有代价。第一个代价是性能下降，这是我们都知道的。这种下降……
