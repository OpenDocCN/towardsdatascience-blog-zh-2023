# 快速入门：连接 PostgreSQL 并将数据导入 Pandas

> 原文：[`towardsdatascience.com/a-quick-start-to-connecting-to-postgresql-and-pulling-data-into-pandas-26fbe7c0fcd5?source=collection_archive---------4-----------------------#2023-02-05`](https://towardsdatascience.com/a-quick-start-to-connecting-to-postgresql-and-pulling-data-into-pandas-26fbe7c0fcd5?source=collection_archive---------4-----------------------#2023-02-05)

## 通过将 PostgreSQL 数据导入 Pandas，快速开始数据分析和模型构建

[](https://medium.com/@broepke?source=post_page-----26fbe7c0fcd5--------------------------------)![Brian Roepke](https://medium.com/@broepke?source=post_page-----26fbe7c0fcd5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----26fbe7c0fcd5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----26fbe7c0fcd5--------------------------------) [Brian Roepke](https://medium.com/@broepke?source=post_page-----26fbe7c0fcd5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff5a92cac16d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-quick-start-to-connecting-to-postgresql-and-pulling-data-into-pandas-26fbe7c0fcd5&user=Brian+Roepke&userId=f5a92cac16d6&source=post_page-f5a92cac16d6----26fbe7c0fcd5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----26fbe7c0fcd5--------------------------------) ·4 min 阅读·2023 年 2 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F26fbe7c0fcd5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-quick-start-to-connecting-to-postgresql-and-pulling-data-into-pandas-26fbe7c0fcd5&user=Brian+Roepke&userId=f5a92cac16d6&source=-----26fbe7c0fcd5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F26fbe7c0fcd5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-quick-start-to-connecting-to-postgresql-and-pulling-data-into-pandas-26fbe7c0fcd5&source=-----26fbe7c0fcd5---------------------bookmark_footer-----------)![](img/b4223994373e20c953c2c141aa932517.png)

照片由 [Sergi Ferrete](https://unsplash.com/@sergiferrete?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# PostgreSQL 是什么？

[PostgreSQL](https://www.postgresql.org) 是一个功能强大的**关系数据库管理系统**（RDBMS），许多组织都在使用它。连接到它非常简单，得益于出色的 Python 生态系统，将数据导入 Pandas 的 Data Frame 也同样简单。我们来看一个简单的例子，帮助你入门。

# 在本地运行 PostgreSQL

运行 PostgreSQL 的一种简单方法是使用[Docker](https://www.docker.com)。如果你还没有安装 Docker，你可以[在这里](https://www.docker.com/products/docker-desktop)下载。安装 Docker 后，你可以运行以下命令来启动一个 PostgreSQL 容器。

前往 [Docker Hub](https://hub.docker.com/) 并搜索 `postgres`。你会看到几个不同的镜像。我们需要的是 `postgres` 镜像，你可以使用以下命令将其拉取下来。

```py
docker pull postgres
```

然后，你可以运行以下命令（根据 Docker Hub 上的文档）来启动一个容器。

```py
docker run --name postgres -p 5432:5432 -e POSTGRES_PASSWORD =postgrespw -d postgres
```
