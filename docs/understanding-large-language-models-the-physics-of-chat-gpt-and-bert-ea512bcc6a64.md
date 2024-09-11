# 理解大型语言模型：**(Chat)**GPT 和 BERT 的物理学

> 原文：[https://towardsdatascience.com/understanding-large-language-models-the-physics-of-chat-gpt-and-bert-ea512bcc6a64?source=collection_archive---------1-----------------------#2023-07-20](https://towardsdatascience.com/understanding-large-language-models-the-physics-of-chat-gpt-and-bert-ea512bcc6a64?source=collection_archive---------1-----------------------#2023-07-20)

## 一位物理学家的见解，关于粒子和力如何帮助我们理解大型语言模型（LLMs）。

[](https://tim-lou.medium.com/?source=post_page-----ea512bcc6a64--------------------------------)[![Tim Lou, PhD](../Images/e4931bb6d59e27730529ceaf00a23822.png)](https://tim-lou.medium.com/?source=post_page-----ea512bcc6a64--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ea512bcc6a64--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ea512bcc6a64--------------------------------) [Tim Lou, PhD](https://tim-lou.medium.com/?source=post_page-----ea512bcc6a64--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8d41b438feef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-large-language-models-the-physics-of-chat-gpt-and-bert-ea512bcc6a64&user=Tim+Lou%2C+PhD&userId=8d41b438feef&source=post_page-8d41b438feef----ea512bcc6a64---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea512bcc6a64--------------------------------) ·12 min 阅读·2023年7月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fea512bcc6a64&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-large-language-models-the-physics-of-chat-gpt-and-bert-ea512bcc6a64&user=Tim+Lou%2C+PhD&userId=8d41b438feef&source=-----ea512bcc6a64---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fea512bcc6a64&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-large-language-models-the-physics-of-chat-gpt-and-bert-ea512bcc6a64&source=-----ea512bcc6a64---------------------bookmark_footer-----------)![](../Images/471184f0f0e17bba6faaa3b9fca21a6c.png)

ChatGPT 和冰晶之间可能比想象中更有共同点（来源：[15414483@pixabay](https://pixabay.com/photos/ice-frost-winter-snow-snowflakes-6538605/)）

ChatGPT，或更广义的大型语言 AI 模型（LLMs），已在我们的生活中变得无处不在。然而，大多数 LLMs 的数学和内部结构对公众来说仍然是晦涩的知识。

那么，我们如何才能超越将像 ChatGPT 这样的 LLMs 视为神秘黑箱的看法？物理学可能提供答案。

每个人对我们的物理世界都有一定的了解。像汽车、桌子和行星这样的物体由数万亿个原子组成，受一组简单的物理定律支配。类似地，像 ChatGPT 这样的复杂生物体也已经出现，并能够生成如艺术和科学等高度复杂的概念。

结果表明，LLM 的构建块的方程式与我们的物理定律类似。因此，通过理解复杂性如何从我们简单的物理定律中产生，我们可能能够对 LLM 的工作原理和原因有所领悟。

# 简单中的复杂性
