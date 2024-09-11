# 线性代数：矩阵运算及其属性，使用 Python

> 原文：[https://towardsdatascience.com/linear-algebra-matrix-operations-and-their-properties-with-python-a0885a159be1?source=collection_archive---------18-----------------------#2023-01-16](https://towardsdatascience.com/linear-algebra-matrix-operations-and-their-properties-with-python-a0885a159be1?source=collection_archive---------18-----------------------#2023-01-16)

## 第二部分：解释线性代数的基础：矩阵运算及其属性

[](https://chaodeyu.medium.com/?source=post_page-----a0885a159be1--------------------------------)[![Chao De-Yu](../Images/8d6805b4797dcc71fa722bbb3d06a91b.png)](https://chaodeyu.medium.com/?source=post_page-----a0885a159be1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a0885a159be1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a0885a159be1--------------------------------) [Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----a0885a159be1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5b7be08f8f4c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-algebra-matrix-operations-and-their-properties-with-python-a0885a159be1&user=Chao+De-Yu&userId=5b7be08f8f4c&source=post_page-5b7be08f8f4c----a0885a159be1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0885a159be1--------------------------------) · 6 分钟阅读 · 2023年1月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa0885a159be1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-algebra-matrix-operations-and-their-properties-with-python-a0885a159be1&user=Chao+De-Yu&userId=5b7be08f8f4c&source=-----a0885a159be1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa0885a159be1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-algebra-matrix-operations-and-their-properties-with-python-a0885a159be1&source=-----a0885a159be1---------------------bookmark_footer-----------)![](../Images/34dc9363b4cc87d7dca834373cc12464.png)

图片由 [Pietro De Grandi](https://unsplash.com/@peter_mc_greats) 提供，来源于 [Unsplash](https://unsplash.com)

[上一篇文章](/linear-algebra-systems-of-linear-equations-and-matrices-with-python-d3e0fcb29e85) 介绍了线性方程和线性系统是什么以及如何将线性系统重写为矩阵形式。因此，了解矩阵上的不同运算及其对应的属性是很重要的。

# 矩阵与向量

矩阵是一个由 m 行 n 列数字组成的矩形数组，其中 m > 1 且 n > 1。 [1]

向量是一个具有单行（行向量）或单列（列向量）的矩阵。[2]

![](../Images/ff13a7058c0c33a3646c2c7b1591c6c1.png)

[左] 矩阵的一般形式。[中] 列向量。[右] 行向量。（图片作者）

## 一些特殊矩阵

+   方阵：一个矩阵，其中 m = n

+   对角矩阵 I：一个方阵，其所有非对角线上的元素都为零

+   单位矩阵：对角矩阵 I，其中 iₓᵧ = 1

+   带状矩阵：大多数元素为零，非零元素分布在主对角线周围的带状区域

+   下三角矩阵：L，lₓᵧ = 0 如果 x < y…
