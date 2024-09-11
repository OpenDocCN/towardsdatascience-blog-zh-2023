# 使用 LangChain 的输出解析器与 ChatGPT 结合以实现结构化输出

> 原文：[https://towardsdatascience.com/use-langchains-output-parser-with-chatgpt-for-structured-outputs-cf536f692685?source=collection_archive---------0-----------------------#2023-06-06](https://towardsdatascience.com/use-langchains-output-parser-with-chatgpt-for-structured-outputs-cf536f692685?source=collection_archive---------0-----------------------#2023-06-06)

## 通过示例用例进行解释。

[](https://sonery.medium.com/?source=post_page-----cf536f692685--------------------------------)[![Soner Yıldırım](../Images/c589572e9d1ee176cd4f5a0008173f1b.png)](https://sonery.medium.com/?source=post_page-----cf536f692685--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cf536f692685--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cf536f692685--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----cf536f692685--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-langchains-output-parser-with-chatgpt-for-structured-outputs-cf536f692685&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----cf536f692685---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cf536f692685--------------------------------) ·6分钟阅读·2023年6月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcf536f692685&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-langchains-output-parser-with-chatgpt-for-structured-outputs-cf536f692685&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----cf536f692685---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcf536f692685&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-langchains-output-parser-with-chatgpt-for-structured-outputs-cf536f692685&source=-----cf536f692685---------------------bookmark_footer-----------)![](../Images/54a0133f94d54e24a9aacf811a4d7d6b.png)

图片由 [Dmitry Ratushny](https://unsplash.com/@ratushny?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/wpi3sDUrSEk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供

ChatGPT 和许多其他 LLM（大型语言模型）在不同领域的 LLM 基于应用程序的创建方面发挥了领导作用。这些模型在处理文本输入和根据查询生成文本输出方面极其强大。然而，它们并非作为开发框架设计。

LangChain是一个开源开发框架，用于使用大型语言模型（LLMs）的应用程序。它以组件的形式提供抽象，以更高效或编程化的方式使用LLM。

这些组件包括：

+   模型：ChatGPT或其他LLM

+   提示：提示模板和输出解析器

+   索引：摄取外部数据，例如文档加载器和向量存储

+   链：组合组件以创建端到端的使用案例。一个简单链的示例可以是提示 + LLM + 输出解析器

+   代理：使LLM使用外部工具

LangChain的主要思想是将多个组件链在一起，以扩展LLM的能力，并创建更具功能性的工具或应用程序。
