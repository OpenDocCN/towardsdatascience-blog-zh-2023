# 奇异值分解与特征分解在降维中的比较

> 原文：[`towardsdatascience.com/singular-value-decomposition-vs-eigendecomposition-for-dimensionality-reduction-fc0d9ac24a8e`](https://towardsdatascience.com/singular-value-decomposition-vs-eigendecomposition-for-dimensionality-reduction-fc0d9ac24a8e)

## 使用两种方法执行主成分分析（PCA）并比较结果

[](https://rukshanpramoditha.medium.com/?source=post_page-----fc0d9ac24a8e--------------------------------)![Rukshan Pramoditha](https://rukshanpramoditha.medium.com/?source=post_page-----fc0d9ac24a8e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fc0d9ac24a8e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fc0d9ac24a8e--------------------------------) [Rukshan Pramoditha](https://rukshanpramoditha.medium.com/?source=post_page-----fc0d9ac24a8e--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fc0d9ac24a8e--------------------------------) ·阅读时间 8 分钟·2023 年 3 月 20 日

--

![](img/249648322ceade3a63b6972cb91564d2.png)

图片来源：[Viktor Peschel](https://pixabay.com/users/1a-photoshop-6724285/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2886285) 来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2886285)

奇异值分解（SVD）和特征分解（ED）都是来自线性代数的矩阵分解方法。

在机器学习（ML）领域，奇异值分解和特征分解都可以作为数据降维的方法。

之前，我们已经详细讨论了 [特征分解](https://medium.com/data-science-365/eigendecomposition-of-a-covariance-matrix-with-numpy-c953334c965d)。今天，我们将更加重点讨论 SVD。

主成分分析（PCA）可以使用这两种方法来执行。PCA 是机器学习中最流行的线性降维技术。SVD 被认为是 PCA 背后的基础数学。流行的机器学习库 Scikit-learn 也在其 `PCA()` 函数中使用 SVD 来执行 PCA。因此，SVD 在降维中比特征分解更受欢迎。

NumPy 提供了高级且易于使用的函数来执行 SVD 和特征分解。

```py
**Topics included:
----------------**
01\. What is singular value decomposition?
02\. SVD equation and its terms
03\. Singular value decomposition in NumPy - svd() function
04\. What is eigendecomposition?
05\. Eigendecomposition equation and its terms
06\. Eigendecomposition in NumPy - eig() function
07\. Performing PCA using singular value decomposition
08\. Performing PCA using eigendecomposition
09\. Compare the results of both methods
10\. Conclusions
```

# 什么是奇异值分解？

奇异值分解（SVD）是一种矩阵分解方法。这是一个来自线性代数的重要数学运算。

有多种方式可以分解（分解 / 拆解）一个矩阵，就像我们可以将数字 16 分解为 2 x 8 = 16、4 x 4 = 16、2 x 2 x 4 = 16、2 x 2 x 2 x 2 = 16 一样。并不是所有的分解方法都同样重要。这取决于具体的使用案例。

同样，我们可以以多种方式对矩阵进行分解，其中一些更为重要。奇异值分解（SVD）和特征分解是矩阵分解中的重要方法。

> 奇异值分解是将矩阵**A**分解为三个矩阵乘积的过程，如下方程所示。

![](img/c7b6ce2cfc1fcf20b9a01bec3589f197.png)

**SVD 方程** (图片由作者提供)

+   **A:** 我们对其执行 SVD 的矩阵。

+   **U:** 一个方阵。这被称为右奇异向量矩阵。

+   **Σ:** 一个对角矩阵。这被称为奇异值矩阵，其大小与**A**相同。

+   **V^T:** 一个方阵。这被称为左奇异向量矩阵。默认情况下，NumPy 的 SVD 函数返回**V^T**，它是**V**的转置。

矩阵**A**可以是方阵也可以是非方阵，因为 SVD 定义于方阵和非方阵。相比之下，特征分解仅对方阵定义。

矩阵**Σ**包含奇异值，这些值总是非负的。可以包含零值。

非零奇异值的数量等于矩阵**A**的秩。

# NumPy 中的奇异值分解

在 NumPy 中，可以使用`svd()`函数轻松地执行 SVD。这里是一个示例。

```py
import numpy as np
A = np.array([[2, 4, 1],
              [5, 7, 6],
              [1, 1, 3]])

U, s, Vt = np.linalg.svd(A)

print("A")
print(A)
print("\nU")
print(U)
print("\ns")
print(s)
print("\nVt")
print(Vt)
```

![](img/a6000f866e46a999252e6f2f05d2d075.png)

(图片由作者提供)

NumPy 的**svd()**函数返回**Σ**（奇异值矩阵）作为一个向量（由**s**表示），而不是对角矩阵。该向量包含了矩阵**A**的所有奇异值。

如果你想直接获得**Σ**，可以通过以下代码进行一些修改。

```py
S = np.zeros(np.shape(A))
np.fill_diagonal(S,s)
print(S)
```

![](img/81082c5e0db19903518e8da2eeb64741.png)

(图片由作者提供)

如果你只需要计算奇异值而不需要 U 和 Vt 矩阵，你可以运行以下代码。

```py
s = np.linalg.svd(A, compute_uv=False)
print(s)
```

![](img/b0a958c19f33e309dde1407c08eae037.png)

(图片由作者提供)

这就是我们如何在 NumPy 中执行 SVD。它比你想象的要简单得多。接下来，我们将进入特征分解部分。

# 什么是特征分解？

特征分解是另一种重要的矩阵分解方法。

> 特征分解是将方阵 A 分解为特征值和特征向量的乘积的过程，如下方程所示。

![](img/93f7324a37f8fe17c6cb97ebaa464bd9.png)

**方阵 A 及其特征值和特征向量对之间的关系** (图片由作者提供)

+   **A:** 我们对其执行特征分解的矩阵。它应该是一个方阵。

+   **λ:** 一个称为特征值的标量。

+   **x:** 一个称为特征向量的向量。

矩阵**A**应该是方阵，因为特征分解仅对方阵定义。

特征值可以是正值或负值。

> *特征值和特征向量是成对出现的。这种配对被称为* ***特征对****。因此，矩阵* ***A*** *可以有多个这样的特征对。上述方程展示了* ***A*** *与其一个特征对之间的关系* [ref: [用 NumPy 进行协方差矩阵的特征分解](https://medium.com/data-science-365/eigendecomposition-of-a-covariance-matrix-with-numpy-c953334c965d)]

# NumPy 中的特征分解

在 NumPy 中，可以使用 `eig()` 函数轻松地执行特征分解。这里是一个示例。

```py
import numpy as np
A = np.array([[2, 4, 1],
              [5, 7, 6],
              [1, 1, 3]])

eigen_vals, eigen_vecs = np.linalg.eig(A)

print("A")
print(A)
print("\nEigenvalues")
print(eigen_vals)
print("\nEigenvectors")
print(eigen_vecs)
```

![](img/ec500f583fcaf1f81e031b0932cafcc6.png)

(作者提供的图片)

NumPy 的 **eig()** 函数返回特征值作为一个向量。该向量包含了 **A** 的所有特征值。

我们对同一个矩阵 **A** 执行了 SVD 和特征分解。通过查看输出，我们可以说：

> 奇异值分解和特征分解即使在矩阵为方阵时也不是相同的东西。

# 使用奇异值分解执行 PCA

PCA 通常通过对标准化数据的协方差矩阵应用 SVD 来执行。标准化数据的协方差矩阵与非标准化数据的相关矩阵完全相同。

在 SVD 之前我们标准化数据，因为奇异值对原始特征的相对范围非常敏感。

为了演示使用 SVD 的 PCA 过程，我们将使用包含 13 个输入特征的 Wine 数据集。

**步骤 1:** 获取 Wine 数据。

```py
from sklearn.datasets import load_wine

wine = load_wine()
X = wine.data
y = wine.target

print("Wine dataset size:", X.shape)
```

![](img/b663ea243afd2ae6e9336d66fcfd0cba.png)

(作者提供的图片)

**步骤 2:** 标准化数据。

```py
from sklearn.preprocessing import StandardScaler

X_scaled = StandardScaler().fit_transform(X)
```

**步骤 3:** 计算标准化数据的协方差矩阵。

```py
import numpy as np
cov_mat = np.cov(X_scaled.T)
```

**步骤 4:** 对协方差矩阵执行 SVD 并获取协方差矩阵的奇异值。

```py
U, s, Vt = np.linalg.svd(cov_mat)
print(s)
```

![](img/68288b1015e9d7208bc07f7c08a37e2a.png)

**奇异值** (作者提供的图片)

这些奇异值的总和等于数据中的总方差。每个奇异值表示每个组件捕获的方差量。为了计算这些内容，我们需要将奇异值转换为解释的方差。

**步骤 5:** 将奇异值转换为解释的方差。

```py
exp_var = (s / np.sum(s)) * 100
print(exp_var)
```

![](img/394112bd6155fb0378806a7b75bd823b.png)

(作者提供的图片)

第一个组件捕获了数据中的 36.2% 方差。第二个组件捕获了数据中的 19.2% 方差，以此类推。

**步骤 6:** 可视化奇异值以选择正确的组件数量

不是所有组件对模型的贡献相同。我们可以丢弃那些对数据方差贡献不大的组件，只保留最重要的组件。为此，我们需要通过创建 [*累积解释方差图*](https://medium.com/data-science-365/2-plots-that-help-me-to-choose-the-right-number-of-principal-components-351a87e15a9f) 来可视化所有奇异值。

```py
cum_exp_var = np.cumsum(exp_var)

# a = Number of input features + 1
a = X.shape[1] + 1

import matplotlib.pyplot as plt
plt.bar(range(1, a), exp_var, align='center',
        label='Individual explained variance')

plt.step(range(1, a), cum_exp_var, where='mid',
         label='Cumulative explained variance', color='red')

plt.ylabel('Explained variance percentage')
plt.xlabel('Principal component index')
plt.xticks(ticks=list(range(1, a)))
plt.legend(loc='best')
plt.tight_layout()

plt.savefig("cumulative explained variance plot.png")
```

![](img/77ddeacfd777ca9be48faf980e421099.png)

**累积解释方差图** (作者提供的图片)

因此，很明显前 7 个主成分捕获了数据中约 90%的方差。因此，我们可以选择前 7 个主成分用于葡萄酒数据集。

查看所有选择标准 这里。

# 使用特征分解进行主成分分析

主成分分析也可以通过对标准化数据的协方差矩阵应用特征分解来进行。

前 3 步与之前相同。因此，我将继续进行第四步。

**步骤 1、步骤 2、步骤 3：** 与之前相同。

**步骤 4：** 对协方差矩阵进行特征分解，并获得协方差矩阵的特征值。

```py
eigen_vals, eigen_vecs = np.linalg.eig(cov_mat)
print(eigen_vals)
```

![](img/7b2205e5e8489f299d4811f258afc84b.png)

**特征值**（作者提供的图片）

特征值与奇异值完全相同。原因在于协方差矩阵是对称的。

![](img/782200fd556cd02404edb3ea4da8d15d.png)

**标准化葡萄酒数据的协方差矩阵**（作者提供的图片）

一般来说，我们可以说：

> 对于对称矩阵，特征值与奇异值完全相同。

换句话说，

> 对于对称矩阵，奇异值分解和特征分解是相同的。

与**svd()**函数不同，**eig()**函数返回的特征值不是按降序排列的。因此，我们需要手动将它们从大到小排序。

```py
# Sort the eigenvalues in descending order
eigen_vals = np.sort(eigen_vals)[::-1]
print(eigen_vals)
```

![](img/88f8995a3da863e8c9203d03f6e4a1fd.png)

**按降序排列的特征值**（作者提供的图片）

**步骤 5、步骤 6：** 与之前相同。

我们可以像之前一样可视化特征值。你会得到完全相同的图。

# 结论

默认情况下，主成分分析是通过使用奇异值分解（SVD）进行的。也可以通过特征分解来进行。由于协方差矩阵是对称的，两种方法给出的结果相同。

> 一般来说，奇异值分解和特征分解是完全不同的，但对于像主成分分析中使用的协方差矩阵这样的对称矩阵，两者是相同的！

今天的文章到此结束。

**如果您有任何问题或反馈，请告诉我。**

## 阅读下一篇（推荐）

+   [**主成分分析与降维特别合集**](https://rukshanpramoditha.medium.com/list/pca-and-dimensionality-reduction-special-collection-146045a5acb5)

## 有兴趣了解 AI 课程吗？

+   [**神经网络与深度学习课程**](https://rukshanpramoditha.medium.com/list/neural-networks-and-deep-learning-course-a2779b9c3f75)

## 支持我作为写作者

*我希望你喜欢阅读这篇文章。如果你想支持我作为写作者，请考虑* [***注册会员***](https://rukshanpramoditha.medium.com/membership) *以获得对 Medium 的无限访问。每月只需 5 美元，我将获得你的会员费用的一部分。*

[](https://rukshanpramoditha.medium.com/membership?source=post_page-----fc0d9ac24a8e--------------------------------) [## 使用我的推荐链接加入 Medium - Rukshan Pramoditha

### 阅读 Rukshan Pramoditha 的每个故事（以及 Medium 上其他成千上万的作家的故事）。你的会员费直接…

rukshanpramoditha.medium.com](https://rukshanpramoditha.medium.com/membership?source=post_page-----fc0d9ac24a8e--------------------------------)

## 加入我的私人邮件列表

*不要再错过我讲的精彩故事了。通过* [***订阅我的邮件列表***](https://rukshanpramoditha.medium.com/subscribe)*，你将会在我发布故事后第一时间直接收到。*

非常感谢你的持续支持！下篇文章见。祝大家学习愉快！

## 葡萄酒数据集信息

+   **数据集来源：** 你可以在 [这里](https://archive.ics.uci.edu/ml/datasets/Wine) 下载原始数据集。

+   **数据集许可证：** 本数据集在 [*CC BY 4.0*](https://creativecommons.org/licenses/by/4.0/) (*创意共享署名 4.0*) 许可证下提供。

+   **引用：** Lichman, M. (2013). UCI 机器学习库 [[`archive.ics.uci.edu/ml`](https://archive.ics.uci.edu/ml)]。加利福尼亚州欧文：加利福尼亚大学信息与计算机科学学院。

[Rukshan Pramoditha](https://medium.com/u/f90a3bb1d400?source=post_page-----fc0d9ac24a8e--------------------------------)

**2023–03–20**
