# RAG与微调 — 哪种是提升LLM应用的最佳工具？

> 原文：[https://towardsdatascience.com/rag-vs-finetuning-which-is-the-best-tool-to-boost-your-llm-application-94654b1eaba7?source=collection_archive---------0-----------------------#2023-08-24](https://towardsdatascience.com/rag-vs-finetuning-which-is-the-best-tool-to-boost-your-llm-application-94654b1eaba7?source=collection_archive---------0-----------------------#2023-08-24)

## 为你的使用案例选择正确方法的终极指南

[](https://heiko-hotz.medium.com/?source=post_page-----94654b1eaba7--------------------------------)[![Heiko Hotz](../Images/d08394d46d41d5cd9e76557a463be95e.png)](https://heiko-hotz.medium.com/?source=post_page-----94654b1eaba7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----94654b1eaba7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----94654b1eaba7--------------------------------) [Heiko Hotz](https://heiko-hotz.medium.com/?source=post_page-----94654b1eaba7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F993c21f1b30f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frag-vs-finetuning-which-is-the-best-tool-to-boost-your-llm-application-94654b1eaba7&user=Heiko+Hotz&userId=993c21f1b30f&source=post_page-993c21f1b30f----94654b1eaba7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----94654b1eaba7--------------------------------) ·19 min read·2023年8月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F94654b1eaba7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frag-vs-finetuning-which-is-the-best-tool-to-boost-your-llm-application-94654b1eaba7&user=Heiko+Hotz&userId=993c21f1b30f&source=-----94654b1eaba7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F94654b1eaba7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frag-vs-finetuning-which-is-the-best-tool-to-boost-your-llm-application-94654b1eaba7&source=-----94654b1eaba7---------------------bookmark_footer-----------)![](../Images/f87aad7c078e00a5f9db9a00ad4debad.png)

作者图片

# 序言

随着对大型语言模型（LLMs）兴趣的激增，许多开发者和组织忙于构建利用其力量的应用。然而，当预训练的LLMs直接使用时未能达到预期效果时，如何提升LLM应用的性能成为了问题。最终，我们会问自己：我们应该使用 [检索增强生成](https://arxiv.org/abs/2005.11401)（RAG）还是模型微调来改善结果？

在深入讨论之前，让我们先解密这两种方法：

**RAG**：这种方法将检索（或搜索）的力量融入到LLM文本生成中。它结合了一个检索系统，该系统从大型语料库中提取相关的文档片段，以及一个LLM，使用这些片段中的信息生成回答。本质上，RAG帮助模型“查找”外部信息以改善其响应。

![](../Images/37f3ba773a6c401ffdd3eead8bdf76b7.png)

作者提供的图片
