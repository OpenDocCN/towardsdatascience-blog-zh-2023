# 使用 LangChain 和 GPT-3 构建透明的问答机器人

> 原文：[`towardsdatascience.com/build-a-transparent-question-answering-bot-for-your-documents-with-langchain-and-gpt-3-7f6a71f379f8?source=collection_archive---------0-----------------------#2023-07-22`](https://towardsdatascience.com/build-a-transparent-question-answering-bot-for-your-documents-with-langchain-and-gpt-3-7f6a71f379f8?source=collection_archive---------0-----------------------#2023-07-22)

## 关于开发信息丰富的问答机器人及其使用的展示源的指南

[](https://konstantin-rink.medium.com/?source=post_page-----7f6a71f379f8--------------------------------)![Konstantin Rink](https://konstantin-rink.medium.com/?source=post_page-----7f6a71f379f8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7f6a71f379f8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7f6a71f379f8--------------------------------) [Konstantin Rink](https://konstantin-rink.medium.com/?source=post_page-----7f6a71f379f8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F337427fde9f0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-transparent-question-answering-bot-for-your-documents-with-langchain-and-gpt-3-7f6a71f379f8&user=Konstantin+Rink&userId=337427fde9f0&source=post_page-337427fde9f0----7f6a71f379f8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7f6a71f379f8--------------------------------) · 11 分钟阅读 · 2023 年 7 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7f6a71f379f8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-transparent-question-answering-bot-for-your-documents-with-langchain-and-gpt-3-7f6a71f379f8&user=Konstantin+Rink&userId=337427fde9f0&source=-----7f6a71f379f8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7f6a71f379f8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-transparent-question-answering-bot-for-your-documents-with-langchain-and-gpt-3-7f6a71f379f8&source=-----7f6a71f379f8---------------------bookmark_footer-----------)![](img/c24d8ab8c013ee7e02974a92cc9beff8.png)

图片由 [Justin Ha](https://unsplash.com/@mekanizm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/de/fotos/XNn3SpMhiNE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

问答系统在分析大量数据或文档时可以提供很大帮助。然而，模型用于生成答案的来源（即文档的部分）通常不会在最终答案中显示出来。

理解响应的背景和来源不仅对寻求准确答案的用户有价值，对希望不断改进其问答机器人的开发者也同样重要。通过在答案中包含来源，开发者可以获得有关模型决策过程的宝贵见解，促进迭代改进和精细调整。

**本文展示了如何使用 LangChain 和 GPT-3（text-davinci-003）创建一个透明的问答机器人，该机器人通过两个示例展示生成答案所使用的来源。**

在第一个例子中，你将学习如何创建一个透明的问答机器人，该机器人利用你的网站内容来回答问题。在第二个例子中，我们将探讨使用来自不同 YouTube 视频的转录内容，包括带有时间戳和不带时间戳的内容。

# 处理数据并创建向量存储
