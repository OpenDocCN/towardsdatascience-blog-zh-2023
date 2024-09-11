# 在编写 Apache Beam 流水线时使用示例的 Map、Filter 和 CombinePerKey 转换

> 原文：[https://towardsdatascience.com/map-filter-and-combineperkey-transforms-in-writing-apache-beam-pipelines-with-examples-e06926124a02?source=collection_archive---------12-----------------------#2023-07-12](https://towardsdatascience.com/map-filter-and-combineperkey-transforms-in-writing-apache-beam-pipelines-with-examples-e06926124a02?source=collection_archive---------12-----------------------#2023-07-12)

![](../Images/3c0bc71612c5632ec1b351c55f68aa03.png)

照片由 [JJ Ying](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 让我们用一些真实的数据来练习

[](https://rashida00.medium.com/?source=post_page-----e06926124a02--------------------------------)[![Rashida Nasrin Sucky](../Images/42bd057e8eca255907c43c29a498f2ca.png)](https://rashida00.medium.com/?source=post_page-----e06926124a02--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e06926124a02--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e06926124a02--------------------------------) [Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----e06926124a02--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a36b941a136&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmap-filter-and-combineperkey-transforms-in-writing-apache-beam-pipelines-with-examples-e06926124a02&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=post_page-8a36b941a136----e06926124a02---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e06926124a02--------------------------------) ·8 min read·2023年7月12日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe06926124a02&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmap-filter-and-combineperkey-transforms-in-writing-apache-beam-pipelines-with-examples-e06926124a02&source=-----e06926124a02---------------------bookmark_footer-----------)

Apache Beam 作为高效且可移植的大数据处理管道的统一编程模型，正在获得越来越多的关注。它可以处理批处理和流处理数据。这也是名字的由来。Beam 是 Batch 和 Stream 的组合词：

B(from **B**atch) + eam(from str**eam**)= Beam

移植性也是一个很棒的特点。你只需专注于运行管道，它可以在任何地方运行，例如 Spark、Flink、Apex 或 Cloud Dataflow。你无需为此更改逻辑或语法。

在本文中，我们将重点学习如何使用示例编写一些 ETL 管道。我们将尝试一些转换操作，使用一个好的数据集，希望你会发现这些转换操作在你的工作中也很有用。

请随意下载这个 [公共数据集](https://creativecommons.org/publicdomain/zero/1.0/) 并跟随操作：

[示例销售数据 | Kaggle](https://www.kaggle.com/datasets/kyanyoga/sample-sales-data)

这个练习使用了一个 Google Colab notebook。因此，安装非常简单。只需使用这一行代码：

```py
!pip install --quiet apache_beam
```
