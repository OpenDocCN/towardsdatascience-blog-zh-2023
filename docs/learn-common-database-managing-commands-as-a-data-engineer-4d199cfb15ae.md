# 我们应该了解的用于管理表的 MySQL 数据定义语言（DDL）重要命令

> 原文：[https://towardsdatascience.com/learn-common-database-managing-commands-as-a-data-engineer-4d199cfb15ae?source=collection_archive---------11-----------------------#2023-02-28](https://towardsdatascience.com/learn-common-database-managing-commands-as-a-data-engineer-4d199cfb15ae?source=collection_archive---------11-----------------------#2023-02-28)

## 作为数据工程师，学习常见的数据库管理命令

[](https://lynn-kwong.medium.com/?source=post_page-----4d199cfb15ae--------------------------------)[![Lynn G. Kwong](../Images/b9a05b6587db5ca41c1d8264adda5b06.png)](https://lynn-kwong.medium.com/?source=post_page-----4d199cfb15ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4d199cfb15ae--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4d199cfb15ae--------------------------------) [Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----4d199cfb15ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff649eccbbc3d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearn-common-database-managing-commands-as-a-data-engineer-4d199cfb15ae&user=Lynn+G.+Kwong&userId=f649eccbbc3d&source=post_page-f649eccbbc3d----4d199cfb15ae---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4d199cfb15ae--------------------------------) · 8分钟阅读 · 2023年2月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4d199cfb15ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearn-common-database-managing-commands-as-a-data-engineer-4d199cfb15ae&user=Lynn+G.+Kwong&userId=f649eccbbc3d&source=-----4d199cfb15ae---------------------clap_footer-----------)

[]

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4d199cfb15ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearn-common-database-managing-commands-as-a-data-engineer-4d199cfb15ae&source=-----4d199cfb15ae---------------------bookmark_footer-----------)![](../Images/9ab632bea48c228ba8500f79c5cc4dc1.png)

图片由 geralt 提供于 Pixabay

作为数据工程师，检查和更新表的模式是我们的基本工作。虽然网上已经有很多教程，但很少有专注于应该遵循的约定。SQL 非常灵活，可以以“稳健”的方式工作。你可以使用小写和大写的查询，并以任何你想要的方式命名你的数据库/表/列/索引/视图。然而，代价是可读性降低，维护变得困难，因为不同的人可能有不同的 SQL 查询编写方式。

在这篇文章中，我们将介绍一些用于管理 MySQL 表结构的常见命令，重点讲解每个操作的惯例和最佳实践。这可以作为新数据工程师的手册（需做适当调整）。

## 准备工作

我们将使用 Docker 启动一个 MySQL 8 容器，它将作为本文的 MySQL 服务器。

```py
# Create a volume to persist the data.
$ docker volume create mysql8-data

# Create the container for MySQL.
$ docker run --name mysql8 -d -e MYSQL_ROOT_PASSWORD=root -p 13306:3306 -v…
```
