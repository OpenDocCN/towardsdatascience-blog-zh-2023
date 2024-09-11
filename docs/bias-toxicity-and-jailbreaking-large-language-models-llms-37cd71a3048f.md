# 偏见、毒性与破解大型语言模型（LLMs）

> 原文：[https://towardsdatascience.com/bias-toxicity-and-jailbreaking-large-language-models-llms-37cd71a3048f?source=collection_archive---------5-----------------------#2023-11-28](https://towardsdatascience.com/bias-toxicity-and-jailbreaking-large-language-models-llms-37cd71a3048f?source=collection_archive---------5-----------------------#2023-11-28)

## 最近关于大型语言模型（LLMs）相关特征的研究综述

[](https://rachel-draelos.medium.com/?source=post_page-----37cd71a3048f--------------------------------)[![Rachel Draelos, MD, PhD](../Images/edc30d41f9fea7e57dcf0c44caf68165.png)](https://rachel-draelos.medium.com/?source=post_page-----37cd71a3048f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----37cd71a3048f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----37cd71a3048f--------------------------------) [Rachel Draelos, MD, PhD](https://rachel-draelos.medium.com/?source=post_page-----37cd71a3048f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F209c0f742bcf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbias-toxicity-and-jailbreaking-large-language-models-llms-37cd71a3048f&user=Rachel+Draelos%2C+MD%2C+PhD&userId=209c0f742bcf&source=post_page-209c0f742bcf----37cd71a3048f---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----37cd71a3048f--------------------------------) · 17分钟阅读·2023年11月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F37cd71a3048f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbias-toxicity-and-jailbreaking-large-language-models-llms-37cd71a3048f&user=Rachel+Draelos%2C+MD%2C+PhD&userId=209c0f742bcf&source=-----37cd71a3048f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F37cd71a3048f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbias-toxicity-and-jailbreaking-large-language-models-llms-37cd71a3048f&source=-----37cd71a3048f---------------------bookmark_footer-----------)![](../Images/7ca68df720b68138e8d971d351c51b49.png)

特色图片来源于[维基共享资源中的盖尔顿箱视频](https://en.wikipedia.org/wiki/File:Galton_box.webm)（知识共享署名-相同方式共享 4.0 国际许可协议）。

内容警告：本帖子包含了由LLMs生成的偏见和有毒文本的示例。

本文深入探讨了关于偏见、毒性以及大型语言模型（LLMs）越狱的最新研究，特别是ChatGPT和GPT-4。我将讨论公司在LLM开发中目前使用的伦理指南，以及它们为防止生成不良内容而采取的方法。然后，我将从多个角度审视近期研究论文，探讨毒性内容生成、越狱以及偏见：包括性别、种族、医学、政治、职场和虚构内容。

偏见指的是对特定群体、个人或事物的偏向或歧视，而毒性则指的是不尊重、粗俗、无礼或有害的内容。大型语言模型（LLMs）存在偏见，并且有生成毒性内容的能力，因为它们是在大量的互联网数据上进行训练的，这些数据不幸地代表了人性的好与坏，包括我们的所有偏见和毒性。幸运的是，像OpenAI和Google这样的LLM开发者已经采取了措施，以减少LLM生成明显偏见或毒性内容的可能性。然而，正如我们将看到的，这并不意味着这些模型是……
