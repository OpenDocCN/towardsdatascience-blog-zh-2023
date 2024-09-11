# 实验分析中的表格演算

> 原文：[https://towardsdatascience.com/a-tableau-calculus-for-the-analysis-of-experiments-53655dd04eb8?source=collection_archive---------10-----------------------#2023-10-03](https://towardsdatascience.com/a-tableau-calculus-for-the-analysis-of-experiments-53655dd04eb8?source=collection_archive---------10-----------------------#2023-10-03)

## 揭示实验分析的基本数据结构

[](https://dataneversleeps.medium.com/?source=post_page-----53655dd04eb8--------------------------------)[![Jared M. Maruskin, PhD](../Images/771dcac046565d4760077afecb3fadef.png)](https://dataneversleeps.medium.com/?source=post_page-----53655dd04eb8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----53655dd04eb8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----53655dd04eb8--------------------------------) [Jared M. Maruskin, PhD](https://dataneversleeps.medium.com/?source=post_page-----53655dd04eb8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F37ef2450ad04&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-tableau-calculus-for-the-analysis-of-experiments-53655dd04eb8&user=Jared+M.+Maruskin%2C+PhD&userId=37ef2450ad04&source=post_page-37ef2450ad04----53655dd04eb8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----53655dd04eb8--------------------------------) · 21分钟阅读 · 2023年10月3日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F53655dd04eb8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-tableau-calculus-for-the-analysis-of-experiments-53655dd04eb8&user=Jared+M.+Maruskin%2C+PhD&userId=37ef2450ad04&source=-----53655dd04eb8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F53655dd04eb8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-tableau-calculus-for-the-analysis-of-experiments-53655dd04eb8&source=-----53655dd04eb8---------------------bookmark_footer-----------)![](../Images/b26caa5a37392803d3b6c958678d500a.png)

摄影： [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 通过 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

实验分析通常涉及分析包含不同数量元素的组；例如，每个层次内每个处理分配的单元数量不同。因此，我们会遇到*类似矩阵*的对象，只是它们不是完美的矩形块；也就是说，它们并不总是“填满”的。

在这份笔记中，我们定义了一种新的结构，称为*表格（tableau）*，它可以看作是一个部分填充的矩阵，并力图规范化在实验分析中使用的表格操作。然后，我们展示了*表格符号*如何用于在各种统计背景下表达关键方程，包括分层、聚类和平方和分解。此外，我们将这些方程表示为*不变量*和*指标*形式：

1.  *不变量符号（无坐标形式）* — 以*对象*和*算子*来定义，就像矩阵-向量乘积A⋅x一样，

1.  *指标符号（坐标形式）* — 明确地定义为索引数组和多个索引的求和，就像将矩阵-向量乘积表示为*∑ⱼAᵢⱼ xⱼ*一样。

## 大纲
