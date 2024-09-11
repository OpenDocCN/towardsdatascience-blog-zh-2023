# 实现大型语言模型的更大自我一致性

> 原文：[https://towardsdatascience.com/achieving-greater-self-consistency-in-large-language-models-6e6cb5f3c5b7?source=collection_archive---------9-----------------------#2023-12-01](https://towardsdatascience.com/achieving-greater-self-consistency-in-large-language-models-6e6cb5f3c5b7?source=collection_archive---------9-----------------------#2023-12-01)

[](https://medium.com/@alcarazanthony1?source=post_page-----6e6cb5f3c5b7--------------------------------)[![安东尼·阿尔卡拉兹](../Images/6a71a1752677bd07c384246fb0c7f7e8.png)](https://medium.com/@alcarazanthony1?source=post_page-----6e6cb5f3c5b7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6e6cb5f3c5b7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6e6cb5f3c5b7--------------------------------) [安东尼·阿尔卡拉兹](https://medium.com/@alcarazanthony1?source=post_page-----6e6cb5f3c5b7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F30bc9ffd2f4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fachieving-greater-self-consistency-in-large-language-models-6e6cb5f3c5b7&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=post_page-30bc9ffd2f4b----6e6cb5f3c5b7---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6e6cb5f3c5b7--------------------------------) ·8分钟阅读·2023年12月1日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6e6cb5f3c5b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fachieving-greater-self-consistency-in-large-language-models-6e6cb5f3c5b7&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=-----6e6cb5f3c5b7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6e6cb5f3c5b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fachieving-greater-self-consistency-in-large-language-models-6e6cb5f3c5b7&source=-----6e6cb5f3c5b7---------------------bookmark_footer-----------)

*人工智能软件被用于提升本文的语法、流畅性和可读性。*

当大型语言模型用于评估文本的正确性、准确性或相关性等特质时，一致性至关重要。如果LLM表现出不一致的判断，那么它的评估将变得不可靠和不值得信赖。

如果一个大型语言模型（LLM）评估论证的推理质量，但通过将无效的论证评为比完全有效的论证更具逻辑性来自我矛盾，那么它作为推理仲裁者就失败了。由于模型自身缺乏逻辑一致性，其评估失去了可信度。

当出现这样的不一致时，就没有稳定的基础来比较LLM对不同文本的评估。如果模型随意自相矛盾，那么句子就无法根据模型不一致的评分进行可靠的排序。

本质上，不一致性破坏了评估最初旨在提供的比较基础。如果一个LLM无法展示评估标准的一致应用，那么用它来评估文本就会失去所有的有效性和实用性。

因此，对于用于评分或评判文本质量和特征的LLM，一致性的判断和评估是必要的。如果其评估没有高水平的稳定性，且没有基于对所评估概念的一致理解，那么基础就会…
