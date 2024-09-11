# 优化数据仓库存储：视图与表的使用

> 原文：[https://towardsdatascience.com/optimize-data-warehouse-storage-with-views-and-tables-659dd588345d?source=collection_archive---------10-----------------------#2023-03-24](https://towardsdatascience.com/optimize-data-warehouse-storage-with-views-and-tables-659dd588345d?source=collection_archive---------10-----------------------#2023-03-24)

## 表和视图的区别以及如何使用它们

[](https://madison-schott.medium.com/?source=post_page-----659dd588345d--------------------------------)[![Madison Schott](../Images/0b82d0dd48629641abb439cef23ebe04.png)](https://madison-schott.medium.com/?source=post_page-----659dd588345d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----659dd588345d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----659dd588345d--------------------------------) [Madison Schott](https://madison-schott.medium.com/?source=post_page-----659dd588345d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3ed0ce2dcf93&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimize-data-warehouse-storage-with-views-and-tables-659dd588345d&user=Madison+Schott&userId=3ed0ce2dcf93&source=post_page-3ed0ce2dcf93----659dd588345d---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----659dd588345d--------------------------------) ·6分钟阅读·2023年3月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F659dd588345d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimize-data-warehouse-storage-with-views-and-tables-659dd588345d&user=Madison+Schott&userId=3ed0ce2dcf93&source=-----659dd588345d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F659dd588345d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimize-data-warehouse-storage-with-views-and-tables-659dd588345d&source=-----659dd588345d---------------------bookmark_footer-----------)![](../Images/836edeb9f20b6e4112718edec85e7a93.png)

[Sophia Baboolal](https://unsplash.com/@sophiababoolal?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 的照片，来源于 [Unsplash](https://unsplash.com/s/photos/table?orientation=landscape&utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

随着现代数据堆栈的兴起，许多公司正在将其数据库从本地迁移到云端。他们开始利用像 Snowflake、Redshift 和 DuckDB 这样的数据仓库工具，以充分发挥云计算的所有优势。

尽管这些数据仓库通常帮助较小的公司节省开支，但云端的计算成本可能会迅速增加。至关重要的是，你需要优化你的数据仓库以降低存储和计算成本。这意味着你需要了解最佳的数据存储方式，以便数据团队能以成本效益高的方式利用这些数据。

在这篇文章中，我们将讨论视图与表的区别、数据仓库中存在的不同类型的视图，以及每种视图的使用场景。通过这篇文章，你应该能够识别出在节省成本的同时，存储不同数据集的最佳选择。

# 什么是视图？

视图是**一个定义好的查询，建立在表之上**。与表不同，它不存储实际数据。它总是包含最新数据，因为它在每次查询时都会重新运行。而表只会保持到上次更新时的数据新鲜度…
