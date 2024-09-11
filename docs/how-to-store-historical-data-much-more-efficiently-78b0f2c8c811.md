# 如何更高效地存储历史数据

> 原文：[https://towardsdatascience.com/how-to-store-historical-data-much-more-efficiently-78b0f2c8c811?source=collection_archive---------2-----------------------#2023-09-10](https://towardsdatascience.com/how-to-store-historical-data-much-more-efficiently-78b0f2c8c811?source=collection_archive---------2-----------------------#2023-09-10)

## 一份使用 PySpark 的动手教程，展示如何仅存储 DataFrame 中 0.01% 的行而不丢失任何信息。

[](https://medium.com/@tomergabay?source=post_page-----78b0f2c8c811--------------------------------)[![Tomer Gabay](../Images/1fb1d408bc89415918c1aa6733df44e1.png)](https://medium.com/@tomergabay?source=post_page-----78b0f2c8c811--------------------------------)[](https://towardsdatascience.com/?source=post_page-----78b0f2c8c811--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----78b0f2c8c811--------------------------------) [Tomer Gabay](https://medium.com/@tomergabay?source=post_page-----78b0f2c8c811--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc9c352dba00a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-store-historical-data-much-more-efficiently-78b0f2c8c811&user=Tomer+Gabay&userId=c9c352dba00a&source=post_page-c9c352dba00a----78b0f2c8c811---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----78b0f2c8c811--------------------------------) ·10 min read·2023年9月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F78b0f2c8c811&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-store-historical-data-much-more-efficiently-78b0f2c8c811&user=Tomer+Gabay&userId=c9c352dba00a&source=-----78b0f2c8c811---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F78b0f2c8c811&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-store-historical-data-much-more-efficiently-78b0f2c8c811&source=-----78b0f2c8c811---------------------bookmark_footer-----------)![](../Images/e6a3ccf160e7ca3d2ab0eba9ba6199b7.png)

图片由 [Supratik Deshmukh](https://unsplash.com/@supratikdeshmukh?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在一个企业和组织收集数据比以往任何时候都多的时代，数据集往往会积累数百万行不包含任何新信息或有价值信息的冗余数据。在这篇文章中，我们将重点讨论数据管理中的一个关键方面：使用 PySpark* 删除数据集中无附加值的行。

> *[PySpark](https://spark.apache.org/docs/latest/api/python/index.html#:~:text=PySpark%20is%20the%20Python%20API,for%20interactively%20analyzing%20your%20data.) 被用于处理非常大的数据集，因为它可以跨多个计算机处理数据，从而使处理速度更快，扩展性更强。Pandas 适用于可以放在单台计算机内存中的较小数据集，但对于大数据可能变得缓慢甚至不切实际。

让我们设想以下情况：你在一家房地产公司的维护部门担任数据工程师/科学家。过去十年里，你的公司从一个外部数据库中提取了所有维护数据，包含建筑物的状况，并将其存储在公司的云存储中。这些数据可能看起来像这样：

数据集中包含三列：

+   `id` -> 用于建筑物的 ID。
