# 用最简单的方法将 Google Pub/Sub 数据导入 BigQuery

> 原文：[https://towardsdatascience.com/google-pub-sub-to-bigquery-the-simple-way-de116234fb87?source=collection_archive---------9-----------------------#2023-09-21](https://towardsdatascience.com/google-pub-sub-to-bigquery-the-simple-way-de116234fb87?source=collection_archive---------9-----------------------#2023-09-21)

## 实现 BigQuery 订阅的操作指南，用于简化消息和流数据的摄取

[](https://jim-barlow.medium.com/?source=post_page-----de116234fb87--------------------------------)[![Jim Barlow](../Images/1494580717cb92defb17328e4bae1b13.png)](https://jim-barlow.medium.com/?source=post_page-----de116234fb87--------------------------------)[](https://towardsdatascience.com/?source=post_page-----de116234fb87--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----de116234fb87--------------------------------) [Jim Barlow](https://jim-barlow.medium.com/?source=post_page-----de116234fb87--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4cdc1c30aebe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogle-pub-sub-to-bigquery-the-simple-way-de116234fb87&user=Jim+Barlow&userId=4cdc1c30aebe&source=post_page-4cdc1c30aebe----de116234fb87---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----de116234fb87--------------------------------) ·8分钟阅读·2023年9月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fde116234fb87&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogle-pub-sub-to-bigquery-the-simple-way-de116234fb87&user=Jim+Barlow&userId=4cdc1c30aebe&source=-----de116234fb87---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fde116234fb87&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogle-pub-sub-to-bigquery-the-simple-way-de116234fb87&source=-----de116234fb87---------------------bookmark_footer-----------)![](../Images/b31e6bb2ff00e2b71bba4bbff6d010fc.png)

Google 最新的行星级数据仓库基于订阅的流数据摄取解决方案：BigSub。在这种情况下，Pub 从未达到正式发布，因此你需要去其他地方获取。照片由[Thomas Haas](https://unsplash.com/@thomashaas?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 动机

在过去，我遇到过很多希望将 Pub/Sub 消息导入 BigQuery 表的情况，但从未找到特别简单的方法来实现这一点。

你可以设置一个[数据流管道](https://cloud.google.com/dataflow/docs/tutorials/dataflow-stream-to-bigquery)，但这需要额外的基础设施来理解、配置、管理和调试。而且Dataflow（这是一个托管的Apache Beam服务）设计用于高吞吐量的流式处理，因此对于一个简单的消息日志或监控系统来说，总是感觉过于复杂。

而且是Java。不过Python 😀！还有Java… 😫！

```py
public static string args void main... public static string args void main... public static string args void main... public static string args void main... public static string args void main... arrrrrrrrrrrrgh
```

*抱歉，我仍然会回想起上个世纪我第一次尝试学习编程时用Java的经历。请不要尝试使用那段代码片段……离开那段代码片段吧。*

然后我偶然发现了[这个](https://medium.com/google-cloud/streaming-from-google-cloud-pub-sub-to-bigquery-without-the-middlemen-327ef24f4d15)，尽管它承诺了简便性，但似乎比之前的方法还要复杂……
