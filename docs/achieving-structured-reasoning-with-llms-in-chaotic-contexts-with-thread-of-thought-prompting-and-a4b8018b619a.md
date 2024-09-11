# 在混乱的环境中通过思维链提示和并行知识图谱检索实现结构化推理

> 原文：[`towardsdatascience.com/achieving-structured-reasoning-with-llms-in-chaotic-contexts-with-thread-of-thought-prompting-and-a4b8018b619a?source=collection_archive---------4-----------------------#2023-11-18`](https://towardsdatascience.com/achieving-structured-reasoning-with-llms-in-chaotic-contexts-with-thread-of-thought-prompting-and-a4b8018b619a?source=collection_archive---------4-----------------------#2023-11-18)

[](https://medium.com/@alcarazanthony1?source=post_page-----a4b8018b619a--------------------------------)![Anthony Alcaraz](https://medium.com/@alcarazanthony1?source=post_page-----a4b8018b619a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a4b8018b619a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4b8018b619a--------------------------------) [Anthony Alcaraz](https://medium.com/@alcarazanthony1?source=post_page-----a4b8018b619a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F30bc9ffd2f4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fachieving-structured-reasoning-with-llms-in-chaotic-contexts-with-thread-of-thought-prompting-and-a4b8018b619a&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=post_page-30bc9ffd2f4b----a4b8018b619a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4b8018b619a--------------------------------) · 8 分钟阅读 · 2023 年 11 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa4b8018b619a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fachieving-structured-reasoning-with-llms-in-chaotic-contexts-with-thread-of-thought-prompting-and-a4b8018b619a&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=-----a4b8018b619a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa4b8018b619a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fachieving-structured-reasoning-with-llms-in-chaotic-contexts-with-thread-of-thought-prompting-and-a4b8018b619a&source=-----a4b8018b619a---------------------bookmark_footer-----------)

*人工智能软件被用来增强本文的语法、流畅性和可读性。*

大型语言模型（LLMs）展示了令人印象深刻的少量样本学习能力，能够迅速适应新任务，只需少量示例即可。

[](https://ai.plainenglish.io/why-rag-retrieval-augmented-generation-will-become-a-cornerstone-of-system-design-using-llm-719657fbfebe?source=post_page-----a4b8018b619a--------------------------------) [## 为什么 RAG（检索增强生成）将成为使用 LLM 的系统设计基石…

### 最近，大型语言模型（LLMs）如 GPT-3 [1] 展示了强大的少样本学习能力——…

[ai.plainenglish.io](https://ai.plainenglish.io/why-rag-retrieval-augmented-generation-will-become-a-cornerstone-of-system-design-using-llm-719657fbfebe?source=post_page-----a4b8018b619a--------------------------------)

尽管大语言模型（LLMs）取得了进展，它们在涉及混乱背景和过多分散事实的复杂推理中仍面临局限性。为了解决这一挑战，研究人员探索了如链式思维提示等技术，这些技术引导模型逐步分析信息。然而，单靠这些方法仍难以全面捕捉广泛背景下的所有关键细节。

本文提出了一种技术，将思维链（ToT）提示与检索增强生成（RAG）框架结合起来，后者可以并行访问多个知识图谱。虽然 ToT 作为推理的“支柱”来结构化思维，但 RAG 系统扩展了可用知识以填补空白。与顺序检索相比，多源并行查询提高了效率和覆盖面。综合来看，这…
