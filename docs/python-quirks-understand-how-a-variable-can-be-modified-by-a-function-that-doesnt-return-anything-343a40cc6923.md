# Python 的奇特之处：理解一个变量如何被一个不返回任何东西的函数修改

> 原文：[`towardsdatascience.com/python-quirks-understand-how-a-variable-can-be-modified-by-a-function-that-doesnt-return-anything-343a40cc6923?source=collection_archive---------8-----------------------#2023-04-13`](https://towardsdatascience.com/python-quirks-understand-how-a-variable-can-be-modified-by-a-function-that-doesnt-return-anything-343a40cc6923?source=collection_archive---------8-----------------------#2023-04-13)

## 深入探讨 Python 如何传递参数和可变性，以防止意外的错误

[](https://mikehuls.medium.com/?source=post_page-----343a40cc6923--------------------------------)![Mike Huls](https://mikehuls.medium.com/?source=post_page-----343a40cc6923--------------------------------)[](https://towardsdatascience.com/?source=post_page-----343a40cc6923--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----343a40cc6923--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----343a40cc6923--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ffb62c607ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-quirks-understand-how-a-variable-can-be-modified-by-a-function-that-doesnt-return-anything-343a40cc6923&user=Mike+Huls&userId=7ffb62c607ee&source=post_page-7ffb62c607ee----343a40cc6923---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----343a40cc6923--------------------------------) · 8 分钟阅读 · 2023 年 4 月 13 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F343a40cc6923&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-quirks-understand-how-a-variable-can-be-modified-by-a-function-that-doesnt-return-anything-343a40cc6923&source=-----343a40cc6923---------------------bookmark_footer-----------)![](img/66a1b3b46f16c0c485439cbce8fc83b8.png)

追踪意外错误（图片由 [cottonbro studio](https://www.pexels.com/@cottonbro/) 提供，来源于 [Pexels](https://www.pexels.com/photo/photo-of-person-taking-down-notes-7319070/)）

在这篇文章中，我们将戴上侦探帽，解决一个“*Python 神秘*”。在这一集里，我们将了解**一个没有返回值的函数如何改变变量**。（请参见下面的例子）。这还不是全部：**它仅对某些类型的变量“有效”**。此外，很容易陷入这种行为的陷阱，因此了解其原因非常重要。

我们将专注于理解神秘背后的机制。更好地理解 Python 不仅会使你成为**更好的开发者**，还会**节省你解决难以理解的 bug 的大量挫折**。让我们开始编码吧！

# 神秘 — 一个例子

首先，让我们更深入地分析一下我们的“*Python 神秘*”：假设我们有两个函数，它们：

+   ***接受*** 变量

+   ***修改*** 变量

+   ***不要返回*** 变量

```py
def change_string(input_string:str) -> None:
    """ Notice that this functions doesn't return anything! """
    input_string += 'a'

def change_list(input_list:list) ->…
```
