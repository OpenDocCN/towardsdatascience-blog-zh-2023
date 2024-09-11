# 使用 Python 写入 Parquet 数据的 4 种方法：比较

> 原文：[`towardsdatascience.com/4-ways-to-write-data-to-parquet-with-python-a-comparison-3c4f54ee5fec?source=collection_archive---------1-----------------------#2023-03-13`](https://towardsdatascience.com/4-ways-to-write-data-to-parquet-with-python-a-comparison-3c4f54ee5fec?source=collection_archive---------1-----------------------#2023-03-13)

## 学习如何使用 Pandas、FastParquet、PyArrow 或 PySpark 高效地将数据写入 Parquet 格式。

[](https://anbento4.medium.com/?source=post_page-----3c4f54ee5fec--------------------------------)![Antonello Benedetto](https://anbento4.medium.com/?source=post_page-----3c4f54ee5fec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3c4f54ee5fec--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3c4f54ee5fec--------------------------------) [Antonello Benedetto](https://anbento4.medium.com/?source=post_page-----3c4f54ee5fec--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2d93b58a0f8a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-ways-to-write-data-to-parquet-with-python-a-comparison-3c4f54ee5fec&user=Antonello+Benedetto&userId=2d93b58a0f8a&source=post_page-2d93b58a0f8a----3c4f54ee5fec---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3c4f54ee5fec--------------------------------) ·7 分钟阅读·2023 年 3 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3c4f54ee5fec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-ways-to-write-data-to-parquet-with-python-a-comparison-3c4f54ee5fec&user=Antonello+Benedetto&userId=2d93b58a0f8a&source=-----3c4f54ee5fec---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3c4f54ee5fec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-ways-to-write-data-to-parquet-with-python-a-comparison-3c4f54ee5fec&source=-----3c4f54ee5fec---------------------bookmark_footer-----------)![](img/39041e5a101311da2fe0002dd298f589.png)

[照片来源：Dominika Roseclay](https://www.pexels.com/photo/background-of-pile-of-firewood-with-rough-surface-4318196/)

## 按需课程 | 推荐

*一些读者联系我，询问有关* ***使用 Python 和 PySpark 进行数据工程*** *的按需课程。这些是我推荐的 3 个优秀资源：*

+   [**使用 Apache Kafka 和 Apache Spark 进行数据流处理纳米学位 (UDACITY)**](https://imp.i115008.net/zaX10r)

+   [**数据工程纳米学位 (UDACITY)**](https://imp.i115008.net/zaX10r)

+   [**Spark 和 Python 用于大数据与 PySpark (UDEMY)**](https://click.linksynergy.com/deeplink?id=533LxfDBSaM&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fspark-and-python-for-big-data-with-pyspark%2F)

*还不是 Medium 会员？考虑通过我的* [*推荐链接*](https://anbento4.medium.com/membership) *注册，每月仅需 $5 即可访问 Medium 提供的所有内容！*

# 介绍

在当今数据驱动的世界中，大规模数据集的高效存储和处理是许多企业和组织的关键需求。这就是 Parquet 文件格式的作用所在。

[Parquet 是一种**列式存储格式**，旨在优化数据处理和查询性能，同时最小化存储空间。](https://www.databricks.com/glossary/what-is-parquet)
