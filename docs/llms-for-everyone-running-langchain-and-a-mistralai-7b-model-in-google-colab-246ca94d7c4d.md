# 面向所有人的 LLM：在 Google Colab 中运行 LangChain 和 MistralAI 7B 模型

> 原文：[`towardsdatascience.com/llms-for-everyone-running-langchain-and-a-mistralai-7b-model-in-google-colab-246ca94d7c4d?source=collection_archive---------0-----------------------#2023-12-05`](https://towardsdatascience.com/llms-for-everyone-running-langchain-and-a-mistralai-7b-model-in-google-colab-246ca94d7c4d?source=collection_archive---------0-----------------------#2023-12-05)

## 免费实验大型语言模型

[](https://dmitryelj.medium.com/?source=post_page-----246ca94d7c4d--------------------------------)![Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----246ca94d7c4d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----246ca94d7c4d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----246ca94d7c4d--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----246ca94d7c4d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllms-for-everyone-running-langchain-and-a-mistralai-7b-model-in-google-colab-246ca94d7c4d&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----246ca94d7c4d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----246ca94d7c4d--------------------------------) ·10 分钟阅读·2023 年 12 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F246ca94d7c4d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllms-for-everyone-running-langchain-and-a-mistralai-7b-model-in-google-colab-246ca94d7c4d&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=-----246ca94d7c4d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F246ca94d7c4d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllms-for-everyone-running-langchain-and-a-mistralai-7b-model-in-google-colab-246ca94d7c4d&source=-----246ca94d7c4d---------------------bookmark_footer-----------)![](img/260367e3c69b2a0784e0677fcb3983bb.png)

LangChain 的艺术表现，照片由 Ruan Richard Rodrigues 提供，[Unsplash](https://unsplash.com/@heeybooy)

大家都知道，大型语言模型本质上是大型的。甚至不久前，它们仅对高端硬件的拥有者开放，或者至少对那些支付云访问费用或每次 API 调用费用的人开放。现在，时代在变化。本文中，我将展示如何在 Google Colab 中完全免费运行 LangChain Python 库、FAISS 向量数据库和 Mistral-7B 模型，并进行一些有趣的实验。

## 组件

这里的 TDS 上有许多关于在 Python 中使用大型语言模型的文章，但通常很难重现这些文章中的内容。例如，许多使用 LangChain 库的示例使用了一个 [OpenAI](https://python.langchain.com/docs/integrations/llms/openai) 类，其中第一个参数（猜猜是什么？）是 OPENAI_API_KEY。一些其他的 RAG（检索增强生成）和向量数据库示例使用了 Weaviate；我们打开他们的网站后第一个看到的就是“定价”。在这里，我将使用一组完全可以免费使用的开源库：

+   [LangChain](https://github.com/langchain-ai/langchain) 是一个用于开发语言模型驱动应用程序的 Python 框架。它也是模型无关的，相同的代码可以与不同的模型重复使用。
