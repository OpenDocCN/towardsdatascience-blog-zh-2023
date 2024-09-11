# 使用 BERT 计算语义文本相似度

> 原文：[https://towardsdatascience.com/semantic-textual-similarity-with-bert-fc800656e7a3?source=collection_archive---------1-----------------------#2023-02-15](https://towardsdatascience.com/semantic-textual-similarity-with-bert-fc800656e7a3?source=collection_archive---------1-----------------------#2023-02-15)

## 如何使用 BERT 计算两个文本之间的语义相似度

[](https://medium.com/@marcellusruben?source=post_page-----fc800656e7a3--------------------------------)[![Ruben Winastwan](../Images/15ad0dd03bf5892510abdf166a1e91e1.png)](https://medium.com/@marcellusruben?source=post_page-----fc800656e7a3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fc800656e7a3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fc800656e7a3--------------------------------) [Ruben Winastwan](https://medium.com/@marcellusruben?source=post_page-----fc800656e7a3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5dae9da73c9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsemantic-textual-similarity-with-bert-fc800656e7a3&user=Ruben+Winastwan&userId=5dae9da73c9b&source=post_page-5dae9da73c9b----fc800656e7a3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fc800656e7a3--------------------------------) ·11 分钟阅读·2023年2月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffc800656e7a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsemantic-textual-similarity-with-bert-fc800656e7a3&user=Ruben+Winastwan&userId=5dae9da73c9b&source=-----fc800656e7a3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffc800656e7a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsemantic-textual-similarity-with-bert-fc800656e7a3&source=-----fc800656e7a3---------------------bookmark_footer-----------)![](../Images/618fe6986fde72b9eaae6d126ce78716.png)

图片来源：Leeloo Thefirst: [https://www.pexels.com/photo/brown-wooden-ruler-and-colored-pencils-on-papers-8970296/](https://www.pexels.com/photo/brown-wooden-ruler-and-colored-pencils-on-papers-8970296/)

自2017年由Google Brain团队首次提出以来，Transformer模型迅速成为计算机视觉和自然语言处理领域的**最先进**模型。其卓越的性能促使了多个**最先进**模型的开发，如BERT及其变体distilBERT和RoBERTa。

BERT 在各种自然语言处理任务中表现优于旧的递归模型，例如文本分类、命名实体识别（NER）、问答系统，甚至是我们在本文中要重点关注的任务，即语义文本相似度（STS）。

因此，在本文中，我们将看到如何利用 Sentence Transformers 库训练一个 BERT 模型来完成 STS 任务。接下来，我们将使用训练好的模型来预测未知数据。但作为开始，我们需要首先了解 STS 任务到底是什么，以及我们将用于此任务的数据集。

# 语义文本相似度及数据集

语义文本相似度（STS）是指将一个文本与另一个文本进行相似度比较的任务。
