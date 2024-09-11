# 《维度诅咒的解密》

> 原文：[`towardsdatascience.com/the-curse-of-dimensionality-demystified-2fc9b0bb1126?source=collection_archive---------9-----------------------#2023-10-06`](https://towardsdatascience.com/the-curse-of-dimensionality-demystified-2fc9b0bb1126?source=collection_archive---------9-----------------------#2023-10-06)

## 理解维度诅咒背后的数学直觉

[](https://reza-bagheri79.medium.com/?source=post_page-----2fc9b0bb1126--------------------------------)![Reza Bagheri](https://reza-bagheri79.medium.com/?source=post_page-----2fc9b0bb1126--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2fc9b0bb1126--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2fc9b0bb1126--------------------------------) [Reza Bagheri](https://reza-bagheri79.medium.com/?source=post_page-----2fc9b0bb1126--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fda2d000eaa4d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-curse-of-dimensionality-demystified-2fc9b0bb1126&user=Reza+Bagheri&userId=da2d000eaa4d&source=post_page-da2d000eaa4d----2fc9b0bb1126---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2fc9b0bb1126--------------------------------) ·23 分钟阅读·2023 年 10 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2fc9b0bb1126&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-curse-of-dimensionality-demystified-2fc9b0bb1126&user=Reza+Bagheri&userId=da2d000eaa4d&source=-----2fc9b0bb1126---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2fc9b0bb1126&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-curse-of-dimensionality-demystified-2fc9b0bb1126&source=-----2fc9b0bb1126---------------------bookmark_footer-----------)![](img/61dacdeae7a371398548e2bad19472fe.png)

图片来源: [`pixabay.com/illustrations/ancient-art-background-cosmos-dark-764930/`](https://pixabay.com/illustrations/ancient-art-background-cosmos-dark-764930/)

*维度诅咒* 指的是分析高维数据时出现的问题。数据集的*维度* 或 *维度* 指的是数据集中线性独立特征的数量，因此*高维* 数据集是特征数量较多的数据集。这个术语最早由贝尔曼于 1961 年提出，他观察到，为了以某种精度估计任意函数所需的样本数量会随着函数参数数量的增加而指数增长。

在这篇文章中，我们详细探讨了分析高维数据集时出现的数学问题。尽管这些问题可能看起来违反直觉，但可以直观地解释它们。我们不仅进行纯理论讨论，还使用 Python 创建和分析高维数据集，并观察维度诅咒如何在实践中显现。*在本文中，所有图片，除非另有说明，均为作者所作。*

**数据集的维度**

如前所述，数据集的维度被定义为其具有的线性独立特征的数量。线性独立特征不能被写作其他特征的线性组合……
