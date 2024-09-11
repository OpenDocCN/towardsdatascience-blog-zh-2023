# Docker 化 Apache Zeppelin 和 Apache Spark 以便捷部署

> 原文：[`towardsdatascience.com/dockerizing-apache-zeppelin-and-apache-spark-for-easy-deployment-1814d0c1c245?source=collection_archive---------8-----------------------#2023-01-24`](https://towardsdatascience.com/dockerizing-apache-zeppelin-and-apache-spark-for-easy-deployment-1814d0c1c245?source=collection_archive---------8-----------------------#2023-01-24)

## 了解如何使用 Docker-Compose 和卷构建一个便携且可扩展的数据分析环境。

[](https://anbento4.medium.com/?source=post_page-----1814d0c1c245--------------------------------)![Antonello Benedetto](https://anbento4.medium.com/?source=post_page-----1814d0c1c245--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1814d0c1c245--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1814d0c1c245--------------------------------) [Antonello Benedetto](https://anbento4.medium.com/?source=post_page-----1814d0c1c245--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2d93b58a0f8a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdockerizing-apache-zeppelin-and-apache-spark-for-easy-deployment-1814d0c1c245&user=Antonello+Benedetto&userId=2d93b58a0f8a&source=post_page-2d93b58a0f8a----1814d0c1c245---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1814d0c1c245--------------------------------) ·9 分钟阅读·2023 年 1 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1814d0c1c245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdockerizing-apache-zeppelin-and-apache-spark-for-easy-deployment-1814d0c1c245&user=Antonello+Benedetto&userId=2d93b58a0f8a&source=-----1814d0c1c245---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1814d0c1c245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdockerizing-apache-zeppelin-and-apache-spark-for-easy-deployment-1814d0c1c245&source=-----1814d0c1c245---------------------bookmark_footer-----------)![](img/dcd662fabf5ce0f263fe35015b17886d.png)

照片由 [Tom Fisk](https://www.pexels.com/@tomfisk/) 提供，发布在 [Pexels](https://www.pexels.com/photo/birds-eye-view-photo-of-freight-containers-2226458/)

## 按需课程 | 推荐

*一些读者联系我，询问有关* ***使用 Python 的 Apache Spark**** 的按需课程。这些是我推荐的 3 个优秀资源：*

+   [**使用 Apache Kafka 和 Apache Spark 的数据流处理与 Apache Spark 纳米学位（UDACITY）**](https://imp.i115008.net/zaX10r)

+   [**数据工程纳米学位 (UDACITY)**](https://imp.i115008.net/zaX10r)

+   [**大数据与 PySpark 的 Spark 和 Python (UDEMY)**](https://click.linksynergy.com/deeplink?id=533LxfDBSaM&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fspark-and-python-for-big-data-with-pyspark%2F)

*还不是 Medium 会员？考虑通过我的* [*推荐链接*](https://anbento4.medium.com/membership) *注册，以最低每月$5 的费用获取 Medium 的所有服务！*

# 介绍

Docker 彻底改变了我们部署和管理应用程序以及数据分析工具的方式。它允许轻松扩展，并能轻松定制服务以满足特定需求。

在本教程中，我将展示如何使用`docker-compose.yaml`文件快速部署 Apache Zeppelin 和 Apache Spark，并利用卷来管理服务之间的数据依赖。
