# 无监督学习方法系列 — 探索 K 均值聚类

> 原文：[`towardsdatascience.com/unsupervised-learning-method-series-exploring-k-means-clustering-d129fff3ab6a?source=collection_archive---------8-----------------------#2023-04-05`](https://towardsdatascience.com/unsupervised-learning-method-series-exploring-k-means-clustering-d129fff3ab6a?source=collection_archive---------8-----------------------#2023-04-05)

## 让我们探索其中一种最著名的无监督学习方法，以及它如何利用距离将相似的实例映射在一起

[](https://ivopbernardo.medium.com/?source=post_page-----d129fff3ab6a--------------------------------)![Ivo Bernardo](https://ivopbernardo.medium.com/?source=post_page-----d129fff3ab6a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d129fff3ab6a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d129fff3ab6a--------------------------------) [Ivo Bernardo](https://ivopbernardo.medium.com/?source=post_page-----d129fff3ab6a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F74eec53531c0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-method-series-exploring-k-means-clustering-d129fff3ab6a&user=Ivo+Bernardo&userId=74eec53531c0&source=post_page-74eec53531c0----d129fff3ab6a---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d129fff3ab6a--------------------------------) ·13 min read·2023 年 4 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd129fff3ab6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-method-series-exploring-k-means-clustering-d129fff3ab6a&user=Ivo+Bernardo&userId=74eec53531c0&source=-----d129fff3ab6a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd129fff3ab6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-method-series-exploring-k-means-clustering-d129fff3ab6a&source=-----d129fff3ab6a---------------------bookmark_footer-----------)![](img/d3f57dc819639ea669371626800d21bd.png)

图片由 [alexlanting](https://unsplash.com/pt-br/@alexlanting) 提供 @Unsplash.com

无监督学习是一种神秘而有趣的艺术。尽管没有明确的真实标签来预测，而且评估我们提出的解决方案可能更困难，但无监督学习方法是理解数据结构和减少其复杂性的极有趣的技术。

除了可视化和降维技术外，聚类是重要的无监督机器学习方法之一，帮助我们通过丢失一些原始数据的信号，将单个实例合并为更少的例子。在这个无监督学习系列中，我们将首先探讨`k-means`聚类，这是一种非常有趣且著名的基于距离的聚类方法。

# K-means 算法

K-means 算法通过将每个观察值映射到数据集中固定数量的(*k*)簇中来工作，基于距离进行分类。

让我们从一个示例开始，在这个示例中，我们通过`年龄`和`年收入`在二维图上绘制了客户数据：
