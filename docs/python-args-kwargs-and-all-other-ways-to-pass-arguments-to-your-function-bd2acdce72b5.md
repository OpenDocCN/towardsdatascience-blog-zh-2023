# Python 的 `args`、`kwargs` 以及传递参数给函数的所有其他方式

> 原文：[https://towardsdatascience.com/python-args-kwargs-and-all-other-ways-to-pass-arguments-to-your-function-bd2acdce72b5?source=collection_archive---------8-----------------------#2023-03-07](https://towardsdatascience.com/python-args-kwargs-and-all-other-ways-to-pass-arguments-to-your-function-bd2acdce72b5?source=collection_archive---------8-----------------------#2023-03-07)

## 在**6个例子**中巧妙地设计你的函数参数

[](https://mikehuls.medium.com/?source=post_page-----bd2acdce72b5--------------------------------)[![Mike Huls](../Images/8f9f55a0d25db00799c5d37383b7f5b6.png)](https://mikehuls.medium.com/?source=post_page-----bd2acdce72b5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bd2acdce72b5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bd2acdce72b5--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----bd2acdce72b5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ffb62c607ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-args-kwargs-and-all-other-ways-to-pass-arguments-to-your-function-bd2acdce72b5&user=Mike+Huls&userId=7ffb62c607ee&source=post_page-7ffb62c607ee----bd2acdce72b5---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd2acdce72b5--------------------------------) ·8分钟阅读·2023年3月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbd2acdce72b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-args-kwargs-and-all-other-ways-to-pass-arguments-to-your-function-bd2acdce72b5&user=Mike+Huls&userId=7ffb62c607ee&source=-----bd2acdce72b5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbd2acdce72b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-args-kwargs-and-all-other-ways-to-pass-arguments-to-your-function-bd2acdce72b5&source=-----bd2acdce72b5---------------------bookmark_footer-----------)![](../Images/232d65cc44f26ade0195ce94e644d795.png)

斜杠和星号的作用是什么？（图片由作者提供）

本文深入探讨了函数参数的设计。我们将了解 `*args` 和 `**kwargs` 的作用，`/` 和 `*` 的功能，以及如何以最佳方式设计函数参数。具有良好设计的函数参数更容易被其他开发者理解和使用。本文探讨了**6个问题**，展示了**你需要了解的一切**，以**成为参数专家**。让我们开始编程吧！

# 准备：定义和传递参数

在这一部分，我们将快速浏览术语以及 Python 提供的处理将参数传递给函数的所有方法。

[](/six-levels-of-python-decorators-1f12c9067b23?source=post_page-----bd2acdce72b5--------------------------------) [## 理解 Python 装饰器：从初学者到专家的六个层次]

### 装饰器是如何工作的，何时使用它们，以及 6 个逐渐复杂的示例

[towardsdatascience.com](/six-levels-of-python-decorators-1f12c9067b23?source=post_page-----bd2acdce72b5--------------------------------)

## 参数和实参之间有什么区别？

许多人将这些术语互换使用，但它们之间存在差异。参数会被初始化为由参数提供的值：
