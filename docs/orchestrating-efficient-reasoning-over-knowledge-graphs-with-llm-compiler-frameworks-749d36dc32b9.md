# **高效推理在知识图谱中的编排与LLM编译器框架**

> 原文：[https://towardsdatascience.com/orchestrating-efficient-reasoning-over-knowledge-graphs-with-llm-compiler-frameworks-749d36dc32b9?source=collection_archive---------3-----------------------#2023-12-29](https://towardsdatascience.com/orchestrating-efficient-reasoning-over-knowledge-graphs-with-llm-compiler-frameworks-749d36dc32b9?source=collection_archive---------3-----------------------#2023-12-29)

[](https://medium.com/@alcarazanthony1?source=post_page-----749d36dc32b9--------------------------------)[![Anthony Alcaraz](../Images/6a71a1752677bd07c384246fb0c7f7e8.png)](https://medium.com/@alcarazanthony1?source=post_page-----749d36dc32b9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----749d36dc32b9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----749d36dc32b9--------------------------------) [Anthony Alcaraz](https://medium.com/@alcarazanthony1?source=post_page-----749d36dc32b9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F30bc9ffd2f4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Forchestrating-efficient-reasoning-over-knowledge-graphs-with-llm-compiler-frameworks-749d36dc32b9&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=post_page-30bc9ffd2f4b----749d36dc32b9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----749d36dc32b9--------------------------------) ·6分钟阅读·2023年12月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F749d36dc32b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Forchestrating-efficient-reasoning-over-knowledge-graphs-with-llm-compiler-frameworks-749d36dc32b9&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=-----749d36dc32b9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F749d36dc32b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Forchestrating-efficient-reasoning-over-knowledge-graphs-with-llm-compiler-frameworks-749d36dc32b9&source=-----749d36dc32b9---------------------bookmark_footer-----------)

*人工智能软件用于提升本文文本的语法、流畅性和可读性。*

最近的大型语言模型（LLM）设计创新带来了少量样本学习和推理能力的快速进步。然而，尽管取得了这些进展，LLM在处理涉及大量互联知识的复杂现实世界情境时，仍然面临着局限性。

为了应对这一挑战，*检索增强生成*（RAG）系统提出了一种有前景的方法。RAG将LLMs的自适应学习优势与来自外部知识源（如知识图谱KGs）的可扩展检索结合起来。RAG不是尝试静态地将所有信息编码到模型中，而是允许在需要时从索引的知识图谱中动态查询必要的背景。

然而，有效地协调跨互联知识源的推理和检索带来了自身的挑战。简单地在离散步骤中检索和连接信息的幼稚方法往往无法完全捕捉到密集知识图谱中的细微差别。概念的互联性质意味着，如果不相互关联地进行分析，重要的背景细节可能会被遗漏。

最近，一个名为LLM Compiler的有趣框架在优化LLMs中多个函数调用的调度方面展示了早期的成功，通过自动处理…
