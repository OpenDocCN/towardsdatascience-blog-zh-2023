# 使用Rust扩展提升你的Python代码

> 原文：[https://towardsdatascience.com/boost-your-python-code-with-rust-extensions-108afdbd4e13?source=collection_archive---------6-----------------------#2023-07-14](https://towardsdatascience.com/boost-your-python-code-with-rust-extensions-108afdbd4e13?source=collection_archive---------6-----------------------#2023-07-14)

## 通过几个数量级的提升速度并增强代码安全性

[](https://medium.com/@nic-obert?source=post_page-----108afdbd4e13--------------------------------)[![Nicholas Obert](../Images/d70330063c9edc2f63e53f62a78f82ec.png)](https://medium.com/@nic-obert?source=post_page-----108afdbd4e13--------------------------------)[](https://towardsdatascience.com/?source=post_page-----108afdbd4e13--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----108afdbd4e13--------------------------------) [Nicholas Obert](https://medium.com/@nic-obert?source=post_page-----108afdbd4e13--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Feeb884cbf705&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fboost-your-python-code-with-rust-extensions-108afdbd4e13&user=Nicholas+Obert&userId=eeb884cbf705&source=post_page-eeb884cbf705----108afdbd4e13---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----108afdbd4e13--------------------------------) ·6分钟阅读·2023年7月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F108afdbd4e13&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fboost-your-python-code-with-rust-extensions-108afdbd4e13&user=Nicholas+Obert&userId=eeb884cbf705&source=-----108afdbd4e13---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F108afdbd4e13&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fboost-your-python-code-with-rust-extensions-108afdbd4e13&source=-----108afdbd4e13---------------------bookmark_footer-----------)![](../Images/ec001b47bea181b0429a89366a8512e8.png)

图片由作者提供，使用DALLE生成

众所周知，Python是一种通用编程语言，优化了简洁性和易用性。尽管它在处理轻量任务时表现出色，但代码执行速度很快就可能成为程序中的一个主要瓶颈。

在这篇文章中，我们将探讨为什么Python在与其他编程语言相比时速度如此缓慢。接着，我们将学习如何为Python编写一个基本的Rust扩展，并将其性能与原生Python实现进行比较。

## 为什么Python会慢

在我们开始之前，我想指出，编程语言本身并没有快慢之分：其实现方式才是关键。如果你想了解语言与其实现之间的区别，可以查看这篇文章：

[https://betterprogramming.pub/the-fastest-programming-language-myth-65eaaf7e5d1a?source=post_page-----108afdbd4e13--------------------------------](https://betterprogramming.pub/the-fastest-programming-language-myth-65eaaf7e5d1a?source=post_page-----108afdbd4e13--------------------------------) [## 速度最快的编程语言神话

### 一种需要消除的常见编程误解

[https://betterprogramming.pub/the-fastest-programming-language-myth-65eaaf7e5d1a?source=post_page-----108afdbd4e13--------------------------------](https://betterprogramming.pub/the-fastest-programming-language-myth-65eaaf7e5d1a?source=post_page-----108afdbd4e13--------------------------------)

首先，Python 是动态类型的，这意味着变量的类型仅在运行时确定，而不是在编译时。这种设计选择虽然允许代码更灵活，但 Python…
