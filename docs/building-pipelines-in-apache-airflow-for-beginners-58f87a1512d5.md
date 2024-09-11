# 在 Apache Airflow 中构建管道 - 初学者指南

> 原文：[https://towardsdatascience.com/building-pipelines-in-apache-airflow-for-beginners-58f87a1512d5?source=collection_archive---------3-----------------------#2023-03-15](https://towardsdatascience.com/building-pipelines-in-apache-airflow-for-beginners-58f87a1512d5?source=collection_archive---------3-----------------------#2023-03-15)

## 一个简单快速的演示，展示如何在 Airflow 上运行 DAGs

[](https://medium.com/@aashishnair?source=post_page-----58f87a1512d5--------------------------------)[![Aashish Nair](../Images/23f4b3839e464419332b690a4098d824.png)](https://medium.com/@aashishnair?source=post_page-----58f87a1512d5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----58f87a1512d5--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----58f87a1512d5--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----58f87a1512d5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-pipelines-in-apache-airflow-for-beginners-58f87a1512d5&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----58f87a1512d5---------------------post_header-----------) 发表在[数据科学前沿](https://towardsdatascience.com/?source=post_page-----58f87a1512d5--------------------------------) ·9分钟阅读·2023年3月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F58f87a1512d5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-pipelines-in-apache-airflow-for-beginners-58f87a1512d5&user=Aashish+Nair&userId=3087ba81e065&source=-----58f87a1512d5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F58f87a1512d5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-pipelines-in-apache-airflow-for-beginners-58f87a1512d5&source=-----58f87a1512d5---------------------bookmark_footer-----------)![](../Images/4fbe919cb98598fa62ac516da138073a.png)

图片由[凯利·西克马](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Apache Airflow 在数据科学和数据工程领域非常受欢迎。它拥有许多功能，使用户能够以编程方式创建、管理和监控复杂的工作流。

然而，该平台的多种功能可能会无意中成为初学者的障碍。新用户在探索 Apache Airflow 的文档和教程时，可能会被新术语、工具和概念淹没。

为了提供一个更易于理解的该平台的介绍，我们提供一个基础版本的Apache Airflow演示，包括编码和运行Airflow管道。

## 术语

在熟悉以下Airflow术语后，跟随演示将会更加轻松。

1.  **有向无环图（DAG）：** 有向无环图是Airflow用于表示工作流的图。DAG由表示任务的节点和表示任务之间关系的箭头组成。

![](../Images/4ced24b547d325303b4af3419555b16a.png)

示例DAG（作者创建）
