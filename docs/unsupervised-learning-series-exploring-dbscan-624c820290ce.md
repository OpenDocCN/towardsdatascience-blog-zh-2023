# 无监督学习系列 — 探索 DBScan

> 原文：[https://towardsdatascience.com/unsupervised-learning-series-exploring-dbscan-624c820290ce?source=collection_archive---------10-----------------------#2023-11-21](https://towardsdatascience.com/unsupervised-learning-series-exploring-dbscan-624c820290ce?source=collection_archive---------10-----------------------#2023-11-21)

## 在使用 Python 的 sklearn 时，了解著名的基于密度的聚类算法的理论

[](https://ivopbernardo.medium.com/?source=post_page-----624c820290ce--------------------------------)[![Ivo Bernardo](../Images/39887b6f3e63a67c0545e87962ad5df0.png)](https://ivopbernardo.medium.com/?source=post_page-----624c820290ce--------------------------------)[](https://towardsdatascience.com/?source=post_page-----624c820290ce--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----624c820290ce--------------------------------) [Ivo Bernardo](https://ivopbernardo.medium.com/?source=post_page-----624c820290ce--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F74eec53531c0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-series-exploring-dbscan-624c820290ce&user=Ivo+Bernardo&userId=74eec53531c0&source=post_page-74eec53531c0----624c820290ce---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----624c820290ce--------------------------------) · 11 min 阅读 · 2023 年 11 月 21 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F624c820290ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-series-exploring-dbscan-624c820290ce&user=Ivo+Bernardo&userId=74eec53531c0&source=-----624c820290ce---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F624c820290ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-series-exploring-dbscan-624c820290ce&source=-----624c820290ce---------------------bookmark_footer-----------)![](../Images/74b7d631749ea7e4f95f5041fd619497.png)

图片来源于 [Kier in Sight Archives](https://unsplash.com/pt-br/@kierinsightarchives) @Unsplash.com

聚类算法是数据科学领域最广泛使用的解决方案之一，最受欢迎的算法被分为基于距离的方法和基于密度的方法。尽管常被忽视，基于密度的聚类方法是对普遍存在的[*k-means*](/unsupervised-learning-method-series-exploring-k-means-clustering-d129fff3ab6a?sk=0618fadb196a71ce4b9f378f9ee83107)和[*层次*](/unsupervised-learning-series-exploring-hierarchical-clustering-15d992467aa8?sk=87714b0d7aa252330dd7a8d3b1c620f4)方法的有趣替代方案。

一些著名的基于密度的聚类技术包括*DBScan*（基于密度的空间聚类应用于噪声）或Mean-Shift，这两种算法使用数据点的质心来将观察值聚集在一起。

在这篇博客文章中，我们将探讨DBScan，这是一种聚类算法，当你的数据包含以下一些特征时特别有用：

+   聚类具有不规则的形状。例如，非球形的形状。

+   与其他方法相比，DBScan不对数据的潜在分布做任何假设。

+   你的数据集包含一些不相关的离群值，这些离群值不应影响簇的质心的映射。
