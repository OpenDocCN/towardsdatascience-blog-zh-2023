# 如何（以及为何）保护你的 API 密钥

> 原文：[https://towardsdatascience.com/how-you-can-and-why-you-should-secure-your-api-keys-e433acc2f22d?source=collection_archive---------15-----------------------#2023-03-07](https://towardsdatascience.com/how-you-can-and-why-you-should-secure-your-api-keys-e433acc2f22d?source=collection_archive---------15-----------------------#2023-03-07)

## 保护 API 密钥的简单最佳实践

[](https://medium.com/@aashishnair?source=post_page-----e433acc2f22d--------------------------------)[![Aashish Nair](../Images/23f4b3839e464419332b690a4098d824.png)](https://medium.com/@aashishnair?source=post_page-----e433acc2f22d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e433acc2f22d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e433acc2f22d--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----e433acc2f22d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-you-can-and-why-you-should-secure-your-api-keys-e433acc2f22d&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----e433acc2f22d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e433acc2f22d--------------------------------) ·4 分钟阅读·2023 年 3 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe433acc2f22d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-you-can-and-why-you-should-secure-your-api-keys-e433acc2f22d&user=Aashish+Nair&userId=3087ba81e065&source=-----e433acc2f22d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe433acc2f22d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-you-can-and-why-you-should-secure-your-api-keys-e433acc2f22d&source=-----e433acc2f22d---------------------bookmark_footer-----------)![](../Images/41c6b18ecfb3f3b57ea5f82d0acd5b4d.png)

照片由 [regularguy.eth](https://unsplash.com/@moneyphotos?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

API 密钥在识别发起请求的应用程序中发挥了重要作用。它们是一种安全措施，确保未经身份验证的人无法访问给定服务器上的信息。然而，如果这些密钥没有得到充分保护，外部人员很容易获取这些密钥并造成实际损害。

为了避免这种结果，企业可能会雇佣IT管理员，他们负责使用高级工具（例如，AWS Secrets Manager）为多个人员和项目存储、管理和轮换API密钥。

然而，由于共享/泄露API密钥的严重后果，确保其安全的责任不应仅仅落在管理员身上；每个团队成员都应了解保护API密钥安全的重要性。

在这里，我们将深入探讨泄露API密钥可能造成的损害，并探索保护API密钥的简单最佳实践。

## 共享API密钥的后果

未能保护API密钥在多方面都是有害的。

首先，这会导致人们或企业承担他们甚至没有使用过的API调用的费用。一个显著的受害者是……
