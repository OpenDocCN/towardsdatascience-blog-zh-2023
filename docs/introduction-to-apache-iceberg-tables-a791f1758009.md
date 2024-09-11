# Apache Iceberg 表格介绍

> 原文：[`towardsdatascience.com/introduction-to-apache-iceberg-tables-a791f1758009?source=collection_archive---------3-----------------------#2023-04-10`](https://towardsdatascience.com/introduction-to-apache-iceberg-tables-a791f1758009?source=collection_archive---------3-----------------------#2023-04-10)

## 选择 Apache Iceberg 作为数据湖的几个 compelling 理由

[](https://mshakhomirov.medium.com/?source=post_page-----a791f1758009--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----a791f1758009--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a791f1758009--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a791f1758009--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----a791f1758009--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe06a48b3dd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-apache-iceberg-tables-a791f1758009&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=post_page-e06a48b3dd48----a791f1758009---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a791f1758009--------------------------------) ·8 分钟阅读·2023 年 4 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa791f1758009&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-apache-iceberg-tables-a791f1758009&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=-----a791f1758009---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa791f1758009&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-apache-iceberg-tables-a791f1758009&source=-----a791f1758009---------------------bookmark_footer-----------)![](img/8757d57a9e2fc3aae50cadf617507568.png)

图片来源：[Annie Spratt](https://unsplash.com/@anniespratt?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**Apache Iceberg**：它是什么？Apache Iceberg —— 是一种新的数据湖文件格式吗？还是一种表格格式？它为何如此优秀？数据湖的时间旅行？

我将在这个故事中尝试回答所有这些问题。

> 事务一致的数据湖表格与时间点快照隔离就是我们所需要的。

选择表格格式是追求数据湖或数据网格策略的关键决策。需要考虑的一些重要数据湖平台功能包括**模式演变支持**、**读写时间**、**可扩展性**（数据处理是否支持 Hadoop 分割？）、**压缩**效率和**时间旅行**等。

根据业务需求选择合适的文件和表格格式将决定数据平台的速度和成本效益。

**Iceberg** 是一种***表格格式***，引擎和文件格式无关。Iceberg 一般不是**Apache Hive**等之前技术的开发。这样其实是件好事，因为从旧技术开发新东西可能会有局限性。一个好的例子是模式如何随时间变化，Iceberg 可以简便地处理这种变化。
