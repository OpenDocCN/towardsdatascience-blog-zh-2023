# 线性代数：矩阵运算及其属性，使用 Python

> 原文：[`towardsdatascience.com/linear-algebra-matrix-operations-and-their-properties-with-python-a0885a159be1`](https://towardsdatascience.com/linear-algebra-matrix-operations-and-their-properties-with-python-a0885a159be1)

## 第二部分：解释线性代数的基础：矩阵运算及其属性

[](https://chaodeyu.medium.com/?source=post_page-----a0885a159be1--------------------------------)![Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----a0885a159be1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a0885a159be1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0885a159be1--------------------------------) [Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----a0885a159be1--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0885a159be1--------------------------------) ·6 分钟阅读·2023 年 1 月 16 日

--

![](img/34dc9363b4cc87d7dca834373cc12464.png)

图片由 [Pietro De Grandi](https://unsplash.com/@peter_mc_greats) 提供，来源于 [Unsplash](https://unsplash.com)

上一篇文章 介绍了线性方程和线性系统的定义，以及线性系统如何重写成矩阵的形式。因此，了解矩阵的不同运算及其属性是非常重要的。

# 矩阵和向量

矩阵是一个具有 m 行和 n 列的矩形数字数组，其中 m > 1 且 n > 1。[1]

向量是只有一行（行向量）或一列（列向量）的矩阵。[2]

![](img/ff13a7058c0c33a3646c2c7b1591c6c1.png)

[左] 矩阵。[中] 列向量。[右] 行向量。（图像由作者提供）

## 一些特殊矩阵

+   方阵：一个矩阵，其中 m = n

+   对角矩阵 I：一个方阵，所有非对角线的元素均为零

+   单位矩阵：对角矩阵 I，其中 iₓᵧ = 1

+   带状矩阵：大多数条目为零，非零条目位于主对角线周围的带状区域

+   下三角矩阵：L，若 x < y，则 lₓᵧ = 0，1 ≤ x ≤ m 和 1 ≤ y ≤ n

+   上三角矩阵：U，若 x > y，则 uₓᵧ = 0，1 ≤ x ≤ m 和 1 ≤ y ≤ n

![](img/9b976567d54ec1be7cfa0d9cbc5c8410.png)

[左] 对角矩阵。[中] 单位矩阵。[右] 带状矩阵。（图像由作者提供）

![](img/b6346409d1db8d50d395a2e3d898ef2b.png)

[左] 下三角矩阵。[右] 上三角矩阵。（图像由作者提供）

# 矩阵的基本运算

## 加法和减法

矩阵的加法和减法通过简单地加或减相应的条目来完成。矩阵必须具有相同的维度才能进行加法或减法。

![](img/f94fe5f66d9ffcd2aa5eb754b1ee1fd0.png)

矩阵加法和减法的表达式。（作者提供的图片）

## 乘法

为了执行矩阵乘法，两矩阵的维度必须是**可比的**，即第一个矩阵的列数必须等于第二个矩阵的行数。

+   标量-矩阵乘法：cA

    矩阵中的每个条目都乘以一个实数（标量）。

![](img/0dc9c1ef86e686dcddb88a4e1a3a3657.png)

标量矩阵乘法的表达式。（作者提供的图片）

+   内积：ab

    内积是行向量和列向量的乘法。

![](img/444351c3740b1b59a8ac919f02d761e8.png)

内积的表达式。（作者提供的图片）

+   矩阵-向量乘法：Ab

    它可以**按行**执行，即 A 的每一行与向量 b 的内积，或**按列**执行，即 A 的每一列与向量 b 的每个标量条目的标量矩阵乘法。

![](img/df918f7e89561e4e0746f139944f3cf5.png)

按行的 Ab 表达式。（作者提供的图片）

![](img/6c83a73b9e4cd9da39b61c4713acfe52.png)

按行的 Ab 示例。（作者提供的图片）

![](img/e94bbf4fae24bdaa7fbc601991017edf.png)

按列的 Ab 表达式。（作者提供的图片）

![](img/d3c1a518569b9af2d133c2445debdee8.png)

按列的 Ab 示例。（作者提供的图片）

+   矩阵-矩阵乘法：AB

    类似于矩阵-向量乘法，矩阵-矩阵乘法可以**按行**执行，即 A 的每一行与 B 的每一列的内积，或**按列**执行，即矩阵 A 与 B 的每一列的矩阵-向量乘法。

![](img/1e553f3eb41cf5c054dbbe11b040588a.png)

按行的 AB 表达式。（作者提供的图片）

![](img/e289b228ef95ca2d0d848dbc7a089245.png)

按列的 AB 表达式。（作者提供的图片）

![](img/753646aafa0a53d5069f4fe9cf1ae9db.png)

basic_matrix_operations.py 的输出

# 基本矩阵操作的性质

给定：A，B，C = 矩阵，r，s = 实数 / 标量

+   A + 0 = A

+   A + B = B + A

+   A + (B + C) = (A + B) + C

+   A (BC) = (AB) C

+   A (B ± C) = AB ± AC

+   (B ± C) A = BA ± CA

+   r (A ± B) = rA + rB

+   (r ± s) C = rC ± sC

+   r (sC) = (rs) C

+   r (AB) = (rA) B= A (rB)

+   单位矩阵：I，AI = A 和 IA = A

+   矩阵乘法不是交换律的：通常 AB ≠ BA

+   取消律并不总是成立：AB = AC <≠> B = C

+   AB = 0 <≠> A = 0 或 B = 0

# 矩阵的转置

转置矩阵可以通过互换矩阵的行和列来找到。矩阵的迹（主对角线上的条目之和）等于对应的转置矩阵的迹。[3]

![](img/68198883d31af165994098f6ec2baa9c.png)

转置矩阵操作的表达式。（作者提供的图片）

![](img/1d9189b5eb657099e2e8ab7fa5fa4ffe.png)

转置矩阵操作的示例。（作者提供的图片）

![](img/5739c7af8fb878a27d8b4c29284d1b63.png)

matrix_transpose.py 的输出

## 性质：

![](img/1983da677fdad00febc08b57fd3dfc25.png)

# 矩阵的逆

如果 A 是一个方阵，并且存在一个矩阵 B 使得 AB = BA = I，则 A 称为可逆矩阵，B 称为 A 的逆矩阵。如果没有这样的矩阵 B，则 A 称为奇异矩阵。换句话说，矩阵 A 称为奇异矩阵，如果它没有逆矩阵。 [4]

对于一个可逆矩阵，只有一个逆矩阵。

![](img/2dc88f66ea64cece6fd896ead9dd73e7.png)

matrix_inverse.py 的输出

## 属性：

![](img/c8d0318c23036b89d4ea7d92d5cab454.png)

矩阵的幂如果它是一个方阵且可逆：

![](img/8648fa45a5c940710a4093e449356fd9.png)

# 对称矩阵

如果一个矩阵等于它的转置矩阵，则称该矩阵为对称矩阵，即 A = Aᵗ。

如果 A 和 B 是对称的：

1.  A + B 和 A - B 是对称的

1.  kA 是对称的

1.  对于任何矩阵 A，AAᵗ是一个方阵且对称矩阵

1.  如果 A 是一个可逆对称矩阵，则 A 的逆矩阵是对称的

1.  然而，积 AB 通常不是对称的。只有当 AB = BA 时，A 和 B 才是可交换的

# 总结

在这篇文章中，你将深入了解矩阵和向量，以及矩阵的不同运算及其性质。文章解释了三种基本的矩阵运算：矩阵的加法、减法和乘法。接着是转置矩阵、逆矩阵和对称矩阵。最后，它还包括一些关于如何使用 numpy 包进行矩阵运算的代码。

# 推荐阅读

[](/linear-algebra-systems-of-linear-equations-and-matrices-with-python-d3e0fcb29e85?source=post_page-----a0885a159be1--------------------------------) ## 线性代数：线性方程组和矩阵，与 Python

### 第一部分：解释线性代数的基础：线性方程组和矩阵

towardsdatascience.com [](/linear-algebra-finding-inverse-matrix-with-python-18dd988f4df?source=post_page-----a0885a159be1--------------------------------) ## 线性代数：求逆矩阵，与 Python

### 第三部分：使用初等行变换找逆矩阵的全面逐步指南和…

towardsdatascience.com [](/linear-algebra-lu-decomposition-with-python-5a7b3fd87f96?source=post_page-----a0885a159be1--------------------------------) ## 线性代数：LU 分解，与 Python

### 第四部分：使用 LU 分解解决线性系统的全面逐步指南

towardsdatascience.com [](/linear-algebra-euclidean-vector-space-9f88f69cf240?source=post_page-----a0885a159be1--------------------------------) [## 线性代数：欧几里得向量空间

### 第五部分：欧几里得向量空间的温和介绍

[线性代数：欧几里得向量空间](https://towardsdatascience.com/linear-algebra-euclidean-vector-space-9f88f69cf240?source=post_page-----a0885a159be1--------------------------------) [](https://towardsdatascience.com/linear-algebra-orthogonal-vectors-aaf26de8146a?source=post_page-----a0885a159be1--------------------------------) [## 线性代数：正交向量

### 第六部分：正交向量的温和介绍

[线性代数：正交向量](https://towardsdatascience.com/linear-algebra-orthogonal-vectors-aaf26de8146a?source=post_page-----a0885a159be1--------------------------------) [](https://medium.com/analytics-vidhya/linear-algebra-general-vector-space-0dd3d74e9070?source=post_page-----a0885a159be1--------------------------------) [## 线性代数：一般向量空间

### 第七部分：一般向量空间的综合介绍；子空间、基、秩和零度概念

[线性代数：发现特征值和特征向量以便于对角化](https://medium.com/analytics-vidhya/linear-algebra-general-vector-space-0dd3d74e9070?source=post_page-----a0885a159be1--------------------------------) [](https://medium.com/analytics-vidhya/linear-algebra-discovering-eigenvalues-and-eigenvectors-for-diagonalization-2c3090f9be44?source=post_page-----a0885a159be1--------------------------------) [## 线性代数：发现特征值和特征向量以便于对角化

### 第八部分：系统性深入解析特征值和特征向量以便于对角化…

[线性代数：发现特征值和特征向量以便于对角化](https://medium.com/analytics-vidhya/linear-algebra-discovering-eigenvalues-and-eigenvectors-for-diagonalization-2c3090f9be44?source=post_page-----a0885a159be1--------------------------------)

# 参考文献

[1] [矩阵（数学） — 维基百科](https://en.wikipedia.org/wiki/Matrix_(mathematics))

[2] [向量 — 维基百科](https://en.wikipedia.org/wiki/Vector)

[3] [转置 — 维基百科](https://en.wikipedia.org/wiki/Transpose)

[4] [可逆矩阵 — 维基百科](https://en.wikipedia.org/wiki/Invertible_matrix)

[4] 矩阵运算的性质。（无日期）。检索于 2023 年 1 月 8 日，来自 [`math.mit.edu/~dyatlov/54summer10/matalg.pdf`](https://math.mit.edu/~dyatlov/54summer10/matalg.pdf)
