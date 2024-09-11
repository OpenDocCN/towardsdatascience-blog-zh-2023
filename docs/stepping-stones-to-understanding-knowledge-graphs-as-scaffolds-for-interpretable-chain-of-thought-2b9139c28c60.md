# 理解的垫脚石：知识图谱作为可解释的思维链推理的支撑

> 原文：[https://towardsdatascience.com/stepping-stones-to-understanding-knowledge-graphs-as-scaffolds-for-interpretable-chain-of-thought-2b9139c28c60?source=collection_archive---------11-----------------------#2023-11-21](https://towardsdatascience.com/stepping-stones-to-understanding-knowledge-graphs-as-scaffolds-for-interpretable-chain-of-thought-2b9139c28c60?source=collection_archive---------11-----------------------#2023-11-21)

[](https://medium.com/@alcarazanthony1?source=post_page-----2b9139c28c60--------------------------------)[![安东尼·阿尔卡拉斯](../Images/6a71a1752677bd07c384246fb0c7f7e8.png)](https://medium.com/@alcarazanthony1?source=post_page-----2b9139c28c60--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2b9139c28c60--------------------------------)[![数据科学的进展](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2b9139c28c60--------------------------------) [安东尼·阿尔卡拉斯](https://medium.com/@alcarazanthony1?source=post_page-----2b9139c28c60--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F30bc9ffd2f4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstepping-stones-to-understanding-knowledge-graphs-as-scaffolds-for-interpretable-chain-of-thought-2b9139c28c60&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=post_page-30bc9ffd2f4b----2b9139c28c60---------------------post_header-----------) 发表在 [数据科学的进展](https://towardsdatascience.com/?source=post_page-----2b9139c28c60--------------------------------) ·7分钟阅读·2023年11月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2b9139c28c60&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstepping-stones-to-understanding-knowledge-graphs-as-scaffolds-for-interpretable-chain-of-thought-2b9139c28c60&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=-----2b9139c28c60---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2b9139c28c60&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstepping-stones-to-understanding-knowledge-graphs-as-scaffolds-for-interpretable-chain-of-thought-2b9139c28c60&source=-----2b9139c28c60---------------------bookmark_footer-----------)

*人工智能软件被用来提升本文的语法、流畅性和可读性。*

大型语言模型（LLMs）在海量文本数据上进行训练，已引发了人工智能领域的革命。它们仅凭短小的文本提示就能生成极为优雅、连贯的语言，这为从创意写作到对话助手等多个领域开辟了新的视野。

然而，单靠语言表达的掌握并不等于真正的智能。大型语言模型仍然缺乏对概念的语义理解和进行情境理解及复杂问题解决所需的逻辑推理能力。它们的知识仍然局限于从训练数据中辨识出的表面模式，而非关于现实世界的确凿事实。

随着我们向这些模型提出更多开放式、多面向的问题，它们的局限性变得越来越明显。它们不能从不同文档中逻辑性地综合细节，也不能进行跨多步的推理来得出答案。

一旦查询开始偏离训练数据的分布，就会出现虚假的或矛盾的回答。

为了应对这些陷阱，人工智能社区已将重点转向检索增强生成（RAG）框架。这些系统旨在协同语言的**语言能力**……
