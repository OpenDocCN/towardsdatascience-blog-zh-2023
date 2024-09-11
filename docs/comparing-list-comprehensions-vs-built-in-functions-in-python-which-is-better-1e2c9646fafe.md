# 比较 Python 中的列表推导式与内置函数：哪个更好？

> 原文：[https://towardsdatascience.com/comparing-list-comprehensions-vs-built-in-functions-in-python-which-is-better-1e2c9646fafe?source=collection_archive---------2-----------------------#2023-03-21](https://towardsdatascience.com/comparing-list-comprehensions-vs-built-in-functions-in-python-which-is-better-1e2c9646fafe?source=collection_archive---------2-----------------------#2023-03-21)

## 对语法、可读性和性能的深入分析

[](https://thomasdorfer.medium.com/?source=post_page-----1e2c9646fafe--------------------------------)[![托马斯·A·多费尔](../Images/9258a1735cee805f1d9b02e2adf01096.png)](https://thomasdorfer.medium.com/?source=post_page-----1e2c9646fafe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1e2c9646fafe--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1e2c9646fafe--------------------------------) [托马斯·A·多费尔](https://thomasdorfer.medium.com/?source=post_page-----1e2c9646fafe--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7c54f9b62b90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomparing-list-comprehensions-vs-built-in-functions-in-python-which-is-better-1e2c9646fafe&user=Thomas+A+Dorfer&userId=7c54f9b62b90&source=post_page-7c54f9b62b90----1e2c9646fafe---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1e2c9646fafe--------------------------------) · 9 分钟阅读 · 2023年3月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1e2c9646fafe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomparing-list-comprehensions-vs-built-in-functions-in-python-which-is-better-1e2c9646fafe&user=Thomas+A+Dorfer&userId=7c54f9b62b90&source=-----1e2c9646fafe---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1e2c9646fafe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomparing-list-comprehensions-vs-built-in-functions-in-python-which-is-better-1e2c9646fafe&source=-----1e2c9646fafe---------------------bookmark_footer-----------)![](../Images/24a86e8397f03f62c53d4cf68e844fb2.png)

图片由作者提供。

你是否曾在一个雨天翻阅 Netflix，面对无尽的电影和节目选择感到不知所措？

在编程中，选择的悖论同样让人感到难以承受。由于有这么多的库和框架提供实现相同目标的无数种方式，很容易在选择的海洋中迷失。

在 Python 中，这种情况通常发生在程序员需要在**函数式编程**方法（如内置函数 `map()`、`filter()` 和 `reduce()`）和更 Pythonic 的**列表推导式**之间进行选择时。

在本文中，我们将通过语法、可读性和性能的角度探讨这些不同方法的优缺点。

# 列表推导式

在 Python 中，列表推导式是一种简洁的方法，它基于已有的列表生成一个新列表。简单来说，它本质上是一个**for 循环**的一行代码，并且可以在末尾包含一个**if 条件**。语法可以分解如下：
