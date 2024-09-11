# 利用类似GPT的LLMs分析您的文档或记录

> 原文：[https://towardsdatascience.com/leverage-llms-like-gpt-to-analyze-your-documents-or-transcripts-c640a266ad52?source=collection_archive---------1-----------------------#2023-03-31](https://towardsdatascience.com/leverage-llms-like-gpt-to-analyze-your-documents-or-transcripts-c640a266ad52?source=collection_archive---------1-----------------------#2023-03-31)

## 使用提示工程以类似ChatGPT的方式通过langchain和openai分析您的文档

[](https://konstantin-rink.medium.com/?source=post_page-----c640a266ad52--------------------------------)[![Konstantin Rink](../Images/41bfc069d7382a0fd56f081eea7eb2d9.png)](https://konstantin-rink.medium.com/?source=post_page-----c640a266ad52--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c640a266ad52--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c640a266ad52--------------------------------) [Konstantin Rink](https://konstantin-rink.medium.com/?source=post_page-----c640a266ad52--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F337427fde9f0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleverage-llms-like-gpt-to-analyze-your-documents-or-transcripts-c640a266ad52&user=Konstantin+Rink&userId=337427fde9f0&source=post_page-337427fde9f0----c640a266ad52---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c640a266ad52--------------------------------) ·6分钟阅读·2023年3月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc640a266ad52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleverage-llms-like-gpt-to-analyze-your-documents-or-transcripts-c640a266ad52&user=Konstantin+Rink&userId=337427fde9f0&source=-----c640a266ad52---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc640a266ad52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleverage-llms-like-gpt-to-analyze-your-documents-or-transcripts-c640a266ad52&source=-----c640a266ad52---------------------bookmark_footer-----------)![](../Images/7dd6687f2e85da1c40832aaf26002fc7.png)

（原始）照片由 [Laura Rivera](https://unsplash.com/@laurar1vera?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，发布在 [Unsplash](https://unsplash.com/de/fotos/9ZQzrLWV52M?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

ChatGPT 无疑是最受欢迎的大型语言模型（LLMs）之一。自2022年底发布测试版以来，大家可以使用便捷的聊天功能提问或与语言模型互动。

**但是如果我们想要向 ChatGPT 提问关于我们自己的文档或我们刚刚听过的播客内容怎么办？**

本文的目标是展示如何利用像 GPT 这样的 LLMs 来分析我们的文档或记录，然后以 ChatGPT 的方式提出问题并获得关于文档内容的答案。

# tl;dr

+   本文使用了 OpenAI 的 ChatGPT `gpt-3.5-turbo` 模型，这需要一个 [API 密钥](https://platform.openai.com/account/api-keys)。

+   `langchain` 包是一个围绕 LLMs 构建的框架，用于加载和处理我们的文档（Prompt Engineering）以及与模型互动。

+   包含本文所有代码的 colab notebook 可以在 [这里](https://colab.research.google.com/drive/1oG6TXgXJTd8qyCj_ncD0Uy1pGEDMlly3?usp=sharing) 找到。

# 前提条件

在编写所有代码之前，我们必须确保所有必要的包都已安装…
