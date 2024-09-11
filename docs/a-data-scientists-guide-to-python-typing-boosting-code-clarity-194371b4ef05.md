# 数据科学家的 Python 类型指南：提升代码清晰度

> 原文：[https://towardsdatascience.com/a-data-scientists-guide-to-python-typing-boosting-code-clarity-194371b4ef05?source=collection_archive---------4-----------------------#2023-07-31](https://towardsdatascience.com/a-data-scientists-guide-to-python-typing-boosting-code-clarity-194371b4ef05?source=collection_archive---------4-----------------------#2023-07-31)

## 类型的重要性以及如何在 Python 中实现

[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----194371b4ef05--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----194371b4ef05--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----194371b4ef05--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-scientists-guide-to-python-typing-boosting-code-clarity-194371b4ef05&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----194371b4ef05---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----194371b4ef05--------------------------------) ·4 分钟阅读·2023年7月31日

--

![](../Images/76d1ae8f42e8d81de522593a525b0fd6.png)

图片由 [Pankaj Patel](https://unsplash.com/@pankajpatel?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 什么是“类型”？

这里的“类型”指的不是我们物理上接触键盘的动作，而是我们在 Python 代码中变量（和函数）所采用的数据类型！

Python 本质上是一个[**动态语言**](https://en.wikipedia.org/wiki/Dynamic_programming_language)，这意味着没有正式的要求来声明我们的变量采用何种数据类型。例如，一个变量可能在开始时是整数，但在代码的其他地方可能会变成字符串。这种灵活性常常会导致在运行时出现错误，调试起来可能会很困难。

其他语言是[**静态类型**](https://www.techopedia.com/definition/22321/statically-typed#:~:text=Statically%20typed%20is%20a%20programming,with%20variables%2C%20not%20with%20values.)的，这意味着它们的变量类型需要显式声明，并且在运行时不能改变。如果一个变量被声明为整数，它必须在程序的整个运行过程中保持整数类型。静态类型语言的例子包括[**Fortran**](https://medium.com/towards-data-science/why-you-should-consider-using-fortran-as-a-data-scientist-5511e05ef89)和 C++。

然而，近年来 Python 已经发展出了对类型的支持，如今这已成为行业标准。这对于需要将稳健的机器学习模型部署到生产环境中的数据科学家尤其重要。

在这篇文章中，我想带你了解 Python 中类型的基本语法和过程，以及如何…
