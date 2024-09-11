# 如何使用 Python 执行 KMeans 聚类

> 原文：[https://towardsdatascience.com/how-to-perform-kmeans-clustering-using-python-7cc296cec092?source=collection_archive---------0-----------------------#2023-01-17](https://towardsdatascience.com/how-to-perform-kmeans-clustering-using-python-7cc296cec092?source=collection_archive---------0-----------------------#2023-01-17)

## 使用 Python 对 KMeans 聚类及其实现进行全面概述

[](https://zoumanakeita.medium.com/?source=post_page-----7cc296cec092--------------------------------)[![Zoumana Keita](../Images/34a15c1d03687816dbdbc065f5719f80.png)](https://zoumanakeita.medium.com/?source=post_page-----7cc296cec092--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7cc296cec092--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7cc296cec092--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----7cc296cec092--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe6ae785a30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-perform-kmeans-clustering-using-python-7cc296cec092&user=Zoumana+Keita&userId=e6ae785a30d&source=post_page-e6ae785a30d----7cc296cec092---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7cc296cec092--------------------------------) ·7分钟阅读·2023年1月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7cc296cec092&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-perform-kmeans-clustering-using-python-7cc296cec092&user=Zoumana+Keita&userId=e6ae785a30d&source=-----7cc296cec092---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7cc296cec092&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-perform-kmeans-clustering-using-python-7cc296cec092&source=-----7cc296cec092---------------------bookmark_footer-----------)

# 介绍

想象一下，你是一个在零售公司工作的数据科学家，你的老板要求你将客户根据消费行为划分为以下组别：低消费、普通消费、中等消费或铂金客户，以便进行有针对性的营销和产品推荐。

*知道那些客户没有任何历史标签的情况下，如何对他们进行分类呢？*

这时聚类技术可以提供帮助。它是一种无监督的机器学习技术，用于将未标记的数据分组为类似的类别或簇。

本文将主要关注 K-means 聚类方法，这是无监督机器学习中的众多技术之一。它将首先提供 K-means 聚类的概述，然后通过使用流行的`Scikit-learn`库逐步引导你实现。

# 什么是 K-Means 聚类？

K-means 聚类的思想是将数据集划分为指定的...
