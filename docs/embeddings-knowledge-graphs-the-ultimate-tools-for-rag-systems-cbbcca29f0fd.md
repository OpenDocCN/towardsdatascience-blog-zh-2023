# 嵌入 + 知识图谱：RAG 系统的终极工具

> 原文：[https://towardsdatascience.com/embeddings-knowledge-graphs-the-ultimate-tools-for-rag-systems-cbbcca29f0fd?source=collection_archive---------1-----------------------#2023-11-14](https://towardsdatascience.com/embeddings-knowledge-graphs-the-ultimate-tools-for-rag-systems-cbbcca29f0fd?source=collection_archive---------1-----------------------#2023-11-14)

[](https://medium.com/@alcarazanthony1?source=post_page-----cbbcca29f0fd--------------------------------)[![安东尼·阿尔卡拉斯](../Images/6a71a1752677bd07c384246fb0c7f7e8.png)](https://medium.com/@alcarazanthony1?source=post_page-----cbbcca29f0fd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cbbcca29f0fd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cbbcca29f0fd--------------------------------) [安东尼·阿尔卡拉斯](https://medium.com/@alcarazanthony1?source=post_page-----cbbcca29f0fd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F30bc9ffd2f4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fembeddings-knowledge-graphs-the-ultimate-tools-for-rag-systems-cbbcca29f0fd&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=post_page-30bc9ffd2f4b----cbbcca29f0fd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cbbcca29f0fd--------------------------------) · 10分钟阅读 · 2023年11月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcbbcca29f0fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fembeddings-knowledge-graphs-the-ultimate-tools-for-rag-systems-cbbcca29f0fd&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=-----cbbcca29f0fd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcbbcca29f0fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fembeddings-knowledge-graphs-the-ultimate-tools-for-rag-systems-cbbcca29f0fd&source=-----cbbcca29f0fd---------------------bookmark_footer-----------)

*人工智能软件用于提升本文的语法、流畅性和可读性。*

大型语言模型（LLMs）的出现，经过大量文本数据训练，已成为自然语言处理领域最重要的突破之一。这些模型仅凭简短的提示就能生成流畅且连贯的文本，这为对话式AI、创意写作及其他广泛应用开辟了新可能。

然而，尽管大型语言模型（LLMs）口才出众，但它们存在一些关键的限制。它们的知识仅限于从训练数据中辨别出的模式，这意味着它们对世界的理解并不真实。

它们的推理能力也有限——它们不能进行逻辑推理或从多个来源合成事实。当我们提出更复杂、开放性的问题时，回应开始变得荒谬或矛盾。

为了弥补这些不足，越来越多的关注转向检索增强生成（RAG）系统。其核心思想是从外部来源检索相关知识，为大型语言模型提供上下文，以便做出更为明智的回应。

大多数现有系统通过向量嵌入的语义相似性来检索段落。然而，这种方法有其自身的缺陷，比如缺乏真正的相关性、无法聚合事实以及没有推理链。
