# 微调句子变换器与 MNR 损失

> 原文：[`towardsdatascience.com/fine-tuning-sentence-transformers-with-mnr-loss-cd6a26685b81?source=collection_archive---------5-----------------------#2023-02-03`](https://towardsdatascience.com/fine-tuning-sentence-transformers-with-mnr-loss-cd6a26685b81?source=collection_archive---------5-----------------------#2023-02-03)

## [NLP 语义搜索](https://jamescalam.medium.com/list/nlp-for-semantic-search-d3a4b96a52fe)

## 下一代句子嵌入与多负样本排序损失

[](https://jamescalam.medium.com/?source=post_page-----cd6a26685b81--------------------------------)![James Briggs](https://jamescalam.medium.com/?source=post_page-----cd6a26685b81--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cd6a26685b81--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cd6a26685b81--------------------------------) [James Briggs](https://jamescalam.medium.com/?source=post_page-----cd6a26685b81--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb9d77a4ca1d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tuning-sentence-transformers-with-mnr-loss-cd6a26685b81&user=James+Briggs&userId=b9d77a4ca1d1&source=post_page-b9d77a4ca1d1----cd6a26685b81---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cd6a26685b81--------------------------------) ·8 分钟阅读·2023 年 2 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcd6a26685b81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tuning-sentence-transformers-with-mnr-loss-cd6a26685b81&user=James+Briggs&userId=b9d77a4ca1d1&source=-----cd6a26685b81---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcd6a26685b81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tuning-sentence-transformers-with-mnr-loss-cd6a26685b81&source=-----cd6a26685b81---------------------bookmark_footer-----------)![](img/1281f41ecb283e0989e5a4230b2c20f6.png)

图片由 [Lars Kienle](https://unsplash.com/@larskienle?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。 [文章最初发布于 pinecone.io](https://www.pinecone.io/learn/fine-tune-sentence-transformers-mnr/) 作者在该网站工作。

Transformer 生成的句子嵌入在短时间内取得了长足的进步。从 BERT 交叉编码器的缓慢但准确的相似性预测开始，随着 2019 年 SBERT 的引入，[句子嵌入](https://apps-showcase--optimistic-curran-b817a8.netlify.app/learn/sentence-embeddings/)的世界被点燃。此后，许多新的句子变换器相继问世。这些模型很快使得最初的 SBERT 过时。

这些新型句子变换器如何迅速超越 SBERT？答案是*多重负样本排序（MNR）损失*。

本文将涵盖 MNR 损失是什么、所需数据以及如何实施它来微调我们自己的高质量句子变换器。

实施将涵盖两种训练方法。第一种方法更为详细，概述了微调模型的确切步骤。第二种方法利用了`sentence-transformers`库中出色的微调工具。
