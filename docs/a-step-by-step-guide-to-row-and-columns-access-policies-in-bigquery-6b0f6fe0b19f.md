# 《BigQuery 中行和列访问策略的逐步指南》

> 原文：[https://towardsdatascience.com/a-step-by-step-guide-to-row-and-columns-access-policies-in-bigquery-6b0f6fe0b19f?source=collection_archive---------6-----------------------#2023-01-19](https://towardsdatascience.com/a-step-by-step-guide-to-row-and-columns-access-policies-in-bigquery-6b0f6fe0b19f?source=collection_archive---------6-----------------------#2023-01-19)

## 谷歌的数据仓库提供了限制对表和视图中信息访问的惊人方法，无论是垂直维度还是水平维度，让我们来探索如何设置这些限制。

[](https://pl-bescond.medium.com/?source=post_page-----6b0f6fe0b19f--------------------------------)[![皮埃尔-路易斯·贝斯康](../Images/bb236055962b420fb3ab22088ab28f11.png)](https://pl-bescond.medium.com/?source=post_page-----6b0f6fe0b19f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6b0f6fe0b19f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6b0f6fe0b19f--------------------------------) [皮埃尔-路易斯·贝斯康](https://pl-bescond.medium.com/?source=post_page-----6b0f6fe0b19f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ef7c1e10597&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-row-and-columns-access-policies-in-bigquery-6b0f6fe0b19f&user=Pierre-Louis+Bescond&userId=4ef7c1e10597&source=post_page-4ef7c1e10597----6b0f6fe0b19f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b0f6fe0b19f--------------------------------) ·7 min read·2023年1月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6b0f6fe0b19f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-row-and-columns-access-policies-in-bigquery-6b0f6fe0b19f&user=Pierre-Louis+Bescond&userId=4ef7c1e10597&source=-----6b0f6fe0b19f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6b0f6fe0b19f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-row-and-columns-access-policies-in-bigquery-6b0f6fe0b19f&source=-----6b0f6fe0b19f---------------------bookmark_footer-----------)![](../Images/726d73195957ee54f18bfda9b8bf529a.png)

图片来源：[马丁·奥尔森](https://unsplash.com/@martinols3n) 于 [Unsplash](https://unsplash.com/photos/szfPB4X-FWk)

管理数据仓库意味着多个具有不同角色和权限的用户应该能够查询他们需要的数据。

你可能决定为每个角色创建一个特定的表格，但这将消耗大量时间和资源，更不用说维护这个系统的困难。

在本教程中，我们将创建一个“EMPLOYEES”表，包含各种层次的信息，从员工所在的“国家”…到他的/她的薪水…并考虑3个角色：

![](../Images/03aacdca9a85d7f58da7ffedc6af0a45.png)

3个角色的示意图 — 图片由作者使用[www.freepik.com](http://www.freepik.com)资源创建

+   **人力资源**：他们应当访问所有数据以进行日常工作，不受任何限制。

+   **数据科学家**：他们可能只会访问一些经过选择和匿名化的列用于建模目的。

+   **数据公民**：大多数列…
