# 线性代数入门：第二部分

> 原文：[https://towardsdatascience.com/a-primer-on-linear-algebra-part-2-eba53c564a90?source=collection_archive---------20-----------------------#2023-04-25](https://towardsdatascience.com/a-primer-on-linear-algebra-part-2-eba53c564a90?source=collection_archive---------20-----------------------#2023-04-25)

## 数据科学必备的基本概念和操作的温故知新

[](https://medium.com/@dataforyou?source=post_page-----eba53c564a90--------------------------------)[![Rob Taylor, PhD](../Images/5e4e86da7b77404ed42d00a60ea5eacf.png)](https://medium.com/@dataforyou?source=post_page-----eba53c564a90--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eba53c564a90--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----eba53c564a90--------------------------------) [Rob Taylor, PhD](https://medium.com/@dataforyou?source=post_page-----eba53c564a90--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F98de080592fc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-primer-on-linear-algebra-part-2-eba53c564a90&user=Rob+Taylor%2C+PhD&userId=98de080592fc&source=post_page-98de080592fc----eba53c564a90---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eba53c564a90--------------------------------) · 6 分钟阅读 · 2023年4月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feba53c564a90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-primer-on-linear-algebra-part-2-eba53c564a90&user=Rob+Taylor%2C+PhD&userId=98de080592fc&source=-----eba53c564a90---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feba53c564a90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-primer-on-linear-algebra-part-2-eba53c564a90&source=-----eba53c564a90---------------------bookmark_footer-----------)![](../Images/7d24980120b3e252016f552354d93c84.png)

图片由[Viktor Forgacs](https://unsplash.com/@sonance?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)提供

## 介绍

在我之前的[文章](https://medium.com/@dataforyou/a-primer-on-linear-algebra-414111d195ca)中，我介绍了一些线性代数的基本操作和概念。这包括向量和矩阵，以及转置、点积和矩阵乘法运算符。在这篇文章中，我将介绍一些补充之前讨论过的概念。如果你还没有阅读我的线性代数入门文章，可以在[这里](https://medium.com/@dataforyou/a-primer-on-linear-algebra-414111d195ca)查看。

## 线性独立性

在定义线性*独立性*之前，我们首先需要定义线性*依赖性*。简单来说，向量序列是线性依赖的，如果至少有一个可以表示为其他向量的线性组合。具体来说，假设我们有一个由 *n* 个向量 **v**₁、**v**₂、⋯、**v**ₙ 组成的矩阵 *V* 的列。线性依赖成立当且仅当存在 *n* 个标量 *a₁*、*a₂*、⋯、*aₙ*，使得：

![](../Images/b4ef04b634d5350653f4bf61f0660d33.png)

线性依赖性的条件（作者提供的图片）。

其中 **0** 表示*零向量*，且至少有一个 *aᵢ* *不等于零*。

这个要求很重要，因为没有它，你可以将所有 *a* 设置为零并得到结果*。* 那么线性独立性的定义就是相反的情况；即，向量序列*不是*线性依赖的情况。这意味着以下条件成立：

![](../Images/57dadb68457242cc0437ce62adda4535.png)

线性独立性的条件（作者提供的图片）。

因此需要*所有*标量都为零。在这些条件下，序列中的任何向量都不能表示为其他剩余向量的线性组合。

例如，假设我们有两个向量**v**₁和**v**₂，每个向量都是ℝ²。为了保持线性独立性，我们需要一组系数，使得：

![](../Images/4bcec5c3903aff6ba2b2de02919d381c.png)

展示2 x 2矩阵线性独立性的示例（作者提供的图片）。

其中 *a*₁ 和 *a*₂ 都等于零。

## 行列式

*行列式* 是一个标量值，它是方阵中元素的函数。如果矩阵的维度较小，手动计算行列式相对简单。例如，设 *A* 为一个2 *×* 2的矩阵；在这种情况下，行列式就是：

![](../Images/615eacf5bc7e5102bd3d0b039127feea.png)

2 x 2矩阵的行列式（作者提供的图片）。

我们还可以计算一个 3 *× 3* 矩阵的行列式，虽然这个过程稍微复杂一些。我不会在这里详细说明，但这个情况的解是：

![](../Images/cf7ade7b78e77aa08ab9eff957b29973.png)

3 x 3矩阵的行列式（作者提供的图片）。

这个解被称为*莱布尼茨公式*，并且可以推广到更高维度。同样，我不会在这里深入细节，但会提供通用公式：

![](../Images/5e91cda35e9b73094c6c5ed7e8d8a2ef.png)

3 x 3矩阵的行列式（作者提供的图片）。

其中*sgn*是群 *Sₙ* 中排列的符号函数，*σ*表示一个重新排序——或*排列*——整数集合的函数。

虽然行列式的公式并不是特别直观，但它提供的信息却是*直观*的。行列式本质上是几何的，告诉我们图像在变换下如何变化。再次考虑一个简单的2 *×* 2矩阵，行列式实际上是平行四边形的面积，这个平行四边形代表了单位正方形在矩阵给定的变换下的图像。

这同样适用于更高维度，虽然此时行列式对应的是体积，而不是面积。例如，一个3 *× 3*矩阵的行列式是一个平行六面体的体积，而任何*n* *× n*矩阵的行列式是一个*n*维平行六面体的超体积。

## 秩

从定义上讲，矩阵的*秩*决定了*线性独立*列的最大数量；虽然更正式地，它对应于由其列张成的向量空间的维度。通常，我们希望矩阵具有*满秩*，因为这一条件意味着列向量之间没有冗余。任何存在列间线性相关的矩阵都不会具有满秩，被称为*秩亏损*。

举个例子，考虑一个方阵*n* *×* *n*的矩阵*A*。如果这个矩阵中的所有列都是线性独立的，那么这个矩阵被称为具有*满列秩*，它将等于*n*。由于矩阵是方阵，我们也可以考虑其行是否线性独立。如果是，那么矩阵也具有*满行秩*，这也将等于*n*。因为这些是等价的，一个方阵被认为具有满秩，如果所有的行和列都是线性独立的，这表示rank(*A*) = *n*。

实际上，对于方阵，只有当其行列式非零时，满秩才是可能的。因此，我们实际上可以使用行列式来测试方阵中的线性独立性。

但，如果矩阵不是方阵怎么办？在这种情况下，满秩的定义略有不同。假设我们有一个非方阵*B*，它有*m*行和*n*列，那么满秩定义为给定矩阵形状下可能的最高行或列秩。违反直觉的是，这将等于较*小*的维度。

例如，如果*B*的行数相对于列数更多（即，*m* > *n*），则满秩要求*B*具有满列秩，因此rank(*B*) = *n*。相反，如果行数少于列数（即，*m < n*），则*B*必须具有满行秩，因此rank(*B*) = *m*。这是因为如果矩阵不是方阵，那么它的行或列必定线性相关。

## 矩阵求逆

从定义上讲，一个*n* × *n*的方阵*A* 被认为是*可逆*的，如果存在另一个*n* × *n*的方阵*B*，使得以下条件成立：

![](../Images/6a845c04b13e0f9540be32cbc3ce0519.png)

矩阵可逆性的条件（作者提供的图像）。

这表明，如果矩阵*A*和*B*的矩阵乘积是*单位矩阵*，那么矩阵的可逆性成立。如果这确实正确，那么*B*由*A*唯一确定，我们称矩阵*B*是*A*的*乘法逆矩阵*，记作*A*⁻¹。*矩阵求逆*的任务是尝试找到一个满足可逆性条件的矩阵*B*。不过，我这里不会深入讨论矩阵求逆中使用的数值方法。

请注意，只有在矩阵具有*满秩*的情况下，矩阵才可以被求逆，这意味着*A*的列是线性独立的。任何不能被求逆的矩阵都被称为*退化*或*奇异*。

## 最后的备注

本文轻描淡写地介绍了线性代数中的一些基本概念。像任何话题一样，你可以深入探讨细节，因此这篇文章并不是完全详尽的，只是触及了表面。不过，本文讨论的概念在构建数学模型时至关重要，因此数据科学家需要了解。在后续文章中，我们将看到这些概念以及我早期入门文章中介绍的那些概念是如何应用于线性回归模型的。敬请期待！

## 相关文章

+   [线性代数入门](https://medium.com/@dataforyou/a-primer-on-linear-algebra-414111d195ca)

感谢阅读！

如果你喜欢这篇文章并希望保持更新，请考虑 [在 Medium 上关注我](https://medium.com/@dataforyou)。这将确保你不会错过任何新内容。

若想无限访问所有内容，请考虑注册 [Medium 订阅](https://medium.com/membership)。

你也可以在 [Twitter](https://twitter.com/dataforyounz)、[LinkedIn](https://www.linkedin.com/in/dataforyou/) 上关注我，或者查看我的 [GitHub](https://github.com/dataforyounz)，如果你更喜欢这个😉
