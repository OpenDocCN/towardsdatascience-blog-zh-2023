# 将文本划分为段落

> 原文：[`towardsdatascience.com/segmenting-text-into-paragraphs-e8bed99b6ebd?source=collection_archive---------6-----------------------#2023-02-25`](https://towardsdatascience.com/segmenting-text-into-paragraphs-e8bed99b6ebd?source=collection_archive---------6-----------------------#2023-02-25)

## 基于监督学习的统计 NLP 方法

[](https://jagota-arun.medium.com/?source=post_page-----e8bed99b6ebd--------------------------------)![Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----e8bed99b6ebd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e8bed99b6ebd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e8bed99b6ebd--------------------------------) [Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----e8bed99b6ebd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fef9ed921edad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsegmenting-text-into-paragraphs-e8bed99b6ebd&user=Arun+Jagota&userId=ef9ed921edad&source=post_page-ef9ed921edad----e8bed99b6ebd---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e8bed99b6ebd--------------------------------) ·11 分钟阅读·2023 年 2 月 25 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe8bed99b6ebd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsegmenting-text-into-paragraphs-e8bed99b6ebd&source=-----e8bed99b6ebd---------------------bookmark_footer-----------)![](img/8e2618a1773575c3a2cb635554d5915f.png)

图片由 [Gordon Johnson](https://pixabay.com/users/gdj-1086657/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4385160) 提供，来源于 [Pixabay](https://pixabay.com/)

在之前的一篇 Medium 文章中，我们讨论了将文本划分为句子的问题 [3]。现在我们来看一个相关的问题：将文本划分为段落。

乍一看，这两个问题似乎本质上是相同的，只是划分的层级不同。实际上，将文本划分为段落的问题要有趣得多。

其一，句子边界有明确的信号，比如句号、问号或感叹号。通常，问题在于：这些出现的标志中哪些是实际的边界，哪些是嵌入在句子中的？这就是误报的问题。

将文本拆分成段落更加复杂。这样考虑：假设你有一长串没有段落分隔的句子，段落边界应该在哪里？这不是一个容易解决的问题，也不一定有唯一的解决方案。这意味着将句子序列拆分成段落可能有多个合理的方式。

将文本拆分成段落可以视为一种特定形式的文本分割[1]。文本段是一个连续的片段，它保持某种连贯性，比如主题一致。根据这种连贯性的衡量标准，一个段落会…
