# 现代数据仓储

> 原文：[https://towardsdatascience.com/modern-data-warehousing-2b1b0486ce4a?source=collection_archive---------0-----------------------#2023-12-16](https://towardsdatascience.com/modern-data-warehousing-2b1b0486ce4a?source=collection_archive---------0-----------------------#2023-12-16)

## 最先进的数据平台设计

[](https://mshakhomirov.medium.com/?source=post_page-----2b1b0486ce4a--------------------------------)[![💡Mike Shakhomirov](../Images/bc6895c7face3244d488feb97ba0f68e.png)](https://mshakhomirov.medium.com/?source=post_page-----2b1b0486ce4a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2b1b0486ce4a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2b1b0486ce4a--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----2b1b0486ce4a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe06a48b3dd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodern-data-warehousing-2b1b0486ce4a&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=post_page-e06a48b3dd48----2b1b0486ce4a---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2b1b0486ce4a--------------------------------) ·12分钟阅读·2023年12月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2b1b0486ce4a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodern-data-warehousing-2b1b0486ce4a&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=-----2b1b0486ce4a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2b1b0486ce4a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodern-data-warehousing-2b1b0486ce4a&source=-----2b1b0486ce4a---------------------bookmark_footer-----------)![](../Images/61b0eba3203a5c89ddf3b0bd67553f9a.png)

图片由[Nubelson Fernandes](https://unsplash.com/@nublson?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这个故事中，我将尝试阐明现代数据仓库解决方案（DWH）相较于其他数据平台架构类型的好处。我敢说，DWH 目前是数据工程师中最受欢迎的平台。它相比其他解决方案类型提供了无价的好处，但也有一些众所周知的限制。想学习数据工程吗？这个故事是一个很好的起点，因为它解释了数据工程的核心——架构图中心的 DWH 解决方案。我们将看到如何在市场上不同的 DWH 中摄取和转换数据。

我也希望与有经验的用户展开讨论。如果能知道你的观点，并听听你对这个话题的看法，那将是很好的。

## 数据仓库的关键特征

一个无服务器、分布式 SQL 引擎（如 BigQuery、Snowflake、Redshift、Microsoft Azure Synapse、Teradata）就是我们所称的现代数据仓库（DWH）。这是一种 SQL 优先的数据架构 [1]，数据存储在数据仓库中，我们可以利用非规范化星型架构 [2] 数据集的所有优势，因为大多数现代数据仓库是分布式的且具有良好的扩展性，这意味着不需要担心表键的问题……
