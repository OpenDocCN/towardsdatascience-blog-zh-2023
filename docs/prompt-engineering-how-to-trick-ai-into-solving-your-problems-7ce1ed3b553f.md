# 提示工程：如何巧妙地让AI解决你的问题

> 原文：[https://towardsdatascience.com/prompt-engineering-how-to-trick-ai-into-solving-your-problems-7ce1ed3b553f?source=collection_archive---------1-----------------------#2023-08-25](https://towardsdatascience.com/prompt-engineering-how-to-trick-ai-into-solving-your-problems-7ce1ed3b553f?source=collection_archive---------1-----------------------#2023-08-25)

## 7个提示技巧、LangChain和Python示例代码

[](https://shawhin.medium.com/?source=post_page-----7ce1ed3b553f--------------------------------)[![Shaw Talebi](../Images/1449cc7c08890e2078f9e5d07897e3df.png)](https://shawhin.medium.com/?source=post_page-----7ce1ed3b553f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7ce1ed3b553f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7ce1ed3b553f--------------------------------) [Shaw Talebi](https://shawhin.medium.com/?source=post_page-----7ce1ed3b553f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff3998e1cd186&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-engineering-how-to-trick-ai-into-solving-your-problems-7ce1ed3b553f&user=Shaw+Talebi&userId=f3998e1cd186&source=post_page-f3998e1cd186----7ce1ed3b553f---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7ce1ed3b553f--------------------------------) ·14分钟阅读·2023年8月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7ce1ed3b553f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-engineering-how-to-trick-ai-into-solving-your-problems-7ce1ed3b553f&user=Shaw+Talebi&userId=f3998e1cd186&source=-----7ce1ed3b553f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7ce1ed3b553f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-engineering-how-to-trick-ai-into-solving-your-problems-7ce1ed3b553f&source=-----7ce1ed3b553f---------------------bookmark_footer-----------)

这是关于在实践中使用大型语言模型（LLMs）的[系列文章](/a-practical-introduction-to-llms-65194dda1148)中的第四篇。在这里，我将讨论提示工程（PE）以及如何使用它来构建基于LLM的应用程序。我将首先回顾一些关键的PE技术，然后通过Python示例代码演示如何使用LangChain构建LLM驱动的应用程序。

![](../Images/51cee05215f7e6b279687f028ed20dcc.png)

照片由[Jason Leung](https://unsplash.com/@ninjason?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

当第一次听到提示工程时，许多技术人员（包括我自己）往往对这一想法嗤之以鼻。我们可能会想，“*提示工程？哼，真无聊。告诉我如何从头开始构建一个LLM。*”

然而，在更深入研究之后，我会建议开发者不要自动忽视提示工程。我甚至可以进一步说，**提示工程能够实现大多数LLM使用案例的80%价值**，且（相对来说）投入的努力非常低。

我写这篇文章的目标是通过对提示工程的实际回顾和说明性示例来传达这一点。尽管提示工程确实存在一些局限性，但它为我们发现简单而聪明的解决方案打开了大门。
