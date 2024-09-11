# ChatGPT 审核 API：输入/输出控制

> 原文：[https://towardsdatascience.com/chatgpt-moderation-api-input-output-artificial-intelligence-chatgpt3-data-4754389ec9c8?source=collection_archive---------1-----------------------#2023-07-23](https://towardsdatascience.com/chatgpt-moderation-api-input-output-artificial-intelligence-chatgpt3-data-4754389ec9c8?source=collection_archive---------1-----------------------#2023-07-23)

## 使用 OpenAI 的 Moderation Endpoint 进行负责任的 AI 使用

[](https://medium.com/@andvalenzuela?source=post_page-----4754389ec9c8--------------------------------)[![Andrea Valenzuela](../Images/ddfc1534af92413fd91076f826cc49b6.png)](https://medium.com/@andvalenzuela?source=post_page-----4754389ec9c8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4754389ec9c8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4754389ec9c8--------------------------------) [Andrea Valenzuela](https://medium.com/@andvalenzuela?source=post_page-----4754389ec9c8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa6f3f1654c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-moderation-api-input-output-artificial-intelligence-chatgpt3-data-4754389ec9c8&user=Andrea+Valenzuela&userId=a6f3f1654c3&source=post_page-a6f3f1654c3----4754389ec9c8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4754389ec9c8--------------------------------) · 9 分钟阅读 · 2023年7月23日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4754389ec9c8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-moderation-api-input-output-artificial-intelligence-chatgpt3-data-4754389ec9c8&user=Andrea+Valenzuela&userId=a6f3f1654c3&source=-----4754389ec9c8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4754389ec9c8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-moderation-api-input-output-artificial-intelligence-chatgpt3-data-4754389ec9c8&source=-----4754389ec9c8---------------------bookmark_footer-----------)![](../Images/733cc16aa1f0f391b10378392a4daae2.png)

自制 gif。

大型语言模型（LLMs）无疑改变了我们与技术互动的方式。作为众多著名 LLMs 中的一个，ChatGPT 已证明是一种宝贵的工具，为用户提供了大量的信息和有用的回应。然而，像任何技术一样，**ChatGPT 也有其局限性**。

最近的讨论突显了一个重要问题——**ChatGPT 生成不当或有偏见的回应的潜在风险**。这个问题源于它的训练数据，这些数据包含了来自不同背景和时代的个人集体著作。**虽然这种多样性丰富了模型的理解，但也带来了现实世界中普遍存在的偏见和成见**。

因此，ChatGPT 生成的一些回应可能会反映这些偏见。*但公平地说*，**不当的回应可能会被不当的用户查询引发**。

在本文中，我们将探讨在构建 LLM 驱动的应用程序时，积极调节模型的输入和输出的重要性。为此，我们将使用所谓的 ***OpenAI Moderation API*，它有助于识别不当内容并采取相应措施**。
