# 关于模型如ChatGPT的工作原理的直观理解

> 原文：[https://towardsdatascience.com/an-intuition-for-how-models-like-chatgpt-work-c7f01616bd6d?source=collection_archive---------1-----------------------#2023-12-30](https://towardsdatascience.com/an-intuition-for-how-models-like-chatgpt-work-c7f01616bd6d?source=collection_archive---------1-----------------------#2023-12-30)

## 提供对流行的变换器模型如ChatGPT和其他大型语言模型（LLMs）背后思想的直观理解

[](https://dkhundley.medium.com/?source=post_page-----c7f01616bd6d--------------------------------)[![David Hundley](../Images/1779ef96ec3d338f8fe4a9567ba7b194.png)](https://dkhundley.medium.com/?source=post_page-----c7f01616bd6d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c7f01616bd6d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c7f01616bd6d--------------------------------) [David Hundley](https://dkhundley.medium.com/?source=post_page-----c7f01616bd6d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F82498630db6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-intuition-for-how-models-like-chatgpt-work-c7f01616bd6d&user=David+Hundley&userId=82498630db6&source=post_page-82498630db6----c7f01616bd6d---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c7f01616bd6d--------------------------------) ·10分钟阅读·2023年12月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc7f01616bd6d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-intuition-for-how-models-like-chatgpt-work-c7f01616bd6d&user=David+Hundley&userId=82498630db6&source=-----c7f01616bd6d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc7f01616bd6d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-intuition-for-how-models-like-chatgpt-work-c7f01616bd6d&source=-----c7f01616bd6d---------------------bookmark_footer-----------)![](../Images/6298bdd024c30f5750f9870f35f92f5a.png)

作者制作的标题卡

随着2023年的结束，令人难以置信的是生成式AI已经对我们的日常生活产生了多大的影响。从2022年11月ChatGPT发布开始，这一领域的发展如此之快，以至于很难相信在短短一年内所有这些进展都出现了。

尽管结果相当惊人，但其潜在的复杂性使得很多人公开猜测这些大型语言模型（LLMs）是如何工作的。有些人猜测这些模型是从预设的响应数据库中提取内容的，有些人甚至猜测这些LLMs已经达到了人类级别的意识。这些都是极端的观点，正如你可能猜到的，两者都不正确。

你可能听说过这些**大型语言模型（LLMs）是下一个词预测器，意思是它们使用概率来确定句子中应该出现的下一个词**。这个理解从技术上来说是正确的，但对于充分理解这些模型来说，略显高层次。为了建立更强的直觉，我们需要深入探讨。**这篇文章的意图是为商业领袖提供足够深入的理解**…
