# Python 断言，或检查一只猫是否是狗

> 原文：[https://towardsdatascience.com/python-assertions-or-checking-if-a-cat-is-a-dog-ce11c55d143?source=collection_archive---------6-----------------------#2023-03-02](https://towardsdatascience.com/python-assertions-or-checking-if-a-cat-is-a-dog-ce11c55d143?source=collection_archive---------6-----------------------#2023-03-02)

## PYTHON 编程

## 了解在 Python 中使用断言的规则——以及不使用它们的规则

[](https://medium.com/@nyggus?source=post_page-----ce11c55d143--------------------------------)[![Marcin Kozak](../Images/d7faf62e48ed81dab5d8ad92819fff54.png)](https://medium.com/@nyggus?source=post_page-----ce11c55d143--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ce11c55d143--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ce11c55d143--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----ce11c55d143--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-assertions-or-checking-if-a-cat-is-a-dog-ce11c55d143&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----ce11c55d143---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce11c55d143--------------------------------) ·13 分钟阅读·2023年3月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fce11c55d143&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-assertions-or-checking-if-a-cat-is-a-dog-ce11c55d143&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----ce11c55d143---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fce11c55d143&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-assertions-or-checking-if-a-cat-is-a-dog-ce11c55d143&source=-----ce11c55d143---------------------bookmark_footer-----------)![](../Images/84e60e7f48e20b9ca7e0acd560d557f3.png)

错误的断言应该让你停下来：有问题了！照片由[Jose Aragones](https://unsplash.com/@jodaarba?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

断言是你用来测试对程序的假设的语句。这个简短的定义一方面很清晰，另一方面却远未解释你何时应该使用断言。

`assert` 语句是 Python 中主要的断言工具，它与内置的 `__debug__` 对象紧密相关。在我学习 Python 的某个阶段，我对这个对象并不知情，所以我猜许多数据科学家和 Python 开发者也不知道它。在阅读完这篇文章后，你将知道如何使用 `__debug__` 和断言——以及如何 *不* 使用它们。

你会在测试中找到主要的断言位置。无论你使用什么测试框架，它都使用断言。虽然 `unittest` 使用专门的方法来进行特定类型的断言（如 `.AssertTrue()`、`.AssertFalse()`、`.AssertEqual()`），`pytest` 更倾向于使用裸 `assert` 语句。我个人喜欢后者的简单性。如果你想断言 `x` 是 10，你可以用这种简单的方法来实现：

```py
assert x == 10
```

当你想断言 `x` 是一个整数时，可以如下进行：
