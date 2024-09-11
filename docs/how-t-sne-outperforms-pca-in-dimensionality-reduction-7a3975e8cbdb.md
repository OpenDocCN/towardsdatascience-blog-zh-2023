# 如何 t-SNE 超越 PCA 实现维度减少

> 原文：[https://towardsdatascience.com/how-t-sne-outperforms-pca-in-dimensionality-reduction-7a3975e8cbdb?source=collection_archive---------5-----------------------#2023-05-23](https://towardsdatascience.com/how-t-sne-outperforms-pca-in-dimensionality-reduction-7a3975e8cbdb?source=collection_archive---------5-----------------------#2023-05-23)

## PCA 与 t-SNE 在低维空间中可视化高维数据的对比

[](https://rukshanpramoditha.medium.com/?source=post_page-----7a3975e8cbdb--------------------------------)[![Rukshan Pramoditha](../Images/b80426aff64ff186cb915795644590b1.png)](https://rukshanpramoditha.medium.com/?source=post_page-----7a3975e8cbdb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7a3975e8cbdb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7a3975e8cbdb--------------------------------) [Rukshan Pramoditha](https://rukshanpramoditha.medium.com/?source=post_page-----7a3975e8cbdb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff90a3bb1d400&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-t-sne-outperforms-pca-in-dimensionality-reduction-7a3975e8cbdb&user=Rukshan+Pramoditha&userId=f90a3bb1d400&source=post_page-f90a3bb1d400----7a3975e8cbdb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7a3975e8cbdb--------------------------------) ·15 min 阅读·2023年5月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7a3975e8cbdb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-t-sne-outperforms-pca-in-dimensionality-reduction-7a3975e8cbdb&user=Rukshan+Pramoditha&userId=f90a3bb1d400&source=-----7a3975e8cbdb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7a3975e8cbdb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-t-sne-outperforms-pca-in-dimensionality-reduction-7a3975e8cbdb&source=-----7a3975e8cbdb---------------------bookmark_footer-----------)![](../Images/3b7e20bb0c5cf927b95d2c4314e06247.png)

照片由 [Pat Whelen](https://unsplash.com/@patwhelen?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/Rxt252RzQlY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在机器学习中，**维度减少**指的是减少数据集中的输入变量数量。输入变量的数量即为数据集的**维度**。

维度减少技术主要分为两大类：*线性* 和 *非线性（流形）*。

在线性方法下，我们讨论了[*主成分分析（PCA）*](https://medium.com/data-science-365/3-easy-steps-to-perform-dimensionality-reduction-using-principal-component-analysis-pca-79121998b991)、[*因子分析（FA）*](/factor-analysis-on-women-track-records-data-with-r-and-python-6731a73cd2e0)、[*线性判别分析（LDA）*](/lda-is-highly-effective-than-pca-for-dimensionality-reduction-in-classification-datasets-4489eade632)和[*非负矩阵分解（NMF）*](/non-negative-matrix-factorization-nmf-for-dimensionality-reduction-in-image-data-8450f4cae8fa)。

在非线性方法下，我们讨论了[*自编码器（AEs）*](/how-autoencoders-outperform-pca-in-dimensionality-reduction-1ae44c68b42f)和[*核主成分分析（Kernel PCA）*](/dimensionality-reduction-for-linearly-inseparable-data-5030f0dc0f5e)。

**t-分布随机邻居嵌入（t-SNE）**也是一种*非线性*降维方法，用于在低维空间中可视化高维数据，以便找到数据中的重要簇或群体。

> 所有降维技术都属于无监督机器学习的范畴，在这种方法中，我们可以揭示数据中的隐藏模式和重要关系，而无需标签。
> 
> 因此，降维算法处理的是未标记的数据。在训练这些算法时，…
