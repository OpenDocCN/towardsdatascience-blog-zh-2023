# 从零开始训练 BERT 的终极指南：标记器

> 原文：[https://towardsdatascience.com/the-ultimate-guide-to-training-bert-from-scratch-the-tokenizer-ddf30f124822?source=collection_archive---------1-----------------------#2023-09-06](https://towardsdatascience.com/the-ultimate-guide-to-training-bert-from-scratch-the-tokenizer-ddf30f124822?source=collection_archive---------1-----------------------#2023-09-06)

## 从文本到标记：BERT 标记化的逐步指南

[](https://dpoulopoulos.medium.com/?source=post_page-----ddf30f124822--------------------------------)[![Dimitris Poulopoulos](../Images/ce535a1679779f5a2ec8b024e6691e50.png)](https://dpoulopoulos.medium.com/?source=post_page-----ddf30f124822--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ddf30f124822--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ddf30f124822--------------------------------) [Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----ddf30f124822--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cc87df5b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-guide-to-training-bert-from-scratch-the-tokenizer-ddf30f124822&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=post_page-7cc87df5b1----ddf30f124822---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ddf30f124822--------------------------------) ·13 分钟阅读·2023年9月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fddf30f124822&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-guide-to-training-bert-from-scratch-the-tokenizer-ddf30f124822&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=-----ddf30f124822---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fddf30f124822&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-guide-to-training-bert-from-scratch-the-tokenizer-ddf30f124822&source=-----ddf30f124822---------------------bookmark_footer-----------)![](../Images/5e6bf3a97ca45029e42154b1648e0172.png)

图片由 [Glen Carrie](https://unsplash.com/@glencarrie?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/oHoBIbDj7lo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> 这个故事的 [第一部分](/the-ultimate-guide-to-training-bert-from-scratch-introduction-b048682c795f)、[第三部分](/the-ultimate-guide-to-training-bert-from-scratch-prepare-the-dataset-beaae6febfd5) 和 [第四部分](/the-ultimate-guide-to-training-bert-from-scratch-final-act-eab78b0657bb) 现在已经上线。

你知道吗，文本的分词方式可能决定你的语言模型的成败？你是否曾希望对稀有语言或专业领域的文档进行分词？将文本分割成标记，不仅仅是一项任务；*它是将语言转化为可操作智能的关键。* 本故事将教你有关分词的一切知识，不仅是为了 BERT，也适用于任何 LLM。

在我上一个故事中，我们讨论了 BERT，探讨了它的理论基础和训练机制，并讨论了如何微调它并创建问答系统。现在，当我们深入探讨这个开创性模型的复杂性时，是时候重点介绍其中一个被低估的英雄：*分词*。

[](/the-ultimate-guide-to-training-bert-from-scratch-introduction-b048682c795f?source=post_page-----ddf30f124822--------------------------------) [## 从头训练 BERT 的终极指南：介绍

### 揭开 BERT 的神秘面纱：定义及该模型在 NLP 领域中的各种应用。

towardsdatascience.com](/the-ultimate-guide-to-training-bert-from-scratch-introduction-b048682c795f?source=post_page-----ddf30f124822--------------------------------)

> [第三部分](/the-ultimate-guide-to-training-bert-from-scratch-prepare-the-dataset-beaae6febfd5) 的故事现在已经上线。
