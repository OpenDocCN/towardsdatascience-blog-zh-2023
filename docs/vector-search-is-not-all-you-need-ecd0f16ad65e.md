# 向量搜索并不是你所需要的一切

> 原文：[https://towardsdatascience.com/vector-search-is-not-all-you-need-ecd0f16ad65e?source=collection_archive---------1-----------------------#2023-09-18](https://towardsdatascience.com/vector-search-is-not-all-you-need-ecd0f16ad65e?source=collection_archive---------1-----------------------#2023-09-18)

[](https://medium.com/@alcarazanthony1?source=post_page-----ecd0f16ad65e--------------------------------)[![安东尼·阿尔卡拉斯](../Images/6a71a1752677bd07c384246fb0c7f7e8.png)](https://medium.com/@alcarazanthony1?source=post_page-----ecd0f16ad65e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ecd0f16ad65e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ecd0f16ad65e--------------------------------) [安东尼·阿尔卡拉斯](https://medium.com/@alcarazanthony1?source=post_page-----ecd0f16ad65e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F30bc9ffd2f4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvector-search-is-not-all-you-need-ecd0f16ad65e&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=post_page-30bc9ffd2f4b----ecd0f16ad65e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ecd0f16ad65e--------------------------------) ·6 min read·2023年9月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fecd0f16ad65e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvector-search-is-not-all-you-need-ecd0f16ad65e&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=-----ecd0f16ad65e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fecd0f16ad65e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvector-search-is-not-all-you-need-ecd0f16ad65e&source=-----ecd0f16ad65e---------------------bookmark_footer-----------)

*人工智能软件用于增强本文的语法、流畅性和可读性。*

# 介绍

检索增强生成（RAG）彻底改变了开放领域问答，能够使系统对各种查询生成类人的回答。RAG 的核心是一个检索模块，它扫描大量文献以找到相关的上下文段落，然后由一个神经生成模块——通常是像 GPT-3 这样的预训练语言模型——来形成最终的回答。

虽然这种方法非常有效，但也并非没有局限性。

其中最关键的一个部分，即对嵌入段落的向量搜索，具有固有的限制，这可能会妨碍系统以细致的方式进行推理。这在需要跨多个文档进行复杂的多跳推理时尤其明显。

向量搜索是指使用数据的向量表示来搜索信息。它包括两个关键步骤：

1.  **将数据编码为向量**

首先，被搜索的数据被编码为数值向量表示。对于文本数据，如段落或…
