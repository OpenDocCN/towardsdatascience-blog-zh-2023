# 混合模型、潜在变量和期望最大化算法

> 原文：[https://towardsdatascience.com/mixture-models-latent-variables-and-the-expectation-maximization-algorithm-e5b18e15faa?source=collection_archive---------1-----------------------#2023-03-19](https://towardsdatascience.com/mixture-models-latent-variables-and-the-expectation-maximization-algorithm-e5b18e15faa?source=collection_archive---------1-----------------------#2023-03-19)

[](https://reoneo.medium.com/?source=post_page-----e5b18e15faa--------------------------------)[![Reo Neo](../Images/a3c192dafc1222b06b2e7fcf4d35cb27.png)](https://reoneo.medium.com/?source=post_page-----e5b18e15faa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e5b18e15faa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e5b18e15faa--------------------------------) [Reo Neo](https://reoneo.medium.com/?source=post_page-----e5b18e15faa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9fb220b09dcf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmixture-models-latent-variables-and-the-expectation-maximization-algorithm-e5b18e15faa&user=Reo+Neo&userId=9fb220b09dcf&source=post_page-9fb220b09dcf----e5b18e15faa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e5b18e15faa--------------------------------) · 8分钟阅读 · 2023年3月19日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe5b18e15faa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmixture-models-latent-variables-and-the-expectation-maximization-algorithm-e5b18e15faa&source=-----e5b18e15faa---------------------bookmark_footer-----------)![](../Images/eae2fea5e6268b616a5acfcaca0178b9.png)

照片由 [Sung Shin](https://unsplash.com/ko/@ironstagram?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

无监督学习一直令我着迷。这是一种无需人工标注的方式来了解数据，并允许识别数据集中的模式。在各种无监督学习技术中，最简单的就是聚类。本质上，聚类算法旨在找到彼此相似的数据点。通过将数据点进行聚类，我们可以获得关于数据集的宝贵洞见以及各个簇所代表的内容。

本文旨在深入探讨高斯混合模型聚类算法，了解它如何对数据进行建模，更重要的是如何利用期望最大化（Expectation-Maximization）在新数据集上拟合模型。

# **什么是混合模型？**

本质上，混合模型（或混合分布）是将**多个概率分布组合成一个单一的分布**。

要将这些分布组合在一起，我们需要为每个组件分布分配一个**权重**，使得分布下的总概率和为 1。一个简单的例子是由 2 个高斯分布组成的混合分布。我们可以有 2 个具有不同均值和方差的分布，并通过不同的权重将这 2 个分布组合在一起。
