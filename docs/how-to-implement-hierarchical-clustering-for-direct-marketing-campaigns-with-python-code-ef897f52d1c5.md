# 如何使用 Python 代码实现层次聚类用于直接营销活动

> 原文：[https://towardsdatascience.com/how-to-implement-hierarchical-clustering-for-direct-marketing-campaigns-with-python-code-ef897f52d1c5?source=collection_archive---------7-----------------------#2023-08-28](https://towardsdatascience.com/how-to-implement-hierarchical-clustering-for-direct-marketing-campaigns-with-python-code-ef897f52d1c5?source=collection_archive---------7-----------------------#2023-08-28)

## 了解层次聚类的方方面面，以及它如何应用于银行业的营销活动分析。

[](https://zoumanakeita.medium.com/?source=post_page-----ef897f52d1c5--------------------------------)[![Zoumana Keita](../Images/34a15c1d03687816dbdbc065f5719f80.png)](https://zoumanakeita.medium.com/?source=post_page-----ef897f52d1c5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ef897f52d1c5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ef897f52d1c5--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----ef897f52d1c5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe6ae785a30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-implement-hierarchical-clustering-for-direct-marketing-campaigns-with-python-code-ef897f52d1c5&user=Zoumana+Keita&userId=e6ae785a30d&source=post_page-e6ae785a30d----ef897f52d1c5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ef897f52d1c5--------------------------------) ·11分钟阅读·2023年8月28日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fef897f52d1c5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-implement-hierarchical-clustering-for-direct-marketing-campaigns-with-python-code-ef897f52d1c5&user=Zoumana+Keita&userId=e6ae785a30d&source=-----ef897f52d1c5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fef897f52d1c5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-implement-hierarchical-clustering-for-direct-marketing-campaigns-with-python-code-ef897f52d1c5&source=-----ef897f52d1c5---------------------bookmark_footer-----------)![](../Images/3282ab1d6eae7b1221f28b0d9665653d.png)

图片由 [Frederick Warren](https://unsplash.com/@carnations) 提供，来源于 [Unsplash](https://unsplash.com/photos/lOg_fQLHo7s)

# 动机

想象一下你是一家领先金融机构的数据科学家，你的任务是帮助团队将现有客户分类为`low`、`average`、`medium`和`platinum`，以便于贷款审批。

但这里有个问题：

> 这些客户没有附加任何历史标签，那么如何进行这些类别的创建呢？

这就是聚类可以发挥作用的地方，聚类是一种无监督的机器学习技术，用于将未标记的数据分组到相似的类别中。

存在多种聚类技术，但本教程将更侧重于`hierarchical clustering`方法。

它首先提供了`hierarchical clustering`的概述，然后带你逐步实现`Python`中使用流行的`Scipy`库的过程。

# 什么是层次聚类？
