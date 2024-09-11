# 如何评估你的机器学习/人工智能模型的表现

> 原文：[https://towardsdatascience.com/how-to-evaluate-the-performance-of-your-ml-ai-models-ba1debc6f2fa?source=collection_archive---------2-----------------------#2023-05-20](https://towardsdatascience.com/how-to-evaluate-the-performance-of-your-ml-ai-models-ba1debc6f2fa?source=collection_archive---------2-----------------------#2023-05-20)

## 准确的评估是性能提升的唯一途径

[](https://saraametwalli.medium.com/?source=post_page-----ba1debc6f2fa--------------------------------)[![Sara A. Metwalli](../Images/d6861f7bc1879bf68d4b7116c335c7e5.png)](https://saraametwalli.medium.com/?source=post_page-----ba1debc6f2fa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ba1debc6f2fa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ba1debc6f2fa--------------------------------) [Sara A. Metwalli](https://saraametwalli.medium.com/?source=post_page-----ba1debc6f2fa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7938431b336a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-evaluate-the-performance-of-your-ml-ai-models-ba1debc6f2fa&user=Sara+A.+Metwalli&userId=7938431b336a&source=post_page-7938431b336a----ba1debc6f2fa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ba1debc6f2fa--------------------------------) ·8 min read·2023年5月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fba1debc6f2fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-evaluate-the-performance-of-your-ml-ai-models-ba1debc6f2fa&user=Sara+A.+Metwalli&userId=7938431b336a&source=-----ba1debc6f2fa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fba1debc6f2fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-evaluate-the-performance-of-your-ml-ai-models-ba1debc6f2fa&source=-----ba1debc6f2fa---------------------bookmark_footer-----------)![](../Images/179ab3fd989ac42746ac78c4f5aab453.png)

图片由 [Scott Graham](https://unsplash.com/@homajob?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

通过实践学习是学习任何事物的最佳方法之一，无论是技术、新语言还是烹饪新菜肴。一旦你掌握了一个领域或应用的基础知识，你就可以通过行动来巩固这些知识。为各种应用构建模型是让你对机器学习和人工智能的知识变得更加具体的最佳方式。

虽然这两个领域（或者确切地说是子领域，因为它们有重叠）在各种上下文中都有应用，但学习如何构建模型的步骤基本上无论目标应用领域如何都是相同的。

人工智能语言模型如[ChatGPT](https://openai.com/blog/chatgpt)和[Bard](https://bard.google.com/)因其在日常生活中的广泛实用性，正受到技术新手和普通大众的关注和喜爱。

现在越来越多的模型被发布和展示，人们可能会问，什么才是一个“*好的*” AI 或 ML 模型，我们如何评估其性能？

这就是本文将要讨论的内容。但再次强调，我们假设您已经建立了一个 AI 或 ML 模型。现在，您想评估并改进其性能（如果有必要）。但是，无论模型的类型如何，您都可以…
