# 这就是我在使用Apache Airflow 6年中学到的东西

> 原文：[https://towardsdatascience.com/here-is-what-i-learned-using-apache-airflow-over-6-years-15d88b9922d9?source=collection_archive---------7-----------------------#2023-01-09](https://towardsdatascience.com/here-is-what-i-learned-using-apache-airflow-over-6-years-15d88b9922d9?source=collection_archive---------7-----------------------#2023-01-09)

## 一段空中飞行Apache Airflow从实验到无压力的生产之旅

[](https://chengzhizhao.medium.com/?source=post_page-----15d88b9922d9--------------------------------)[![Chengzhi Zhao](../Images/186bba91822dbcc0f926426e56faf543.png)](https://chengzhizhao.medium.com/?source=post_page-----15d88b9922d9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----15d88b9922d9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----15d88b9922d9--------------------------------) [Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----15d88b9922d9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff956c63a9571&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhere-is-what-i-learned-using-apache-airflow-over-6-years-15d88b9922d9&user=Chengzhi+Zhao&userId=f956c63a9571&source=post_page-f956c63a9571----15d88b9922d9---------------------post_header-----------) 发表在[数据科学前沿](https://towardsdatascience.com/?source=post_page-----15d88b9922d9--------------------------------) ·8 min阅读·2023年1月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F15d88b9922d9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhere-is-what-i-learned-using-apache-airflow-over-6-years-15d88b9922d9&user=Chengzhi+Zhao&userId=f956c63a9571&source=-----15d88b9922d9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F15d88b9922d9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhere-is-what-i-learned-using-apache-airflow-over-6-years-15d88b9922d9&source=-----15d88b9922d9---------------------bookmark_footer-----------)![](../Images/2e84fe2488de04a2676a5ff7e1454684.png)

照片由[Karsten Würth](https://unsplash.com/@karsten_wuerth?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/photos/UbGYPMbMYP8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上发布

Apache Airflow 毋庸置疑是多年来最受欢迎的数据工程开源项目。它在合适的时间获得了关注，伴随了 [数据工程师的崛起](https://medium.com/free-code-camp/the-rise-of-the-data-engineer-91be18f1e603)，并且将代码作为一等公民的核心概念而不是拖放式的数据管道（即 ETL）是一个里程碑。Apache Airflow 在 2016 年 3 月成为 Apache Incubator 项目，并在 2019 年 1 月成为顶级项目。我自 2017 年以来作为用户使用 Apache Airflow。在这个过程中，我也为 Apache Airflow 做出了贡献。今天，我想分享我与 Airflow 的历程以及我在 6 年中学到的东西。

# **什么是 Airflow**

> Airflow 是一个由社区创建的平台，用于以编程方式编写、调度和监控工作流。— [Airflow 官方文档](https://airflow.apache.org/)

Apache Airflow 的开发者是 [Maxime Beauchemin](https://medium.com/@maximebeauchemin)，他曾在 Airbnb 和 Facebook 工作。他在 Airbnb 开发了 Airflow（*你可以从项目名称看出来*）。然而，激发他灵感的核心想法是 Facebook 内部使用的一个工具。

Apache Airflow 的主要用户是数据工程师或需要调度工作流的工程师，主要是 ETL（提取、转换…）。
