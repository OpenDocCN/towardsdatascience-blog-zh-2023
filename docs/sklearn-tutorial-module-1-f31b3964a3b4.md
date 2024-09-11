# Sklearn 教程：模块 1

> 原文：[https://towardsdatascience.com/sklearn-tutorial-module-1-f31b3964a3b4?source=collection_archive---------2-----------------------#2023-11-22](https://towardsdatascience.com/sklearn-tutorial-module-1-f31b3964a3b4?source=collection_archive---------2-----------------------#2023-11-22)

## **我参加了官方的 sklearn MOOC 教程。这是我的收获。**

[](https://mocquin.medium.com/?source=post_page-----f31b3964a3b4--------------------------------)[![Yoann Mocquin](../Images/b30a0f70c56972aabd2bc0a74baa90bb.png)](https://mocquin.medium.com/?source=post_page-----f31b3964a3b4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f31b3964a3b4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f31b3964a3b4--------------------------------) [Yoann Mocquin](https://mocquin.medium.com/?source=post_page-----f31b3964a3b4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F173731d06320&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsklearn-tutorial-module-1-f31b3964a3b4&user=Yoann+Mocquin&userId=173731d06320&source=post_page-173731d06320----f31b3964a3b4---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f31b3964a3b4--------------------------------) · 9 分钟阅读 · 2023年11月22日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff31b3964a3b4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsklearn-tutorial-module-1-f31b3964a3b4&source=-----f31b3964a3b4---------------------bookmark_footer-----------)

在使用 Python 科学计算栈（NumPy、Matplotlib、SciPy、Pandas 和 Seaborn）多年后，我意识到下一步应该是 scikit-learn，即“sklearn”。

![](../Images/8bd6d36b3e9ecfab66a7bc1ed7c91f80.png)

图片来自 [Thought Catalog](https://unsplash.com/@thoughtcatalog?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 但为什么选择 sklearn？

在机器学习库中，scikit-learn是事实上的最简单、最容易学习的框架。它基于科学计算栈（主要是NumPy），专注于线性回归/支持向量机/降维等传统而强大的算法，并提供了许多围绕这些算法构建的工具（如模型评估和选择、超参数优化、数据预处理和特征选择）。

**但其主要优势无疑是文档和用户指南。你可以通过scikit-learn网站学到几乎所有内容，里面有很多例子。**

注意，其他流行的框架有TensorFlow和PyTorch，但它们有更陡峭的学习曲线，且侧重于计算机视觉和神经网络等更复杂的主题。由于这是我第一次真正接触机器学习，我决定从sklearn开始。

我几个月前已经开始阅读文档，但由于文档的庞大，感觉有点迷茫……
