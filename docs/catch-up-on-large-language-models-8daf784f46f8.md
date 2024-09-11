# 了解大型语言模型

> 原文：[https://towardsdatascience.com/catch-up-on-large-language-models-8daf784f46f8?source=collection_archive---------1-----------------------#2023-09-05](https://towardsdatascience.com/catch-up-on-large-language-models-8daf784f46f8?source=collection_archive---------1-----------------------#2023-09-05)

## 一份不带炒作的大型语言模型实用指南

[](https://medium.com/@marcopeixeiro?source=post_page-----8daf784f46f8--------------------------------)[![Marco Peixeiro](../Images/7cf0a81d87281d35ff47f51e3026a3e9.png)](https://medium.com/@marcopeixeiro?source=post_page-----8daf784f46f8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8daf784f46f8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8daf784f46f8--------------------------------) [Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----8daf784f46f8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F741c1c8fcfbd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcatch-up-on-large-language-models-8daf784f46f8&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=post_page-741c1c8fcfbd----8daf784f46f8---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8daf784f46f8--------------------------------) ·15 min read·2023年9月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8daf784f46f8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcatch-up-on-large-language-models-8daf784f46f8&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=-----8daf784f46f8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8daf784f46f8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcatch-up-on-large-language-models-8daf784f46f8&source=-----8daf784f46f8---------------------bookmark_footer-----------)![](../Images/adb61491bc3d90e8c3f4676819450577.png)

[Gary Bendig](https://unsplash.com/@kris_ricepees?utm_source=medium&utm_medium=referral)拍摄的照片，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果你在这里，意味着你像我一样被**大型语言模型**（LLMs）周围不断涌现的信息和炒作帖子所压倒。

本文是我尝试在没有炒作的情况下帮助你了解大型语言模型的主题。毕竟，这是一个变革性的技术，我认为我们理解它是很重要的，希望能激发你进一步学习并用它创造一些东西。

在接下来的部分中，我们将定义LLMs是什么以及它们的工作原理，当然也会涵盖Transformer架构。我们还将探讨训练LLMs的不同方法，并通过一个实际项目总结本文，其中我们将使用Flan-T5进行Python情感分析。

让我们开始吧！

# LLMs和生成式AI：它们是相同的吗？

生成式AI是机器学习的一个子集，专注于那些主要功能是生成*某物*的模型：文本、图像、视频、代码等。

生成模型在大量由人类创建的数据上进行训练，以学习模式和结构，从而使它们能够生成新数据。
