# 如何利用 LLMs 自动化从 PDF 中提取实体

> 原文：[https://towardsdatascience.com/how-to-automate-entity-extraction-from-pdf-using-llms-ea9c1351f531?source=collection_archive---------1-----------------------#2023-06-15](https://towardsdatascience.com/how-to-automate-entity-extraction-from-pdf-using-llms-ea9c1351f531?source=collection_archive---------1-----------------------#2023-06-15)

## 利用零样本标注

[![Walid Amamou](../Images/c5ae089c59a5ff070f0f90ad63ee3817.png)](https://walidamamou.medium.com/?source=post_page-----ea9c1351f531--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ea9c1351f531--------------------------------) [Walid Amamou](https://walidamamou.medium.com/?source=post_page-----ea9c1351f531--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F706f7e2641d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-automate-entity-extraction-from-pdf-using-llms-ea9c1351f531&user=Walid+Amamou&userId=706f7e2641d7&source=post_page-706f7e2641d7----ea9c1351f531---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea9c1351f531--------------------------------) ·5 分钟阅读·2023 年 6 月 15 日

--

![](../Images/1fdba51610cfa80948066af630a9b08a.png)

照片由 [Google DeepMind](https://unsplash.com/@deepmind?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/large-language-AI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在现代机器学习应用中，高质量标注数据的重要性无法过分强调。从提高我们模型的性能到确保公平性，标注数据的力量是巨大的。不幸的是，创建这些数据集所需的时间和精力同样巨大。但如果我们能将这项任务所需的时间从数天减少到仅仅几小时，同时保持或甚至提高标注质量呢？这是不是一个乌托邦式的梦想？不再是了。

机器学习中的新兴范式——零样本学习、少样本学习和模型辅助标注——为这一关键过程提供了变革性的方案。这些技术利用先进算法的力量，减少了对大量标注数据集的需求，实现了更快、更高效且效果显著的数据标注。

在本教程中，我们将介绍一种利用大型语言模型（LLM）在上下文学习能力来自动标注非结构化和半结构化文档的方法。

# 从SDS中提取信息

与传统的监督模型需要大量标注数据来训练以解决特定任务不同，LLM能够从少量数据中进行泛化和推断……
