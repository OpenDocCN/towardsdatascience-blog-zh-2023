# 大型语言模型作为零样本标注器

> 原文：[https://towardsdatascience.com/large-language-models-as-zero-shot-labelers-d26aa2642c88?source=collection_archive---------10-----------------------#2023-03-20](https://towardsdatascience.com/large-language-models-as-zero-shot-labelers-d26aa2642c88?source=collection_archive---------10-----------------------#2023-03-20)

## 使用大型语言模型（LLMs）为监督模型获取标签

[](https://medium.com/@devins?source=post_page-----d26aa2642c88--------------------------------)[![Devin Soni](../Images/ad0e4be26a861f2584abc643daf52203.png)](https://medium.com/@devins?source=post_page-----d26aa2642c88--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d26aa2642c88--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d26aa2642c88--------------------------------) [Devin Soni](https://medium.com/@devins?source=post_page-----d26aa2642c88--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5f4d2b8b896d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flarge-language-models-as-zero-shot-labelers-d26aa2642c88&user=Devin+Soni&userId=5f4d2b8b896d&source=post_page-5f4d2b8b896d----d26aa2642c88---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d26aa2642c88--------------------------------) ·5 min read·2023年3月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd26aa2642c88&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flarge-language-models-as-zero-shot-labelers-d26aa2642c88&user=Devin+Soni&userId=5f4d2b8b896d&source=-----d26aa2642c88---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd26aa2642c88&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flarge-language-models-as-zero-shot-labelers-d26aa2642c88&source=-----d26aa2642c88---------------------bookmark_footer-----------)![](../Images/4517fd2ff5ae2bea5a595bff399475b2.png)

由 [h heyerlein](https://unsplash.com/es/@heyerlein?utm_source=medium&utm_medium=referral) 拍摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

标注数据是构建监督机器学习模型的关键步骤，因为标签的数量和质量通常是决定模型性能的主要因素。

然而，标注数据可能非常耗时且昂贵，特别是对于涉及领域知识或需要阅读大量数据的复杂任务。

最近几年，大型语言模型（LLMs）已经成为获取文本数据标签的强大解决方案。通过零样本学习，我们可以仅通过LLM的输出获取未标记数据的标签，而无需人类来获取这些标签。这可以显著降低获取标签的成本，并使得这一过程更加可扩展。

在这篇文章中，我们将进一步探讨零样本学习的概念以及如何使用LLM实现这一目的。

# 什么是零样本学习？

![](../Images/e6b0b7b78d31241482486c84fdd7e723.png)

照片由 [Alex Knight](https://unsplash.com/@agk42?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上

零样本学习（ZSL）是机器学习中的一个问题设置，在这个设置中模型是…
