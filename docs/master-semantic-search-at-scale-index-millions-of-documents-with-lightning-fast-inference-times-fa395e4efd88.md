# 掌握大规模语义搜索：使用 FAISS 和句子变换器以闪电般的推断速度索引数百万份文档

> 原文：[`towardsdatascience.com/master-semantic-search-at-scale-index-millions-of-documents-with-lightning-fast-inference-times-fa395e4efd88?source=collection_archive---------2-----------------------#2023-03-31`](https://towardsdatascience.com/master-semantic-search-at-scale-index-millions-of-documents-with-lightning-fast-inference-times-fa395e4efd88?source=collection_archive---------2-----------------------#2023-03-31)

## 深入了解一个高性能语义搜索引擎的端到端演示，该引擎利用 GPU 加速、有效的索引技术和强大的句子编码器，处理多达 100 万份文档的数据集，实现 50 毫秒的推断时间

[](https://medium.com/@luisroque?source=post_page-----fa395e4efd88--------------------------------)![路易斯·罗克](https://medium.com/@luisroque?source=post_page-----fa395e4efd88--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fa395e4efd88--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fa395e4efd88--------------------------------) [路易斯·罗克](https://medium.com/@luisroque?source=post_page-----fa395e4efd88--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaster-semantic-search-at-scale-index-millions-of-documents-with-lightning-fast-inference-times-fa395e4efd88&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----fa395e4efd88---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fa395e4efd88--------------------------------) ·15 分钟阅读·2023 年 3 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffa395e4efd88&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaster-semantic-search-at-scale-index-millions-of-documents-with-lightning-fast-inference-times-fa395e4efd88&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----fa395e4efd88---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffa395e4efd88&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaster-semantic-search-at-scale-index-millions-of-documents-with-lightning-fast-inference-times-fa395e4efd88&source=-----fa395e4efd88---------------------bookmark_footer-----------)

# 介绍

在搜索和信息检索领域，语义搜索已经成为一个颠覆性的技术。它允许我们基于文档的意义或概念而不仅仅是关键词匹配来进行搜索和检索。与传统的基于关键词的搜索方法相比，语义搜索能带来更复杂和相关的结果。然而，挑战在于如何扩展语义搜索，以处理大量文档，而不被分析每个文档语义内容的计算复杂性所困扰。

在本文中，我们通过利用两种前沿技术的力量来应对实现可扩展的语义搜索的挑战：FAISS 用于高效地对语义向量进行索引，以及 Sentence Transformers 用于将句子编码成这些向量。FAISS 是一个杰出的库，旨在快速检索高维空间中的最近邻，即使在大规模情况下也能实现快速的语义最近邻搜索……
