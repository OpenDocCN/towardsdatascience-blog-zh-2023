# 如何在 Docker 中使用 Cloud SQL Auth Proxy 连接到 GCP Cloud SQL

> 原文：[`towardsdatascience.com/how-to-connect-to-gcp-cloud-sql-with-cloud-sql-auth-proxy-in-docker-99bdf810c498?source=collection_archive---------5-----------------------#2023-02-20`](https://towardsdatascience.com/how-to-connect-to-gcp-cloud-sql-with-cloud-sql-auth-proxy-in-docker-99bdf810c498?source=collection_archive---------5-----------------------#2023-02-20)

## 学习一种标准方法，将 docker 化应用程序连接到 GCP Cloud SQL 实例

[](https://lynn-kwong.medium.com/?source=post_page-----99bdf810c498--------------------------------)![Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----99bdf810c498--------------------------------)[](https://towardsdatascience.com/?source=post_page-----99bdf810c498--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----99bdf810c498--------------------------------) [Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----99bdf810c498--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff649eccbbc3d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-connect-to-gcp-cloud-sql-with-cloud-sql-auth-proxy-in-docker-99bdf810c498&user=Lynn+G.+Kwong&userId=f649eccbbc3d&source=post_page-f649eccbbc3d----99bdf810c498---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----99bdf810c498--------------------------------) ·9 分钟阅读·2023 年 2 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F99bdf810c498&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-connect-to-gcp-cloud-sql-with-cloud-sql-auth-proxy-in-docker-99bdf810c498&user=Lynn+G.+Kwong&userId=f649eccbbc3d&source=-----99bdf810c498---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F99bdf810c498&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-connect-to-gcp-cloud-sql-with-cloud-sql-auth-proxy-in-docker-99bdf810c498&source=-----99bdf810c498---------------------bookmark_footer-----------)![](img/ab898b9635bf96960353af4de7d4cb3b.png)

图片来源：WilliamsCreativity (Servers Data) 在 Pixabay

Google Cloud Platform (GCP) 上的 Cloud SQL 是一个很棒的服务，如果你想在云端托管你的关系数据库。连接到 Cloud SQL 的标准方法包括 GCP 资源，如 Cloud Run、Compute Engine 等。然而，关于如何将 docker 化应用程序连接到 Cloud SQL 的文档较少。

Cloud SQL Auth 代理是将您的 Docker 应用程序连接到 Cloud SQL 的推荐方式。它提供了对 Cloud SQL 实例的安全访问，无需授权网络或配置 SSL。

在这篇文章中，我们将介绍如何以多种方式使用 Cloud SQL Auth 代理，重点讲解如何编写 `docker-compose.yaml` 文件，以便将 Docker 应用程序连接到 Cloud SQL 实例。

## 准备工作

在这一点上，我假设您已经拥有一个 GCP Cloud SQL 实例。Cloud SQL 实例的创建通常由系统或 DevOps 工程师完成。然而，如果您自己做项目或只是出于学习目的想要创建一个实例，您可以自行创建。前往 GCP 控制台并按照……
