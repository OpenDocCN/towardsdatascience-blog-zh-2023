# 线性代数：使用 Python 查找逆矩阵

> 原文：[`towardsdatascience.com/linear-algebra-finding-inverse-matrix-with-python-18dd988f4df?source=collection_archive---------13-----------------------#2023-01-19`](https://towardsdatascience.com/linear-algebra-finding-inverse-matrix-with-python-18dd988f4df?source=collection_archive---------13-----------------------#2023-01-19)

## 第三部分：使用初等行变换和矩阵行列式计算逆矩阵的全面分步指南

[](https://chaodeyu.medium.com/?source=post_page-----18dd988f4df--------------------------------)![Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----18dd988f4df--------------------------------)[](https://towardsdatascience.com/?source=post_page-----18dd988f4df--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----18dd988f4df--------------------------------) [Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----18dd988f4df--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5b7be08f8f4c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-algebra-finding-inverse-matrix-with-python-18dd988f4df&user=Chao+De-Yu&userId=5b7be08f8f4c&source=post_page-5b7be08f8f4c----18dd988f4df---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----18dd988f4df--------------------------------) ·5 分钟阅读·2023 年 1 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F18dd988f4df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-algebra-finding-inverse-matrix-with-python-18dd988f4df&user=Chao+De-Yu&userId=5b7be08f8f4c&source=-----18dd988f4df---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F18dd988f4df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-algebra-finding-inverse-matrix-with-python-18dd988f4df&source=-----18dd988f4df---------------------bookmark_footer-----------)![](img/bdc89b0ab6a2f627646ef764c26d4202.png)

[摄影师 Raimond Klavins](https://unsplash.com/@raimondklavins) 拍摄于 [Unsplash](https://unsplash.com)

[上一篇文章](https://medium.com/towards-data-science/linear-algebra-matrix-operations-and-their-properties-with-python-a0885a159be1) 解释了不同的矩阵运算及其对应的操作。本文将深入探讨两种不同的方法来计算逆矩阵：使用初等行变换和使用矩阵的行列式。

# 逆矩阵

**逆矩阵**类似于**数字的倒数**，使得**一个数字与其倒数的乘积**为**1**，以及**一个矩阵与其逆矩阵的乘积**为**单位矩阵**。然而，要找到矩阵的逆矩阵，矩阵必须是行数和列数相等的方阵。找到矩阵逆矩阵的主要方法有两种：

# 方法 1：使用初等行变换

回忆一下用于解线性系统的[3 种行变换](https://medium.com/towards-data-science/linear-algebra-systems-of-linear-equations-and-matrices-with-python-d3e0fcb29e85)：交换、缩放和主元化。这些操作可以写成**初等矩阵**。而左乘增广线性系统矩阵表示**初等行变换**。
