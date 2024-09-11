# 如何在您的领域中使用大型语言模型（LLM）

> 原文：[`towardsdatascience.com/how-to-use-large-language-models-llm-in-your-own-domains-b4dff2d08464?source=collection_archive---------0-----------------------#2023-03-20`](https://towardsdatascience.com/how-to-use-large-language-models-llm-in-your-own-domains-b4dff2d08464?source=collection_archive---------0-----------------------#2023-03-20)

## 一些经典的、经过研究验证的技术，旨在将大型语言模型适配到特定领域的任务中，并解释这些技术为何有效。

[](https://eileen-code4fun.medium.com/?source=post_page-----b4dff2d08464--------------------------------)![Eileen Pangu](https://eileen-code4fun.medium.com/?source=post_page-----b4dff2d08464--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b4dff2d08464--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b4dff2d08464--------------------------------) [Eileen Pangu](https://eileen-code4fun.medium.com/?source=post_page-----b4dff2d08464--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F893d6b8a519f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-large-language-models-llm-in-your-own-domains-b4dff2d08464&user=Eileen+Pangu&userId=893d6b8a519f&source=post_page-893d6b8a519f----b4dff2d08464---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b4dff2d08464--------------------------------) · 9 分钟阅读 · 2023 年 3 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb4dff2d08464&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-large-language-models-llm-in-your-own-domains-b4dff2d08464&user=Eileen+Pangu&userId=893d6b8a519f&source=-----b4dff2d08464---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb4dff2d08464&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-large-language-models-llm-in-your-own-domains-b4dff2d08464&source=-----b4dff2d08464---------------------bookmark_footer-----------)![](img/692a8d93c1ea132c6b6c0f7ed0224264.png)

来源：[`unsplash.com/`](https://unsplash.com/)

自从 2022 年底 ChatGPT 推广以来，大型语言模型（LLM）引起了研究和工业界的极大关注。虽然通用聊天机器人是大型语言模型的一个显而易见的应用，企业们正在考虑如何将大型语言模型融入其业务工作流，以利用这一最新的 AI 进展。**不过，业务特定集成的一个基本前提是能够将大型语言模型适应于定制的业务领域**，因为 LLM 通常在开放的互联网信息上进行训练，这些信息中含有过多的噪音，并且不总是与特定业务背景紧密相关。

尽管已有许多优秀的博客文章详细讲述了大型语言模型本身，但似乎缺乏关于如何利用 LLM 的扎实介绍。在这篇博客文章中，我们将探讨一些来自近期研究文献的将 LLM 适应于领域特定任务的经典方法。我们的目标是激发一些灵感，实际实现 LLM 的民主化，使其对更广泛的世界开放。
