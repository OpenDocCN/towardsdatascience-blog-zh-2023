# 如何解读线性回归系数 | 完整指南

> 原文：[https://towardsdatascience.com/how-to-interpret-linear-regression-coefficients-8f4450e38001?source=collection_archive---------4-----------------------#2023-05-24](https://towardsdatascience.com/how-to-interpret-linear-regression-coefficients-8f4450e38001?source=collection_archive---------4-----------------------#2023-05-24)

## 从简单到高级模型的完整指南

[](https://medium.com/@quentin.gallea?source=post_page-----8f4450e38001--------------------------------)[![Quentin Gallea, PhD](../Images/457af55dd9c6121da7ec97f8e2991c43.png)](https://medium.com/@quentin.gallea?source=post_page-----8f4450e38001--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8f4450e38001--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8f4450e38001--------------------------------) [Quentin Gallea, PhD](https://medium.com/@quentin.gallea?source=post_page-----8f4450e38001--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa52dcb9793ad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-interpret-linear-regression-coefficients-8f4450e38001&user=Quentin+Gallea%2C+PhD&userId=a52dcb9793ad&source=post_page-a52dcb9793ad----8f4450e38001---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8f4450e38001--------------------------------) ·16分钟阅读·2023年5月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8f4450e38001&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-interpret-linear-regression-coefficients-8f4450e38001&user=Quentin+Gallea%2C+PhD&userId=a52dcb9793ad&source=-----8f4450e38001---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8f4450e38001&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-interpret-linear-regression-coefficients-8f4450e38001&source=-----8f4450e38001---------------------bookmark_footer-----------)![](../Images/fba36a522cb3c2362814bc3156e8b333.png)

图片来自 [Unsplash](https://unsplash.com/fr/photos/cx8cofxiKYc) 的 Vitalii Khodzinskyi

在网上查找如何解释线性回归系数，就像查找如何在Python中导入CSV文件一样，许多人可以在脑海中获取这些信息。在过去十年里向1万名学生教授统计学之后，我仍然有时需要在一些特殊情况下（例如，二元结果和对数转换的解释变量）再次核对解释。这就是为什么我决定撰写这篇文章，其中包含了大量不同的线性回归模型，并解释了每种情况下系数的解释，包括对数转换变量、二元变量或交互项。

请注意，要完全理解（但不是必须的）本文的内容，你需要熟悉两个数学概念：[偏导数](https://en.wikipedia.org/wiki/Partial_derivative)和[条件期望](https://en.wikipedia.org/wiki/Conditional_expectation)。

在列出不同情况之前，让我介绍一些重要的定义和考虑因素（模型定义、准确持平、二元变量、多重共线性）。

## 目录表：

[**0\. 定义和重要考虑因素**](#95e4)
