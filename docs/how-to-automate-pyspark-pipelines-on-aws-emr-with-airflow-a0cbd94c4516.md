# 如何使用 Airflow 在 AWS EMR 上自动化 PySpark 管道

> 原文：[https://towardsdatascience.com/how-to-automate-pyspark-pipelines-on-aws-emr-with-airflow-a0cbd94c4516?source=collection_archive---------7-----------------------#2023-08-23](https://towardsdatascience.com/how-to-automate-pyspark-pipelines-on-aws-emr-with-airflow-a0cbd94c4516?source=collection_archive---------7-----------------------#2023-08-23)

## 优化大数据工作流的编排。

[](https://anbento4.medium.com/?source=post_page-----a0cbd94c4516--------------------------------)[![Antonello Benedetto](../Images/bf802bb46dce03dd3bd4e17c1dffe5b7.png)](https://anbento4.medium.com/?source=post_page-----a0cbd94c4516--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a0cbd94c4516--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a0cbd94c4516--------------------------------) [Antonello Benedetto](https://anbento4.medium.com/?source=post_page-----a0cbd94c4516--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2d93b58a0f8a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-automate-pyspark-pipelines-on-aws-emr-with-airflow-a0cbd94c4516&user=Antonello+Benedetto&userId=2d93b58a0f8a&source=post_page-2d93b58a0f8a----a0cbd94c4516---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0cbd94c4516--------------------------------) ·8分钟阅读·2023年8月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa0cbd94c4516&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-automate-pyspark-pipelines-on-aws-emr-with-airflow-a0cbd94c4516&user=Antonello+Benedetto&userId=2d93b58a0f8a&source=-----a0cbd94c4516---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa0cbd94c4516&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-automate-pyspark-pipelines-on-aws-emr-with-airflow-a0cbd94c4516&source=-----a0cbd94c4516---------------------bookmark_footer-----------)![](../Images/8d92446cdf5a73adbbd00d940c246dee.png)

图片来源 [Tom Fisk](https://www.pexels.com/photo/trees-near-ocean-2246950/) 于 [Pexels](https://www.pexels.com/@tomfisk/)

# 按需课程 | 推荐

*一些读者联系我，询问是否有按需课程可以帮助你* ***成为*** *一名优秀的* ***数据工程师****。以下是我推荐的3个优秀资源：

+   [**数据工程纳米学位 (UDACITY)**](https://imp.i115008.net/zaX10r)

+   [**数据流处理与 Apache Kafka & Apache Spark 纳米学位** **(UDACITY)**](https://imp.i115008.net/zaX10r)

+   [**使用PySpark的Spark和Python大数据课程（UDEMY）**](https://click.linksynergy.com/deeplink?id=533LxfDBSaM&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fspark-and-python-for-big-data-with-pyspark%2F)

***还不是Medium会员？*** *考虑使用我的* [*推荐链接*](https://anbento4.medium.com/membership) *注册，以每月仅需$5的价格获取Medium的全部内容！*

# **简介**

在数据工程和分析的动态环境中，构建可扩展和自动化的管道至关重要。

一些对Spark感兴趣，并且已经使用Airflow一段时间的人可能会想知道：

> 如何使用Airflow在远程集群上执行Spark作业？
> 
> 如何使用AWS EMR和Airflow自动化Spark管道？

在本教程中，我们将通过展示如何来整合这两种技术：

1.  配置并获取基本参数来自…
