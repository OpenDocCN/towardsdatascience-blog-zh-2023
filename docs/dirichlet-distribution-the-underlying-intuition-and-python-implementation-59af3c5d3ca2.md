# Dirichlet 分布：基本直觉与 Python 实现

> 原文：[https://towardsdatascience.com/dirichlet-distribution-the-underlying-intuition-and-python-implementation-59af3c5d3ca2?source=collection_archive---------6-----------------------#2023-08-01](https://towardsdatascience.com/dirichlet-distribution-the-underlying-intuition-and-python-implementation-59af3c5d3ca2?source=collection_archive---------6-----------------------#2023-08-01)

## 你需要了解的关于 Dirichlet 分布的一切

[](https://reza-bagheri79.medium.com/?source=post_page-----59af3c5d3ca2--------------------------------)[![Reza Bagheri](../Images/7c5a7dc9e6e31048ce31c8d49055987c.png)](https://reza-bagheri79.medium.com/?source=post_page-----59af3c5d3ca2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----59af3c5d3ca2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----59af3c5d3ca2--------------------------------) [Reza Bagheri](https://reza-bagheri79.medium.com/?source=post_page-----59af3c5d3ca2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fda2d000eaa4d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdirichlet-distribution-the-underlying-intuition-and-python-implementation-59af3c5d3ca2&user=Reza+Bagheri&userId=da2d000eaa4d&source=post_page-da2d000eaa4d----59af3c5d3ca2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----59af3c5d3ca2--------------------------------) · 27 min read · 2023年8月1日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F59af3c5d3ca2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdirichlet-distribution-the-underlying-intuition-and-python-implementation-59af3c5d3ca2&user=Reza+Bagheri&userId=da2d000eaa4d&source=-----59af3c5d3ca2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F59af3c5d3ca2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdirichlet-distribution-the-underlying-intuition-and-python-implementation-59af3c5d3ca2&source=-----59af3c5d3ca2---------------------bookmark_footer-----------)![](../Images/d401dd1e32b5386d115f5f45826440fe.png)

图片来源: [https://pixabay.com/vectors/cubes-dice-platonic-solids-numbers-160400/](https://pixabay.com/vectors/cubes-dice-platonic-solids-numbers-160400/)

Dirichlet 分布是 beta 分布的推广。在贝叶斯统计中，它通常用作多项式分布的共轭先验，因此可以用来建模概率随机向量的不确定性。它在贝叶斯分析、文本挖掘、统计遗传学和非参数推断等领域有广泛应用。本文对 Dirichlet 分布进行了直观的介绍，并展示了它与多项式分布的联系。此外，文章还展示了如何在 Python 中对其进行建模和可视化。

**定义**

设连续随机变量 *X*₁, *X*₂, …*Xₖ* (*k*≥2) 构成随机向量 ***X***，定义如下：

我们还定义向量 ***α*** 为：

其中

现在，若随机向量 ***X*** 具有参数 ***α*** 的 *Dirichlet 分布*，则其联合 PDF 如下：

![](../Images/ebb95005954b232af99dee7d20387b43.png)
