# 《开源大型语言模型的温和介绍》

> 原文：[`towardsdatascience.com/a-gentle-introduction-to-open-source-large-language-models-3643f5ca774?source=collection_archive---------1-----------------------#2023-08-11`](https://towardsdatascience.com/a-gentle-introduction-to-open-source-large-language-models-3643f5ca774?source=collection_archive---------1-----------------------#2023-08-11)

## 开放语言模型

## 为什么每个人都在谈论美洲驼、羊驼、猎鹰及其他动物

[](https://donatoriccio.medium.com/?source=post_page-----3643f5ca774--------------------------------)![Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----3643f5ca774--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3643f5ca774--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3643f5ca774--------------------------------) [Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----3643f5ca774--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe384fc71d292&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-introduction-to-open-source-large-language-models-3643f5ca774&user=Donato+Riccio&userId=e384fc71d292&source=post_page-e384fc71d292----3643f5ca774---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3643f5ca774--------------------------------) · 11 分钟阅读 · 2023 年 8 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3643f5ca774&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-introduction-to-open-source-large-language-models-3643f5ca774&user=Donato+Riccio&userId=e384fc71d292&source=-----3643f5ca774---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3643f5ca774&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-introduction-to-open-source-large-language-models-3643f5ca774&source=-----3643f5ca774---------------------bookmark_footer-----------)![](img/b78c1db423fd8ee4ce173a12b054118c.png)

作者提供的图片（使用 Midjourney 生成）

除非你在过去一年中过得像在石头下，否则你一定见证了 ChatGPT 的革命，以及它如何让每个人都无法停止使用。在本文中，我们将探索它的替代品，深入了解开源模型。该系列的第一篇文章*开源语言模型*对希望入门和理解开源大型语言模型的人很有帮助，并探讨如何以及为什么使用它们。

# 目录

— 我们为什么需要开源模型？

— 越大越好？训练大型语言模型

— 微调大型语言模型

— 最佳开源大型语言模型

— 在你的计算机上运行大型语言模型

— 局限性

— 结论

## 什么是大型语言模型？

**大型语言模型（LLM）**是一种能够理解和生成自然语言的人工智能。其核心是一种称为转换器的神经网络，它…
