# Delta Lake — 自动模式演化

> 原文：[https://towardsdatascience.com/delta-lake-automatic-schema-evolution-11d32bd1aa99?source=collection_archive---------1-----------------------#2023-03-10](https://towardsdatascience.com/delta-lake-automatic-schema-evolution-11d32bd1aa99?source=collection_archive---------1-----------------------#2023-03-10)

## 当合并进化型 DataFrame 时会发生什么，以及你可以/不能做什么

[](https://medium.com/@vitorf24?source=post_page-----11d32bd1aa99--------------------------------)[![Vitor Teixeira](../Images/db450ae1e572a49357c02e9ba3eb4f9d.png)](https://medium.com/@vitorf24?source=post_page-----11d32bd1aa99--------------------------------)[](https://towardsdatascience.com/?source=post_page-----11d32bd1aa99--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----11d32bd1aa99--------------------------------) [Vitor Teixeira](https://medium.com/@vitorf24?source=post_page-----11d32bd1aa99--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b05068b69d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdelta-lake-automatic-schema-evolution-11d32bd1aa99&user=Vitor+Teixeira&userId=6b05068b69d8&source=post_page-6b05068b69d8----11d32bd1aa99---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----11d32bd1aa99--------------------------------) · 5 分钟阅读 · 2023年3月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F11d32bd1aa99&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdelta-lake-automatic-schema-evolution-11d32bd1aa99&user=Vitor+Teixeira&userId=6b05068b69d8&source=-----11d32bd1aa99---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F11d32bd1aa99&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdelta-lake-automatic-schema-evolution-11d32bd1aa99&source=-----11d32bd1aa99---------------------bookmark_footer-----------)![](../Images/1b8dfc96acde282413eda93c212cb337.png)

图片由 [McDobbie Hu](https://unsplash.com/@hjx518756?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在上一篇文章中，我们讨论了事务日志和 [如何保持 Delta 表的快速和干净](https://medium.com/p/3c9d4f9e2f5e)。这次我们将探讨 Delta 表中的自动模式演化。

模式演变是数据管理中的关键方面。数据源随着时间的推移通常会演变并适应新的业务需求，这可能意味着需要向现有数据模式中添加或删除字段。作为数据使用者，快速而灵活地适应数据源的新特征是至关重要的，而自动模式演变使我们能够无缝地适应这些变化。

在这篇文章中，我们将讨论在使用 [Delta](https://delta.io/) 和 [people10m 公共数据集](https://learn.microsoft.com/en-us/azure/databricks/dbfs/databricks-datasets#create-a-table-based-on-a-databricks-dataset) 时的自动模式演变，这个数据集可以在 Databricks Community Edition 上找到。我们会在多个场景中测试添加和删除字段的情况。

# 设置

自动模式演变可以通过两种方式启用，具体取决于我们的工作负载。如果我们进行盲目追加，只需启用 ***mergeSchema*** 选项即可。
