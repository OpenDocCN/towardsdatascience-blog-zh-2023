# Delta Lake — 分区、Z-Order 和液态聚类

> 原文：[https://towardsdatascience.com/delta-lake-partitioning-z-order-and-liquid-clustering-944030ff1828?source=collection_archive---------1-----------------------#2023-11-08](https://towardsdatascience.com/delta-lake-partitioning-z-order-and-liquid-clustering-944030ff1828?source=collection_archive---------1-----------------------#2023-11-08)

## Delta 中是如何实现不同的分区/聚类方法的？它们在实践中是如何工作的？

[](https://medium.com/@vitorf24?source=post_page-----944030ff1828--------------------------------)[![Vitor Teixeira](../Images/db450ae1e572a49357c02e9ba3eb4f9d.png)](https://medium.com/@vitorf24?source=post_page-----944030ff1828--------------------------------)[](https://towardsdatascience.com/?source=post_page-----944030ff1828--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----944030ff1828--------------------------------) [Vitor Teixeira](https://medium.com/@vitorf24?source=post_page-----944030ff1828--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b05068b69d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdelta-lake-partitioning-z-order-and-liquid-clustering-944030ff1828&user=Vitor+Teixeira&userId=6b05068b69d8&source=post_page-6b05068b69d8----944030ff1828---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----944030ff1828--------------------------------) ·10分钟阅读·2023年11月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F944030ff1828&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdelta-lake-partitioning-z-order-and-liquid-clustering-944030ff1828&user=Vitor+Teixeira&userId=6b05068b69d8&source=-----944030ff1828---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F944030ff1828&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdelta-lake-partitioning-z-order-and-liquid-clustering-944030ff1828&source=-----944030ff1828---------------------bookmark_footer-----------)![](../Images/19486b33b135625836f8ae64791213b1.png)

照片由 [frame harirak](https://unsplash.com/@framemily?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

使大数据处理变得困难的一个问题一直在于它的名字——它很“大”。分区，特别是当做得好的时候，一直是通过将需要读取的数据减少到一个子集来提高大数据集上的查询执行时间的一个方法。然而，分区数据是复杂的，需要仔细考虑和一些前期规划，因为今天的需求可能不适合未来的需求。例如，在Hive风格的分区中，列可能需要更改或甚至增加它们的基数，导致数据过度分区（小文件问题），这需要对数据进行彻底重构，这并不是理想的。

Z-Order聚类是另一种用于数据跳过的技术，也避免了完全的数据扫描。然而，这种技术有一些局限性。其中之一是新摄取的数据默认情况下并没有排序，用户需要重新聚类，这意味着已经聚类的数据将被重新聚类和重写，从而增加了操作的时间。Z-Order用户还需要在每次运行命令时定义聚类列，因为它们不是任何表属性的一部分。
