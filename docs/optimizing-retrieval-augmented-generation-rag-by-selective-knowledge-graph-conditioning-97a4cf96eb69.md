# 通过选择性知识图谱条件优化检索增强生成（RAG）

> 原文：[https://towardsdatascience.com/optimizing-retrieval-augmented-generation-rag-by-selective-knowledge-graph-conditioning-97a4cf96eb69?source=collection_archive---------4-----------------------#2023-12-28](https://towardsdatascience.com/optimizing-retrieval-augmented-generation-rag-by-selective-knowledge-graph-conditioning-97a4cf96eb69?source=collection_archive---------4-----------------------#2023-12-28)

## 如何通过有针对性的增强显著提高知识相关性，同时保持语言流畅性

[](https://medium.com/@alcarazanthony1?source=post_page-----97a4cf96eb69--------------------------------)[![Anthony Alcaraz](../Images/6a71a1752677bd07c384246fb0c7f7e8.png)](https://medium.com/@alcarazanthony1?source=post_page-----97a4cf96eb69--------------------------------)[](https://towardsdatascience.com/?source=post_page-----97a4cf96eb69--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----97a4cf96eb69--------------------------------) [Anthony Alcaraz](https://medium.com/@alcarazanthony1?source=post_page-----97a4cf96eb69--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F30bc9ffd2f4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimizing-retrieval-augmented-generation-rag-by-selective-knowledge-graph-conditioning-97a4cf96eb69&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=post_page-30bc9ffd2f4b----97a4cf96eb69---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----97a4cf96eb69--------------------------------) · 7分钟阅读 · 2023年12月28日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F97a4cf96eb69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimizing-retrieval-augmented-generation-rag-by-selective-knowledge-graph-conditioning-97a4cf96eb69&source=-----97a4cf96eb69---------------------bookmark_footer-----------)

*使用了人工智能软件来增强本文的语法、流畅性和可读性。*

生成预训练模型在用作对话代理时表现出令人印象深刻的流畅性和连贯性。然而，它们面临的一个关键局限是缺乏对外部知识的基础。如果仅依靠预训练参数，这些模型往往生成看似合理但事实错误的回应，也称为幻觉。

以前的减轻方法涉及将整个知识图谱与聊天中提到的实体进行增强对话上下文。然而，这种对大型知识图谱的盲目条件化带来了自身的问题：

原始知识图谱增强的局限性：

+   大部分1跳上下文可能与对话无关，插入了不必要的噪音

+   编码整个知识子图会使序列长度限制变得紧张

+   没有保证模型会使用相关的事实进行生成

+   尽管进行了知识基础的处理，幻觉的风险仍然存在
