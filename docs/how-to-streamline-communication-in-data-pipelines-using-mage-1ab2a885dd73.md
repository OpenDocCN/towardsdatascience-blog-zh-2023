# 如何使用 Mage 精简数据管道中的沟通

> 原文：[https://towardsdatascience.com/how-to-streamline-communication-in-data-pipelines-using-mage-1ab2a885dd73?source=collection_archive---------12-----------------------#2023-07-13](https://towardsdatascience.com/how-to-streamline-communication-in-data-pipelines-using-mage-1ab2a885dd73?source=collection_archive---------12-----------------------#2023-07-13)

## 让机器人处理我们难以沟通的问题

[](https://medium.com/@xiaoxugao?source=post_page-----1ab2a885dd73--------------------------------)[![Xiaoxu Gao](../Images/8712a7e5f3bad0d2abd7e04792fad66f.png)](https://medium.com/@xiaoxugao?source=post_page-----1ab2a885dd73--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1ab2a885dd73--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1ab2a885dd73--------------------------------) [Xiaoxu Gao](https://medium.com/@xiaoxugao?source=post_page-----1ab2a885dd73--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2adc5a07e772&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-streamline-communication-in-data-pipelines-using-mage-1ab2a885dd73&user=Xiaoxu+Gao&userId=2adc5a07e772&source=post_page-2adc5a07e772----1ab2a885dd73---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ab2a885dd73--------------------------------) · 6 分钟阅读 · 2023年7月13日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1ab2a885dd73&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-streamline-communication-in-data-pipelines-using-mage-1ab2a885dd73&user=Xiaoxu+Gao&userId=2adc5a07e772&source=-----1ab2a885dd73---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1ab2a885dd73&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-streamline-communication-in-data-pipelines-using-mage-1ab2a885dd73&source=-----1ab2a885dd73---------------------bookmark_footer-----------)![](../Images/34e20992bb781ce6ecdc54e0203739a7.png)

照片由 [Volodymyr Hryshchenko](https://unsplash.com/@lunarts) 提供，发布于 [Unsplash](https://unsplash.com/)

你是否曾遇到过下游数据管道因为一个 Google Sheets 中的小错误而被阻塞的情况？有时，那个表格甚至不属于你的团队，你只能追着表格的拥有者修复它。与此同时，许多其他关键管道也因此失败，你还需要处理这些问题。

你感到筋疲力尽。最糟糕的是，作为工程师你真的无能为力。这一切都是关于无尽的沟通和利益相关者管理。Google 表格问题只是各种规模的源问题的一个例子。暂停片刻，考虑一个与你产生共鸣的问题，我们将深入探讨这篇文章。

改善这种情况的关键是**自动化数据管道中的通信生命周期**。如果你的管道已经有了警报机制，那么这是一个好的开始。然而，警报主要是针对数据工程团队，而不是外部团队。

根据我的经验，与源团队或最终用户建立积极主动的沟通同样至关重要，以确保他们了解正在进行的情况，并能够采取……
