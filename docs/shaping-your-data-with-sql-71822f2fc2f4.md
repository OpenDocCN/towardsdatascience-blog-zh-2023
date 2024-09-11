# 用 SQL 整形你的数据

> 原文：[https://towardsdatascience.com/shaping-your-data-with-sql-71822f2fc2f4?source=collection_archive---------15-----------------------#2023-04-18](https://towardsdatascience.com/shaping-your-data-with-sql-71822f2fc2f4?source=collection_archive---------15-----------------------#2023-04-18)

## 使用不同的数据整形技术来提升和优化你的数据分析过程

[![Chi Nguyen](../Images/cc6e778a0c64c1c8b3c4d2d96fd62b4f.png)](https://nphchi223.medium.com/?source=post_page-----71822f2fc2f4--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----71822f2fc2f4--------------------------------) [Chi Nguyen](https://nphchi223.medium.com/?source=post_page-----71822f2fc2f4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe982f12a6925&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshaping-your-data-with-sql-71822f2fc2f4&user=Chi+Nguyen&userId=e982f12a6925&source=post_page-e982f12a6925----71822f2fc2f4---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----71822f2fc2f4--------------------------------) · 9 分钟阅读 · 2023年4月18日

--

![](../Images/f1698cdffbc741feba22e618c4552b93.png)

图片由 [OB OA](https://unsplash.com/@oboa?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 什么是数据整形？

数据没有“一刀切”的解决方案。根据不同的目的和使用情况，数据会进行相应的定制。你对数据后续使用的了解越多，就越能有效地向最终用户呈现数据。

例如，用于深入分析的数据与提供给高层管理的汇总数据不同。

另一个例子是，业务发展团队更关注每个区域的总体成本来吸引新用户，而营销经理则关注具体区域的推广费用。

话虽如此，将现有数据结构转换为任何替代的旋转或非旋转结构是任何数据操作和分析过程中的一个不可或缺的步骤。

在本文中，我将介绍一些技术，用于在特定情况下对数据进行整形和切片。通常，我会用 PostgreSQL 来展示我的示例。

现在，让我们开始看看我们有什么吧！

# 数据
