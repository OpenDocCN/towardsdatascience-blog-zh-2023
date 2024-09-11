# 理解 TF-IDF：自然语言处理中特征抽取的传统方法

> 原文：[https://towardsdatascience.com/understanding-tf-idf-a-traditional-approach-to-feature-extraction-in-nlp-a5bfbe04723f?source=collection_archive---------3-----------------------#2023-03-30](https://towardsdatascience.com/understanding-tf-idf-a-traditional-approach-to-feature-extraction-in-nlp-a5bfbe04723f?source=collection_archive---------3-----------------------#2023-03-30)

## 学习 TF-IDF 的基础知识以及如何在 Python 中从头开始实现

[](https://itsuncheng.medium.com/?source=post_page-----a5bfbe04723f--------------------------------)[![Raymond Cheng](../Images/09f3d83726274188c58f8eebc79abb0e.png)](https://itsuncheng.medium.com/?source=post_page-----a5bfbe04723f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a5bfbe04723f--------------------------------)[![数据科学进阶](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a5bfbe04723f--------------------------------) [Raymond Cheng](https://itsuncheng.medium.com/?source=post_page-----a5bfbe04723f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4c697cd55840&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-tf-idf-a-traditional-approach-to-feature-extraction-in-nlp-a5bfbe04723f&user=Raymond+Cheng&userId=4c697cd55840&source=post_page-4c697cd55840----a5bfbe04723f---------------------post_header-----------) 发表在[数据科学进阶](https://towardsdatascience.com/?source=post_page-----a5bfbe04723f--------------------------------) ·9 分钟阅读·2023年3月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa5bfbe04723f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-tf-idf-a-traditional-approach-to-feature-extraction-in-nlp-a5bfbe04723f&user=Raymond+Cheng&userId=4c697cd55840&source=-----a5bfbe04723f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa5bfbe04723f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-tf-idf-a-traditional-approach-to-feature-extraction-in-nlp-a5bfbe04723f&source=-----a5bfbe04723f---------------------bookmark_footer-----------)![](../Images/93fb9b8062226265d6274ca8a0717cfe.png)

照片作者[Aaron Burden](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

特征提取是[NLP](https://en.m.wikipedia.org/wiki/Natural_language_processing)中的一个重要初步步骤，它涉及将文本数据转换为数学表示，通常是以向量形式存在的[词嵌入](https://en.wikipedia.org/wiki/Word_embedding)。存在各种词嵌入方法，从经典方法如[word2vec](https://proceedings.neurips.cc/paper/2013/file/9aa42b31882ec039965f3c4923ce901b-Paper.pdf)和[GloVe](https://nlp.stanford.edu/pubs/glove.pdf)到更现代的[BERT](https://arxiv.org/pdf/1810.04805.pdf)嵌入。虽然基于[transformer](https://arxiv.org/pdf/1706.03762.pdf)的嵌入今天主导了NLP领域，但了解之前方法的发展是有帮助的。

在本文中，我们将探讨一种更传统的特征提取方法——[TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)，这是一种基于统计分析的技术。我们将深入了解TF-IDF及其实现，并提供一个额外的应用来帮助巩固你的理解。所以，请跟随我们直到最后，一起揭开TF-IDF的细节吧！

# 什么是TF-IDF？

TF-IDF，全称是词频-逆文档频率，是NLP中常用的技术，用于确定单词在文档或语料库中的重要性。为了提供一些背景，2015年进行的一项[调查](http://nbn-resolving.de/urn:nbn:de:bsz:352-0-311312)显示83%…
