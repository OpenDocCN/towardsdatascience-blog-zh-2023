# 使用 TfidfVectorizer 将文本转换为数字形式：逐步指南

> 原文：[https://towardsdatascience.com/converting-texts-to-numeric-form-with-tfidfvectorizer-a-step-by-step-guide-bb9330562ae3?source=collection_archive---------7-----------------------#2023-10-25](https://towardsdatascience.com/converting-texts-to-numeric-form-with-tfidfvectorizer-a-step-by-step-guide-bb9330562ae3?source=collection_archive---------7-----------------------#2023-10-25)

![](../Images/d2844dbee6bc63d8e734f8a44ae8298f.png)

图片由 [Mohamed Nohassi](https://unsplash.com/@coopery?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 如何手动计算 Tfidf 值以及使用 sklearn

[](https://rashida00.medium.com/?source=post_page-----bb9330562ae3--------------------------------)[![Rashida Nasrin Sucky](../Images/42bd057e8eca255907c43c29a498f2ca.png)](https://rashida00.medium.com/?source=post_page-----bb9330562ae3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bb9330562ae3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bb9330562ae3--------------------------------) [Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----bb9330562ae3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a36b941a136&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconverting-texts-to-numeric-form-with-tfidfvectorizer-a-step-by-step-guide-bb9330562ae3&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=post_page-8a36b941a136----bb9330562ae3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb9330562ae3--------------------------------) ·6 分钟阅读·2023年10月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbb9330562ae3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconverting-texts-to-numeric-form-with-tfidfvectorizer-a-step-by-step-guide-bb9330562ae3&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=-----bb9330562ae3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbb9330562ae3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconverting-texts-to-numeric-form-with-tfidfvectorizer-a-step-by-step-guide-bb9330562ae3&source=-----bb9330562ae3---------------------bookmark_footer-----------)

TFIDF是将文本转换为机器学习或AI模型中的数值形式的方法。换句话说，TFIDF是从文本中提取特征的方法。这是一种比我在[上一篇文章](/countvectorizer-to-extract-features-from-texts-in-python-in-detail-0e7147c10753)中讨论的CountVectorizer()方法更为复杂的方法。

TFIDF方法为每个单词提供一个分数，代表该单词的有用性或相关性。它衡量了该单词的使用与文档中其他单词的比较。

本文将手动计算TFIDF分数，以便您清楚地了解TFIDF的概念。在接近结尾时，我们将看到如何使用sklearn库中的TFIDF向量化器。

它有两个部分：TF 和 IDF。让我们看看每个部分是如何工作的。

> **TF**

TF详细说明了‘词频’。可以计算TF如下：

TF = 单词在文档中出现的次数

或

TF = （文档中的出现次数） / （文档中的单词总数）

让我们来举个例子。我们将为本文档中的每个单词找到TF：
