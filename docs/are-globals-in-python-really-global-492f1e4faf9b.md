# Python 中的全局变量真的是全局的吗？

> 原文：[https://towardsdatascience.com/are-globals-in-python-really-global-492f1e4faf9b?source=collection_archive---------10-----------------------#2023-11-17](https://towardsdatascience.com/are-globals-in-python-really-global-492f1e4faf9b?source=collection_archive---------10-----------------------#2023-11-17)

## PYTHON 编程

## 学习一个技巧，使得 Python 对象真正全局。

[](https://medium.com/@nyggus?source=post_page-----492f1e4faf9b--------------------------------)[![Marcin Kozak](../Images/d7faf62e48ed81dab5d8ad92819fff54.png)](https://medium.com/@nyggus?source=post_page-----492f1e4faf9b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----492f1e4faf9b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----492f1e4faf9b--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----492f1e4faf9b--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fare-globals-in-python-really-global-492f1e4faf9b&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----492f1e4faf9b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----492f1e4faf9b--------------------------------) ·8 分钟阅读·2023 年 11 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F492f1e4faf9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fare-globals-in-python-really-global-492f1e4faf9b&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----492f1e4faf9b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F492f1e4faf9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fare-globals-in-python-really-global-492f1e4faf9b&source=-----492f1e4faf9b---------------------bookmark_footer-----------)![](../Images/4f883bbad7e256509273e34303fb4715.png)

真正的全局意味着可以从任何地方访问。照片由[Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python 是否提供全局变量？

立即的回应是，确实如此。足以查看[Python 官方文档](https://docs.python.org/3/faq/programming.html#what-are-the-rules-for-local-and-global-variables-in-python)，了解到…

> 在 Python 中，仅在函数内部引用的变量被隐式视为全局的。如果在函数体内的任何地方为变量赋值，除非明确声明为全局，否则被认为是局部变量。

所以，Python 确实提供了全局变量。此外，全局变量是一个颇具争议的话题，因为使用它们可能会给开发者和用户带来严重的困难。

你可能会想，为什么我们要使用一个如此有争议的编程工具。这是一个合理的问题——但答案很简单。全局变量是编程工具中的一种，在正确使用的情况下可以非常有用。然而，如果使用不当，它们可能会弊大于利。

> 全局变量是编程工具中的一种，在正确使用的情况下可以非常有用。然而，如果使用不当，它们可能会弊大于利。
