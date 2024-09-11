# 破解 OpenAI (Python) API

> 原文：[https://towardsdatascience.com/cracking-open-the-openai-python-api-230e4cae7971?source=collection_archive---------3-----------------------#2023-07-21](https://towardsdatascience.com/cracking-open-the-openai-python-api-230e4cae7971?source=collection_archive---------3-----------------------#2023-07-21)

## **一个完整的适合初学者的介绍，附带示例代码**

[](https://shawhin.medium.com/?source=post_page-----230e4cae7971--------------------------------)[![Shaw Talebi](../Images/1449cc7c08890e2078f9e5d07897e3df.png)](https://shawhin.medium.com/?source=post_page-----230e4cae7971--------------------------------)[](https://towardsdatascience.com/?source=post_page-----230e4cae7971--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----230e4cae7971--------------------------------) [Shaw Talebi](https://shawhin.medium.com/?source=post_page-----230e4cae7971--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff3998e1cd186&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcracking-open-the-openai-python-api-230e4cae7971&user=Shaw+Talebi&userId=f3998e1cd186&source=post_page-f3998e1cd186----230e4cae7971---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----230e4cae7971--------------------------------) ·12 分钟阅读·2023 年 7 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F230e4cae7971&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcracking-open-the-openai-python-api-230e4cae7971&user=Shaw+Talebi&userId=f3998e1cd186&source=-----230e4cae7971---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F230e4cae7971&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcracking-open-the-openai-python-api-230e4cae7971&source=-----230e4cae7971---------------------bookmark_footer-----------)![](../Images/2c856f26ccf574ede1e5d700f75668e9.png)

图片由 [Martin Sanchez](https://unsplash.com/@martinsanchez?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是关于实际使用大语言模型（LLMs）的 [系列文章](/a-practical-introduction-to-llms-65194dda1148) 的第二篇文章。在这里，我提供了一个适合初学者的 OpenAI API 介绍。这使你能够超越像 ChatGPT 这样的限制性聊天界面，更好地利用 LLMs 以满足你独特的使用案例。Python 示例代码如下所示，并可以在 [GitHub 仓库](https://github.com/ShawhinT/YouTube-Blog/tree/main/LLMs/openai-api) 中找到。

## 目录：

1.  什么是 API？

1.  OpenAI 的 (Python) API

1.  入门指南（4 个步骤）

1.  示例代码

在[第一篇文章](/a-practical-introduction-to-llms-65194dda1148)中，我将[**提示工程**](/cracking-open-the-openai-python-api-230e4cae7971)描述为**最容易使用大型语言模型的方法**。最简单（也是最受欢迎）的方法是通过像ChatGPT这样的工具，这些工具提供了直观、无需费用且无需编码的与大型语言模型互动的方式。

[## 大型语言模型的实用介绍](https://towardsdatascience.com/a-practical-introduction-to-llms-65194dda1148?source=post_page-----230e4cae7971--------------------------------)

### 实际使用大型语言模型的3个层次

[关于大型语言模型的实用介绍](https://towardsdatascience.com/a-practical-introduction-to-llms-65194dda1148?source=post_page-----230e4cae7971--------------------------------)

然而，这种**易用性是有代价的**。即，聊天界面具有局限性，并且在许多实际应用中表现不佳…
