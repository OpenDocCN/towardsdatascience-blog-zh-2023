# 高级指南：通过自动批处理输入避免微软翻译 API 的最大字符限制

> 原文：[`towardsdatascience.com/advanced-guide-avoiding-max-character-limits-on-the-microsoft-translator-api-by-auto-batching-8a106e5f9f80?source=collection_archive---------23-----------------------#2023-01-31`](https://towardsdatascience.com/advanced-guide-avoiding-max-character-limits-on-the-microsoft-translator-api-by-auto-batching-8a106e5f9f80?source=collection_archive---------23-----------------------#2023-01-31)

## 充分利用免费订阅

[](https://namiyousef96.medium.com/?source=post_page-----8a106e5f9f80--------------------------------)![Yousef Nami](https://namiyousef96.medium.com/?source=post_page-----8a106e5f9f80--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8a106e5f9f80--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8a106e5f9f80--------------------------------) [Yousef Nami](https://namiyousef96.medium.com/?source=post_page-----8a106e5f9f80--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F63d10d93acd8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-guide-avoiding-max-character-limits-on-the-microsoft-translator-api-by-auto-batching-8a106e5f9f80&user=Yousef+Nami&userId=63d10d93acd8&source=post_page-63d10d93acd8----8a106e5f9f80---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8a106e5f9f80--------------------------------) ·24 min read·2023 年 1 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8a106e5f9f80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-guide-avoiding-max-character-limits-on-the-microsoft-translator-api-by-auto-batching-8a106e5f9f80&user=Yousef+Nami&userId=63d10d93acd8&source=-----8a106e5f9f80---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8a106e5f9f80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-guide-avoiding-max-character-limits-on-the-microsoft-translator-api-by-auto-batching-8a106e5f9f80&source=-----8a106e5f9f80---------------------bookmark_footer-----------)![](img/af8adbcc9c869a0985b8e5a4eed8d533.png)

图片来源：[Unsplash](https://unsplash.com/photos/5Z8mR4vqJD4)，感谢[Edurne](https://unsplash.com/@edurnetx)。

微软翻译 API [[1](https://www.microsoft.com/en-us/translator/business/translator-api/)] 是最容易设置的翻译服务之一，而且它非常强大，为你提供了多种低资源和高资源语言的翻译服务，且免费使用。然而，它每次请求有**50000 字符的最大限制** [[2](https://learn.microsoft.com/en-us/azure/cognitive-services/translator/request-limits)]，每小时有**200 万字符的最大限制**（在免费版本中）。所以虽然服务本身非常容易设置和使用，但要可靠地使用它却相当困难。

在本文中，我将探讨确保以下方法：

+   任何任意数量的文本都可以通过自动批处理输入来翻译成任意数量的目标语言，同时遵守最大字符限制。

+   连续请求的延迟使其遵守每小时的最大字符限制

在最糟的情况下（免费订阅），这些方法将减少部分翻译中的浪费字符，而在最好的情况下（付费订阅），它们将节省你的费用。

本教程针对那些对微软翻译 API 有一定工作知识的人。如果你不熟悉并且想要一个适合初学者的设置指南，请查看我的…
