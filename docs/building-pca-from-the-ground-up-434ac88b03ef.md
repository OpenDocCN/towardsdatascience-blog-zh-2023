# 从基础构建 PCA

> 原文：[`towardsdatascience.com/building-pca-from-the-ground-up-434ac88b03ef`](https://towardsdatascience.com/building-pca-from-the-ground-up-434ac88b03ef)

## 通过一步步的推导，超级提升你对主成分分析的理解

[](https://harrisonfhoffman.medium.com/?source=post_page-----434ac88b03ef--------------------------------)![哈里森·霍夫曼](https://harrisonfhoffman.medium.com/?source=post_page-----434ac88b03ef--------------------------------)[](https://towardsdatascience.com/?source=post_page-----434ac88b03ef--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----434ac88b03ef--------------------------------) [哈里森·霍夫曼](https://harrisonfhoffman.medium.com/?source=post_page-----434ac88b03ef--------------------------------)

·发表于[数据科学前沿](https://towardsdatascience.com/?source=post_page-----434ac88b03ef--------------------------------) ·12 分钟阅读·2023 年 8 月 7 日

--

![](img/09f3176416e889b15a24ea18524b299a.png)

热气球。图像由作者提供。

主成分分析（PCA）是一种常用于降维的[旧技术](https://en.wikipedia.org/wiki/Principal_component_analysis#History:~:text=PCA%20was%20invented%20in%201901)。尽管在数据科学家中这是一个广为人知的话题，但 PCA 的推导通常被忽视，留下了关于数据本质和微积分、统计学以及线性代数之间关系的宝贵见解。

在本文中，我们将通过思想实验推导 PCA，从二维开始，扩展到任意维度。随着我们逐步推进每一项推导，我们将看到看似不同的数学分支之间的和谐互动，最终达到优雅的坐标变换。这一推导将揭示 PCA 的机制，并揭示数学概念的迷人相互联系。让我们开始这段启发性的 PCA 探索之旅。

# 在二维中热身

作为生活在三维世界中的人类，我们通常理解二维概念，这也是本文的起点。从二维开始将简化我们的首次思想实验，并使我们更好地理解问题的本质。

## 理论

我们有一个数据集，长得像这样（请注意，每个特征应该被缩放到均值为 0 和方差为 1）：

![](img/364f66e80e7272963610ae918440bb91.png)

(1) 相关数据。图像由作者提供。

我们立即注意到这些数据位于由***x1***和***x2***描述的坐标系中，这些变量是相关的。*我们的目标是找到一个由数据的协方差结构决定的新坐标系。* 特别是，坐标系中的第一个基向量应解释将原始数据投影到其上的大部分方差。

我们的首要任务是找到一个向量，使得当我们将原始数据投影到这个向量上时，保留最大量的方差。换句话说，理想的向量指向方差最大化的方向，正如数据所定义的那样。

这个向量可以通过它与 x 轴逆时针方向所成的角度来定义：

![](img/3dfa6fba3e6d1cb3026078a984be9ac7.png)

(2) 通过旋转向量来寻找最大方差的方向。图像由作者提供。

在上面的动画中，我们通过与 x 轴的角度来定义一个向量，我们可以看到向量在 0 到 180 度之间的每个角度指向的方向。从视觉上看，我们可以看到一个接近 45 度的***θ***值指向数据方差的最大方向。

为了重新阐述我们的目标，我们希望找到一个角度，使得向量指向方差最大化的方向。从数学上讲，我们希望找到一个最大化这个方程的***θ***：

![](img/803d660009c0ee2e4cdb6c686edbedab.png)

(3) 最佳的θ将最大化这个目标函数。图像由作者提供。

在这里，***N***是数据中的观察次数。我们将每个观察值投影到由***[cos(θ) sin(θ)]***定义的轴上，这是一个由角度***θ***定义的单位向量，并对结果进行平方。

当我们改变***θ***时，这个方程给出了当数据投影到由***θ***定义的轴上的方差。让我们计算方程中的点积并重写这个表达式，使其更易于处理：

![](img/d922b2bb66f5a070baa481bae0416302.png)

(4) 重写后的方差方程。图像由作者提供。

幸运的是，这是一个凸函数，我们可以通过计算它的导数并将其设为 0 来最大化它。我们还需要计算方差方程的二阶导数，以确定我们是否找到了最小值或最大值。***var(θ)***的一阶和二阶导数如下所示：

![](img/64a2408a12fe159b516ab05bff0fd001.png)

(5) var(θ)的一阶和二阶导数。图像由作者提供。

接下来，我们可以将一阶导数设为 0，并重排方程以隔离***θ***：

![](img/28b55012957951891efdc1b5fd90a4dc.png)

(6) 将 var(θ)的一阶导数设为 0 并重排。图像由作者提供。

最后，利用常见的三角恒等式和一些代数，我们得到了一个使***θ***最小化或最大化***var(θ)***的封闭形式解：

![](img/9643601112402e5343b2cdaac7ca86b5.png)

(7) 计算θ的方程，用于找到最大或最小方差的方向。图像由作者提供。

值得注意的是，这个方程是我们在二维空间进行 PCA 所需的全部。二阶导数将告诉我们***θ***是否对应于局部最小值或最大值。由于只有另一个主成分，它必须通过将***θ***偏移 90 度来定义。因此，两个主成分角度是：

![](img/ad642bb49557fa023e74a1fe79fd6874.png)

(8) 定义二维主成分的角度。图像由作者提供。

如前所述，我们可以使用***var(θ)***的二阶导数来确定哪个***θ***属于主成分 1（最大方差的方向），哪个***θ***属于主成分 2（最小方差的方向）。另外，我们也可以将两个***θs***代入***var(θ)***中，看看哪个结果更高。

一旦我们知道哪个***θ***对应于每个主成分，我们就将每一个代入二维单位向量的三角函数方程（***[cos(θ) sin(θ)]***）。具体来说，我们做如下操作：

![](img/ca1eafaf1bf4f3dd1282647a2286fe02.png)

(9) 从最大化或最小化 var(θ) 的 θs 确定两个主成分。图像由作者提供。

就是这样——***pc1***和***pc2***是主成分向量。通过思考主成分分析的目标，我们能够在二维空间中从零开始推导出主成分。为了验证这个结果是否正确，让我们编写一些 Python 代码来实现我们的策略。

## 代码

第一个函数找到从方差方程的导数中推导出的主角度之一。由于这是一个封闭形式的方程，实现起来非常简单：

```py
import numpy as np

def find_principal_angle(x1: np.ndarray, x2: np.ndarray) -> float:
    """
    Find the angle corresponding to one of the principal components
    in two dimensions.

    Parameters
    ----------
    x1 : numpy.ndarray
        First input vector with shape (n,).
    x2 : numpy.ndarray
        Second input vector with shape (n,).

    Returns
    -------
    float
        The principal angle in radians.
    """

    cov = -2 * np.sum(x1 * x2)
    var_diff = np.sum(x2**2 - x1**2)

    return 0.5 * np.arctan(cov / var_diff)
```

根据方差方程的性质，`find_principal_angle()` 恢复一个主角度，另一个主角度则相差 90 度。为了确定 `find_principal_angle()` 返回的是哪个主角度，我们可以使用方差方程的二阶导数或 Hessian 矩阵：

```py
def compute_pca_cost_hessian(x1: np.ndarray,
                             x2: np.ndarray,
                             theta: float) -> float:
    """
    Compute the Hessian of the cost function for Principal Component
    Analysis (PCA) in two dimensions.

    Parameters
    ----------
    x1 : numpy.ndarray
        First input vector with shape (n,).
    x2 : numpy.ndarray
        Second input vector with shape (n,).
    theta : float
        An angle in radians for which the cost function is evaluated.

    Returns
    -------
    float
        The Hessian of the PCA cost function evaluated at the given theta.
    """

    return np.sum(
        2 * (x2**2 - x1**2) * np.cos(2 * theta) -
        4 * x1 * x2 * np.sin(2 * theta)
    )
```

这个函数的逻辑直接来源于图 (5)。我们需要的最后一个函数是从 `find_principal_angle()` 和 `compute_pca_cost_hessian()` 确定两个主成分：

```py
def find_principal_components_2d(x1: np.ndarray, x2: np.ndarray) -> tuple:
    """
    Find the principal components of a two-dimensional dataset.

    Parameters
    ----------
    x1 : numpy.ndarray
        First input vector with shape (n,).
    x2 : numpy.ndarray
        Second input vector with shape (n,).

    Returns
    -------
    tuple
        A tuple containing the two principal components represented as
        numpy arrays.
    """

    theta0 = find_principal_angle(x1, x2)
    theta0_hessian = compute_pca_cost_hessian(x1, x2, theta0)

    if theta0_hessian > 0:
        pc1 = np.array([np.cos(theta0 + (np.pi / 2)),
                        np.sin(theta0 + (np.pi / 2))])
        pc2 = np.array([np.cos(theta0), np.sin(theta0)])
    else:
        pc1 = np.array([np.cos(theta0), np.sin(theta0)])
        pc2 = np.array([np.cos(theta0 + (np.pi / 2)),
              np.sin(theta0 + (np.pi / 2))])

    return pc1, pc2
```

在 `find_principal_components_2d()` 中，`theta0` 是其中一个主角度，`theta0_hessian` 是方差方程的二阶导数。因为 `theta0` 是方差方程的极值点，`theta0_hessian` 告诉我们 `theta0` 是最小值还是最大值。特别地，如果 `theta0_hessian` 为正，则 `theta0` 必定是最小值，对应于第二主成分。否则，如果 `theta0_hessian` 为负，`theta0` 则是最大值，对应于第一主成分。

为了验证`find_principal_components_2d()`是否做了我们想要的事情，让我们找到一个二维数据集的主成分，并将其与[scikit-learn 中的 PCA 实现](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html)的结果进行比较。以下是代码：

```py
import numpy as np
from sklearn.decomposition import PCA

# Generate two random correlated arrays
rng = np.random.default_rng(seed=80)
x1 = rng.normal(size=1000)
x2 = x1 + rng.normal(size=len(x1))

# Normalize the data
x1 = (x1 - x1.mean()) / x1.std()
x2 = (x2 - x2.mean()) / x2.std()

# Find the principal components using the 2D logic
pc1, pc2 = find_principal_components_2d(x1, x2)

# Find the principal components using sklearn PCA
model = PCA(n_components=2)
model.fit(np.array([x1, x2]).T)
pc1_sklearn = model.components_[0, :]
pc2_sklearn = model.components_[1, :]

print(f"Derived PC1: {pc1}")
print(f"Sklearn PC1: {pc1_sklearn} \n")
print(f"Derived PC2: {pc2}")
print(f"Sklearn PC2: {pc2_sklearn}")

"""   
Derived PC1: [0.70710678 0.70710678]
Sklearn PC1: [0.70710678 0.70710678] 

Derived PC2: [ 0.70710678 -0.70710678]
Sklearn PC2: [ 0.70710678 -0.70710678]
"""

# Visualize the results
fig, ax = plt.subplots(figsize=(10, 6))
ax.scatter(x1, x2, alpha=0.5)
ax.quiver(0, 0, pc1[0], pc1[1],  scale_units='xy', scale=1, color='red')
ax.quiver(0, 0, pc2[0], pc2[1],  scale_units='xy', scale=1, color='red', label="Custom PCs")
ax.quiver(0, 0, pc1_sklearn[0], pc1_sklearn[1],  scale_units='xy', scale=1, color='green')
ax.quiver(0, 0, pc2_sklearn[0], pc2_sklearn[1],  scale_units='xy', scale=1, color='green', label="Sklearn PCs")
ax.set_xlabel("$x_1$")
ax.set_ylabel("$x_2$")
ax.set_title("Custom 2D PCA vs Sklearn PCA")
ax.legend()
```

这段代码创建了二维相关数据，进行了标准化，并使用我们的自定义逻辑和[scikit-learn](https://scikit-learn.org/stable/) PCA 实现找到了主成分。我们可以看到，我们得到了与 scikit-learn 完全相同的结果。得到的主成分如下：

![](img/22c10018692acb9165469ef7a78b0667.png)

(10) 我们的二维 PCA 方法与 scikit-learn 实现完全匹配。图像来源：作者。

如预期的那样，两组主成分完全重叠。然而，我们方法的问题是它无法推广到二维以外的情况。在下一节中，我们将推导出任意维度的类似结果。

# 推导一般结果

在二维中，我们利用方差定义、微积分和一点三角学推导出了一个封闭形式的方程来找到主成分。不幸的是，这种方法对于超过二维的数据集很快会变得不可行。为此，我们必须依赖于数学中最强大的分支——线性代数来进行计算。

## 理论

PCA 在更高维度的推导在许多方面类似于二维情况。根本的区别在于，我们必须利用一些线性代数工具来帮助我们考虑主成分与数据原点可能形成的所有角度。

首先，假设我们的数据集有***d***个特征和***n***个观察值。如前所述，每个特征应缩放到均值为 0 且方差为 1。我们将数据排列成如下矩阵：

![](img/2d635700dde60aea98bdce43f6c04407.png)

(11) PCA 的数据矩阵。图像来源：作者。

如前所述，我们的目标是找到一个单位向量，***p***，它指向数据方差最大方向。为此，我们首先需要陈述两个有用的事实。第一个是我们数据的[协方差矩阵](https://en.wikipedia.org/wiki/Covariance_matrix)，在更高维度中的方差的类似物，可以通过将 X 与其转置相乘找到：

![](img/be86b90f50486e6f8c0081705f66ba3a.png)

(12) 计算 X 的协方差矩阵的方程。图像来源：作者。

尽管这很有用，我们并不关心数据本身的方差。**我们想知道数据在投影到新轴上的方差。** 为此，我们回顾一个关于标量量的方差的结果。即，当标量乘以常数时，得到的方差是该常数的平方乘以原始方差：

![](img/9e81fad8387521e3c4a282cc49468c30.png)

(13) 乘以常数的标量方差。图片由作者提供。

类似地，当我们用数据矩阵乘以一个向量（即，当我们将一个向量投影到矩阵上）时，得到的方差可以按如下方式计算：

![](img/f88925c03278849f16cf1af146fa5b0e.png)

(14) 向量投影到数据矩阵上的方差。图片由作者提供。

这给了我们定义 PCA 问题所需的一切。也就是说，我们想找到向量***p***（主成分），使下列方程最大化：

![](img/b68008d831be70a929522701e5ce312e.png)

(15) 在**d**维度下，PCA 问题的陈述是找到向量**p**，使其在投影到数据时的方差最大。图片由作者提供。

在 ***d*** 维度下，PCA 问题的陈述是找到向量***p***，使其在投影到数据时的方差最大。我们还规定***p*** 是一个单位向量，这使得这是一个约束优化问题。因此，就像在二维情况下一样，我们需要使用微积分。

为了解决这个问题，我们可以利用 [拉格朗日乘子](https://en.wikipedia.org/wiki/Lagrange_multiplier)，它允许我们在满足指定约束的同时最小化目标函数。这个问题的拉格朗日表达式如下：

![](img/5f51d128d65caaed839e0a5674ac90d2.png)

(16) PCA 拉格朗日表达式。图片由作者提供。

为了优化这个问题，我们对其求导并将其设为 0：

![](img/48c13934f459ab096b90863c3e67ce64.png)

(17) PCA 拉格朗日表达式的导数。图片由作者提供。

一些重新排列给出了以下表达式——这是线性代数中一个非常熟悉的结果：

![](img/2b1ab5575ce78f5e2cdd9af1d9b772c1.png)

(18) PCA 特征方程。图片由作者提供。

这告诉我们什么？令人惊讶的是，最佳主成分 (***p***) 乘以数据的协方差矩阵 (***S***)，实际上就是那个相同的 ***p*** 乘以一个标量 (***λ***)。换句话说，***p*** 必须是协方差矩阵的** [**特征向量**](https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors)**，我们可以通过计算 ***S*** 的特征值分解来找到 ***p***。我们还观察到：

![](img/a4dbf6605177b0bd6d092dedf00490c8.png)

当数据投影到**p**上时，数据的方差等于**p**的特征值。图片由作者提供。

当数据投影到***p***上时，数据的方差等于对应于***p.***的特征值。我们还知道，***S*** 最多有 ***d*** 个特征值/特征向量，特征向量是正交的，特征值从大到小排序。这意味着，通过对 ***S*** 进行特征值分解，我们可以恢复所有的主成分：

![](img/b62129059503c568dfef653e25879cfc.png)

(20) 所有的 **d** 个主成分都来自于 **S** 的特征值分解。图片由作者提供。

这是一种统计学、微积分和线性代数的美丽交汇，给我们提供了一种优雅的方法来找到高维数据集的主成分。真是一个美丽的结果！

## 代码

如果我们使用 NumPy，PCA 的一般形式实际上比我们推导出的二维版本更容易编码。以下是使用三维数据集的 PCA 示例：

```py
import numpy as np
from sklearn.decomposition import PCA

# Generate three random correlated arrays
rng = np.random.default_rng(seed=3)
x1 = rng.normal(size=1000)
x2 = x1 + rng.normal(size=len(x1))
x3 = x1 + x2 + rng.normal(size=len(x1))

# Create the data matrix (X)
X = np.array([x1, x2, x3]).T

# Normalize X
X = (X - X.mean(axis=0)) / X.std(axis=0)

# Compute the covariance matrix of X (S)
S = np.cov(X.T)

# Compute the eigenvalue decomposition of S
variances, pcs_derived = np.linalg.eig(S)

# Sort the pcs according to their variance
sorted_idx = np.argsort(variances)[::-1]
pcs_derived = pcs_derived.T[sorted_idx, :]

# Find the pcs using sklearn PCA
model = PCA(n_components=3)
model.fit(X)
pcs_sklearn = model.components_

# Compute the element-wise difference between the pcs
pc_diff = np.round(pcs_derived - pcs_sklearn, 10)

print("Derived Principal Components: \n", pcs_derived)
print("Sklearn Principal Components: \n", pcs_derived)
print("Difference: \n", pc_diff)

"""
Derived Principal Components: 
 [[-0.56530668 -0.57124206 -0.59507215]
 [-0.74248305  0.66666719  0.06537407]
 [-0.35937066 -0.47878738  0.80100897]]
Sklearn Principal Components: 
 [[-0.56530668 -0.57124206 -0.59507215]
 [-0.74248305  0.66666719  0.06537407]
 [-0.35937066 -0.47878738  0.80100897]]
Difference: 
 [[ 0\.  0\.  0.]
 [ 0\.  0\. -0.]
 [ 0\.  0\.  0.]]
""" 
```

正如我们所见，scikit-learn 的 PCA 实现给出的主成分正是协方差矩阵的特征向量。

# 最后的思考

主成分分析（PCA）是一种强大的技术，广泛应用于数据科学和机器学习中，用于减少高维数据集的维度，同时保留最重要的信息。本文探讨了 PCA 的基础原理，首先在二维情况下进行了分析，并使用线性代数将其扩展到任意维度。

在二维情况下，我们通过最大化数据投影到新轴上的方差，推导出了一个闭式解来找到主成分。我们使用了三角学和微积分来确定定义主成分的角度。随后，我们在 Python 中实现了我们的策略，并通过将结果与 scikit-learn 的 PCA 实现进行比较来验证其正确性。

在扩展到任意维度时，我们利用线性代数找到了一种通用的解决方案。通过将 PCA 表达为特征值问题，我们展示了主成分是协方差矩阵的特征向量。相应的特征值表示数据在投影到每个主成分上的方差。

正如我们在数学中常常看到的，那些看似无关的概念，经过不同数学家在不同时间段的发展，最终会汇聚成一个美丽的结果。PCA 是这种复杂交汇的一个典型例子，还有许多类似的例子值得探索！

*成为会员:* [*https://harrisonfhoffman.medium.com/membership*](https://harrisonfhoffman.medium.com/membership)

*请给我买杯咖啡:* [*https://www.buymeacoffee.com/HarrisonfhU*](https://www.buymeacoffee.com/HarrisonfhU)

# 参考文献

1.  *Ali Ghodsi, Lec 1: 主成分分析 —* [`www.youtube.com/watch?v=L-pQtGm3VS8&t=3129s`](https://www.youtube.com/watch?v=L-pQtGm3VS8&t=3129s)

1.  *独立成分分析 2* — [`www.youtube.com/watch?v=olKgmOuAvrc`](https://www.youtube.com/watch?v=olKgmOuAvrc)
