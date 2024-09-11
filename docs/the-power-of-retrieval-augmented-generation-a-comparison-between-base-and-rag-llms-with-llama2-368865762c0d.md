# 检索增强生成的力量：Base 和 RAG LLM 与 Llama2 的比较

> 原文：[`towardsdatascience.com/the-power-of-retrieval-augmented-generation-a-comparison-between-base-and-rag-llms-with-llama2-368865762c0d?source=collection_archive---------0-----------------------#2023-11-29`](https://towardsdatascience.com/the-power-of-retrieval-augmented-generation-a-comparison-between-base-and-rag-llms-with-llama2-368865762c0d?source=collection_archive---------0-----------------------#2023-11-29)

## 深入探讨如何使用 RAG 方法针对特定用例调整预训练的 LLM，包含 LangChain 和 Hugging Face 的集成

[](https://medium.com/@luisroque?source=post_page-----368865762c0d--------------------------------)![Luís Roque](https://medium.com/@luisroque?source=post_page-----368865762c0d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----368865762c0d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----368865762c0d--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----368865762c0d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-retrieval-augmented-generation-a-comparison-between-base-and-rag-llms-with-llama2-368865762c0d&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----368865762c0d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----368865762c0d--------------------------------) ·12 分钟阅读·2023 年 11 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F368865762c0d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-retrieval-augmented-generation-a-comparison-between-base-and-rag-llms-with-llama2-368865762c0d&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----368865762c0d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F368865762c0d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-retrieval-augmented-generation-a-comparison-between-base-and-rag-llms-with-llama2-368865762c0d&source=-----368865762c0d---------------------bookmark_footer-----------)

*这篇文章由 Rafael Guedes 共同撰写。*

# 引言

自从 2022 年 11 月 ChatGPT 发布以来，大型语言模型（LLMs）因其理解和生成类人文本的能力，成为了 AI 社区的热门话题，推动了自然语言处理（NLP）领域的边界。

LLMs 已经通过处理不同行业中的各种用例展示了它们的多功能性，因为它们并不局限于特定任务。它们可以适应多个领域，这使得它们对组织和研究社区具有吸引力。已经探索了使用 LLMs 的多个应用，例如内容生成、聊天机器人、代码生成、创意写作、虚拟助手等。

另一个使大型语言模型（LLMs）如此吸引人的特征是存在开源选项。像 Meta 这样的公司将其预训练的 LLM（Llama2 🦙）提供在像 Hugging Face 🤗这样的代码库中。这些预训练的 LLM 是否足够满足每个公司的特定用例？当然不是。
