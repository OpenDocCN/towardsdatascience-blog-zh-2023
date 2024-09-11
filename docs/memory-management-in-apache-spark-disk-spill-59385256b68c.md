# 《Apache Spark 的内存管理：磁盘溢出》

> 原文：[`towardsdatascience.com/memory-management-in-apache-spark-disk-spill-59385256b68c?source=collection_archive---------3-----------------------#2023-09-15`](https://towardsdatascience.com/memory-management-in-apache-spark-disk-spill-59385256b68c?source=collection_archive---------3-----------------------#2023-09-15)

## 它是什么以及如何处理它

[](https://medium.com/@tomhcorbin?source=post_page-----59385256b68c--------------------------------)![Tom Corbin](https://medium.com/@tomhcorbin?source=post_page-----59385256b68c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----59385256b68c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----59385256b68c--------------------------------) [Tom Corbin](https://medium.com/@tomhcorbin?source=post_page-----59385256b68c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F96fa70c9b31d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmemory-management-in-apache-spark-disk-spill-59385256b68c&user=Tom+Corbin&userId=96fa70c9b31d&source=post_page-96fa70c9b31d----59385256b68c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----59385256b68c--------------------------------) ·12 分钟阅读·2023 年 9 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F59385256b68c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmemory-management-in-apache-spark-disk-spill-59385256b68c&user=Tom+Corbin&userId=96fa70c9b31d&source=-----59385256b68c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F59385256b68c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmemory-management-in-apache-spark-disk-spill-59385256b68c&source=-----59385256b68c---------------------bookmark_footer-----------)![](img/96049b2bcf518a5a2922df320a077a51.png)

图片来源于 [benjamin lehman](https://unsplash.com/@benjaminlehman?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在大数据的世界里，Apache Spark 因其处理海量数据的能力而备受喜爱。作为全球顶尖的大数据处理引擎，学习使用这个工具是任何大数据专业人士技能的基石。了解 Spark 的内存管理系统以及“磁盘溢出”的挑战，是这条道路上的重要一步。

磁盘溢出是当 Spark 无法再将数据放入内存时发生的情况，此时需要将数据存储到磁盘上。Spark 的一个主要优点是其内存处理能力，这比使用磁盘驱动器要快得多。因此，构建会溢出到磁盘的应用程序在某种程度上违背了 Spark 的初衷。

磁盘溢出有许多不良后果，因此学习如何处理它是 Spark 开发者的一项重要技能。这篇文章旨在帮助解决这个问题。我们将深入探讨磁盘溢出是什么，为什么会发生，它的后果是什么，以及如何修复它。通过使用 Spark 的内置 UI，我们将学习如何识别磁盘溢出的迹象并理解其指标。最后，我们将探索一些缓解磁盘溢出的可行策略，例如有效的数据分区、适当的缓存和动态集群调整。
