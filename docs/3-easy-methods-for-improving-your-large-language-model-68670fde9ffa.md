# 改善大型语言模型的3种简单方法

> 原文：[https://towardsdatascience.com/3-easy-methods-for-improving-your-large-language-model-68670fde9ffa?source=collection_archive---------1-----------------------#2023-09-15](https://towardsdatascience.com/3-easy-methods-for-improving-your-large-language-model-68670fde9ffa?source=collection_archive---------1-----------------------#2023-09-15)

## 增强Llama 2的能力

[](https://medium.com/@maartengrootendorst?source=post_page-----68670fde9ffa--------------------------------)[![Maarten Grootendorst](../Images/58e24b9cf7e10ff1cd5ffd75a32d1a26.png)](https://medium.com/@maartengrootendorst?source=post_page-----68670fde9ffa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----68670fde9ffa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----68670fde9ffa--------------------------------) [Maarten Grootendorst](https://medium.com/@maartengrootendorst?source=post_page-----68670fde9ffa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F22405c3b2875&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-easy-methods-for-improving-your-large-language-model-68670fde9ffa&user=Maarten+Grootendorst&userId=22405c3b2875&source=post_page-22405c3b2875----68670fde9ffa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----68670fde9ffa--------------------------------) ·12分钟阅读·2023年9月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F68670fde9ffa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-easy-methods-for-improving-your-large-language-model-68670fde9ffa&user=Maarten+Grootendorst&userId=22405c3b2875&source=-----68670fde9ffa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F68670fde9ffa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-easy-methods-for-improving-your-large-language-model-68670fde9ffa&source=-----68670fde9ffa---------------------bookmark_footer-----------)![](../Images/4bc4b72c825e1d128f2e168a5aabf27e.png)

大型语言模型（LLMs）已经成为长期存在的技术。随着Llama 2的发布，开源LLMs的性能正接近ChatGPT，并且经过适当调整甚至可以超越它。

使用这些LLMs往往不像看起来那么简单，特别是当你想将LLM微调以适应你的特定用例时。

在这篇文章中，我们将探讨提高任何LLM性能的3种最常见的方法：

+   提示工程

+   检索增强生成（RAG）

+   参数高效微调（PEFT）

![](../Images/a22e11f94a498add3609ac5a4fa81b26.png)

还有很多其他方法，但这些是最简单的，而且能在不费太多力气的情况下带来重大改进。

这3种方法从最简单的方法开始，即所谓的低悬果，到提升你的LLM的更复杂的方法。

为了最大程度地发挥LLM的效果，你甚至可以将这三种方法结合使用！
