# 实用的LLMs介绍

> 原文：[https://towardsdatascience.com/a-practical-introduction-to-llms-65194dda1148?source=collection_archive---------0-----------------------#2023-07-13](https://towardsdatascience.com/a-practical-introduction-to-llms-65194dda1148?source=collection_archive---------0-----------------------#2023-07-13)

## 实际使用LLMs的3个层次

[](https://shawhin.medium.com/?source=post_page-----65194dda1148--------------------------------)[![Shaw Talebi](../Images/1449cc7c08890e2078f9e5d07897e3df.png)](https://shawhin.medium.com/?source=post_page-----65194dda1148--------------------------------)[](https://towardsdatascience.com/?source=post_page-----65194dda1148--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----65194dda1148--------------------------------) [Shaw Talebi](https://shawhin.medium.com/?source=post_page-----65194dda1148--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff3998e1cd186&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-practical-introduction-to-llms-65194dda1148&user=Shaw+Talebi&userId=f3998e1cd186&source=post_page-f3998e1cd186----65194dda1148---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----65194dda1148--------------------------------) ·7分钟阅读·2023年7月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F65194dda1148&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-practical-introduction-to-llms-65194dda1148&user=Shaw+Talebi&userId=f3998e1cd186&source=-----65194dda1148---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F65194dda1148&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-practical-introduction-to-llms-65194dda1148&source=-----65194dda1148---------------------bookmark_footer-----------)

这是关于实际使用大型语言模型（LLMs）的[系列文章](https://shawhin.medium.com/list/large-language-models-llms-8e009ae3054c)中的第一篇。在这里，我将介绍LLMs，并展示与它们互动的3个层次。未来的文章将探讨LLMs的实际应用，例如如何使用 [OpenAI的公共API](https://medium.com/towards-data-science/cracking-open-the-openai-python-api-230e4cae7971)，[Hugging Face Transformers](https://medium.com/towards-data-science/cracking-open-the-hugging-face-transformers-library-350aa0ef0161) Python库，如何 [微调LLMs](https://medium.com/towards-data-science/fine-tuning-large-language-models-llms-23473d763b91)，以及 [如何从零开始构建LLM](https://medium.com/towards-data-science/how-to-build-an-llm-from-scratch-8c477768f1f9)。

![](../Images/a92d8deb44c331cde299747e18da5c7f.png)

照片由[Patrick Tomasso](https://unsplash.com/@impatrickt?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

# **什么是LLM？**

**LLM**是**大型语言模型**的缩写，这是一种近期在AI和机器学习领域的创新。这种强大的新型AI在2022年12月通过ChatGPT的发布而广受关注。

对于那些足够开明以至于生活在AI热潮和科技新闻周期之外的人而言，**ChatGPT**是一个基于名为GPT-3的LLM的聊天界面（截至本写作时已升级为GPT-3.5或GPT-4）。

如果你使用过ChatGPT，你会发现这并不是你在[AOL即时通讯](https://en.wikipedia.org/wiki/AIM_(software))或信用卡客服中常见的传统聊天机器人。

这一张*感觉*有所不同。
