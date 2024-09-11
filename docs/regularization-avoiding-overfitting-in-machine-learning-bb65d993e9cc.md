# 正则化：避免机器学习中的过拟合

> 原文：[https://towardsdatascience.com/regularization-avoiding-overfitting-in-machine-learning-bb65d993e9cc?source=collection_archive---------16-----------------------#2023-01-16](https://towardsdatascience.com/regularization-avoiding-overfitting-in-machine-learning-bb65d993e9cc?source=collection_archive---------16-----------------------#2023-01-16)

## 正则化如何工作以及何时使用

[](https://medium.com/@riandolphin?source=post_page-----bb65d993e9cc--------------------------------)[![Rian Dolphin](../Images/86716dde8ad84b693d071e3face37f40.png)](https://medium.com/@riandolphin?source=post_page-----bb65d993e9cc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bb65d993e9cc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bb65d993e9cc--------------------------------) [Rian Dolphin](https://medium.com/@riandolphin?source=post_page-----bb65d993e9cc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2f79d1e1bf6d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregularization-avoiding-overfitting-in-machine-learning-bb65d993e9cc&user=Rian+Dolphin&userId=2f79d1e1bf6d&source=post_page-2f79d1e1bf6d----bb65d993e9cc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb65d993e9cc--------------------------------) ·7 min read·2023年1月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbb65d993e9cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregularization-avoiding-overfitting-in-machine-learning-bb65d993e9cc&user=Rian+Dolphin&userId=2f79d1e1bf6d&source=-----bb65d993e9cc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbb65d993e9cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregularization-avoiding-overfitting-in-machine-learning-bb65d993e9cc&source=-----bb65d993e9cc---------------------bookmark_footer-----------)

## 什么是正则化？

正则化是一种在机器学习中使用的技术，用于解决我们在这个领域面临的一个问题：当模型在训练数据上表现良好，但在新的、未见过的数据上表现较差，这个问题被称为 [过拟合](https://medium.com/towards-data-science/overfitting-in-ml-avoiding-the-pitfalls-d5225b7118d)。

我陷入过拟合的一个明显迹象是，当模型在训练数据上表现良好，但在测试数据上表现很差。发生这种情况的原因是模型学习了训练数据的所有复杂细节，这意味着它不能对未见过的数据进行泛化。

正则化是一种解决此问题的方法，通过对具有大量大值参数的模型进行惩罚来实现。使用这种惩罚项意味着 *模型被鼓励仅学习数据中最重要的模式*，并避免陷入训练集中特定的噪声。

或者，至少，这是一个想法 👀 让我们深入探讨一下它是如何工作的。

## 正则化是如何工作的？

![](../Images/1e6c93cb41b4d3d97a5270d005e2d6d6.png)

图片由 [Ugur Akdemir](https://unsplash.com/ja/@ugur?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/lights?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

一般来说，在机器学习中，我们尝试学习一个模型（函数），它……
