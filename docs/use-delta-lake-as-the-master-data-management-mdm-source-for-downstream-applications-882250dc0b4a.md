# 将Delta Lake用作下游应用程序的主数据管理（MDM）来源

> 原文：[https://towardsdatascience.com/use-delta-lake-as-the-master-data-management-mdm-source-for-downstream-applications-882250dc0b4a?source=collection_archive---------1-----------------------#2023-02-05](https://towardsdatascience.com/use-delta-lake-as-the-master-data-management-mdm-source-for-downstream-applications-882250dc0b4a?source=collection_archive---------1-----------------------#2023-02-05)

## 在本文中，我们将尝试了解如何利用Delta Lake的变更数据源来供给下游应用程序

[](https://mkukreja1.medium.com/?source=post_page-----882250dc0b4a--------------------------------)[![Manoj Kukreja](../Images/de7c954b505e9749bad47975a3bee80b.png)](https://mkukreja1.medium.com/?source=post_page-----882250dc0b4a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----882250dc0b4a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----882250dc0b4a--------------------------------) [Manoj Kukreja](https://mkukreja1.medium.com/?source=post_page-----882250dc0b4a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4076fc16cb6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-delta-lake-as-the-master-data-management-mdm-source-for-downstream-applications-882250dc0b4a&user=Manoj+Kukreja&userId=4076fc16cb6a&source=post_page-4076fc16cb6a----882250dc0b4a---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----882250dc0b4a--------------------------------) ·9分钟阅读·2023年2月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F882250dc0b4a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-delta-lake-as-the-master-data-management-mdm-source-for-downstream-applications-882250dc0b4a&user=Manoj+Kukreja&userId=4076fc16cb6a&source=-----882250dc0b4a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F882250dc0b4a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-delta-lake-as-the-master-data-management-mdm-source-for-downstream-applications-882250dc0b4a&source=-----882250dc0b4a---------------------bookmark_footer-----------)![](../Images/9929918cfc996b208d995d6122853179.png)

图片来源于[Satheesh Sankaran](https://pixabay.com/users/satheeshsankaran-11196627/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7206402)来自[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7206402)

根据ACID规则，隔离理论指出“***任何事务的中间状态不应影响其他事务***”。几乎每个现代数据库都建立在遵循这一规则的基础上。不幸的是，直到最近，Big Data 世界中无法有效地实现这一规则。原因是什么？

现代分布式处理框架，如**Hadoop MapReduce**和**Apache Spark**，会批量执行计算。计算完成后会生成一组输出文件，每个文件存储一系列记录。通常，分区数和减少器的数量会影响生成的输出文件数量。但仍然存在一些问题：

1.  轻微的记录级别更改或新记录的添加（CDC）会迫使你每次都重新计算整个批次——这是一种巨大的计算周期浪费，影响成本和时间。

1.  在批处理重新计算的过程中，下游消费者可能会看到不一致的数据。
