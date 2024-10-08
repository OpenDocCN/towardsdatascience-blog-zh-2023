# 线性代数：线性方程组和矩阵，使用 Python

> 原文：[`towardsdatascience.com/linear-algebra-systems-of-linear-equations-and-matrices-with-python-d3e0fcb29e85`](https://towardsdatascience.com/linear-algebra-systems-of-linear-equations-and-matrices-with-python-d3e0fcb29e85)

## 第一部分：解释线性代数的基本概念：线性方程组和矩阵

[](https://chaodeyu.medium.com/?source=post_page-----d3e0fcb29e85--------------------------------)![Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----d3e0fcb29e85--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d3e0fcb29e85--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d3e0fcb29e85--------------------------------) [Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----d3e0fcb29e85--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d3e0fcb29e85--------------------------------) ·5 分钟阅读·2023 年 1 月 10 日

--

![](img/e8c03e80317077b44cce37272b8f5b53.png)

图片由 [Sergio Rota](https://unsplash.com/@sergio_rota) 提供，来源于 [Unsplash](https://unsplash.com)

# 介绍

线性代数在科学和工程的多个领域中都至关重要。它是几乎所有数学领域的核心。例如，方程通常用于建模现实生活中的问题，多变量问题可以通过方程组来建模。即使是非线性方程组也可以转化为线性方程组。因此，了解如何解决这些线性方程组在现代科学的各个领域都非常有帮助。作为数据科学家，良好的线性代数理解对于使用大多数机器学习方法，尤其是深度学习算法，也是至关重要的。

## 线性方程

+   线性方程是一个方程，其中变量的最高次幂始终为 1。

![](img/6af038eb13721273ae6a6fbe3d4fb35e.png)

图像 1\. 线性方程。图片来源：作者。

## 线性系统

+   **线性方程组** 是一系列线性方程。

![](img/e2a468b634c302d7d857682a7dc6eaa6.png)

图像 2\. 线性方程组。图片来源：作者。

上述线性方程组可以被重写为一种称为 **增广矩阵** 的矩阵形式。一个 m x n 矩阵是一个具有 m 行和 n 列的数字数组，其中 m > 1 且 n > 1。矩阵中的每个数字称为一个条目。

![](img/69617384449c0fa60be0f5041f164072.png)

图像 3\. 增广矩阵。图片来源：作者。

## 齐次线性系统

+   如果常数项 bᵢ 全为零，则称线性系统为齐次的。

![](img/e6777c6ede42212972c3a7e2d5a516e4.png)

图像 4\. 齐次线性方程组。图片来源：作者。

+   xᵢ = 0 称为齐次系统的平凡解。

+   具有比方程更多未知数的齐次系统 (n > m) 有无限多解。

## 线性系统的可能解

对于线性系统的解，有 3 种可能的情况：

1.  零解

1.  单一解

1.  无限多解

具有**无解**的线性系统称为**不一致的**。如果有**至少一个解**，则称为**一致的**。

![](img/e144f47dc0146a6995771791e8b18cb3.png)

图 5\. 线性系统的可能解示例。图片由作者提供。

# 使用基本行操作解线性方程系统的步骤

## 行简化

行简化是一种解线性方程组的算法。对矩阵进行行简化是通过使用一系列基本行操作来修改矩阵，直到矩阵的左下角被填充为零。 [1] 行操作有 3 种类型：

1.  交换 — 一个方程与另一个方程互换，互换 2 行。

1.  重新缩放 — 一个方程的两边都乘以一个非零常数，乘以一个非零常数的行。

1.  主元化 — 一个方程被替换为其本身与另一个方程的倍数的和，将一个行的倍数加到另一行。

## 行阶梯形态

如果一个系统处于行阶梯形态，则该系统处于该状态

+   这是一个阶梯模式

+   每一行中的主元（第一个非零系数的项）位于上面一行的主元的右侧

+   所有主元都是 1

![](img/3f101ba3f0b73720778fb74abe09fac1.png)

图 6\. 行阶梯形态示例。图片由作者提供。

产生**阶梯形态**矩阵的**过程**称为**高斯消元法**。

## 简化阶梯形态

如果一个系统处于简化阶梯形态，则该系统处于该状态

+   每个非零行中的主元都是 1。

+   主元 1 所在列中的所有其他项都是零。

![](img/80266a35527333f049a78ee9afe881ee.png)

图 7\. 简化阶梯形态示例。图片由作者提供。

产生**简化** **阶梯形态**矩阵的**过程**称为**高斯-乔丹消元法**。

在阶梯形态中，**第一个具有非零系数的变量**是**主元变量/领先变量**。**非主元变量**是**自由变量**。

# 示例

求解以下方程系统：

![](img/4649245efb03de3117939d3a0b723c3c.png)

首先将线性方程系统转换为增广矩阵，然后进行一系列基本行操作，将其求解为行阶梯形态。

![](img/3f591faa492f7cfa0e27ada6388833bb.png)

图 8\. 使用基本行操作解线性系统（行阶梯形态）。图片由作者提供。

使用 sympy 包获取简化阶梯形态的代码

![](img/40173547644f5aedec68a7ae65231bcc.png)

使用 sympy 包对示例线性方程系统进行简化阶梯形态处理。

使用 numPy 包获取线性系统解的代码

![](img/bd949cf7be4db54c1d2290ecd57d9b17.png)

使用 numpy 包求解线性方程组的解决方案。

# 总结

在这篇文章中，你了解了线性代数的基础知识、线性方程组和矩阵。你学会了线性方程和线性系统的定义，以及线性系统的三种可能解。此外，你还学会了通过应用行变换序列来求解线性方程，将矩阵化简为行最简形式或简化的行最简形式。除此之外，你还了解了可以用来求解线性方程组的编程包。

# 推荐阅读

[## 线性代数：矩阵运算及其性质，使用 Python](https://towardsdatascience.com/linear-algebra-matrix-operations-and-their-properties-with-python-a0885a159be1?source=post_page-----d3e0fcb29e85--------------------------------)

### 第二部分：解释线性代数的基本概念：矩阵运算及其性质

[## 线性代数：使用 Python 计算逆矩阵](https://towardsdatascience.com/linear-algebra-matrix-operations-and-their-properties-with-python-a0885a159be1?source=post_page-----d3e0fcb29e85--------------------------------)

### 第三部分：使用基本行操作寻找逆矩阵的全面逐步指南

[## 线性代数：LU 分解，使用 Python](https://towardsdatascience.com/linear-algebra-lu-decomposition-with-python-5a7b3fd87f96?source=post_page-----d3e0fcb29e85--------------------------------)

### 第四部分：使用 LU 分解求解线性系统的全面逐步指南

[## 线性代数：欧几里得向量空间](https://towardsdatascience.com/linear-algebra-euclidean-vector-space-9f88f69cf240?source=post_page-----d3e0fcb29e85--------------------------------)

### 第五部分：欧几里得向量空间的温和介绍

[## 线性代数：正交向量](https://towardsdatascience.com/linear-algebra-orthogonal-vectors-aaf26de8146a?source=post_page-----d3e0fcb29e85--------------------------------)

### 第六部分：正交向量的温和介绍

[## 线性代数：一般向量空间](https://medium.com/analytics-vidhya/linear-algebra-general-vector-space-0dd3d74e9070?source=post_page-----d3e0fcb29e85--------------------------------)

### 第七部分：一般向量空间的综合介绍；子空间、基、秩和零度概念

[medium.com](https://medium.com/analytics-vidhya/linear-algebra-general-vector-space-0dd3d74e9070?source=post_page-----d3e0fcb29e85--------------------------------) [](https://medium.com/analytics-vidhya/linear-algebra-discovering-eigenvalues-and-eigenvectors-for-diagonalization-2c3090f9be44?source=post_page-----d3e0fcb29e85--------------------------------) [## 线性代数：发现特征值和特征向量以进行对角化

### 第八部分：深入的系统化讲解，帮助识别特征值和特征向量，以促进对角化…

[medium.com](https://medium.com/analytics-vidhya/linear-algebra-discovering-eigenvalues-and-eigenvectors-for-diagonalization-2c3090f9be44?source=post_page-----d3e0fcb29e85--------------------------------)

# 参考文献

[1] [高斯消去法 — 维基百科](https://en.wikipedia.org/wiki/Gaussian_elimination)

[2] 第二章 矩阵与向量 | MATH0007：联合荣誉学生的代数。 (无日期)。检索于 2023 年 1 月 2 日，来自 [`www.ucl.ac.uk/~ucahmto/0007/_book/2-linalg1.html`](https://www.ucl.ac.uk/~ucahmto/0007/_book/2-linalg1.html)
