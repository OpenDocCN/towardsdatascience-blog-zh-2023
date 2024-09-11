# 在 Power BI 中开发和测试 RLS 规则

> 原文：[https://towardsdatascience.com/develop-and-test-rls-rules-in-power-bi-9dc705945feb?source=collection_archive---------13-----------------------#2023-06-19](https://towardsdatascience.com/develop-and-test-rls-rules-in-power-bi-9dc705945feb?source=collection_archive---------13-----------------------#2023-06-19)

## *通常，并非所有用户都应该拥有访问报告中所有数据的权限。在这里，我将解释如何在 Power BI 中开发 RLS 规则来配置访问权限，并且如何测试这些规则。*

[](https://medium.com/@salvatorecagliari?source=post_page-----9dc705945feb--------------------------------)[![Salvatore Cagliari](../Images/a24b0cefab6e707cfee06cde9e857559.png)](https://medium.com/@salvatorecagliari?source=post_page-----9dc705945feb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9dc705945feb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9dc705945feb--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----9dc705945feb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39cccb39e92a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdevelop-and-test-rls-rules-in-power-bi-9dc705945feb&user=Salvatore+Cagliari&userId=39cccb39e92a&source=post_page-39cccb39e92a----9dc705945feb---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9dc705945feb--------------------------------) ·11 min read·2023年6月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9dc705945feb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdevelop-and-test-rls-rules-in-power-bi-9dc705945feb&user=Salvatore+Cagliari&userId=39cccb39e92a&source=-----9dc705945feb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9dc705945feb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdevelop-and-test-rls-rules-in-power-bi-9dc705945feb&source=-----9dc705945feb---------------------bookmark_footer-----------)![](../Images/3f6630ce10ed9e616cfbb3cea460be9d.png)

图片由 [FLY:D](https://unsplash.com/es/@flyd2069?utm_source=medium&utm_medium=referral) 提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

我的许多客户希望根据特定规则限制对其报告中数据的访问。

数据访问称为行级安全性（RLS，简写为 RLS）。

你可以在 Medium 上找到许多关于 Power BI 中 RLS 的文章。

我在下面的参考文献部分添加了其中的两个。

尽管所有文章都很好地解释了基础知识，但我总是错过了如何开发更复杂规则以及如何轻松测试它们的解释。

在本文中，我将解释 RLS 的基础知识，并逐步增加复杂性。

此外，我将向你展示如何使用 DAX Studio 构建查询，以便在将 RLS 规则添加到数据模型之前进行测试。

所以，我们开始吧。

# 场景

我使用了一个场景，其中用户可以根据公司内的商店或商店的地理位置访问零售销售数据，包括一个……
