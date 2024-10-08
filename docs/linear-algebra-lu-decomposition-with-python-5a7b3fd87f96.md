# 线性代数：LU 分解与 Python

> 原文：[`towardsdatascience.com/linear-algebra-lu-decomposition-with-python-5a7b3fd87f96`](https://towardsdatascience.com/linear-algebra-lu-decomposition-with-python-5a7b3fd87f96)

## 第四部分：LU 分解解决线性系统的全面逐步指南

[](https://chaodeyu.medium.com/?source=post_page-----5a7b3fd87f96--------------------------------)![Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----5a7b3fd87f96--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5a7b3fd87f96--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5a7b3fd87f96--------------------------------) [Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----5a7b3fd87f96--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5a7b3fd87f96--------------------------------) ·阅读时间 4 分钟·2023 年 1 月 31 日

--

![](img/3c437673405fb495da035543b1d149d3.png)

图片来自 [Andry Roby](https://unsplash.com/@andryroby) 在 [Unsplash](https://unsplash.com)

[线性代数系列的第一篇文章](https://medium.com/towards-data-science/linear-algebra-systems-of-linear-equations-and-matrices-with-python-d3e0fcb29e85)介绍了如何使用高斯消元法解决线性系统，[上一篇文章](https://medium.com/towards-data-science/linear-algebra-finding-inverse-matrix-with-python-18dd988f4df)也解释了如何找到逆矩阵以及如何使用逆矩阵解决线性系统。本文将介绍另一种使用 LU 分解解决线性系统的方法。

# LU 分解

下三角-上三角 (LU) 分解是一种将矩阵分解为下三角矩阵和上三角矩阵乘积的方法。它可以被视为高斯消元法的矩阵形式，并且是实现高斯消元法的更好方法，特别是对于具有相同左侧的重复方程的问题，即解决方程 Ax = b，其中 A 相同而 b 的值不同。[1]

![](img/ac830f39aa616a1f196c982d4a143074.png)

A = LU（图由作者提供）

矩阵 A 可以通过初等矩阵分解为下三角矩阵和上三角矩阵的乘积。你可以参考 [上一篇文章](https://medium.com/towards-data-science/linear-algebra-finding-inverse-matrix-with-python-18dd988f4df) 关于三种不同的初等矩阵：交换矩阵、缩放矩阵和主元矩阵。

![](img/a71a099b30757b05250b253e1b935459.png)

矩阵 A 的分解为下三角矩阵和上三角矩阵的乘积。（图由作者提供）

现在让我们来深入了解如何使用 LU 分解解决线性系统。

![](img/47c0b14f26c8c348694dde251dc772ce.png)

使用 LU 分解矩阵逐步解决线性系统。（图片由作者提供）

使用 LU 分解法解决线性系统的示例：

![](img/e9077f3e59f7d361a1e75635ed4a5ed7.png)

给定相同的左侧和不同的右侧，即相同的 A 和不同的 b 值。（图片由作者提供）

**步骤 1, 2, 3:** 将矩阵 A 分解为 L 和 U 矩阵

![](img/abbda379ffebdb6e1932613edaa83193.png)![](img/9379a3c1fb00b6bb05d86d01ae0e60f9.png)

**步骤 4:** 给定 Lc₁ = b₁ 和 Lc₂ = b₂，求解 c₁ 和 c₂！

![](img/ca271d952c9a120488b935471293dee9.png)

**步骤 5:** 给定 Ux₁ = c₁ 和 Ux₂ = c₂，求解 x₁ 和 x₂！

![](img/58bf79d159a5b0bcea826ae6e6a9d6f9.png)

参考 [上一篇文章](https://medium.com/towards-data-science/linear-algebra-finding-inverse-matrix-with-python-18dd988f4df) 了解如何获取逆矩阵。

通过获得 L 和 U 矩阵，我们可以轻松解决上述具有重复左侧的线性系统。

![](img/45d3e81341e65fecfb47e7c09da1b2bb.png)

# 摘要

LU 分解用于解决线性系统和寻找逆矩阵。它被认为是解决具有重复左侧的线性系统的更好方法。在这篇文章中，你将学习如何结合一些代码使用 LU 分解法解决线性系统。

# 推荐阅读

[](/linear-algebra-systems-of-linear-equations-and-matrices-with-python-d3e0fcb29e85?source=post_page-----5a7b3fd87f96--------------------------------) ## 线性代数：线性方程组和矩阵（使用 Python）

### 第一部分：解释线性代数的基础知识：线性方程组和矩阵

towardsdatascience.com [](/linear-algebra-matrix-operations-and-their-properties-with-python-a0885a159be1?source=post_page-----5a7b3fd87f96--------------------------------) ## 线性代数：矩阵运算及其性质（使用 Python）

### 第二部分：解释线性代数的基础知识：矩阵运算及其性质

towardsdatascience.com [](/linear-algebra-finding-inverse-matrix-with-python-18dd988f4df?source=post_page-----5a7b3fd87f96--------------------------------) ## 线性代数：寻找逆矩阵（使用 Python）

### 第三部分：使用初等行变换和 LU 分解法找到逆矩阵的全面分步指南

towardsdatascience.com [](/linear-algebra-euclidean-vector-space-9f88f69cf240?source=post_page-----5a7b3fd87f96--------------------------------) ## 线性代数：欧几里得向量空间

### 第五部分：欧几里得向量空间的温和介绍

towardsdatascience.com [](/linear-algebra-orthogonal-vectors-aaf26de8146a?source=post_page-----5a7b3fd87f96--------------------------------) ## 线性代数：正交向量

### 第六部分：正交向量的温和介绍

[towardsdatascience.com [](https://medium.com/analytics-vidhya/linear-algebra-general-vector-space-0dd3d74e9070?source=post_page-----5a7b3fd87f96--------------------------------) [## 线性代数：一般向量空间

### 第七部分：一般向量空间的全面介绍；子空间、基、秩和零度概念

medium.com](https://medium.com/analytics-vidhya/linear-algebra-general-vector-space-0dd3d74e9070?source=post_page-----5a7b3fd87f96--------------------------------) [](https://medium.com/analytics-vidhya/linear-algebra-discovering-eigenvalues-and-eigenvectors-for-diagonalization-2c3090f9be44?source=post_page-----5a7b3fd87f96--------------------------------) [## 线性代数：发现特征值和特征向量以进行对角化

### 第八部分：对识别特征值和特征向量以促进对角化的深入系统性讲解…

medium.com](https://medium.com/analytics-vidhya/linear-algebra-discovering-eigenvalues-and-eigenvectors-for-diagonalization-2c3090f9be44?source=post_page-----5a7b3fd87f96--------------------------------)

# 参考文献

[1] [LU 分解— 维基百科](https://en.wikipedia.org/wiki/LU_decomposition)

[2] “LU Decomposition • 剑桥大学 • 计算机科学与技术系” [在线]. 可用：[`www.cl.cam.ac.uk/teaching/1314/NumMethods/supporting/mcmaster-kiruba-ludecomp.pdf`](https://www.cl.cam.ac.uk/teaching/1314/NumMethods/supporting/mcmaster-kiruba-ludecomp.pdf)
