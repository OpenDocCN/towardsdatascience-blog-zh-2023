# 构建你的第一个 LLM 应用程序所需了解的一切

> 原文：[`towardsdatascience.com/all-you-need-to-know-to-build-your-first-llm-app-eb982c78ffac?source=collection_archive---------0-----------------------#2023-06-22`](https://towardsdatascience.com/all-you-need-to-know-to-build-your-first-llm-app-eb982c78ffac?source=collection_archive---------0-----------------------#2023-06-22)

## 关于文档加载器、嵌入、向量存储和提示模板的逐步教程

[](https://dmnkplzr.medium.com/?source=post_page-----eb982c78ffac--------------------------------)![Dominik Polzer](https://dmnkplzr.medium.com/?source=post_page-----eb982c78ffac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eb982c78ffac--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb982c78ffac--------------------------------) [Dominik Polzer](https://dmnkplzr.medium.com/?source=post_page-----eb982c78ffac--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3ab8d3143e32&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fall-you-need-to-know-to-build-your-first-llm-app-eb982c78ffac&user=Dominik+Polzer&userId=3ab8d3143e32&source=post_page-3ab8d3143e32----eb982c78ffac---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb982c78ffac--------------------------------) ·26 分钟阅读·2023 年 6 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feb982c78ffac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fall-you-need-to-know-to-build-your-first-llm-app-eb982c78ffac&user=Dominik+Polzer&userId=3ab8d3143e32&source=-----eb982c78ffac---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feb982c78ffac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fall-you-need-to-know-to-build-your-first-llm-app-eb982c78ffac&source=-----eb982c78ffac---------------------bookmark_footer-----------)![](img/215dfd8fde2517ee28b96285604db80a.png)

构建自己的聊天机器人与上下文注入 — 作者提供的图片

## 目录

> 如果你只是在寻找一个简短的教程来说明如何构建一个简单的 LLM 应用程序，你可以跳到“6. 创建向量存储”部分，在那里你可以找到构建一个包含向量存储、提示模板和 LLM 调用的极简 LLM 应用程序所需的所有代码片段。

**简介**

我们为何需要 LLM

微调与上下文注入

什么是 LangChain？

**逐步教程**

1. 使用 LangChain 加载文档

2. 将文档拆分成文本块

3. 从文本块到嵌入

4. 定义你想使用的 LLM

5. 定义我们的提示模板

6. 创建一个向量存储

![](img/abb5a66e45437849d1939f663604994d.png)

目录

# 我们为什么需要 LLMs
