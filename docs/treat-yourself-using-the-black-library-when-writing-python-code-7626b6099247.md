# 在编写 Python 代码时使用“Black”库来提升自己

> 原文：[`towardsdatascience.com/treat-yourself-using-the-black-library-when-writing-python-code-7626b6099247?source=collection_archive---------5-----------------------#2023-01-13`](https://towardsdatascience.com/treat-yourself-using-the-black-library-when-writing-python-code-7626b6099247?source=collection_archive---------5-----------------------#2023-01-13)

![](img/fa1ff0ffea5f82fa2379a98b3a29563a.png)

图片来自 [Pfüderi](https://pixabay.com/users/pf%C3%BCderi-199315/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2125148) [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2125148)

## 获取“正确格式的单一”代码的最简单方法

[](https://christophertao.medium.com/?source=post_page-----7626b6099247--------------------------------)![Christopher Tao](https://christophertao.medium.com/?source=post_page-----7626b6099247--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7626b6099247--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7626b6099247--------------------------------) [Christopher Tao](https://christophertao.medium.com/?source=post_page-----7626b6099247--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb8176fabf308&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftreat-yourself-using-the-black-library-when-writing-python-code-7626b6099247&user=Christopher+Tao&userId=b8176fabf308&source=post_page-b8176fabf308----7626b6099247---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7626b6099247--------------------------------) ·6 分钟阅读·2023 年 1 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7626b6099247&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftreat-yourself-using-the-black-library-when-writing-python-code-7626b6099247&user=Christopher+Tao&userId=b8176fabf308&source=-----7626b6099247---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7626b6099247&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftreat-yourself-using-the-black-library-when-writing-python-code-7626b6099247&source=-----7626b6099247---------------------bookmark_footer-----------)

你是否曾从库文档或 Stack Overflow 复制粘贴过一些示例代码？或者你可能没有纯粹的编程背景，因此没有接受过编写格式正确代码的训练？你是否希望提高代码的可读性，以给他人留下深刻的印象？

Python 编程语言的一个优点是它使用缩进来指示代码块的嵌套级别。因此，从格式的角度来看，编写一些“难以阅读”的代码相对较难。然而，这并不意味着所有的 Python 代码都必须进行格式化。没有任何编程语言能够施加如此多的限制来强迫用户编写格式化代码，我认为这不是一个好主意，因为这将使使用变得过于困难。

在这篇文章中，我将介绍一个名为“Black”的 Python 库。它可以帮助我们以不同的方式格式化代码。所有的样式将遵循 PEP 8 指南。让我们开始创建一致且易读的格式化代码吧！
