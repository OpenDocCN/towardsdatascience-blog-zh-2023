# Delta Lake：保持快速且干净

> 原文：[https://towardsdatascience.com/delta-lake-keeping-it-fast-and-clean-3c9d4f9e2f5e?source=collection_archive---------2-----------------------#2023-02-15](https://towardsdatascience.com/delta-lake-keeping-it-fast-and-clean-3c9d4f9e2f5e?source=collection_archive---------2-----------------------#2023-02-15)

## 想知道如何提升 Delta 表的性能吗？掌握如何保持 Delta 表的快速和干净。

[](https://medium.com/@vitorf24?source=post_page-----3c9d4f9e2f5e--------------------------------)[![Vitor Teixeira](../Images/db450ae1e572a49357c02e9ba3eb4f9d.png)](https://medium.com/@vitorf24?source=post_page-----3c9d4f9e2f5e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3c9d4f9e2f5e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3c9d4f9e2f5e--------------------------------) [Vitor Teixeira](https://medium.com/@vitorf24?source=post_page-----3c9d4f9e2f5e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b05068b69d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdelta-lake-keeping-it-fast-and-clean-3c9d4f9e2f5e&user=Vitor+Teixeira&userId=6b05068b69d8&source=post_page-6b05068b69d8----3c9d4f9e2f5e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3c9d4f9e2f5e--------------------------------) ·11 分钟阅读·2023年2月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3c9d4f9e2f5e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdelta-lake-keeping-it-fast-and-clean-3c9d4f9e2f5e&user=Vitor+Teixeira&userId=6b05068b69d8&source=-----3c9d4f9e2f5e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3c9d4f9e2f5e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdelta-lake-keeping-it-fast-and-clean-3c9d4f9e2f5e&source=-----3c9d4f9e2f5e---------------------bookmark_footer-----------)![](../Images/8e8a76c0c9c9379df0e8dcc11ab6c839.png)

关于如何保持 Delta 表快速且干净的简化流程图（作者图片）

保持 Delta 表快速且干净对于维护数据管道的效率至关重要。Delta 表可能会随着时间的推移变得非常庞大，导致查询性能下降和存储成本增加。然而，有一些操作和权衡可以积极影响表的速度。

在这篇博客文章中，我们将使用 [people10m 公共数据集](https://learn.microsoft.com/en-us/azure/databricks/dbfs/databricks-datasets#create-a-table-based-on-a-databricks-dataset)，这是在 Databricks Community Edition 上提供的数据集，来展示如何使用 Delta 操作保持表的快速和干净，并解释背后的运作情况。

# 分析 delta 日志

我们将从检查数据集中的内容开始。默认情况下，这些数据集在 Databricks 上可用，你可以通过[这里](https://learn.microsoft.com/en-us/azure/databricks/dbfs/databricks-datasets#sql)访问它。

![](../Images/8aaae8e3d80dd2015369c6e8b3546ce6.png)

原始 Delta 表中的文件

我们有 16 个 parquet 条目和一个 *_delta_log* 文件夹，其中包含所有事务日志，这些日志记录了所有的变化，这些变化堆叠在一起创建了我们的 delta 表。
