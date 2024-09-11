# 微调大型语言模型（LLMs）

> 原文：[https://towardsdatascience.com/fine-tuning-large-language-models-llms-23473d763b91?source=collection_archive---------0-----------------------#2023-09-11](https://towardsdatascience.com/fine-tuning-large-language-models-llms-23473d763b91?source=collection_archive---------0-----------------------#2023-09-11)

## 一个带有示例Python代码的概念概述

[](https://shawhin.medium.com/?source=post_page-----23473d763b91--------------------------------)[![Shaw Talebi](../Images/1449cc7c08890e2078f9e5d07897e3df.png)](https://shawhin.medium.com/?source=post_page-----23473d763b91--------------------------------)[](https://towardsdatascience.com/?source=post_page-----23473d763b91--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----23473d763b91--------------------------------) [Shaw Talebi](https://shawhin.medium.com/?source=post_page-----23473d763b91--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff3998e1cd186&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tuning-large-language-models-llms-23473d763b91&user=Shaw+Talebi&userId=f3998e1cd186&source=post_page-f3998e1cd186----23473d763b91---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----23473d763b91--------------------------------) ·14分钟阅读·2023年9月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F23473d763b91&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tuning-large-language-models-llms-23473d763b91&user=Shaw+Talebi&userId=f3998e1cd186&source=-----23473d763b91---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F23473d763b91&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tuning-large-language-models-llms-23473d763b91&source=-----23473d763b91---------------------bookmark_footer-----------)

这是[关于使用大型语言模型](https://medium.com/towards-data-science/a-practical-introduction-to-llms-65194dda1148)（LLMs）实践系列的第5篇文章。在本篇文章中，我们将讨论如何微调（FT）一个预训练的LLM。我们将首先介绍关键的FT概念和技术，然后通过一个具体的示例来展示如何使用Python和Hugging Face的软件生态系统在本地微调一个模型。

![](../Images/d74b7565b127d69e37ddf51e16125896.png)

调整语言模型。图像由作者提供。

在本系列的[上一篇文章](https://medium.com/towards-data-science/prompt-engineering-how-to-trick-ai-into-solving-your-problems-7ce1ed3b553f)中，我们了解了如何通过将提示工程集成到我们的 Python 代码中来构建实用的 LLM 驱动应用程序。对于绝大多数 LLM 用例，这是我推荐的初始方法，因为它比其他方法需要的资源和技术专业知识要少得多，同时仍能提供大部分的好处。

然而，有些情况下，仅仅使用现成的 LLM 进行提示是不够的，这时需要更复杂的解决方案。此时，模型微调可以提供帮助。

# **什么是微调？**
