# 使用 NLP 对文本进行句子分割

> 原文：[`towardsdatascience.com/segmenting-text-into-sentences-using-nlp-35d8ef55c0fd?source=collection_archive---------8-----------------------#2023-01-30`](https://towardsdatascience.com/segmenting-text-into-sentences-using-nlp-35d8ef55c0fd?source=collection_archive---------8-----------------------#2023-01-30)

## **特征工程、统计模型和从反馈中学习**

[](https://jagota-arun.medium.com/?source=post_page-----35d8ef55c0fd--------------------------------)![Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----35d8ef55c0fd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----35d8ef55c0fd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----35d8ef55c0fd--------------------------------) [Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----35d8ef55c0fd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fef9ed921edad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsegmenting-text-into-sentences-using-nlp-35d8ef55c0fd&user=Arun+Jagota&userId=ef9ed921edad&source=post_page-ef9ed921edad----35d8ef55c0fd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----35d8ef55c0fd--------------------------------) · 10 分钟阅读 · 2023 年 1 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F35d8ef55c0fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsegmenting-text-into-sentences-using-nlp-35d8ef55c0fd&user=Arun+Jagota&userId=ef9ed921edad&source=-----35d8ef55c0fd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F35d8ef55c0fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsegmenting-text-into-sentences-using-nlp-35d8ef55c0fd&source=-----35d8ef55c0fd---------------------bookmark_footer-----------)![](img/b2d3d765f44d8d3e011a1660ef32353e.png)

图片来自 [Nile](https://pixabay.com/users/nile-598962/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=623167) 提供的 [Pixabay](https://pixabay.com/)

在 NLP 中，将文本文件分割成句子是一项有用的基本操作。这是许多更复杂的 NLP 任务的第一步。例如，在文本写作过程中检测和修正错误 [1]，或检测命名实体 [2]。

在前者中，常见的错误不会跨越句子边界。这一点在后者中也成立。命名实体也往往不会跨越句子边界。

无论哪种方式，这都会大大简化问题。例如，训练和评估可以基于句子语料库进行。即使是从较长的文档中提取的句子，也可以独立处理。

在这篇文章中，我们将探讨将文本分割成句子的问题。我们将采用一种**苏格拉底式的方法**。这意味着我们将以一种“迭代假设优化”的风格，从零开始逐步建立一个启发式预测器。适当的推测和问题将驱动优化。在这个过程中，ChatGPT 将非常有帮助。

我们最终采用的方法在精神上类似于这个问题的流行 Punkt 算法 [5]。
