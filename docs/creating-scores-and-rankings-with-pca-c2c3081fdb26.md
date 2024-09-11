# 使用 PCA 创建评分和排名

> 原文：[`towardsdatascience.com/creating-scores-and-rankings-with-pca-c2c3081fdb26?source=collection_archive---------6-----------------------#2023-04-10`](https://towardsdatascience.com/creating-scores-and-rankings-with-pca-c2c3081fdb26?source=collection_archive---------6-----------------------#2023-04-10)

## 使用 R 语言为观察数据基于多个变量创建评分

[](https://gustavorsantos.medium.com/?source=post_page-----c2c3081fdb26--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----c2c3081fdb26--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c2c3081fdb26--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c2c3081fdb26--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----c2c3081fdb26--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-scores-and-rankings-with-pca-c2c3081fdb26&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----c2c3081fdb26---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c2c3081fdb26--------------------------------) ·9 分钟阅读·2023 年 4 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc2c3081fdb26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-scores-and-rankings-with-pca-c2c3081fdb26&user=Gustavo+Santos&userId=4429d99b1245&source=-----c2c3081fdb26---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc2c3081fdb26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-scores-and-rankings-with-pca-c2c3081fdb26&source=-----c2c3081fdb26---------------------bookmark_footer-----------)![](img/ad848fe08cc9a8bc744f2e58a7a97f9b.png)

由 [Joshua Golde](https://unsplash.com/@joshgmit?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 摄影，照片来自 [Unsplash](https://unsplash.com/photos/qIu77BsFdds?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

我对主成分分析**（PCA）**的研究越多，就越喜欢这个工具。我已经写过关于这个话题的其他文章，但我不断深入了解这门美妙数学的“内部机制”，当然，我会与您分享这些知识。

PCA 是一组基于数据协方差和相关性的数学转换。因此，它基本上会查看数据点并找到变异性最大的地方。一旦完成，数据就会在那个方向上进行投影。新的数据被投影到一个新的轴上，称为**主成分**。

> 数据被投影到一个新的轴上，以解释尽可能多的变异性。

投影本身就是一种转换。而且新数据具有许多属性，可以帮助我们数据科学家更好地分析数据。例如，我们可以进行因子分析，将相似的变量组合成一个因子，从而减少数据的维度。

另一个有趣的属性是通过观察的相似性创建排名的可能性，就像我们…
