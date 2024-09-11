# PCA/LDA/ICA：组件分析算法比较

> 原文：[https://towardsdatascience.com/pca-lda-ica-a-components-analysis-algorithms-comparison-c5762c4148ff?source=collection_archive---------1-----------------------#2023-02-19](https://towardsdatascience.com/pca-lda-ica-a-components-analysis-algorithms-comparison-c5762c4148ff?source=collection_archive---------1-----------------------#2023-02-19)

## 回顾这些著名算法的概念和差异

[](https://mocquin.medium.com/?source=post_page-----c5762c4148ff--------------------------------)[![Yoann Mocquin](../Images/b30a0f70c56972aabd2bc0a74baa90bb.png)](https://mocquin.medium.com/?source=post_page-----c5762c4148ff--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c5762c4148ff--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c5762c4148ff--------------------------------) [Yoann Mocquin](https://mocquin.medium.com/?source=post_page-----c5762c4148ff--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F173731d06320&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpca-lda-ica-a-components-analysis-algorithms-comparison-c5762c4148ff&user=Yoann+Mocquin&userId=173731d06320&source=post_page-173731d06320----c5762c4148ff---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c5762c4148ff--------------------------------) · 8分钟阅读·2023年2月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc5762c4148ff&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpca-lda-ica-a-components-analysis-algorithms-comparison-c5762c4148ff&user=Yoann+Mocquin&userId=173731d06320&source=-----c5762c4148ff---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc5762c4148ff&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpca-lda-ica-a-components-analysis-algorithms-comparison-c5762c4148ff&source=-----c5762c4148ff---------------------bookmark_footer-----------)

在深入比较这些算法之前，让我们独立地回顾一下它们。

*请注意，本文并不旨在深入解释每个算法，而是比较它们的目标和结果。*

如果你想了解更多关于PCA和ZCA之间的区别，可以查看我之前基于numpy的帖子：

[](/pca-whitening-vs-zca-whitening-a-numpy-2d-visual-518b32033edf?source=post_page-----c5762c4148ff--------------------------------) [## PCA-白化与ZCA-白化：一个numpy 2D可视化

### 数据白化的过程包括一种转换，使得转换后的数据具有单位矩阵作为…

[towardsdatascience.com](/pca-whitening-vs-zca-whitening-a-numpy-2d-visual-518b32033edf?source=post_page-----c5762c4148ff--------------------------------)

# PCA：主成分分析

+   PCA是一种无监督的线性降维技术，旨在找到一组新的正交变量，以捕捉数据中最重要的变异来源。

+   它广泛用于特征提取和数据压缩，也可以用于探索性数据分析或作为机器学习算法的预处理步骤。

+   结果组件按解释的方差量进行排序，可以用于数据的可视化和解释，以及聚类或分类任务。

# LDA：线性判别分析
