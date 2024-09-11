# 词嵌入，解读

> 原文：[`towardsdatascience.com/word-embeddings-explained-c07c5ea44d64?source=collection_archive---------10-----------------------#2023-05-30`](https://towardsdatascience.com/word-embeddings-explained-c07c5ea44d64?source=collection_archive---------10-----------------------#2023-05-30)

[](https://medium.com/@dataemporium?source=post_page-----c07c5ea44d64--------------------------------)![Ajay Halthor](https://medium.com/@dataemporium?source=post_page-----c07c5ea44d64--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c07c5ea44d64--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c07c5ea44d64--------------------------------) [Ajay Halthor](https://medium.com/@dataemporium?source=post_page-----c07c5ea44d64--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb0a3e7e495ca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fword-embeddings-explained-c07c5ea44d64&user=Ajay+Halthor&userId=b0a3e7e495ca&source=post_page-b0a3e7e495ca----c07c5ea44d64---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c07c5ea44d64--------------------------------) ·7 分钟阅读·2023 年 5 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc07c5ea44d64&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fword-embeddings-explained-c07c5ea44d64&user=Ajay+Halthor&userId=b0a3e7e495ca&source=-----c07c5ea44d64---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc07c5ea44d64&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fword-embeddings-explained-c07c5ea44d64&source=-----c07c5ea44d64---------------------bookmark_footer-----------)

在自然语言处理领域，我们处理的是词汇。然而，计算机不能直接理解词汇，因此需要将其转换为数值表示。这些数值表示，称为向量或嵌入，由可以被人类解读或无法解读的数字组成。在这篇博客中，我们将深入探讨这些词汇表示的学习进展。

# 1 N-grams

![](img/b72ba194d24fb7590374f82b5317df2e.png)

图 1：句子的 N-gram 向量表示（图由作者提供）

为了更好地理解这个过程，我们以 n-grams 为例。假设我们有一个句子，希望计算机能够理解。为实现这一目标，我们将句子转换为数字表示。这个表示包括各种单词组合，例如 unigrams（单个单词）、bigrams（词对）、trigrams（三词组），以及更高阶的 n-grams。最终得到的结果是一个可以表示任何英文句子的向量。

在图 1 中，我们考虑对句子“This is a good day”进行编码。假设向量的第一个位置表示在原句中 bigram“good day”出现的次数。由于它出现了一次，所以该位置的数字表示为“1”。以同样的方式，我们可以用向量中的不同位置表示每一个 unigram、bigram 和 trigram。
