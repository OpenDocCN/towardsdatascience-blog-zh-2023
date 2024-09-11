# 什么是 GPT 模型背后的数据驱动 AI 概念？

> 原文：[`towardsdatascience.com/what-are-the-data-centric-ai-concepts-behind-gpt-models-a590071bb727?source=collection_archive---------2-----------------------#2023-03-29`](https://towardsdatascience.com/what-are-the-data-centric-ai-concepts-behind-gpt-models-a590071bb727?source=collection_archive---------2-----------------------#2023-03-29)

## 揭示在 ChatGPT 和 GPT-4 中使用的数据驱动 AI 技术

[](https://medium.com/@a0987284901?source=post_page-----a590071bb727--------------------------------)![Henry Lai](https://medium.com/@a0987284901?source=post_page-----a590071bb727--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a590071bb727--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a590071bb727--------------------------------) [Henry Lai](https://medium.com/@a0987284901?source=post_page-----a590071bb727--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd5548707b59&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-the-data-centric-ai-concepts-behind-gpt-models-a590071bb727&user=Henry+Lai&userId=d5548707b59&source=post_page-d5548707b59----a590071bb727---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a590071bb727--------------------------------) ·8 分钟阅读·2023 年 3 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa590071bb727&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-the-data-centric-ai-concepts-behind-gpt-models-a590071bb727&user=Henry+Lai&userId=d5548707b59&source=-----a590071bb727---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa590071bb727&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-the-data-centric-ai-concepts-behind-gpt-models-a590071bb727&source=-----a590071bb727---------------------bookmark_footer-----------)![](img/fa65e6643cb2712bc70bc825d8ba03a3.png)

[`arxiv.org/abs/2303.10158`](https://arxiv.org/abs/2303.10158)。图片由作者提供。

人工智能（AI）在改变我们生活、工作和与技术互动的方式上取得了令人难以置信的进步。最近，取得显著进展的一个领域是大型语言模型（LLMs）的发展，如 [GPT-3](https://arxiv.org/abs/2005.14165)、[ChatGPT](https://openai.com/blog/chatgpt) 和 [GPT-4](https://cdn.openai.com/papers/gpt-4.pdf)。这些模型能够以令人印象深刻的准确性执行语言翻译、文本总结和问答等任务。

尽管无法忽视 LLM 模型尺寸的不断增大，但也同样重要的是认识到它们的成功在很大程度上归功于用于训练它们的大量高质量数据。

在本文中，我们将从数据中心人工智能的角度概述 LLM 的最新进展，参考我们最近的调查论文[1,2]及[GitHub](https://github.com/daochenzha/data-centric-AI)上的相关技术资源。特别地，我们将通过[数据中心人工智能](https://github.com/daochenzha/data-centric-AI)的视角深入分析 GPT 模型，这是数据科学社区中一个不断发展的概念。我们将通过讨论三个数据中心人工智能目标： [训练数据开发、推理数据开发和数据维护](https://arxiv.org/abs/2303.10158)，来解读 GPT 模型背后的数据中心人工智能概念。

## 大型语言模型…
