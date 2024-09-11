# 🦜🔗LangChain：通过内存扩展提升性能

> 原文：[https://towardsdatascience.com/langchain-enhancing-performance-with-memory-capacity-c7168e097f81?source=collection_archive---------5-----------------------#2023-06-21](https://towardsdatascience.com/langchain-enhancing-performance-with-memory-capacity-c7168e097f81?source=collection_archive---------5-----------------------#2023-06-21)

![](../Images/7ad10b2af7c0deba0e3b68918593a2f0.png)

照片由 [Milad Fakurian](https://unsplash.com/ko/@fakurian?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## LangChain 通过内存扩展技术的提升

[](https://medium.com/@marcellopoliti?source=post_page-----c7168e097f81--------------------------------)[![Marcello Politi](../Images/484e44571bd2e75acfe5fef3146ab3c2.png)](https://medium.com/@marcellopoliti?source=post_page-----c7168e097f81--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c7168e097f81--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c7168e097f81--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----c7168e097f81--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flangchain-enhancing-performance-with-memory-capacity-c7168e097f81&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----c7168e097f81---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c7168e097f81--------------------------------) ·4 min read·2023年6月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc7168e097f81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flangchain-enhancing-performance-with-memory-capacity-c7168e097f81&user=Marcello+Politi&userId=7390355d40fe&source=-----c7168e097f81---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc7168e097f81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flangchain-enhancing-performance-with-memory-capacity-c7168e097f81&source=-----c7168e097f81---------------------bookmark_footer-----------)

我之前已经发表过关于 LangChain 的 [文章](https://medium.com/towards-data-science/develop-applications-powered-by-language-models-with-langchain-d2f7a1d1ad1a)，介绍了这个库及其所有功能。现在我想重点关注一个关键方面，即如何在智能聊天机器人中管理内存。

聊天机器人或代理也需要一个信息存储机制，这可以采用不同的形式并执行不同的功能。**在聊天机器人中实现记忆系统不仅可以使它们变得更加聪明，还能让它们对用户更自然和有用**。

幸运的是，LangChain 提供了简化开发者在其应用程序中实现记忆的 API。在本文中，我们将更详细地探讨这一方面。

## 在 LangChain 中使用记忆

开发聊天机器人时的最佳实践是**保存聊天机器人与用户的所有互动**。这是因为**LLM 的状态可能会根据过去的对话而改变**，事实上，LLM 对两个用户提出的相同问题的回答也会不同，因为他们与聊天机器人有不同的过去对话，因此处于不同的状态。

因此，聊天机器人记忆创建的无非是旧消息的列表，这些消息会在某个时刻被反馈给聊天机器人…
