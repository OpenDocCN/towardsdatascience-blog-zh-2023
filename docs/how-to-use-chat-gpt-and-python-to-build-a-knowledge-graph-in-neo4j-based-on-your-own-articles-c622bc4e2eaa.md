# 如何使用Chat-GPT和Python在Neo4j中基于自己的文章构建知识图谱

> 原文：[https://towardsdatascience.com/how-to-use-chat-gpt-and-python-to-build-a-knowledge-graph-in-neo4j-based-on-your-own-articles-c622bc4e2eaa?source=collection_archive---------0-----------------------#2023-08-26](https://towardsdatascience.com/how-to-use-chat-gpt-and-python-to-build-a-knowledge-graph-in-neo4j-based-on-your-own-articles-c622bc4e2eaa?source=collection_archive---------0-----------------------#2023-08-26)

## 一个包含超过120篇关于数学和数据科学的文章结构化知识的图表

[](https://kaspermuller.medium.com/?source=post_page-----c622bc4e2eaa--------------------------------)[![Kasper Müller](../Images/336cad595544ba68e3376a038d44df62.png)](https://kaspermuller.medium.com/?source=post_page-----c622bc4e2eaa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c622bc4e2eaa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c622bc4e2eaa--------------------------------) [Kasper Müller](https://kaspermuller.medium.com/?source=post_page-----c622bc4e2eaa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F877b7991f678&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-chat-gpt-and-python-to-build-a-knowledge-graph-in-neo4j-based-on-your-own-articles-c622bc4e2eaa&user=Kasper+M%C3%BCller&userId=877b7991f678&source=post_page-877b7991f678----c622bc4e2eaa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c622bc4e2eaa--------------------------------) ·8分钟阅读·2023年8月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc622bc4e2eaa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-chat-gpt-and-python-to-build-a-knowledge-graph-in-neo4j-based-on-your-own-articles-c622bc4e2eaa&user=Kasper+M%C3%BCller&userId=877b7991f678&source=-----c622bc4e2eaa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc622bc4e2eaa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-chat-gpt-and-python-to-build-a-knowledge-graph-in-neo4j-based-on-your-own-articles-c622bc4e2eaa&source=-----c622bc4e2eaa---------------------bookmark_footer-----------)![](../Images/c43ea8409a8a04894916f41afe7075dc.png)

作者截图

在这篇文章中，我将展示如何利用图表技术和一些编程来结构化和探索自己文章的内容。

使用 NLP 技术来结构化非结构化数据的想法并不新鲜，然而，最新的 LLM（大型语言模型）的进展已经带来了无数机会来实现这一点。通过蓬勃发展的技术 Chat-GPT，业余爱好者的可及性引起了很多人对 LLM 和生成模型的关注。

实际上，生成式 AI 已经在许多公司上议程上了！

我们将通过编程语言 Python 使用 OpenAI 的开发者 API 来处理本文中的技术。我们将处理来自 Medium 的数据（有点自我反省的意思？）并构建一个知识图谱。这可能听起来有些复杂，但实际上开始起来非常简单。

# 开始使用

首先，攻击计划如下。

1.  使 API 工作并通过 Python 访问它。
