# Parquet 最佳实践：无需加载即可发现你的数据

> 原文：[`towardsdatascience.com/parquet-best-practices-discover-your-data-without-loading-them-f854c57a45b6?source=collection_archive---------2-----------------------#2023-01-03`](https://towardsdatascience.com/parquet-best-practices-discover-your-data-without-loading-them-f854c57a45b6?source=collection_archive---------2-----------------------#2023-01-03)

## 元数据、行组统计、分区发现及重分区

[](https://medium.com/@arli94?source=post_page-----f854c57a45b6--------------------------------)![Arli](https://medium.com/@arli94?source=post_page-----f854c57a45b6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f854c57a45b6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f854c57a45b6--------------------------------) [Arli](https://medium.com/@arli94?source=post_page-----f854c57a45b6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9b9b5a558522&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fparquet-best-practices-discover-your-data-without-loading-them-f854c57a45b6&user=Arli&userId=9b9b5a558522&source=post_page-9b9b5a558522----f854c57a45b6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f854c57a45b6--------------------------------) ·8 分钟阅读·2023 年 1 月 3 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff854c57a45b6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fparquet-best-practices-discover-your-data-without-loading-them-f854c57a45b6&user=Arli&userId=9b9b5a558522&source=-----f854c57a45b6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff854c57a45b6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fparquet-best-practices-discover-your-data-without-loading-them-f854c57a45b6&source=-----f854c57a45b6---------------------bookmark_footer-----------)

*如果你想亲身体验 Medium，请考虑通过* [***注册会员***](https://medium.com/@arli94/membership)*来支持我和其他成千上万的作者。每月仅需 $5，这对我们作者非常有帮助，而且你还可以访问 Medium 上所有精彩的故事。*

![](img/c510a1ca6e02384122e5f50eb2674840.png)

照片由 [Jakarta Parquet](https://unsplash.com/@lantai_kayu?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

*这篇文章是关于 Parquet 系列文章中的下一篇。如果你对 Parquet 没有任何知识，应该先查看之前的* [*Parquet 文章*](https://medium.com/towards-data-science/easy-parquet-tutorial-best-practices-237955e46cb7) *，但它对于更高级的读者也是一个很好的提醒。如果你想重现本文的输入数据，代码在文末。*

*Apache Parquet* 是一种面向大数据框架的列式存储格式，例如 *Apache Hadoop* 和 *Apache Spark*。它旨在通过使用*列式存储格式* 来提高大数据处理的性能，这种格式以*压缩*和*高效*的方式存储数据。

*Parquet* 的采用率不断上升，因为越来越多的组织转向大数据技术以处理和分析大型数据集。随着这种持续的发展，大家学习一些最佳实践并学会如何在*Parquet* 文件中导航变得非常重要。
