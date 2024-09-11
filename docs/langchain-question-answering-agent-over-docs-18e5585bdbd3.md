# 🦜🔗 LangChain：文档上的问答代理

> 原文：[`towardsdatascience.com/langchain-question-answering-agent-over-docs-18e5585bdbd3?source=collection_archive---------2-----------------------#2023-06-04`](https://towardsdatascience.com/langchain-question-answering-agent-over-docs-18e5585bdbd3?source=collection_archive---------2-----------------------#2023-06-04)

![](img/1c8d2ceae9a87852899c61a5f3be5e5c.png)

由 [Mike Alonzo](https://unsplash.com/@mikezo?utm_source=medium&utm_medium=referral) 拍摄，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 了解关于嵌入和代理的信息，以构建一个 QA 应用程序

[](https://medium.com/@marcellopoliti?source=post_page-----18e5585bdbd3--------------------------------)![Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----18e5585bdbd3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----18e5585bdbd3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----18e5585bdbd3--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----18e5585bdbd3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flangchain-question-answering-agent-over-docs-18e5585bdbd3&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----18e5585bdbd3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----18e5585bdbd3--------------------------------) ·6 分钟阅读·2023 年 6 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F18e5585bdbd3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flangchain-question-answering-agent-over-docs-18e5585bdbd3&user=Marcello+Politi&userId=7390355d40fe&source=-----18e5585bdbd3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F18e5585bdbd3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flangchain-question-answering-agent-over-docs-18e5585bdbd3&source=-----18e5585bdbd3---------------------bookmark_footer-----------)

## 介绍

自然语言处理领域中最常见的用例之一是**与文档相关的问答**。例如，想象一下将一个或多个 pdf 文件输入到机器中，然后提出与这些文件相关的问题。如果你需要准备大学考试并希望向机器询问你没有理解的内容，这将非常有用。实际上，更高级的用例是让机器向你提问，进行一种模拟审讯。

许多研究已经解决了这个任务，开发了许多工具，但今天我们可以利用如 OpenAI 的 GPT-3 等**大型语言模型（LLMs）**的强大功能。

LangChain 是一个非常新的库，允许我们管理和创建基于 LLM 的应用程序。事实上，**LLM 只是更复杂的人工智能架构的一部分**。当我们创建这样的系统时，我们不仅仅要向 OpenAI 模型提出查询并获取响应，还需要例如保存这个响应、正确构建提示等。

在本文中，我们将看到如何使用 LangChain 构建一个简单的文档问答应用程序以及……
