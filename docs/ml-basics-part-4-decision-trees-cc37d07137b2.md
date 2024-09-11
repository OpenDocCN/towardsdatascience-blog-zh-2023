# 机器学习基础（第4部分）：决策树

> 原文：[https://towardsdatascience.com/ml-basics-part-4-decision-trees-cc37d07137b2?source=collection_archive---------24-----------------------#2023-01-04](https://towardsdatascience.com/ml-basics-part-4-decision-trees-cc37d07137b2?source=collection_archive---------24-----------------------#2023-01-04)

## **决策树是什么，如何构建和应用决策树于不同的分类任务**

[](https://azad-wolf.medium.com/?source=post_page-----cc37d07137b2--------------------------------)[![J. Rafid Siddiqui, PhD](../Images/02280890ed87239c75cbcbfa7c5d686c.png)](https://azad-wolf.medium.com/?source=post_page-----cc37d07137b2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cc37d07137b2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cc37d07137b2--------------------------------) [J. Rafid Siddiqui, PhD](https://azad-wolf.medium.com/?source=post_page-----cc37d07137b2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Feb523dd294ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fml-basics-part-4-decision-trees-cc37d07137b2&user=J.+Rafid+Siddiqui%2C+PhD&userId=eb523dd294ec&source=post_page-eb523dd294ec----cc37d07137b2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cc37d07137b2--------------------------------) ·8分钟阅读·2023年1月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcc37d07137b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fml-basics-part-4-decision-trees-cc37d07137b2&user=J.+Rafid+Siddiqui%2C+PhD&userId=eb523dd294ec&source=-----cc37d07137b2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcc37d07137b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fml-basics-part-4-decision-trees-cc37d07137b2&source=-----cc37d07137b2---------------------bookmark_footer-----------)![](../Images/854204edb68a795f1f9cc7135d68d19a.png)

图1：决策树分类器（来源：作者）

在之前的文章中，我们探讨了[*回归*](https://azad-wolf.medium.com/ml-basics-part-1-regression-a-gateway-method-to-machine-learning-36d54d233907)、[*支持向量机*](https://azad-wolf.medium.com/ml-basics-part-2-support-vector-machines-ac4defba2615)*和* [*人工神经网络*](https://azad-wolf.medium.com/ml-basics-part-3-artificial-neural-networks-879851bcd217)*。在本文中，我们将深入了解另一种机器学习概念，即*决策树*。你可以从上述链接中查看其他方法。

**介绍**

决策树可能是机器学习工具箱中最简单且最直观的分类方法之一。决策树首次出现在1959年*威廉·贝尔森*的出版物中。早期的决策树应用主要限于分类法，因为它们与这种数据类型自然契合。之后的发展产生了不局限于特定数据类型（例如，名义型）的决策树分类器，并且同样适用于其他数据类型（例如，数值型）。决策树是一种简单但有效的工具，适用于许多分类问题，在某些情况下，可能会优于其他更复杂的方法，这些复杂方法对于简单数据集可能过于繁琐。

**决策节点、约束和叶节点**

决策树——顾名思义，是一种具有一组…的树形数据结构。
