# Python 类型：Optional 可以意味着强制

> 原文：[https://towardsdatascience.com/python-types-optional-can-mean-mandatory-8e3b7ac2e805?source=collection_archive---------5-----------------------#2023-11-21](https://towardsdatascience.com/python-types-optional-can-mean-mandatory-8e3b7ac2e805?source=collection_archive---------5-----------------------#2023-11-21)

## PYTHON 编程

## 学习如何避免对 typing.Optional 的频繁误用和误解。

[](https://medium.com/@nyggus?source=post_page-----8e3b7ac2e805--------------------------------)[![Marcin Kozak](../Images/d7faf62e48ed81dab5d8ad92819fff54.png)](https://medium.com/@nyggus?source=post_page-----8e3b7ac2e805--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8e3b7ac2e805--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8e3b7ac2e805--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----8e3b7ac2e805--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-types-optional-can-mean-mandatory-8e3b7ac2e805&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----8e3b7ac2e805---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8e3b7ac2e805--------------------------------) ·8 分钟阅读·2023 年 11 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8e3b7ac2e805&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-types-optional-can-mean-mandatory-8e3b7ac2e805&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----8e3b7ac2e805---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8e3b7ac2e805&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-types-optional-can-mean-mandatory-8e3b7ac2e805&source=-----8e3b7ac2e805---------------------bookmark_footer-----------)![](../Images/7d449c5d156e16fcbe145e74f261329c.png)

照片由 [Caroline Hall](https://unsplash.com/@notcarolyn?utm_source=medium&utm_medium=referral) 拍摄，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

根据 [Python 文档](https://docs.python.org/3/library/typing.html#typing.Optional)，`typing.Optional` 是指示对象可以为 `None` 的便捷方式。这是表达这个概念的简洁而优雅的方式，但是否也清晰明了呢？

让我换个方式来提问：当你在 Python 环境中看到“可选”这个词时，你认为它是什么意思？假设你看到一个名为 `x` 的参数，其类型为 `Optional[int]`。`int` 部分相当明确，因为它很可能表示一个整数，但 `Optional` 是什么意思？你的第一反应是什么？

让我们考虑以下两个选项：

1.  我不需要提供 `x` 的值，因为它是可选的。

1.  `x` 的值可以是 `int` 或 `None`。

如果你对 Python 类型提示了解得足够多，你会知道选项 2 是正确的。但当你不了解时…也许我错了，但我无法想象一个不知道 Python 的人会选择选项 2。实际上，选项 1 似乎更有意义。当我看到信息说某个东西是可选的，我的想法是…嗯，它确实是可选的…

这个问题导致了对`typing.Optional`类型的频繁误用。本文旨在…
