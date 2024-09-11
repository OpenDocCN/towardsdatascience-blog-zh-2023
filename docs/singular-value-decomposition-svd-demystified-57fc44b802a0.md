# 奇异值分解（SVD），揭开神秘面纱

> 原文：[`towardsdatascience.com/singular-value-decomposition-svd-demystified-57fc44b802a0?source=collection_archive---------4-----------------------#2023-11-08`](https://towardsdatascience.com/singular-value-decomposition-svd-demystified-57fc44b802a0?source=collection_archive---------4-----------------------#2023-11-08)

## 使用 Python 的 SVD 全面指南

[](https://medium.com/@roiyeho?source=post_page-----57fc44b802a0--------------------------------)![Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----57fc44b802a0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----57fc44b802a0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----57fc44b802a0--------------------------------) [Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----57fc44b802a0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3886620c5cf9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsingular-value-decomposition-svd-demystified-57fc44b802a0&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=post_page-3886620c5cf9----57fc44b802a0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----57fc44b802a0--------------------------------) · 19 分钟阅读 · 2023 年 11 月 8 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F57fc44b802a0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsingular-value-decomposition-svd-demystified-57fc44b802a0&source=-----57fc44b802a0---------------------bookmark_footer-----------)

奇异值分解（SVD）是一种强大的矩阵分解技术，它将一个矩阵分解为三个其他矩阵，揭示了原始矩阵的重要结构特征。它被广泛应用于信号处理、图像压缩和机器学习中的降维等领域。

本文提供了逐步指南，讲解如何计算矩阵的奇异值分解（SVD），包括详细的数值示例。接着，文章演示了如何使用 SVD 进行降维，并提供了 Python 中的示例。最后，文章讨论了 SVD 的各种应用及其一些局限性。

文章假设读者具备基础的线性代数知识。更具体地说，读者应对向量和矩阵范数、矩阵的秩、特征分解（特征向量和特征值）、正交归一向量和线性投影等概念有所了解。

![](img/502e38be1631c3d04de689deedbe2734.png)

图片由 [Peggy und Marco Lachmann-Anke](https://pixabay.com/users/peggy_marco-1553824/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1027571) 提供，来源于 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1027571)

# 数学定义

*m* × *n* 实矩阵 *A* 的奇异值分解是一种形式为 *A* = *U*Σ*Vᵗ* 的分解，其中：

+   *U* 是一个 *m* × *m* **正交矩阵**（即其列和行是正交归一的向量）。*U* 的列是…
