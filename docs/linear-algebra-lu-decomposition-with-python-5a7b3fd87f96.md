# 线性代数：LU分解法与Python

> 原文：[https://towardsdatascience.com/linear-algebra-lu-decomposition-with-python-5a7b3fd87f96?source=collection_archive---------16-----------------------#2023-01-31](https://towardsdatascience.com/linear-algebra-lu-decomposition-with-python-5a7b3fd87f96?source=collection_archive---------16-----------------------#2023-01-31)

## 第4部分：使用LU分解法解决线性系统的全面逐步指南

[](https://chaodeyu.medium.com/?source=post_page-----5a7b3fd87f96--------------------------------)[![Chao De-Yu](../Images/8d6805b4797dcc71fa722bbb3d06a91b.png)](https://chaodeyu.medium.com/?source=post_page-----5a7b3fd87f96--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5a7b3fd87f96--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5a7b3fd87f96--------------------------------) [Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----5a7b3fd87f96--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5b7be08f8f4c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-algebra-lu-decomposition-with-python-5a7b3fd87f96&user=Chao+De-Yu&userId=5b7be08f8f4c&source=post_page-5b7be08f8f4c----5a7b3fd87f96---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----5a7b3fd87f96--------------------------------) · 4分钟阅读 · 2023年1月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5a7b3fd87f96&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-algebra-lu-decomposition-with-python-5a7b3fd87f96&user=Chao+De-Yu&userId=5b7be08f8f4c&source=-----5a7b3fd87f96---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5a7b3fd87f96&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-algebra-lu-decomposition-with-python-5a7b3fd87f96&source=-----5a7b3fd87f96---------------------bookmark_footer-----------)![](../Images/3c437673405fb495da035543b1d149d3.png)

图片由[Andry Roby](https://unsplash.com/@andryroby)提供，来源于[Unsplash](https://unsplash.com)

本系列的[第一篇文章](https://medium.com/towards-data-science/linear-algebra-systems-of-linear-equations-and-matrices-with-python-d3e0fcb29e85)介绍了如何使用高斯消元法解决线性系统，而[上一篇文章](https://medium.com/towards-data-science/linear-algebra-finding-inverse-matrix-with-python-18dd988f4df)则解释了如何求逆矩阵以及如何利用逆矩阵解决线性系统。本文将介绍另一种通过LU分解法解决线性系统的方法。

# LU分解

**LU**分解是一种将矩阵分解为一个下三角矩阵和一个上三角矩阵的乘积的方法。它可以看作是高斯消元的矩阵形式，并且是一种更好的实现高斯消元的方法，特别适用于重复出现相同左边的方程问题，即解决方程Ax = b，其中A相同但b的值不同。[1]

![](../Images/ac830f39aa616a1f196c982d4a143074.png)

A = LU（图片来源：作者）

矩阵A可以使用初等矩阵分解为下三角矩阵和上三角矩阵的乘积。你可以参考[上一篇文章](https://medium.com/towards-data-science/linear-algebra-finding-inverse-matrix-with-python-18dd988f4df)了解关于三种不同初等矩阵的方法…
