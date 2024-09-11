# 如何使用 Gensim 从零开始训练 Word2Vec 模型

> 原文：[`towardsdatascience.com/how-to-train-a-word2vec-model-from-scratch-with-gensim-c457d587e031?source=collection_archive---------1-----------------------#2023-02-06`](https://towardsdatascience.com/how-to-train-a-word2vec-model-from-scratch-with-gensim-c457d587e031?source=collection_archive---------1-----------------------#2023-02-06)

## *在这篇文章中，我们将深入探讨 Gensim，一个非常流行的 Python 库，用于训练基于文本的机器学习模型，以从头开始训练 Word2Vec 模型*

[](https://medium.com/@theDrewDag?source=post_page-----c457d587e031--------------------------------)![Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----c457d587e031--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c457d587e031--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c457d587e031--------------------------------) [Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----c457d587e031--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e8f67b0b09b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-train-a-word2vec-model-from-scratch-with-gensim-c457d587e031&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=post_page-4e8f67b0b09b----c457d587e031---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c457d587e031--------------------------------) · 9 分钟阅读 · 2023 年 2 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc457d587e031&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-train-a-word2vec-model-from-scratch-with-gensim-c457d587e031&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=-----c457d587e031---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc457d587e031&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-train-a-word2vec-model-from-scratch-with-gensim-c457d587e031&source=-----c457d587e031---------------------bookmark_footer-----------)![](img/f5bacd1ddee7fde33777e0a5a0db4449.png)

作者图片。

Word2Vec 是一种机器学习算法，允许你创建单词的向量表示。

这些表示方式称为**嵌入**，在许多自然语言处理任务中使用，例如词汇聚类、分类和文本生成。

Word2Vec 算法的推出标志着自然语言处理领域的一个新时代，谷歌在 2013 年首次推出了这一算法。

它基于由神经网络在非常大的数据语料库上训练生成的词表示。

**Word2Vec 的输出是向量**，每个训练字典中的单词都有一个，**这些向量有效地捕捉了单词之间的关系。**

在向量空间中相互接近的向量基于上下文具有相似的意义，而远离的向量则具有不同的意义。例如，单词“strong”和“mighty”会很接近，而“strong”和“Paris”在向量空间中会相对较远。
