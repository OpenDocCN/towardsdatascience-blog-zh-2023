# UMAP 方差解释

> 原文：[`towardsdatascience.com/umap-variance-explained-b0eacb5b0801?source=collection_archive---------6-----------------------#2023-03-27`](https://towardsdatascience.com/umap-variance-explained-b0eacb5b0801?source=collection_archive---------6-----------------------#2023-03-27)

## [数学统计与生命科学中的机器学习](https://towardsdatascience.com/tagged/stats-ml-life-sciences)

## 解释 UMAP 组件的简单方法

[](https://nikolay-oskolkov.medium.com/?source=post_page-----b0eacb5b0801--------------------------------)![Nikolay Oskolkov](https://nikolay-oskolkov.medium.com/?source=post_page-----b0eacb5b0801--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b0eacb5b0801--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b0eacb5b0801--------------------------------) [尼古拉·奥斯科尔科夫](https://nikolay-oskolkov.medium.com/?source=post_page-----b0eacb5b0801--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8570b484f56c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fumap-variance-explained-b0eacb5b0801&user=Nikolay+Oskolkov&userId=8570b484f56c&source=post_page-8570b484f56c----b0eacb5b0801---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b0eacb5b0801--------------------------------) · 19 分钟阅读 · 2023 年 3 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb0eacb5b0801&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fumap-variance-explained-b0eacb5b0801&user=Nikolay+Oskolkov&userId=8570b484f56c&source=-----b0eacb5b0801---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb0eacb5b0801&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fumap-variance-explained-b0eacb5b0801&source=-----b0eacb5b0801---------------------bookmark_footer-----------)![](img/4a5c3be44e4808f1ca5e658c9ba47eaf.png)

UMAP 计算基于 [MNIST 手写数字黑白图像](https://en.wikipedia.org/wiki/MNIST_database)。图片由作者提供

这是我专栏中第**二十五**篇文章，专栏名为[**数学统计与生命科学中的机器学习**](https://towardsdatascience.com/tagged/stats-ml-life-sciences)，在这里我用通俗的语言讨论计算生物学和生命科学中的分析方法。[**UMAP**](https://arxiv.org/abs/1802.03426)是一种[**降维**](https://en.wikipedia.org/wiki/Dimensionality_reduction)技术，与[**tSNE**](https://en.wikipedia.org/wiki/T-distributed_stochastic_neighbor_embedding)一起，变得越来越受欢迎，*事实上*成为了[**单细胞基因组学**](https://en.wikipedia.org/wiki/Single-cell_sequencing)数据分析的标准工具，而传统方法如[**PCA**](https://en.wikipedia.org/wiki/Principal_component_analysis) **存在局限性**。然而，与 PCA 相比，UMAP 和 tSNE 的一个缺点是其**不可解释**的组件，这些组件与原始数据中的变化之间的关联并不直观。在本文中，我建议一种简单的方法来**估计由主要 UMAP 和 tSNE 组件解释的变化量**。使用经典的[MNIST](https://en.wikipedia.org/wiki/MNIST_database)手写数字黑白图像数据集作为基准，我展示了**主要 UMAP 和 tSNE 组件在解释总数据变化方面不如 PCA 组件**，但令人惊讶的是，它们与数据点的标签的联系更好，即它们解释了**数据中更多的生物学变化而非总变化**。

# 准备 MNIST 数据以进行分析
