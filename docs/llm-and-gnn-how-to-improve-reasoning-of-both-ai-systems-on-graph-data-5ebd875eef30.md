# LLM 和 GNN：如何提升这两种人工智能系统在图数据上的推理能力

> 原文：[`towardsdatascience.com/llm-and-gnn-how-to-improve-reasoning-of-both-ai-systems-on-graph-data-5ebd875eef30?source=collection_archive---------1-----------------------#2023-12-03`](https://towardsdatascience.com/llm-and-gnn-how-to-improve-reasoning-of-both-ai-systems-on-graph-data-5ebd875eef30?source=collection_archive---------1-----------------------#2023-12-03)

[](https://medium.com/@alcarazanthony1?source=post_page-----5ebd875eef30--------------------------------)![Anthony Alcaraz](https://medium.com/@alcarazanthony1?source=post_page-----5ebd875eef30--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5ebd875eef30--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ebd875eef30--------------------------------) [Anthony Alcaraz](https://medium.com/@alcarazanthony1?source=post_page-----5ebd875eef30--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F30bc9ffd2f4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-and-gnn-how-to-improve-reasoning-of-both-ai-systems-on-graph-data-5ebd875eef30&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=post_page-30bc9ffd2f4b----5ebd875eef30---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ebd875eef30--------------------------------) ·9 分钟阅读·2023 年 12 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5ebd875eef30&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-and-gnn-how-to-improve-reasoning-of-both-ai-systems-on-graph-data-5ebd875eef30&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=-----5ebd875eef30---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5ebd875eef30&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-and-gnn-how-to-improve-reasoning-of-both-ai-systems-on-graph-data-5ebd875eef30&source=-----5ebd875eef30---------------------bookmark_footer-----------)

*人工智能软件被用来增强本文的语法、流畅性和可读性。*

图神经网络（GNNs）和大型语言模型（LLMs）已经成为人工智能的两个主要分支，在从图结构数据和自然语言数据中学习方面取得了巨大的成功。

随着图结构数据和自然语言数据在实际应用中变得越来越互联，对能够进行多模态推理的人工智能系统的需求也在不断增长。

本文探讨了集成图-语言架构，这些架构结合了图神经网络（GNNs）和大型语言模型（LLMs）的互补优势，以增强分析能力。

现实世界的场景通常涉及结构和文本模式相互关联的数据。这就提出了需要集成架构的需求，这种架构能够通过统一 GNNs 和 LLMs 的互补优势来执行多方面的推理。

具体来说，尽管 GNNs 通过图上的消息传递来聚合局部模式，但节点嵌入在捕捉丰富特征方面存在限制。

相比之下，LLMs 表现出卓越的语义推理能力，但在对 GNNs 固有理解的结构拓扑进行关系推理时却表现不佳。
