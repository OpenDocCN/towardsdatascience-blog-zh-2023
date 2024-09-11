# 使用 GPT 和 Whisper 创建你的个性化语音助手

> 原文：[https://towardsdatascience.com/creating-your-personalized-voice-assistant-with-gpt-and-whisper-ddb9bd8c419?source=collection_archive---------1-----------------------#2023-05-18](https://towardsdatascience.com/creating-your-personalized-voice-assistant-with-gpt-and-whisper-ddb9bd8c419?source=collection_archive---------1-----------------------#2023-05-18)

## 步骤指南

[](https://donatoriccio.medium.com/?source=post_page-----ddb9bd8c419--------------------------------)[![Donato Riccio](../Images/0af2a026e72a023db4635522cbca50eb.png)](https://donatoriccio.medium.com/?source=post_page-----ddb9bd8c419--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ddb9bd8c419--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ddb9bd8c419--------------------------------) [Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----ddb9bd8c419--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe384fc71d292&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-your-personalized-voice-assistant-with-gpt-and-whisper-ddb9bd8c419&user=Donato+Riccio&userId=e384fc71d292&source=post_page-e384fc71d292----ddb9bd8c419---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ddb9bd8c419--------------------------------) · 6分钟阅读 · 2023年5月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fddb9bd8c419&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-your-personalized-voice-assistant-with-gpt-and-whisper-ddb9bd8c419&user=Donato+Riccio&userId=e384fc71d292&source=-----ddb9bd8c419---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fddb9bd8c419&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-your-personalized-voice-assistant-with-gpt-and-whisper-ddb9bd8c419&source=-----ddb9bd8c419---------------------bookmark_footer-----------)![](../Images/91c085fb9d42bb3ace220b9baec4f77a.png)

图片由 [Ivan Bandura](https://unsplash.com/@unstable_affliction?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文旨在指导你创建一个简单而强大的个性化语音助手。我们将使用两个强大的工具，Whisper 和 GPT，来实现这一目标。你可能已经了解 GPT 及其强大功能，但*Whisper 是什么？*

Whisper 是 OpenAI 提供的先进语音识别模型，能够提供准确的音频转文本服务。

我们将逐步为您介绍每个步骤，包括编码说明。最终，您将拥有自己的语音助手运行起来。

# 在您开始之前

## Open AI API 密钥

如果您已经有一个 OpenAI API 密钥，可以跳过本节。

Whisper 和 GPT API 都需要一个 OpenAI API 密钥才能访问。与 ChatGPT 不同，后者的订阅是固定费用，而 API 密钥的费用是根据您使用的服务量而定。

价格合理。截至目前，Whisper的价格为 $0.006 / 分钟，GPT（使用模型 gpt-3.5-turbo）为 $0.002 / 千个标记（一个标记约为 0.75 个单词）。
