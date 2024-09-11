# 训练句子变换器与 Softmax 损失

> 原文：[`towardsdatascience.com/training-sentence-transformers-with-softmax-loss-c114e74e1583?source=collection_archive---------17-----------------------#2023-01-12`](https://towardsdatascience.com/training-sentence-transformers-with-softmax-loss-c114e74e1583?source=collection_archive---------17-----------------------#2023-01-12)

## [NLP 语义搜索](https://jamescalam.medium.com/list/nlp-for-semantic-search-d3a4b96a52fe)

## 如何构建原始句子变换器（SBERT）

![James Briggs](https://jamescalam.medium.com/?source=post_page-----c114e74e1583--------------------------------) ![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c114e74e1583--------------------------------) [James Briggs](https://jamescalam.medium.com/?source=post_page-----c114e74e1583--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb9d77a4ca1d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-sentence-transformers-with-softmax-loss-c114e74e1583&user=James+Briggs&userId=b9d77a4ca1d1&source=post_page-b9d77a4ca1d1----c114e74e1583---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c114e74e1583--------------------------------) ·9 min read·2023 年 1 月 12 日

--

![](img/e9e629a36ae10fd37e92fbd2fa3327d2.png) 

照片由 [Possessed Photography](https://unsplash.com/@possessedphotography?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上。原文发布于 [NLP for Semantic Search ebook](https://www.pinecone.io/learn/sentence-embeddings/) 在 Pinecone（作者的工作单位）。

搜索正进入黄金时代。得益于*“句子嵌入”*和特别训练的模型*“句子转换器”*，我们现在可以通过概念而非关键词匹配来搜索信息，从而解锁类人信息发现过程。

本文将探讨第一个句子转换器的训练过程，即 *sentence-BERT*——更常被称为 *SBERT*。我们将深入探讨**N**atural **L**anguage **I**nference (NLI) 训练方法中的 *softmax 损失*，以微调模型以生成句子嵌入。

请注意，softmax 损失不再是训练句子转换器的首选方法，而是被 MSE margin 和 [多负样本排序损失](https://www.pinecone.io/learn/fine-tune-sentence-transformers-mnr/) 等其他方法取代。但我们仍将这一训练方法作为句子嵌入不断改进过程中的一个重要里程碑。

本文还涵盖了*两种方法*来进行微调。第一种方法展示了 softmax 损失的 NLI 训练如何工作。第二种方法使用了 `sentence-transformers` 库提供的优秀训练工具——它更具抽象性，使得构建良好的句子转换器模型*容易得多*。
