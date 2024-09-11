# 利用Presto释放分布式SQL的威力：综合指南

> 原文：[https://towardsdatascience.com/what-is-apache-presto-6986d1fbf951?source=collection_archive---------16-----------------------#2023-01-06](https://towardsdatascience.com/what-is-apache-presto-6986d1fbf951?source=collection_archive---------16-----------------------#2023-01-06)

## 了解Presto的所有信息以及如何在你的数据环境中使用它

[](https://medium.com/@niklas_lang?source=post_page-----6986d1fbf951--------------------------------)[![Niklas Lang](../Images/5fa71386db00d248438c588c5ae79c67.png)](https://medium.com/@niklas_lang?source=post_page-----6986d1fbf951--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6986d1fbf951--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6986d1fbf951--------------------------------) [Niklas Lang](https://medium.com/@niklas_lang?source=post_page-----6986d1fbf951--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3631074637a9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-apache-presto-6986d1fbf951&user=Niklas+Lang&userId=3631074637a9&source=post_page-3631074637a9----6986d1fbf951---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6986d1fbf951--------------------------------) ·7分钟阅读·2023年1月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6986d1fbf951&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-apache-presto-6986d1fbf951&user=Niklas+Lang&userId=3631074637a9&source=-----6986d1fbf951---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6986d1fbf951&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-apache-presto-6986d1fbf951&source=-----6986d1fbf951---------------------bookmark_footer-----------)![](../Images/f78de98edb175d35cfbb55e329a4c257.png)

图片由[Anish Prajapati](https://unsplash.com/@anesprajapati?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Presto是一个开源的分布式[SQL](https://databasecamp.de/daten/sql)引擎，适用于查询大量数据。它由Facebook于2012年开发，并随后在Apache许可下开源。该引擎不提供自己的数据库系统，因此通常与知名数据库解决方案一起使用，如[Apache Hadoop](https://databasecamp.de/daten/hadoop-erklaert)或[MongoDB](https://databasecamp.de/daten/mongodb)。

# Presto是如何构建的？

Presto 的结构类似于经典的数据库管理系统（DBMS），这些系统使用所谓的大规模并行处理（MPP）。它使用不同的组件来执行不同的任务：

+   **客户端**：客户端是每个查询的起点和终点。它将 SQL 命令传递给协调器，并从工作节点接收最终结果。

+   **协调器**：协调器接收来自客户端的执行命令，并将其拆解以分析其处理的复杂性。他规划或协调多个命令的执行，并通过调度程序监控它们的处理。根据执行计划，这些命令随后被传递给调度程序。
