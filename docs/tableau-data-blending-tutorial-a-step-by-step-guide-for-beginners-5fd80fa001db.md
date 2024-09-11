# Tableau 数据融合教程——初学者的逐步指南

> 原文：[https://towardsdatascience.com/tableau-data-blending-tutorial-a-step-by-step-guide-for-beginners-5fd80fa001db?source=collection_archive---------13-----------------------#2023-01-30](https://towardsdatascience.com/tableau-data-blending-tutorial-a-step-by-step-guide-for-beginners-5fd80fa001db?source=collection_archive---------13-----------------------#2023-01-30)

## 我们将探讨使用 Tableau 进行数据融合的全面概述，适用于数据科学家和数据分析师。

[](https://zoumanakeita.medium.com/?source=post_page-----5fd80fa001db--------------------------------)[![Zoumana Keita](../Images/34a15c1d03687816dbdbc065f5719f80.png)](https://zoumanakeita.medium.com/?source=post_page-----5fd80fa001db--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5fd80fa001db--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5fd80fa001db--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----5fd80fa001db--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe6ae785a30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftableau-data-blending-tutorial-a-step-by-step-guide-for-beginners-5fd80fa001db&user=Zoumana+Keita&userId=e6ae785a30d&source=post_page-e6ae785a30d----5fd80fa001db---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5fd80fa001db--------------------------------) ·7分钟阅读·2023年1月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5fd80fa001db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftableau-data-blending-tutorial-a-step-by-step-guide-for-beginners-5fd80fa001db&user=Zoumana+Keita&userId=e6ae785a30d&source=-----5fd80fa001db---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5fd80fa001db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftableau-data-blending-tutorial-a-step-by-step-guide-for-beginners-5fd80fa001db&source=-----5fd80fa001db---------------------bookmark_footer-----------)![](../Images/ce0db5bca6af2e232fd171955c150507.png)

图片由 [Lukas Blazek](https://unsplash.com/@goumbik) 提供，来源于 [Unsplash](https://unsplash.com/photos/mcSDtbWXUZU)

# 数据融合——动机

如今，公司利用来自不同来源的数据来解决业务问题。能够高效地收集和组合这些数据已成为所有数据科学家和数据工程师的重要技能，以帮助组织做出明智的决策。

在本教程中，我们将首先构建对一种强大的数据组合方法——数据融合（Data Blending）的理解，然后探索其好处。接着，我们将深入探讨它的一些缺点，最后介绍一些与数据融合（Data Blending）相关的常见问题，并使用Tableau进行解答。

# 数据融合（Data Blending）与Tableau中的连接（Joins）。

在实际生活中，你将处理来自多个来源的信息，如Excel电子表格、SQL、CSV等。作为数据科学家，你需要将它们互联以生成全球业务洞察。

Tableau可以通过两种不同的方法来解决这个问题：连接（joining）、融合（blending）和关系（relationships）。
