# 参数与关键字参数：哪种方式在 Python 中调用函数最快？

> 原文：[https://towardsdatascience.com/args-vs-kwargs-which-is-the-fastest-way-to-call-a-function-in-python-afb2e817120?source=collection_archive---------9-----------------------#2023-02-06](https://towardsdatascience.com/args-vs-kwargs-which-is-the-fastest-way-to-call-a-function-in-python-afb2e817120?source=collection_archive---------9-----------------------#2023-02-06)

## `timeit` 模块的清晰演示

[](https://mikehuls.medium.com/?source=post_page-----afb2e817120--------------------------------)[![Mike Huls](../Images/8f9f55a0d25db00799c5d37383b7f5b6.png)](https://mikehuls.medium.com/?source=post_page-----afb2e817120--------------------------------)[](https://towardsdatascience.com/?source=post_page-----afb2e817120--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----afb2e817120--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----afb2e817120--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ffb62c607ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fargs-vs-kwargs-which-is-the-fastest-way-to-call-a-function-in-python-afb2e817120&user=Mike+Huls&userId=7ffb62c607ee&source=post_page-7ffb62c607ee----afb2e817120---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----afb2e817120--------------------------------) ·3 min read·2023年2月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fafb2e817120&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fargs-vs-kwargs-which-is-the-fastest-way-to-call-a-function-in-python-afb2e817120&user=Mike+Huls&userId=7ffb62c607ee&source=-----afb2e817120---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fafb2e817120&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fargs-vs-kwargs-which-is-the-fastest-way-to-call-a-function-in-python-afb2e817120&source=-----afb2e817120---------------------bookmark_footer-----------)![](../Images/f6dccb8745eef8fdb949b1245807f318.png)

时刻准备好对决（图片由作者提供）

你是否曾经想过，使用关键字调用函数是否比不使用关键字更慢？换句话说：哪种方式更快；**位置参数** (`myfunc('mike', 33)`) 还是 **kwargs** (`myfunc(name='mike', age=33)`)？

在这篇简短而简单的文章中，我们将探讨牺牲**关键字参数**的可读性与以**位置**传递参数之间是否值得。我们使用 `timeit` 模块设立基准并比较结果。没有什么复杂的；让我们开始编码吧！

# 函数

为了基准测试函数调用的性能，我们首先需要一个可以调用的函数。

```py
def the_func(arg1, arg2):
    pass
```

这个函数除了一个`pass`语句外什么也没有，这意味着该函数本身不执行任何操作。这确保了我们可以单独挑出并比较调用函数的方式（即按位置或使用 kwargs）。

[](/cython-for-absolute-beginners-30x-faster-code-in-two-simple-steps-bbb6c10d06ad?source=post_page-----afb2e817120--------------------------------) [## Cython 入门教程：通过两步简单的操作实现 30 倍更快的代码

### 轻松的 Python 代码编译，适用于极其快速的应用程序。

[towardsdatascience.com](/cython-for-absolute-beginners-30x-faster-code-in-two-simple-steps-bbb6c10d06ad?source=post_page-----afb2e817120--------------------------------)

# 基准测试脚本
