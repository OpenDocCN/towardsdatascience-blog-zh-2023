# 线性代数：使用 Python 寻找逆矩阵

> 原文：[`towardsdatascience.com/linear-algebra-finding-inverse-matrix-with-python-18dd988f4df`](https://towardsdatascience.com/linear-algebra-finding-inverse-matrix-with-python-18dd988f4df)

## 第三部分：使用基本行操作和矩阵的行列式来寻找逆矩阵的全面逐步指南

[](https://chaodeyu.medium.com/?source=post_page-----18dd988f4df--------------------------------)![Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----18dd988f4df--------------------------------)[](https://towardsdatascience.com/?source=post_page-----18dd988f4df--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----18dd988f4df--------------------------------) [Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----18dd988f4df--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----18dd988f4df--------------------------------) ·5 分钟阅读·2023 年 1 月 19 日

--

![](img/bdc89b0ab6a2f627646ef764c26d4202.png)

照片来源：[Raimond Klavins](https://unsplash.com/@raimondklavins) 于 [Unsplash](https://unsplash.com)

[上一篇文章](https://medium.com/towards-data-science/linear-algebra-matrix-operations-and-their-properties-with-python-a0885a159be1)解释了不同的矩阵操作及其相应的操作。本文将深入探讨使用基本行操作和矩阵的行列式来获得逆矩阵的两种不同方法。

# 逆矩阵

**逆矩阵**类似于**数的倒数**，因为**一个数与其倒数的乘积**等于**1**，而**一个矩阵与其逆矩阵的乘积**则得到一个**单位矩阵**。然而，要找到矩阵的逆矩阵，矩阵必须是一个方阵，即行数和列数相同。找到矩阵的逆矩阵主要有两种方法：

# 方法 1：使用基本行操作

回顾用于解决线性系统的[3 种行操作](https://medium.com/towards-data-science/linear-algebra-systems-of-linear-equations-and-matrices-with-python-d3e0fcb29e85)：交换、重新缩放和主元操作。这些操作可以写成**初等矩阵**。左乘增广线性系统矩阵表示**初等行操作**。

![](img/852f10e25bbac657846c521883efaece.png)

线性系统的示例增广矩阵。（作者提供的图片）

**交换矩阵**：交换单位矩阵的第 i 行和第 j 行。

![](img/2b4d4deff9a3fe9a7c5dcc7b70f12737.png)

交换矩阵 A 的第 1 行和第 2 行。（作者提供的图片）

**重新缩放矩阵**：如果操作是 k 倍方程 i，则在单位矩阵的位置 (row=i, col=i) 放置数字 k。

![](img/739ff5bf9f22914006398fbbd10f22fd.png)

行 1 乘以常数 3。（图片来源：作者）

**主元矩阵**：如果将方程 i 的倍数加到方程 j 中，则在单位矩阵的 (行=j, 列=i) 位置上填入数字 k。

![](img/081dd7b28a9212bc0ed6b0fca413ded9.png)

5 倍行 2 加到行 1。（图片来源：作者）

我们现在可以**使用初等矩阵**来**找到逆矩阵**。

+   如果 A 是可逆的，那么 Eₖ…E₂E₁A = I

+   两边都乘以 A 的逆矩阵得到：

![](img/88cb37392527a16522bc8044b11b4dce.png)

+   一系列初等行操作可以将 A 化简为 I，相同的初等行操作将 I 转换为矩阵 A 的逆矩阵。

+   如果**A 是一个可逆矩阵**，那么对于每个列向量 b，方程组**Ax = b 有且仅有一个解**。

![](img/cfb9e2d438403ac52f5ee3d98addee6f.png)

使用行操作求逆矩阵 A 的示例：

![](img/5bbcfaca4fa007075fc860bd5d7ab984.png)

使用行操作求 A 的逆。（图片来源：作者）

![](img/7f58d963211fdf8890da4639d439f301.png)

使用初等矩阵求 A 的逆。（图片来源：作者）

![](img/1bdfa16b077c07b8dc09a942071b36cb.png)![](img/0d3aa143f15c9e8adf53dbca477e7734.png)

# 方法 2：使用矩阵的行列式

## 2 x 2 矩阵的行列式和逆矩阵

矩阵的行列式，det(A) 或 |A|，是对方阵有用的值。对于下面的 2x2 矩阵 A，**det(A) = ad — bc**，如果 det(A) ≠ 0，则 A 可逆。对于 2 x 2 矩阵，逆矩阵为：

![](img/1c6589ccbcb327443dffe2c8a5ddd45e.png)

然而，要计算大于 2 x 2 的矩阵的行列式，我们需要获取它的余子式和伴随矩阵。

## 余子式和伴随矩阵

对于方阵 A，**aᵢⱼ 元素的余子式** Mᵢⱼ 被定义为通过删除矩阵 A 的第 i 行和第 j 列所形成的子矩阵的行列式，**aᵢⱼ 元素的伴随矩阵** Cᵢⱼ 如下：

![](img/4df85a73fae6b58047ba61350b290d1f.png)

从小行列式得到的伴随矩阵方程。（图片来源：作者）

![](img/61d2593f03378a916ca636da3108bf60.png)

说明如何从方阵 A 中获得余子式和伴随矩阵。（图片来源：作者）

## 使用伴随矩阵展开得到矩阵的行列式

方阵的行列式可以通过将任何行或列的条目与对应的伴随矩阵相乘并加总结果来计算：

![](img/5a8e9a37b3897dbeb9dc79809df8f32c.png)

最终，**逆矩阵**可以通过**将伴随矩阵的转置**与**1/行列式**相乘来计算。

![](img/c62573f7726a224a54b739bd2fb7c3d2.png)

使用行列式和伴随矩阵求 A 的逆。（图片来源：作者）

![](img/43cbe291e092ec3a5eec44bd84f922e7.png)

# 总结

逆矩阵是解决线性代数中各种问题的有用工具。本文展示的应用之一是解决线性方程组。在这篇文章中，你将学习如何使用两种不同的方法逐步得到逆矩阵，一种是使用初等行变换，另一种是使用矩阵的行列式。

# 推荐阅读

## 线性代数：线性方程组和矩阵，使用 Python

### 第一部分：解释线性代数的基础：线性方程组和矩阵

## 线性代数：矩阵运算及其性质，使用 Python

### 第二部分：解释线性代数的基础：矩阵运算及其性质

## 线性代数：LU 分解法，使用 Python

### 第四部分：LU 分解法解决线性系统的全面逐步指南

## 线性代数：欧几里得向量空间

### 第五部分：欧几里得向量空间的温和介绍

## 线性代数：正交向量

### 第六部分：正交向量的温和介绍

[## 线性代数：一般向量空间](https://medium.com/analytics-vidhya/linear-algebra-general-vector-space-0dd3d74e9070?source=post_page-----18dd988f4df--------------------------------)

### 第七部分：一般向量空间的全面介绍；子空间、基、秩和零度的概念

medium.com](https://medium.com/analytics-vidhya/linear-algebra-general-vector-space-0dd3d74e9070?source=post_page-----18dd988f4df--------------------------------) [](https://medium.com/analytics-vidhya/linear-algebra-discovering-eigenvalues-and-eigenvectors-for-diagonalization-2c3090f9be44?source=post_page-----18dd988f4df--------------------------------) [## 线性代数：发现特征值和特征向量以实现对角化

### 第八部分：深入系统地介绍识别特征值和特征向量以便于对角化…

medium.com](https://medium.com/analytics-vidhya/linear-algebra-discovering-eigenvalues-and-eigenvectors-for-diagonalization-2c3090f9be44?source=post_page-----18dd988f4df--------------------------------)

# 参考文献

[1] [基本矩阵 — 维基百科](https://en.wikipedia.org/wiki/Elementary_matrix)

[2] [余子式 / 代数余子式（线性代数） — 维基百科](https://en.wikipedia.org/wiki/Minor_(linear_algebra))

[3] [行列式 — 维基百科](https://en.wikipedia.org/wiki/Determinant)

[4] [可逆矩阵 — 维基百科](https://en.wikipedia.org/wiki/Invertible_matrix)
