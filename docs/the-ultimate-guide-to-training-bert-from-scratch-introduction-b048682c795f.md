# 从零开始训练 BERT 的终极指南：引言

> 原文：[`towardsdatascience.com/the-ultimate-guide-to-training-bert-from-scratch-introduction-b048682c795f?source=collection_archive---------2-----------------------#2023-09-02`](https://towardsdatascience.com/the-ultimate-guide-to-training-bert-from-scratch-introduction-b048682c795f?source=collection_archive---------2-----------------------#2023-09-02)

## 解密 BERT：定义及该模型在 NLP 领域的多种应用。

[](https://dpoulopoulos.medium.com/?source=post_page-----b048682c795f--------------------------------)![Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----b048682c795f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b048682c795f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b048682c795f--------------------------------) [Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----b048682c795f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cc87df5b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-guide-to-training-bert-from-scratch-introduction-b048682c795f&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=post_page-7cc87df5b1----b048682c795f---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----b048682c795f--------------------------------) ·10 分钟阅读·2023 年 9 月 2 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb048682c795f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-guide-to-training-bert-from-scratch-introduction-b048682c795f&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=-----b048682c795f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb048682c795f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-guide-to-training-bert-from-scratch-introduction-b048682c795f&source=-----b048682c795f---------------------bookmark_footer-----------)![](img/f9d976bd1bc99fc0737ffec178ea1be4.png)

图片由[Ryan Wallace](https://unsplash.com/@accrualbowtie?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 第二部分 和 第三部分 的内容现已上线。

几周前，我使用检索增强生成（RAG）训练并部署了我自己的问答系统。目标是将这样一个系统引入我的学习笔记，并创建一个代理来帮助我连接各个知识点。在这些特定应用中，LangChain 确实表现得非常出色：

由于系统的质量令我惊叹，我忍不住深入探索以理解其背后的魔法。RAG 管道的一个特性是它能够从大量信息中筛选出最相关的上下文。虽然听起来复杂，但它从一个简单而强大的过程开始：将句子编码成信息密集的向量。

免费创建这些句子嵌入的最受欢迎的方式非 SBERT 莫属，它是一个 [句子转换器](https://www.sbert.net/)...
