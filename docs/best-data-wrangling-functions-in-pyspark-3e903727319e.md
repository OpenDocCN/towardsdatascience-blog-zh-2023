# 在 PySpark 中最佳的数据处理函数

> 原文：[`towardsdatascience.com/best-data-wrangling-functions-in-pyspark-3e903727319e?source=collection_archive---------6-----------------------#2023-12-12`](https://towardsdatascience.com/best-data-wrangling-functions-in-pyspark-3e903727319e?source=collection_archive---------6-----------------------#2023-12-12)

## 学习在处理大数据时最有用的 PySpark 函数

[](https://gustavorsantos.medium.com/?source=post_page-----3e903727319e--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----3e903727319e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3e903727319e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3e903727319e--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----3e903727319e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbest-data-wrangling-functions-in-pyspark-3e903727319e&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----3e903727319e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3e903727319e--------------------------------) ·7 分钟阅读·2023 年 12 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3e903727319e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbest-data-wrangling-functions-in-pyspark-3e903727319e&user=Gustavo+Santos&userId=4429d99b1245&source=-----3e903727319e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3e903727319e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbest-data-wrangling-functions-in-pyspark-3e903727319e&source=-----3e903727319e---------------------bookmark_footer-----------)![](img/fa66dae99d44fd5c8cadf67a519d043e.png)

照片由 [Oskar Yildiz](https://unsplash.com/@oskaryil?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来源于 [Unsplash](https://unsplash.com/photos/turned-on-gray-laptop-computer-cOkpTiJMGzA?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

# 介绍

我每天在 Databricks 上使用 PySpark。作为一名数据科学家，我的工作需要处理大量不同表格中的数据。这是一项挑战性很大的工作，很多时候都是如此。

尽管**提取、转换和加载（ETL）**过程听起来很简单，但我可以告诉你，它并不总是如此。当我们处理 Big Data 时，由于两个原因，我们的思维必须发生改变：

1.  数据量远远大于常规数据集。

1.  在集群中进行并行计算时，我们必须考虑到数据将被拆分到许多工作节点中以执行部分任务，然后再作为整体合并。这个过程，很多时候，如果查询过于复杂，可能会变得非常耗时。

了解这一点，我们必须学会如何编写智能的 Big Data 查询。在这篇文章中，我将展示一些我最喜欢的 `pyspark.sql.functions` 模块中的函数，旨在帮助你在 PySpark 中进行数据整理。

# 最佳函数

现在让我们进入本文的内容。
