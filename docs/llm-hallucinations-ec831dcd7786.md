# 理解 LLM 幻觉

> 原文：[`towardsdatascience.com/llm-hallucinations-ec831dcd7786?source=collection_archive---------4-----------------------#2023-05-08`](https://towardsdatascience.com/llm-hallucinations-ec831dcd7786?source=collection_archive---------4-----------------------#2023-05-08)

## 意见

## LLM 如何编造信息以及应对方法

[](https://franklyai.medium.com/?source=post_page-----ec831dcd7786--------------------------------)![Frank Neugebauer](https://franklyai.medium.com/?source=post_page-----ec831dcd7786--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ec831dcd7786--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec831dcd7786--------------------------------) [Frank Neugebauer](https://franklyai.medium.com/?source=post_page-----ec831dcd7786--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F29e3b5503cd1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-hallucinations-ec831dcd7786&user=Frank+Neugebauer&userId=29e3b5503cd1&source=post_page-29e3b5503cd1----ec831dcd7786---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec831dcd7786--------------------------------) · 6 分钟阅读 · 2023 年 5 月 8 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fec831dcd7786&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-hallucinations-ec831dcd7786&user=Frank+Neugebauer&userId=29e3b5503cd1&source=-----ec831dcd7786---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fec831dcd7786&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-hallucinations-ec831dcd7786&source=-----ec831dcd7786---------------------bookmark_footer-----------)![](img/95f952aadd9b08c63fbd537fcf27100e.png)

图片由 [Ahmad Dirini](https://unsplash.com/@ahmadirini?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 主要目标

使用大型语言模型存在一定风险，包括基于所谓的 LLM“幻觉”生成的回应。幻觉可能对 LLM 造成严重问题，因为它们可能导致虚假信息的传播、暴露机密信息，并且对 LLM 的能力产生不切实际的期望。理解幻觉并对其生成的信息持批判态度，有助于解释和缓解幻觉可能引发的问题。

# 什么是 LLM 幻觉？

LLMs（大语言模型）是一种人工智能（AI），它们在大量的文本和代码数据集上进行训练。它们可以生成文本、翻译语言、编写各种创意内容，并以信息性的方式回答问题。然而，LLMs 也容易出现“幻觉”，即生成的文本可能在事实性上不准确或毫无意义。正如经常提到的，“LLMs 可能充满了胡言乱语。”这种幻觉发生的原因是 LLMs 在训练时使用的数据通常是不完整或自相矛盾的。因此，它们可能会学会将某些词汇或短语与某些概念关联起来，即使这些关联…
