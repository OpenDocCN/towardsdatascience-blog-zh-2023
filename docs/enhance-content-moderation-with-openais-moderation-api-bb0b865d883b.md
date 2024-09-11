# 使用OpenAI的Moderation API提升ChatGPT的内容审核

> 原文：[https://towardsdatascience.com/enhance-content-moderation-with-openais-moderation-api-bb0b865d883b?source=collection_archive---------16-----------------------#2023-07-07](https://towardsdatascience.com/enhance-content-moderation-with-openais-moderation-api-bb0b865d883b?source=collection_archive---------16-----------------------#2023-07-07)

## 无缝地将审核端点集成到你的管道中，使用ChatGPT

[](https://idilismiguzel.medium.com/?source=post_page-----bb0b865d883b--------------------------------)[![Idil Ismiguzel](../Images/6846628535770a9f3e13ebb555e82abd.png)](https://idilismiguzel.medium.com/?source=post_page-----bb0b865d883b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bb0b865d883b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bb0b865d883b--------------------------------) [Idil Ismiguzel](https://idilismiguzel.medium.com/?source=post_page-----bb0b865d883b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d965c736f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhance-content-moderation-with-openais-moderation-api-bb0b865d883b&user=Idil+Ismiguzel&userId=6d965c736f2&source=post_page-6d965c736f2----bb0b865d883b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb0b865d883b--------------------------------) ·6分钟阅读·2023年7月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbb0b865d883b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhance-content-moderation-with-openais-moderation-api-bb0b865d883b&user=Idil+Ismiguzel&userId=6d965c736f2&source=-----bb0b865d883b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbb0b865d883b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhance-content-moderation-with-openais-moderation-api-bb0b865d883b&source=-----bb0b865d883b---------------------bookmark_footer-----------)![](../Images/c439a81f26c9a857d503a1d983e4752a.png)

图片由 [Joshua Kettle](https://unsplash.com/@joshuakettle?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 免责声明：本文关注于检查内容是否符合审核指南。因此，可能会涉及暴力、自残、仇恨和性暴力的内容。

随着提示工程的兴起和大型语言模型在生成对我们询问的回应方面取得的显著成就，像ChatGPT这样的聊天机器人正成为我们日常生活以及我们创建的应用程序的一个不可或缺的部分。无论你是出于个人目的使用AI模型，还是利用它们的能力开发先进系统，确保你使用的AI模型生成的内容遵循特定的审核指南和规则是非常重要的。⚠️

在本文中，我们将重点讨论OpenAI的审核端点，这是一个很棒的功能，用于检查内容是否符合OpenAI的使用政策。我们将探讨如何将审核API集成到使用ChatGPT的系统中，并验证输入和输出，以确保它们符合期望的指南。

如果你是**提示工程**的新手，我强烈推荐在深入之前查看我的文章：[掌握提示工程](https://medium.com/towards-data-science/mastering-prompt-engineering-to-unleash-chatgpts-potential-9578a3fe799c)。这将为你提供深入的见解……
