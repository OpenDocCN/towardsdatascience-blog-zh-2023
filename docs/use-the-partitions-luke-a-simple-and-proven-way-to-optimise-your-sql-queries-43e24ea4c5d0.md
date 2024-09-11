# 使用分区吧，卢克！一种简单而有效的优化SQL查询的方法

> 原文：[https://towardsdatascience.com/use-the-partitions-luke-a-simple-and-proven-way-to-optimise-your-sql-queries-43e24ea4c5d0?source=collection_archive---------1-----------------------#2023-12-07](https://towardsdatascience.com/use-the-partitions-luke-a-simple-and-proven-way-to-optimise-your-sql-queries-43e24ea4c5d0?source=collection_archive---------1-----------------------#2023-12-07)

## 如果你曾经写过一个运行时间很长的SQL查询，那么这篇文章就是为你准备的

[](https://medium.com/@mattchapmanmsc?source=post_page-----43e24ea4c5d0--------------------------------)[![Matt Chapman](../Images/7511deb8d9ed408ece21031f6614c532.png)](https://medium.com/@mattchapmanmsc?source=post_page-----43e24ea4c5d0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----43e24ea4c5d0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----43e24ea4c5d0--------------------------------) [Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----43e24ea4c5d0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbf7d13fc53db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-the-partitions-luke-a-simple-and-proven-way-to-optimise-your-sql-queries-43e24ea4c5d0&user=Matt+Chapman&userId=bf7d13fc53db&source=post_page-bf7d13fc53db----43e24ea4c5d0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----43e24ea4c5d0--------------------------------) · 8分钟阅读 · 2023年12月7日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F43e24ea4c5d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-the-partitions-luke-a-simple-and-proven-way-to-optimise-your-sql-queries-43e24ea4c5d0&user=Matt+Chapman&userId=bf7d13fc53db&source=-----43e24ea4c5d0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F43e24ea4c5d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-the-partitions-luke-a-simple-and-proven-way-to-optimise-your-sql-queries-43e24ea4c5d0&source=-----43e24ea4c5d0---------------------bookmark_footer-----------)![](../Images/caa41d8a1a68733fba112f4270c57c15.png)

宝宝尤达喜欢分区。你呢？图像由[Victor Serban](https://unsplash.com/@victorserban)提供，来源于[Unsplash](https://unsplash.com/photos/green-frog-plush-toy-on-brown-textile-ZFN6UNWhstI)

数据科学家们喜欢SQL，但我们确实不擅长编写高性能的查询（也许是因为我们花了太多时间讨论它是发音为“S-Q-L”还是“sequel”？）。

在这篇文章中，我将展示如何使用***SQL分区***来优化你的查询，并编写运行更快、更便宜的代码。如果你已经掌握了SQL的基础知识，并希望开始解锁更高级的数据科学技能，这将是你工具箱中的一个极佳补充。

# 什么是分区表？

一个分区表是将表划分成多个段/分区的表（谁会想到呢？）。

在分区表中，每个分区都存储在服务器上的不同位置。这与普通（未分区）SQL表不同，后者整个表都位于一个位置。

这是一个使用关于我最喜欢的三本书的每日销售的虚拟数据的比较：
