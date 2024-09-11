# 如何使用 Airflow 在 AWS EMR 上自动化 PySpark 管道

> 原文：[`towardsdatascience.com/how-to-automate-pyspark-pipelines-on-aws-emr-with-airflow-a0cbd94c4516?source=collection_archive---------7-----------------------#2023-08-23`](https://towardsdatascience.com/how-to-automate-pyspark-pipelines-on-aws-emr-with-airflow-a0cbd94c4516?source=collection_archive---------7-----------------------#2023-08-23)

## 优化大数据工作流的编排。

[](https://anbento4.medium.com/?source=post_page-----a0cbd94c4516--------------------------------)![Antonello Benedetto](https://anbento4.medium.com/?source=post_page-----a0cbd94c4516--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a0cbd94c4516--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0cbd94c4516--------------------------------) [Antonello Benedetto](https://anbento4.medium.com/?source=post_page-----a0cbd94c4516--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2d93b58a0f8a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-automate-pyspark-pipelines-on-aws-emr-with-airflow-a0cbd94c4516&user=Antonello+Benedetto&userId=2d93b58a0f8a&source=post_page-2d93b58a0f8a----a0cbd94c4516---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0cbd94c4516--------------------------------) ·8 分钟阅读·2023 年 8 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa0cbd94c4516&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-automate-pyspark-pipelines-on-aws-emr-with-airflow-a0cbd94c4516&user=Antonello+Benedetto&userId=2d93b58a0f8a&source=-----a0cbd94c4516---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa0cbd94c4516&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-automate-pyspark-pipelines-on-aws-emr-with-airflow-a0cbd94c4516&source=-----a0cbd94c4516---------------------bookmark_footer-----------)![](img/8d92446cdf5a73adbbd00d940c246dee.png)

图片来源 [Tom Fisk](https://www.pexels.com/photo/trees-near-ocean-2246950/) 于 [Pexels](https://www.pexels.com/@tomfisk/)

# 按需课程 | 推荐

*一些读者联系我，询问是否有按需课程可以帮助你* ***成为*** *一名优秀的* ***数据工程师****。以下是我推荐的 3 个优秀资源：

+   [**数据工程纳米学位 (UDACITY)**](https://imp.i115008.net/zaX10r)

+   [**数据流处理与 Apache Kafka & Apache Spark 纳米学位** **(UDACITY)**](https://imp.i115008.net/zaX10r)

+   [**使用 PySpark 的 Spark 和 Python 大数据课程（UDEMY）**](https://click.linksynergy.com/deeplink?id=533LxfDBSaM&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fspark-and-python-for-big-data-with-pyspark%2F)

***还不是 Medium 会员？*** *考虑使用我的* [*推荐链接*](https://anbento4.medium.com/membership) *注册，以每月仅需$5 的价格获取 Medium 的全部内容！*

# **简介**

在数据工程和分析的动态环境中，构建可扩展和自动化的管道至关重要。

一些对 Spark 感兴趣，并且已经使用 Airflow 一段时间的人可能会想知道：

> 如何使用 Airflow 在远程集群上执行 Spark 作业？
> 
> 如何使用 AWS EMR 和 Airflow 自动化 Spark 管道？

在本教程中，我们将通过展示如何来整合这两种技术：

1.  配置并获取基本参数来自…
