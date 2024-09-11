# 通过可视化社交网络获取更好的洞察：使用 Python 的 NetworkX 库进行社交关系分析与绘图 — 第二部分

> 原文：[https://towardsdatascience.com/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-f4a9cf6b6d57?source=collection_archive---------4-----------------------#2023-06-10](https://towardsdatascience.com/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-f4a9cf6b6d57?source=collection_archive---------4-----------------------#2023-06-10)

## 继续介绍使用 Python 的 NetworkX 库进行社交网络分析的初学者指南

[](https://christineegan42.medium.com/?source=post_page-----f4a9cf6b6d57--------------------------------)[![Christine Egan](../Images/d0a11bde52ceaa53d7162f2dd77c8041.png)](https://christineegan42.medium.com/?source=post_page-----f4a9cf6b6d57--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f4a9cf6b6d57--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f4a9cf6b6d57--------------------------------) [Christine Egan](https://christineegan42.medium.com/?source=post_page-----f4a9cf6b6d57--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e9b4d1cb38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-f4a9cf6b6d57&user=Christine+Egan&userId=8e9b4d1cb38&source=post_page-8e9b4d1cb38----f4a9cf6b6d57---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f4a9cf6b6d57--------------------------------) ·6分钟阅读·2023年6月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff4a9cf6b6d57&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-f4a9cf6b6d57&user=Christine+Egan&userId=8e9b4d1cb38&source=-----f4a9cf6b6d57---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff4a9cf6b6d57&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-f4a9cf6b6d57&source=-----f4a9cf6b6d57---------------------bookmark_footer-----------)

在第1部分中，我们探讨了链接分析，[特别是社交网络分析](/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-efeb82ab853e)在调查和理解个人及实体之间关系中的应用。社交网络分析（SNA）是一种特定的链接分析方法，重点关注人和群体及其关系。我们回顾了SNA的基本概念，包括节点（表示个人）和边（表示个人之间的连接）。然后，我们讨论了如何使用SNA来理解社会影响、群体形成和信息流动，使用的指标包括度中心性和中介中心性，以Billy Corgan及其与Smashing Pumpkins创始成员的关系作为简单示例。

![](../Images/d6936330e0e80414b66cd48f7ba8f4cc.png)

图片由 [戈登·约翰逊](https://pixabay.com/users/gdj-1086657/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2099068) 提供，来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2099068)

在这个示例中，我们保持了网络的简洁性。在本教程中，我们将继续使用Python和NetworkX来检查Billy Corgan的影响范围。我们还将扩展Billy Corgan的网络，使其更加复杂，从而加深对**度中心性**和**中介中心性**的理解。在我们逐步解决这个示例时…
