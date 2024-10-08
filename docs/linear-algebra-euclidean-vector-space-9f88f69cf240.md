# 线性代数：欧几里得向量空间

> 原文：[`towardsdatascience.com/linear-algebra-euclidean-vector-space-9f88f69cf240`](https://towardsdatascience.com/linear-algebra-euclidean-vector-space-9f88f69cf240)

## 第五部分：欧几里得向量空间的温和介绍

[](https://chaodeyu.medium.com/?source=post_page-----9f88f69cf240--------------------------------)![Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----9f88f69cf240--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9f88f69cf240--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f88f69cf240--------------------------------) [Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----9f88f69cf240--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f88f69cf240--------------------------------) ·4 分钟阅读·2023 年 3 月 6 日

--

![](img/7db52c533a6259da2ec2ff9d61a28347.png)

图片来源：[Karsten Würth](https://unsplash.com/@karsten_wuerth) 在 [Unsplash](https://unsplash.com)

# 介绍

在机器学习和深度学习中，我们大多数时候都在处理向量。而向量空间模型可以将数据之间的关系表示为向量。此外，从几何角度来看，它还能够比较两个向量的相似性，无论是使用两个向量之间的距离（欧几里得距离）还是两个向量之间的角度（余弦相似度）。

# 向量

让我们从二维空间中的向量几何开始。

![](img/78d618d48df0427c8e479bda7530c3c7.png)

Image 1\. 二维空间中向量的示例。 （图片来源：作者）

+   如果 u = (u₁, u₂) 和 v = (v₁, v₂) 满足 u₁ = v₁ 且 u₂ = v₂，则这两个向量相等。

+   向量 u 和 v 的和定义为 u + v = (u₁ + v₁, u₂ + v₂)

![](img/7aae0b686667eecd293b85bd76084fef.png)

Image 2\. 向量和的示例。 （图片来源：作者）

+   标量 k 与向量 u 的乘积定义为 ku = (ku₁, ku₂)

![](img/5b9d1f4c84a9f0d1087aa37ae9b0e754.png)

Image 3\. 向量与标量相乘的示例。 图片来源：作者。

+   向量的负向量 -v 被定义为与 v 具有相同大小但方向相反的向量。

+   向量的差定义为 u-v = u + (-v)

![](img/69459a958fc9856941cd9db0168a56a4.png)

Image 4\. 向量差的示例。 （图片来源：作者）

## 范数和距离

+   向量的长度通常称为范数。

![](img/0542d7da9154d3fb76776387f6bf98fe.png)

Image 5\. 向量的长度，范数。 （图片来源：作者）

+   两点之间的距离定义如下：

![](img/1c3e448715211306fecd1850ff1b148b.png)

Image 6\. 两点之间的距离。 （图片来源：作者）

# 欧几里得 n 空间

+   如果 n 是正整数，存在一系列 n 个实数 v₁, v₂, …, vₙ，则我们写作：v = (v₁, v₂, …, vₙ)

+   所有具有 n 个分量的向量的集合称为欧几里得 n 空间，记作 Rⁿ。

**Rⁿ 中向量的属性**

1.  u + v = v + u

1.  u + (v + w) = (u + v) + w

1.  k(u + v) = ku + kv

1.  (k + m)u = ku + mu

1.  u + 0 = 0 + u = u

1.  u + (-u) = 0

1.  1u = u

    其中 u, v, w 是向量，k, m 是常数

## 欧几里得内积

+   如果 u 和 v 是 Rⁿ 中的向量，则欧几里得内积定义为

    u . v = u₁v₁ + u₂v₂ + … + uₙvₙ

由此得到：

![](img/b02db4eee55bf1a0aa2be8beb14b51fb.png)

图片 7\. 柯西-施瓦茨不等式的证明。（作者提供的图片）

![](img/a58a63b5018674ad84deafa0628119fc.png)

图片 8\. 证明 u.v=1/4||u+v||² -1/4||u-v||²

+   属性：

    u.v = v.u

    (u + v).w = u.w + v.w

    (ku).v = k(u.v)

    v.v ≥ 0，v.v = 0 当且仅当 v = 0

    其中 u, v, w 是向量，k 是常数

## **Rⁿ** 中的范数和距离

+   Rⁿ 中的范数定义为：

![](img/7eae3eb7cd19e35272e93faba7f01b01.png)

图片 9\. Rⁿ 中的范数。（作者提供的图片）

+   Rⁿ 中两点之间的欧几里得距离定义为：

![](img/542d7b71fa1af39f00922d84b62467af.png)

图片 10\. Rⁿ 中两点之间的欧几里得距离。（作者提供的图片）

+   属性：

    ||u|| ≥ 0，||u|| = 0 当且仅当 u = 0

    ||ku|| = |k| ||u||

    ||u + v|| ≤ ||u|| + ||v||

    d(u, v) ≥ 0，d(u, v) = 0 当且仅当 u = v

    d(u, v) = d(v, u)

    d(u, v) ≤ d(u, w) + d(w, v)

    其中 u, v, w 是向量，k 是常数

## **Rⁿ** 中的角度

如果 u 和 v 是欧几里得 n 空间中的向量，则 u 和 v 之间的角度 (θ) 定义为：

cosθ 的推导可以参考 **图片 7**。

![](img/b491b41c92e5c833679b172e5e78cc82.png)

图片 11\. Rⁿ 中两向量之间的角度。（作者提供的图片）

# 推荐阅读

[](/linear-algebra-systems-of-linear-equations-and-matrices-with-python-d3e0fcb29e85?source=post_page-----9f88f69cf240--------------------------------) ## 线性代数：线性方程组和矩阵，使用 Python

### 第一部分：讲解线性代数的基础：线性方程组和矩阵

[towardsdatascience.com [](/linear-algebra-matrix-operations-and-their-properties-with-python-a0885a159be1?source=post_page-----9f88f69cf240--------------------------------) ## 线性代数：矩阵运算及其性质，使用 Python

### 第二部分：讲解线性代数的基础：矩阵运算及其性质

[towardsdatascience.com [](/linear-algebra-finding-inverse-matrix-with-python-18dd988f4df?source=post_page-----9f88f69cf240--------------------------------) ## 线性代数：使用 Python 计算逆矩阵

### 第三部分：全面逐步指导如何使用基本行变换找到逆矩阵并…

towardsdatascience.com [](/linear-algebra-lu-decomposition-with-python-5a7b3fd87f96?source=post_page-----9f88f69cf240--------------------------------) ## 线性代数：LU 分解与 Python

### 第四部分：全面逐步指导如何使用 LU 分解解决线性系统

towardsdatascience.com [](/linear-algebra-orthogonal-vectors-aaf26de8146a?source=post_page-----9f88f69cf240--------------------------------) ## 线性代数：正交向量

### 第六部分：正交向量的温和介绍

towardsdatascience.com [](https://medium.com/analytics-vidhya/linear-algebra-general-vector-space-0dd3d74e9070?source=post_page-----9f88f69cf240--------------------------------) [## 线性代数：一般向量空间

### 第七部分：全面介绍一般向量空间；子空间、基、秩和零度概念

medium.com](https://medium.com/analytics-vidhya/linear-algebra-general-vector-space-0dd3d74e9070?source=post_page-----9f88f69cf240--------------------------------) [](https://medium.com/analytics-vidhya/linear-algebra-discovering-eigenvalues-and-eigenvectors-for-diagonalization-2c3090f9be44?source=post_page-----9f88f69cf240--------------------------------) [## 线性代数：发现特征值和特征向量以实现对角化

### 第八部分：系统化深入讲解特征值和特征向量的识别，以便于对角化…

medium.com](https://medium.com/analytics-vidhya/linear-algebra-discovering-eigenvalues-and-eigenvectors-for-diagonalization-2c3090f9be44?source=post_page-----9f88f69cf240--------------------------------)

# 参考文献

[1] [向量空间 — 维基百科](https://en.wikipedia.org/wiki/Vector_space)

[2] [欧几里得空间 — 维基百科](https://en.wikipedia.org/wiki/Euclidean_space)

[3] 国防医学院讲座 — 朱伟达，[欧几里得向量空间，2008](https://www.cs.ccu.edu.tw/~wtchu/courses/2008f_LA/Lectures/Lecture%2012%20Euclidean%20Vector%20Space.pdf)
