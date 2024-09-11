# 句子变换器：伪装中的意义

> 原文：[https://towardsdatascience.com/sentence-transformers-meanings-in-disguise-323cf6ac1e52?source=collection_archive---------10-----------------------#2023-01-03](https://towardsdatascience.com/sentence-transformers-meanings-in-disguise-323cf6ac1e52?source=collection_archive---------10-----------------------#2023-01-03)

## [NLP语义搜索](https://jamescalam.medium.com/list/nlp-for-semantic-search-d3a4b96a52fe)

## 现代语言模型如何捕捉意义

[](https://jamescalam.medium.com/?source=post_page-----323cf6ac1e52--------------------------------)[![James Briggs](../Images/cb34b7011748e4d8607b7ff4a8510a93.png)](https://jamescalam.medium.com/?source=post_page-----323cf6ac1e52--------------------------------)[](https://towardsdatascience.com/?source=post_page-----323cf6ac1e52--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----323cf6ac1e52--------------------------------) [James Briggs](https://jamescalam.medium.com/?source=post_page-----323cf6ac1e52--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb9d77a4ca1d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsentence-transformers-meanings-in-disguise-323cf6ac1e52&user=James+Briggs&userId=b9d77a4ca1d1&source=post_page-b9d77a4ca1d1----323cf6ac1e52---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----323cf6ac1e52--------------------------------) · 12分钟阅读·2023年1月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F323cf6ac1e52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsentence-transformers-meanings-in-disguise-323cf6ac1e52&user=James+Briggs&userId=b9d77a4ca1d1&source=-----323cf6ac1e52---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F323cf6ac1e52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsentence-transformers-meanings-in-disguise-323cf6ac1e52&source=-----323cf6ac1e52---------------------bookmark_footer-----------)![](../Images/edabd3baac27a7fd707b7855ea93c5c2.png)

图片由 [Brian Suh](https://unsplash.com/@_briansuh?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。最初发布在 Pinecone 的 [NLP for Semantic Search 电子书](https://www.pinecone.io/learn/sentence-embeddings/) 中（作者在此工作）。

变换器彻底重塑了自然语言处理（NLP）的格局。在变换器出现之前，我们凭借递归神经网络（RNNs）有了*尚可*的翻译和语言分类 —— 它们的语言理解能力有限，导致许多小错误，并且在处理较大文本块时几乎不可能保持连贯。

自从2017年论文*《注意力机制是你所需的一切》* [1] 中首次介绍了变换器模型以来，NLP 从 RNNs 转向了像 BERT 和 GPT 这样的模型。这些新模型可以回答问题，撰写文章*（也许是 GPT-3 写的这篇）*，实现极为直观的语义搜索——还有更多功能。

有趣的是，对于许多任务，这些模型的后半部分与 RNNs 中的部分相同——通常是几个前馈神经网络，用于输出模型预测。

变化的是这些层的*输入*。由变换器模型创建的[稠密嵌入](https://www.pinecone.io/learn/dense-vector-embeddings-nlp/)信息更加丰富，因此即使使用相同的最终外层，我们也能获得巨大的性能提升。

这些日益丰富的句子嵌入可以用于快速比较句子相似度，以满足各种使用场景。例如：
