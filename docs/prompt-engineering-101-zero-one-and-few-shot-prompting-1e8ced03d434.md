# 提示工程101：零次、一次和少次提示

> 原文：[https://towardsdatascience.com/prompt-engineering-101-zero-one-and-few-shot-prompting-1e8ced03d434?source=collection_archive---------1-----------------------#2023-09-19](https://towardsdatascience.com/prompt-engineering-101-zero-one-and-few-shot-prompting-1e8ced03d434?source=collection_archive---------1-----------------------#2023-09-19)

## 基本提示工程策略简介

[](https://medium.com/@aashishnair?source=post_page-----1e8ced03d434--------------------------------)[![Aashish Nair](../Images/23f4b3839e464419332b690a4098d824.png)](https://medium.com/@aashishnair?source=post_page-----1e8ced03d434--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1e8ced03d434--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1e8ced03d434--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----1e8ced03d434--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-engineering-101-zero-one-and-few-shot-prompting-1e8ced03d434&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----1e8ced03d434---------------------post_header-----------) 发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1e8ced03d434--------------------------------) ·5分钟阅读·2023年9月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1e8ced03d434&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-engineering-101-zero-one-and-few-shot-prompting-1e8ced03d434&user=Aashish+Nair&userId=3087ba81e065&source=-----1e8ced03d434---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1e8ced03d434&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-engineering-101-zero-one-and-few-shot-prompting-1e8ced03d434&source=-----1e8ced03d434---------------------bookmark_footer-----------)![](../Images/dc425a1abba413deb8fc810c761d1831.png)

图片由[Alexandra_Koch](https://pixabay.com/users/alexandra_koch-621802/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7720802)提供，来自[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7720802)

## 介绍

尽管它们看似具有超自然的能力，但LLM（大型语言模型）最终仍然是预测模型，仅根据提供的上下文预测序列中的下一个词。

因此，它们的表现不仅仅依赖于它们接受过的大量数据；它们还严重依赖于用户输入中提供的上下文。

频繁使用LLM驱动聊天机器人的用户知道上下文的重要性。没有足够的上下文，无论是公开服务（例如ChatGPT）还是定制的LLM产品，聊天机器人都难以执行更复杂的指令。

在这里，我们将深入探讨引导大型语言模型（LLMs）正确回答提示的最基本策略之一：在用户提示中提供上下文。这通常通过三种不同的方法进行：零-shot 提示、一-shot 提示和少-shot 提示。

## 零-Shot 提示

如果你之前与一个由LLM驱动的聊天机器人互动过，你很可能已经不自觉地使用了零-shot 提示。零-shot 提示涉及仅依靠LLM的预训练信息……
