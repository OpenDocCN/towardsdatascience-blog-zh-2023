# Lucene 内幕——处理整数编码和压缩

> 原文：[https://towardsdatascience.com/lucene-inside-out-dealing-with-integer-encoding-and-compression-fe28f9dd265d?source=collection_archive---------4-----------------------#2023-06-28](https://towardsdatascience.com/lucene-inside-out-dealing-with-integer-encoding-and-compression-fe28f9dd265d?source=collection_archive---------4-----------------------#2023-06-28)

## 深入了解 PackedInts、VInt、FixedBitSet 和 **RoaringDocIdSet** (**Roaring Bitmaps**)

[](https://peggy1502.medium.com/?source=post_page-----fe28f9dd265d--------------------------------)[![Peggy Chang](../Images/f59e8d3b97ea7db59fc8cb33f05e835b.png)](https://peggy1502.medium.com/?source=post_page-----fe28f9dd265d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fe28f9dd265d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fe28f9dd265d--------------------------------) [Peggy Chang](https://peggy1502.medium.com/?source=post_page-----fe28f9dd265d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb08bdbf6e014&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flucene-inside-out-dealing-with-integer-encoding-and-compression-fe28f9dd265d&user=Peggy+Chang&userId=b08bdbf6e014&source=post_page-b08bdbf6e014----fe28f9dd265d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fe28f9dd265d--------------------------------) ·13 分钟阅读·2023年6月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffe28f9dd265d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flucene-inside-out-dealing-with-integer-encoding-and-compression-fe28f9dd265d&user=Peggy+Chang&userId=b08bdbf6e014&source=-----fe28f9dd265d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffe28f9dd265d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flucene-inside-out-dealing-with-integer-encoding-and-compression-fe28f9dd265d&source=-----fe28f9dd265d---------------------bookmark_footer-----------)![](../Images/3e285f99edf68c33f97c132920235b8a.png)

图片由 [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=1536589) 提供，来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=1536589)

早前，我们学习了使用 [产品量化](https://medium.com/towards-data-science/product-quantization-for-similarity-search-2f1f67c5fddd) 进行相似性搜索的向量压缩方法。

[产品量化用于相似性搜索](https://example.org/product-quantization-for-similarity-search-2f1f67c5fddd?source=post_page-----fe28f9dd265d--------------------------------)  

### 如何在内存中压缩和适配大量的向量以进行相似性搜索，并进行不对称距离计算…  

towardsdatascience.com](/product-quantization-for-similarity-search-2f1f67c5fddd?source=post_page-----fe28f9dd265d--------------------------------)  

在这篇文章中，我们将深入了解Lucene中整数如何被编码和压缩，探索反向索引如何成为核心。

# Lucene — 简介  

[Lucene](https://lucene.apache.org/) 是一个用Java编写的开源搜索引擎库。由Doug Cutting于1999年创建，以全文搜索和索引闻名。  

这个开源软件项目隶属于[Apache 软件基金会](https://www.apache.org/)，在经过二十多年的发展后仍在积极开发中。多年来，它已经演变成一个功能强大、性能优越的搜索引擎库。  

毋庸置疑，Lucene的成功在很大程度上归功于其强大的社区和贡献者所做的出色工作…  
