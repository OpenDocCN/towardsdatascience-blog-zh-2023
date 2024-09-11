# 无监督学习系列：探索层次聚类

> 原文：[https://towardsdatascience.com/unsupervised-learning-series-exploring-hierarchical-clustering-15d992467aa8?source=collection_archive---------10-----------------------#2023-06-20](https://towardsdatascience.com/unsupervised-learning-series-exploring-hierarchical-clustering-15d992467aa8?source=collection_archive---------10-----------------------#2023-06-20)

## 让我们探讨层次聚类的工作原理以及它如何基于成对距离构建聚类。

[](https://ivopbernardo.medium.com/?source=post_page-----15d992467aa8--------------------------------)[![伊沃·贝尔纳多](../Images/39887b6f3e63a67c0545e87962ad5df0.png)](https://ivopbernardo.medium.com/?source=post_page-----15d992467aa8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----15d992467aa8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----15d992467aa8--------------------------------) [伊沃·贝尔纳多](https://ivopbernardo.medium.com/?source=post_page-----15d992467aa8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F74eec53531c0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-series-exploring-hierarchical-clustering-15d992467aa8&user=Ivo+Bernardo&userId=74eec53531c0&source=post_page-74eec53531c0----15d992467aa8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----15d992467aa8--------------------------------) ·11 分钟阅读·2023 年 6 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F15d992467aa8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-series-exploring-hierarchical-clustering-15d992467aa8&user=Ivo+Bernardo&userId=74eec53531c0&source=-----15d992467aa8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F15d992467aa8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-series-exploring-hierarchical-clustering-15d992467aa8&source=-----15d992467aa8---------------------bookmark_footer-----------)![](../Images/1ad73696743c551e19d34fe0ff09b9b0.png)

Nathan Anderson @unsplash.com 拍摄的照片

在我上一篇无监督学习系列的文章中，我们探讨了其中一个最著名的聚类方法，[K均值聚类](https://medium.com/towards-data-science/unsupervised-learning-method-series-exploring-k-means-clustering-d129fff3ab6a)。在本文中，我们将讨论另一种重要的聚类技术——**层次聚类**！

这种方法也基于距离（欧几里得距离、曼哈顿距离等），并使用数据的层次表示来结合数据点。与*k-means*不同，它不包含任何关于质心数量的超参数（例如*k*），这些超参数是数据科学家可以配置的。

通常，层次聚类可以分为两类：聚合型聚类和分裂型聚类。在聚合型聚类中，数据点被视为单个单位，并根据距离聚合到附近的数据点。在分裂型聚类中，我们将所有数据点视为一个单一的簇，并根据某些标准开始将其划分。由于聚合型版本是最著名和广泛使用的（[sklearn的内置实现遵循此协议](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.AgglomerativeClustering.html)），这就是我们在这篇文章中要探讨的层次类型。
