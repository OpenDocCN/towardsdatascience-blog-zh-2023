# 教授语言模型使用工具

> 原文：[`towardsdatascience.com/teaching-language-models-to-use-tools-7fd58916c66b?source=collection_archive---------1-----------------------#2023-08-27`](https://towardsdatascience.com/teaching-language-models-to-use-tools-7fd58916c66b?source=collection_archive---------1-----------------------#2023-08-27)

## 使用工具让我们作为人类变得更有能力。这对大语言模型（LLMs）是否也一样？

[](https://wolfecameron.medium.com/?source=post_page-----7fd58916c66b--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----7fd58916c66b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7fd58916c66b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7fd58916c66b--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----7fd58916c66b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fteaching-language-models-to-use-tools-7fd58916c66b&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----7fd58916c66b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7fd58916c66b--------------------------------) · 17 分钟阅读 · 2023 年 8 月 27 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7fd58916c66b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fteaching-language-models-to-use-tools-7fd58916c66b&source=-----7fd58916c66b---------------------bookmark_footer-----------)![](img/72793c5d61d43b3d37b876b47eba27a0.png)

（图片来源：[Barn Images](https://unsplash.com/@barnimages?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 于 [Unsplash](https://unsplash.com/photos/t5YUoHW6zRo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

随着我们对大型语言模型（LLMs）的了解越来越多，它们变得越来越有趣。这些模型可以准确解决各种复杂任务。然而，它们在某些我们认为基本的功能上却表现不佳！例如，LLMs 通常会出现算术错误，缺乏当前信息的访问，甚至难以理解时间的进程。鉴于这些限制，我们可能会想知道如何使 LLMs 更具能力。*LLMs 是否注定要永远承受这些限制？*

人类的许多进步都受到新型和创新工具（例如，[印刷机](https://www.history.com/news/printing-press-renaissance)或[计算机](https://www.youtube.com/watch?v=L40B08nWoMk)）的催化。*相同的发现是否适用于 LLMs？* 在这个概述中，我们将深入研究一个新的研究方向，旨在教会 LLMs 如何使用外部工具，这些工具通过简单的文本到文本 API 提供。利用这些工具，LLMs 可以将执行算术运算或查找当前信息等任务委托给专门的工具。然后，这些工具返回的信息可以作为 LLM 生成输出时的上下文，从而导致更准确且有根据的回答。
