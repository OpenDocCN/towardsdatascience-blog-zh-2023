# DynamoDB 机器学习工程

> 原文：[https://towardsdatascience.com/ml-engineering-with-dynamodb-398ff2e6394e?source=collection_archive---------13-----------------------#2023-02-07](https://towardsdatascience.com/ml-engineering-with-dynamodb-398ff2e6394e?source=collection_archive---------13-----------------------#2023-02-07)

## 如何利用这一强大的 NoSQL 数据库进行在线推断

[](https://medium.com/@achad?source=post_page-----398ff2e6394e--------------------------------)[![Avi Chad-Friedman](../Images/d703a18c3cce123cbeeac789dc597598.png)](https://medium.com/@achad?source=post_page-----398ff2e6394e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----398ff2e6394e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----398ff2e6394e--------------------------------) [Avi Chad-Friedman](https://medium.com/@achad?source=post_page-----398ff2e6394e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F22da9d6fd08a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fml-engineering-with-dynamodb-398ff2e6394e&user=Avi+Chad-Friedman&userId=22da9d6fd08a&source=post_page-22da9d6fd08a----398ff2e6394e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----398ff2e6394e--------------------------------) ·5 分钟阅读·2023年2月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F398ff2e6394e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fml-engineering-with-dynamodb-398ff2e6394e&user=Avi+Chad-Friedman&userId=22da9d6fd08a&source=-----398ff2e6394e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F398ff2e6394e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fml-engineering-with-dynamodb-398ff2e6394e&source=-----398ff2e6394e---------------------bookmark_footer-----------)

## 为什么考虑 DynamoDB？

在处理离线批量 ML 系统时，基于 SQL 的工作流程对数据仓库如 Snowflake 构成了业务分析的支柱，[描述性统计](/anomaly-detection-in-sql-2bcd8648f7a8)和预测建模。这是一种理想状态，其中复杂的转换可以推送到分布式数据库引擎，而特征则在进行推断时使用的语言中定义。

避免离开这一理想状态，除非你因实际需要进行实时推断而被迫离开。如果你发现自己进入了这个新的、混乱的世界，尤其是相关特征以高频率变化，需要低延迟读写时，你可能需要 NoSQL。

AWS DynamoDB 是一个 [极具可扩展性](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf) 的托管 NoSQL 数据库。像其他 NoSQL 数据库一样，它在一致性和可用性之间进行权衡，使其成为构建低延迟 ML 预测器的理想数据存储。本文的目标是展示一些设计模式，帮助你构建灵活的生产工作流，以处理常见的 ML 访问模式。

![](../Images/509e98e06ba6dc121d8d2fc4b316fb54.png)

由作者在 [https://imgflip.com/](https://imgflip.com/) 生成的图像

## **DynamoDB 索引入门**
