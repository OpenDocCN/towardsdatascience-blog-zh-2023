# 线性代数：线性方程组和矩阵，使用 Python

> 原文：[`towardsdatascience.com/linear-algebra-systems-of-linear-equations-and-matrices-with-python-d3e0fcb29e85?source=collection_archive---------24-----------------------#2023-01-10`](https://towardsdatascience.com/linear-algebra-systems-of-linear-equations-and-matrices-with-python-d3e0fcb29e85?source=collection_archive---------24-----------------------#2023-01-10)

## 第一部分：解释线性代数的基础：线性方程组和矩阵

[](https://chaodeyu.medium.com/?source=post_page-----d3e0fcb29e85--------------------------------)![Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----d3e0fcb29e85--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d3e0fcb29e85--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d3e0fcb29e85--------------------------------) [Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----d3e0fcb29e85--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5b7be08f8f4c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-algebra-systems-of-linear-equations-and-matrices-with-python-d3e0fcb29e85&user=Chao+De-Yu&userId=5b7be08f8f4c&source=post_page-5b7be08f8f4c----d3e0fcb29e85---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d3e0fcb29e85--------------------------------) ·5 分钟阅读·2023 年 1 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd3e0fcb29e85&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-algebra-systems-of-linear-equations-and-matrices-with-python-d3e0fcb29e85&user=Chao+De-Yu&userId=5b7be08f8f4c&source=-----d3e0fcb29e85---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd3e0fcb29e85&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-algebra-systems-of-linear-equations-and-matrices-with-python-d3e0fcb29e85&source=-----d3e0fcb29e85---------------------bookmark_footer-----------)![](img/e8c03e80317077b44cce37272b8f5b53.png)

图片由 [Sergio Rota](https://unsplash.com/@sergio_rota) 提供，来源于 [Unsplash](https://unsplash.com)

# 介绍

线性代数在科学和工程的多个领域中至关重要。它几乎贯穿所有数学领域。例如，方程通常用于建模现实问题，多变量问题可以通过方程组建模。甚至非线性方程组也可以转化为线性方程组。因此，了解如何解这些线性方程组在现代科学的每个领域中都非常有用。作为数据科学家，深入了解线性代数也是必不可少的，以便处理大多数机器学习方法，特别是深度学习算法。

## 线性方程

+   线性方程是一个方程，其中变量的最高次幂始终为 1。

![](img/6af038eb13721273ae6a6fbe3d4fb35e.png)

图 1\. 线性方程。图片作者。

## 线性系统

+   **线性方程组**是一系列线性方程。
