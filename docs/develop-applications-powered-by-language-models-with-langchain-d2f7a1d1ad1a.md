# 🦜🔗 LangChain：开发由语言模型驱动的应用程序

> 原文：[https://towardsdatascience.com/develop-applications-powered-by-language-models-with-langchain-d2f7a1d1ad1a?source=collection_archive---------4-----------------------#2023-04-26](https://towardsdatascience.com/develop-applications-powered-by-language-models-with-langchain-d2f7a1d1ad1a?source=collection_archive---------4-----------------------#2023-04-26)

![](../Images/eadb36548cf05e78b4161169a551f8aa.png)

图片由[Choong Deng Xiang](https://unsplash.com/@dengxiangs?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 开始使用Python中的LangChain来利用LLMs

[](https://medium.com/@marcellopoliti?source=post_page-----d2f7a1d1ad1a--------------------------------)[![Marcello Politi](../Images/484e44571bd2e75acfe5fef3146ab3c2.png)](https://medium.com/@marcellopoliti?source=post_page-----d2f7a1d1ad1a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d2f7a1d1ad1a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d2f7a1d1ad1a--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----d2f7a1d1ad1a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdevelop-applications-powered-by-language-models-with-langchain-d2f7a1d1ad1a&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----d2f7a1d1ad1a---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d2f7a1d1ad1a--------------------------------) ·12分钟阅读·2023年4月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd2f7a1d1ad1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdevelop-applications-powered-by-language-models-with-langchain-d2f7a1d1ad1a&user=Marcello+Politi&userId=7390355d40fe&source=-----d2f7a1d1ad1a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd2f7a1d1ad1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdevelop-applications-powered-by-language-models-with-langchain-d2f7a1d1ad1a&source=-----d2f7a1d1ad1a---------------------bookmark_footer-----------)

# 介绍

**LangChain是一个框架，使得快速和轻松地开发利用大型语言模型（如GPT-3）的应用程序成为可能**。

这个框架确实引入了额外的可能性，例如，轻松使用外部数据源，如维基百科，以增强模型提供的能力。我相信你们可能都尝试过使用Chat-GPT，并发现它在回答发生在某个日期之后的事件时会出现问题。在这种情况下，搜索维基百科可以帮助GPT回答更多问题。

# LangChain 结构

该框架组织成六个模块，每个模块允许你管理与LLM交互的不同方面。让我们看看这些模块是什么。

+   [**模型**](https://python.langchain.com/en/latest/modules/models.html)：允许你实例化和使用不同的模型。

+   [**提示**](https://python.langchain.com/en/latest/modules/prompts.html)：提示是我们与模型交互以尝试获得输出的方式。目前，了解如何编写有效的提示至关重要。这个框架模块允许我们更好地管理提示。例如，通过创建我们可以重复使用的模板。
