# OpenAI 函数调用在数据管道中的强大功能：综合指南

> 原文：[https://towardsdatascience.com/the-power-of-openais-function-calling-in-language-learning-models-a-comprehensive-guide-cce8cd84dc3c?source=collection_archive---------1-----------------------#2023-06-22](https://towardsdatascience.com/the-power-of-openais-function-calling-in-language-learning-models-a-comprehensive-guide-cce8cd84dc3c?source=collection_archive---------1-----------------------#2023-06-22)

## 使用 OpenAI 的函数调用功能转变数据管道：利用 PostgreSQL 和 FastAPI 实现电子邮件发送工作流

[](https://medium.com/@luisroque?source=post_page-----cce8cd84dc3c--------------------------------)[![Luís Roque](../Images/e281d470b403375ba3c6f521b1ccf915.png)](https://medium.com/@luisroque?source=post_page-----cce8cd84dc3c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cce8cd84dc3c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cce8cd84dc3c--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----cce8cd84dc3c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-openais-function-calling-in-language-learning-models-a-comprehensive-guide-cce8cd84dc3c&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----cce8cd84dc3c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cce8cd84dc3c--------------------------------) ·11 分钟阅读·2023年6月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcce8cd84dc3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-openais-function-calling-in-language-learning-models-a-comprehensive-guide-cce8cd84dc3c&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----cce8cd84dc3c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcce8cd84dc3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-openais-function-calling-in-language-learning-models-a-comprehensive-guide-cce8cd84dc3c&source=-----cce8cd84dc3c---------------------bookmark_footer-----------)

# 介绍

人工智能的激动人心的世界通过 OpenAI 最新的大型语言模型（LLMs）引入的函数调用功能又迈出了新的一步。这个新功能增强了人类与 AI 之间的互动，将其从简单的问答格式转变为更具动态性和互动性的对话。

那么，这些函数调用能力究竟是什么？从本质上讲，它们允许 LLM 在对话过程中根据输入指令调用预定义的函数。这可以是任何操作，从发送电子邮件到根据对话上下文从数据库中提取数据。

使用函数调用能力的好处和应用广泛。这显著提高了 AI 在各种应用中的动态实用性，从客户服务到我们构建数据管道的方式。

想象一个客户服务 AI，它可以回答问题并执行诸如预约、向电子邮件地址发送信息或实时更新客户详细信息等操作。或者考虑一个数据管道，其中 LLM 代理可以根据命令提取、更新和操作数据。
