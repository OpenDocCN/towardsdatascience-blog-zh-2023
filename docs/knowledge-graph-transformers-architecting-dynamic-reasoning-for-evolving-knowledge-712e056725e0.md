# 知识图谱变换器：为不断演变的知识构建动态推理

> 原文：[`towardsdatascience.com/knowledge-graph-transformers-architecting-dynamic-reasoning-for-evolving-knowledge-712e056725e0?source=collection_archive---------1-----------------------#2023-10-28`](https://towardsdatascience.com/knowledge-graph-transformers-architecting-dynamic-reasoning-for-evolving-knowledge-712e056725e0?source=collection_archive---------1-----------------------#2023-10-28)

[](https://medium.com/@alcarazanthony1?source=post_page-----712e056725e0--------------------------------)![安东尼·阿尔卡拉斯](https://medium.com/@alcarazanthony1?source=post_page-----712e056725e0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----712e056725e0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----712e056725e0--------------------------------) [安东尼·阿尔卡拉斯](https://medium.com/@alcarazanthony1?source=post_page-----712e056725e0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F30bc9ffd2f4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fknowledge-graph-transformers-architecting-dynamic-reasoning-for-evolving-knowledge-712e056725e0&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=post_page-30bc9ffd2f4b----712e056725e0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----712e056725e0--------------------------------) ·7 分钟阅读·2023 年 10 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F712e056725e0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fknowledge-graph-transformers-architecting-dynamic-reasoning-for-evolving-knowledge-712e056725e0&user=Anthony+Alcaraz&userId=30bc9ffd2f4b&source=-----712e056725e0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F712e056725e0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fknowledge-graph-transformers-architecting-dynamic-reasoning-for-evolving-knowledge-712e056725e0&source=-----712e056725e0---------------------bookmark_footer-----------)

*人工智能软件用于增强本文的语法、流畅性和可读性。*

知识图谱将事实表示为相互关联的实体，已成为增强 AI 系统能力的重要技术，这种技术能够同化和理解知识。

然而，现实世界中的知识不断演变，需要动态表示以捕捉世界的流动性和时效性复杂性。

时间序列知识图谱（TKGs）通过引入时间维度来满足这一需求，每个关系都标记有一个时间戳，表示其有效期。TKGs 不仅允许建模实体之间的连接，还能建模这些关系的动态，为人工智能开辟了新的潜力。

尽管 TKGs 已经引起了大量研究关注，但它们在专业领域的应用仍然是一个开放的前沿。特别是金融领域具有快速发展的市场和多样化的文本数据，这些领域可以从动态知识图谱中获得显著的好处。然而，对高质量金融知识图谱的开发不足限制了这一领域的进展。

针对这一差距，Xiaohui Victor Li（2023）引入了一种创新的开源金融动态知识图谱（FinDKG），其依托于一种名为“Knowledge”的新型时间序列知识图谱学习模型。
