# 奇异值分解与特征值分解在降维中的比较

> 原文：[https://towardsdatascience.com/singular-value-decomposition-vs-eigendecomposition-for-dimensionality-reduction-fc0d9ac24a8e?source=collection_archive---------9-----------------------#2023-03-20](https://towardsdatascience.com/singular-value-decomposition-vs-eigendecomposition-for-dimensionality-reduction-fc0d9ac24a8e?source=collection_archive---------9-----------------------#2023-03-20)

## 使用这两种方法进行PCA，并比较结果

[](https://rukshanpramoditha.medium.com/?source=post_page-----fc0d9ac24a8e--------------------------------)[![Rukshan Pramoditha](../Images/b80426aff64ff186cb915795644590b1.png)](https://rukshanpramoditha.medium.com/?source=post_page-----fc0d9ac24a8e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fc0d9ac24a8e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fc0d9ac24a8e--------------------------------) [Rukshan Pramoditha](https://rukshanpramoditha.medium.com/?source=post_page-----fc0d9ac24a8e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff90a3bb1d400&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsingular-value-decomposition-vs-eigendecomposition-for-dimensionality-reduction-fc0d9ac24a8e&user=Rukshan+Pramoditha&userId=f90a3bb1d400&source=post_page-f90a3bb1d400----fc0d9ac24a8e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fc0d9ac24a8e--------------------------------) · 8 min read · 2023年3月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffc0d9ac24a8e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsingular-value-decomposition-vs-eigendecomposition-for-dimensionality-reduction-fc0d9ac24a8e&user=Rukshan+Pramoditha&userId=f90a3bb1d400&source=-----fc0d9ac24a8e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffc0d9ac24a8e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsingular-value-decomposition-vs-eigendecomposition-for-dimensionality-reduction-fc0d9ac24a8e&source=-----fc0d9ac24a8e---------------------bookmark_footer-----------)![](../Images/249648322ceade3a63b6972cb91564d2.png)

图片来源：[Viktor Peschel](https://pixabay.com/users/1a-photoshop-6724285/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2886285) 来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2886285)

奇异值分解（SVD）和特征值分解（ED）都是源自线性代数的矩阵分解方法。

在机器学习（ML）领域，这两者都可以用作数据降维方法（即用于降维）。

我们之前已经详细讨论了[eigendecomposition](https://medium.com/data-science-365/eigendecomposition-of-a-covariance-matrix-with-numpy-c953334c965d)。今天，我们将更多地强调讨论SVD。

主成分分析（PCA）可以使用这两种方法进行。PCA是ML中最流行的线性降维技术。SVD被认为是PCA背后的基础数学。流行的ML库Scikit-learn也在其`PCA()`函数中使用SVD来执行PCA。因此，SVD在降维方面比eigendecomposition更为流行。

NumPy提供了高层次且易于使用的函数来执行SVD和eigendecomposition。

```py
**Topics included:
----------------**
01\. What is singular value decomposition?
02\. SVD equation and its terms
03\. Singular value decomposition in NumPy - svd() function
04\. What is eigendecomposition?
05\. Eigendecomposition equation and its terms
06\. Eigendecomposition in…
```
