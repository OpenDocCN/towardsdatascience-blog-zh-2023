# Python Dictcomp 管道示例

> 原文：[`towardsdatascience.com/python-dictcomp-pipelines-in-examples-4c4ec4e66f3?source=collection_archive---------10-----------------------#2023-04-20`](https://towardsdatascience.com/python-dictcomp-pipelines-in-examples-4c4ec4e66f3?source=collection_archive---------10-----------------------#2023-04-20)

## PYTHON 编程

## 见识一下 dictcomp 管道的强大功能

![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----4c4ec4e66f3--------------------------------) [](https://towardsdatascience.com/?source=post_page-----4c4ec4e66f3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4c4ec4e66f3--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----4c4ec4e66f3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-dictcomp-pipelines-in-examples-4c4ec4e66f3&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----4c4ec4e66f3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4c4ec4e66f3--------------------------------) · 11 分钟阅读 · 2023 年 4 月 20 日 [点赞](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4c4ec4e66f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-dictcomp-pipelines-in-examples-4c4ec4e66f3&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----4c4ec4e66f3---------------------clap_footer-----------)

--

![](img/2d7b967369a43cb84c51293abe58fee0.png)

管道一个接一个地处理任务。照片由 [Daniel Schludi](https://unsplash.com/fr/@schluditsch?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文的灵感来自于我几年前在实际项目中参与的一个任务。在 提出理解管道的概念 后，我发现可以使用 dictcomp 管道很好的实现这个解决方案，并且借助我在 [另一篇文章](https://medium.com/pythoniq/an-optionalbool-type-for-python-none-false-or-true-7667e9cc6dd8) 中提出的 `OptionalBool` 数据结构。

本文旨在展示如何实现这样的管道。我将深入一些细节，以便代码变得清晰和令人信服。你可以将其视为展示 dictcomp 管道实现的案例研究。

我们已经讨论了生成器管道在 Python 中的强大功能：

## 构建 Python 中的生成器管道

### 本文提出了一种优雅的构建生成器管道的方法。

towardsdatascience.com

后来，我提出了一个针对 Python 的特定概念——理解管道（comprehension pipelines）：
