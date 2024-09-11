# ChatGPT的工作原理：聊天机器人的模型

> 原文：[https://towardsdatascience.com/how-chatgpt-works-the-models-behind-the-bot-1ce5fca96286?source=collection_archive---------0-----------------------#2023-01-30](https://towardsdatascience.com/how-chatgpt-works-the-models-behind-the-bot-1ce5fca96286?source=collection_archive---------0-----------------------#2023-01-30)

## 对你不断听说的聊天机器人背后的直觉和方法的简要介绍。

[](https://medium.com/@molly.ruby?source=post_page-----1ce5fca96286--------------------------------)[![Molly Ruby](../Images/2a493bd01057722138857a90035347cd.png)](https://medium.com/@molly.ruby?source=post_page-----1ce5fca96286--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1ce5fca96286--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1ce5fca96286--------------------------------) [Molly Ruby](https://medium.com/@molly.ruby?source=post_page-----1ce5fca96286--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7a38f8e9fb80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-chatgpt-works-the-models-behind-the-bot-1ce5fca96286&user=Molly+Ruby&userId=7a38f8e9fb80&source=post_page-7a38f8e9fb80----1ce5fca96286---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ce5fca96286--------------------------------) ·9分钟阅读·2023年1月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1ce5fca96286&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-chatgpt-works-the-models-behind-the-bot-1ce5fca96286&user=Molly+Ruby&userId=7a38f8e9fb80&source=-----1ce5fca96286---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1ce5fca96286&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-chatgpt-works-the-models-behind-the-bot-1ce5fca96286&source=-----1ce5fca96286---------------------bookmark_footer-----------)![](../Images/0e4bc81c17a6711eda750f7e522f100e.png)

本文将温和地介绍驱动ChatGPT的机器学习模型，从大型语言模型的介绍开始，深入探讨使GPT-3得以训练的革命性自注意力机制，然后探究使ChatGPT卓越的强化学习技术。

# 大型语言模型

ChatGPT 是一种机器学习自然语言处理模型的外推，被称为大型语言模型（LLMs）。LLMs 消化大量文本数据，并推断文本中单词之间的关系。随着计算能力的进步，这些模型在过去几年中不断发展。LLMs 的能力随着输入数据集和参数空间的大小增加而增强。

语言模型的最基本训练涉及预测词语序列中的一个词。最常见的方式是下一个词预测和掩码语言建模。
