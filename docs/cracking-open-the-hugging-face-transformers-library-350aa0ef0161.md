# 破解 Hugging Face Transformers 库

> 原文：[`towardsdatascience.com/cracking-open-the-hugging-face-transformers-library-350aa0ef0161?source=collection_archive---------1-----------------------#2023-08-05`](https://towardsdatascience.com/cracking-open-the-hugging-face-transformers-library-350aa0ef0161?source=collection_archive---------1-----------------------#2023-08-05)

## 使用开源 LLM 的快速入门指南

[](https://shawhin.medium.com/?source=post_page-----350aa0ef0161--------------------------------)![Shaw Talebi](https://shawhin.medium.com/?source=post_page-----350aa0ef0161--------------------------------)[](https://towardsdatascience.com/?source=post_page-----350aa0ef0161--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----350aa0ef0161--------------------------------) [Shaw Talebi](https://shawhin.medium.com/?source=post_page-----350aa0ef0161--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff3998e1cd186&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcracking-open-the-hugging-face-transformers-library-350aa0ef0161&user=Shaw+Talebi&userId=f3998e1cd186&source=post_page-f3998e1cd186----350aa0ef0161---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----350aa0ef0161--------------------------------) ·10 分钟阅读·2023 年 8 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F350aa0ef0161&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcracking-open-the-hugging-face-transformers-library-350aa0ef0161&user=Shaw+Talebi&userId=f3998e1cd186&source=-----350aa0ef0161---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F350aa0ef0161&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcracking-open-the-hugging-face-transformers-library-350aa0ef0161&source=-----350aa0ef0161---------------------bookmark_footer-----------)

这是关于在实践中使用大型语言模型 (LLMs) 的 系列文章 的第三篇文章。在这里，我将提供一个适合初学者的 Hugging Face Transformers 库指南，该库提供了一种简单且免费使用多种开源语言模型的方法。我将首先回顾关键概念，然后深入示例 Python 代码。

![](img/4250c47bc3e0aefc26a5d1c0a7bb88e2.png)

图片由 [Jéan Béller](https://unsplash.com/@chinatravelchannel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

在 上一篇文章 中，我们探讨了 OpenAI Python API 并使用它来制作一个自定义聊天机器人。然而，这个 API 的一个缺点是 API 调用需要付费，这可能对某些使用场景不太适用。

在这些情况下，转向开源解决方案可能是有利的。一种流行的方法是通过 Hugging Face 的 Transformers 库。

# **什么是 Hugging Face？**

**Hugging Face** 是一家 **已经成为开源机器学习（ML）主要中心的 AI 公司**。他们的平台有三个主要元素，允许用户访问和分享…
